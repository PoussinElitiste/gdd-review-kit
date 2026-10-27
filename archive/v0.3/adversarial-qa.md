# Adversarial QA — Round 1

Reviewer lens: contradictions, edge cases, soft-locks, exploits, undefined fail states.

---

## 1. Gravity + "spawn from the top" breaks the median-line invariant and the "no random alignment" rule — BLOCKING

**Problem.** The movement rules promise that a column "garde toujours au moins une bille sur la ligne médiane" and "reste toujours sélectionnable." The elimination rules break that promise. Take a 5-marble column moved all the way down so its *first* marble sits on the median line (Schémas 4a: rows L8–L12, which is allowed). If that marble is part of an elimination, nothing sits above it to fall. The column is now L9–L12, and no marble is on the median line. The refill rule says the new marble "apparaît toujours par le haut de la colonne", which here means L8, the median line. That causes two failures:
- If the spawn really lands at L8, a randomly generated marble is placed directly on the median line. That contradicts "la génération des billes [...] ne crée jamais d'alignement tout fait sur la ligne médiane" unless the generator is constrained, and no such constraint is specified. It can also trigger a free combo.
- If the spawn goes anywhere else, the column has left the median line and can't be selected again. That's a permanent dead column and a soft-lock path.

**Source.** GDD "Règle de déplacement" (line 68), "Alignement et élimination" (line 83), "Pas d'alignement dû au hasard" (line 93), Schémas 4a (line 355). Nobody defines what "tomber" means for a floating column, or which way the column re-anchors after it loses its median marble.

**Severity:** BLOCKING. This is the core loop, and the Gameplay agent can't implement it without inventing a rule.

---

## 2. The game-over / win condition is wrong or undefined at the boundary — BLOCKING

**Problem.** "Plateau bloqué" triggers game over when the player "ne peut plus faire aucun move" because every column is blocked both ways. By the stated limits, a column of length 1–14 can always move at least one step. So the condition only fires when all 5 columns hold 15 marbles (75 marbles). Three holes follow:
- **The switch is ignored.** At a full board the player can still switch the median row. Because the switch wraps c5 to c1, it can create a contiguous triple (e.g. `R R B G R` becomes `R R R B G` after one switch), clear marbles and un-block the board. The game declares defeat while a legal, scoring action still exists. The doc also never says whether a wrapped c4-c5-c1 group counts as aligned.
- **Overflow is undefined.** Attack waves "s'empilent en haut de ses colonnes", and continuous fill keeps adding marbles. The doc doesn't say what happens when the target column is already at 15, or when its top marble is at L1 and it can't extend upward. Does the column get pushed down, do marbles go to another column, are they lost, or is it instant defeat? The wave rule ("colonnes les plus courtes") breaks once every column is full.
- **Simultaneous block in VS/Arcade.** Both boards can fill on the same tick, for example from continuous fill. That's a draw case with no rule, inside a best-of-1/3/5 match structure.

**Source.** GDD "Plateau bloqué" (line 97), "Remplissage continu" (line 95), Combo system "Attaque" (lines 282, 292), Schémas 4b and "une colonne de 15 billes ne bouge plus" (lines 356–358).

**Severity:** BLOCKING. The lose/win condition for three of the four modes is either exploitable or unimplementable as written.

---

## 3. Releasing the counter can become impossible, which soft-locks Solo — MAJOR

**Problem.** Releasing the counter is the only way to fill the Solo gauge and the only way to attack in VS/Arcade. It needs a full 5-marble line of one colour that is *not* the counter colour. Nothing guarantees that 5 marbles of a non-counter colour exist on the board. The spawn guarantee also contradicts itself:
- line 83: a new marble appears "pour garantir au moins 5 billes **par colonne**";
- line 91: a new marble is generated "pour garantir au minimum 5 billes **de même couleur**".

These are different invariants. Even the colour version is satisfied by 5 marbles of the *counter* colour, which can't trigger a release. Separately, the "no random alignment on the median" constraint biases generation in ways nobody has specified. A Solo run can therefore reach a state where the gauge is mathematically impossible to fill before the timer runs out, and nothing signals it to the player. Zen has the same dead state, and there it never ends because Zen has no game over.

**Source.** GDD "Alignement et élimination" (lines 83, 85), "Cas limites" (lines 89, 91, 93), Combo system "Libération du compteur" (line 272).

**Severity:** MAJOR.

---

## 4. MVP content and systems are referenced but don't exist (level counts, difficulty, bonus/malus) — MAJOR

**Problem.**
- **Level count contradiction.** Solo offers runs of "5, 10 ou 15 niveaux" and Arcade offers "5, 10 ou 15 adversaires [...] à travers différents niveaux", but the MVP ships "5 niveaux". Nothing says how a 15-level run maps onto 5 themes (repeat? cycle with higher difficulty?). Nothing says how difficulty keeps rising past level 5 when it is tied to themes that are entirely "À définir".
- **Difficulty is unparameterised.** "Augmente à chaque niveau" appears for gauge size, spawn frequency and AI aggression, with no values. The Themes doc defers this explicitly. The Agent Test can't verify "la boucle de jeu de chaque niveau" against an undefined target.
- **Bonus/malus marbles are in MVP scope but have no spec.** They appear in Solo, Arcade, VS and Zen (Zen even has "options pour choisir les bonus/malus actifs"), and there's an MVP-priority-adjacent sound/visual event for them. The linked "Billes bonus/malus" document doesn't exist (no link, no content). The Solo mode description at line 13 doesn't mention them, but Progression says they appear in Solo. Unknown effects means unknown interactions with every rule above: a malus that adds marbles at 15, a bonus that creates a median alignment, and so on.

**Source.** GDD "Mode de jeu" (lines 13–15), "Progression et thèmes" (lines 133–159), MVP (lines 187–191), "Documents liés" (line 233), Themes_niveaux.md (whole table "À définir").

**Severity:** MAJOR.

---

## 5. Local two-player input and first-game onboarding are undefined — MAJOR

**Problem.**
- **Shared-keyboard VS.** Local VS is an MVP pillar ("party game"), and the doc says it works "en local à deux sur toutes les plateformes". But the PC scheme gives *one* player both arrows and WASD, and both Space and right Enter, plus the mouse wheel. Nobody says which keys belong to P1 or P2, whether gamepads are required, or how two players share a mouse-wheel move. On mobile, nobody says how two players share one touchscreen with two boards.
- **Onboarding trigger is exploitable or easy to lose.** The passive hints and the alignment highlighting are active "uniquement lors de la toute première partie du joueur, quel que soit le mode". Edge cases:
  - The player's first game is local VS. Whose first game is it, and are hints shown on both boards, which gives an unfair assist in a competitive match?
  - The player's first game is a timed Solo run they lose in seconds, or quit from the menu. The tutorial is then gone permanently.
  - Zen auto-restarts. Does the restart count as a new game and switch the hints off?
  - Zen is billed as "tutoriel", yet its hint system expires after one game.

**Source.** GDD "Contrôles et onboarding" (lines 103–129), "Mode Zen" (line 19), "Mode VS" (line 17).

**Severity:** MAJOR.

---

### Also noted (not counted)
- The first attack wave has no initial countdown. Only the 1.5 s gap *between* waves is specified, so wave 1 may land before the target can react to it (Combo system, line 290).
- "Il n'est pas possible d'annuler un switch" contradicts line 75, which says 4 right switches equal 1 left switch, so a switch can always be undone at the cost of time.
- The Solo leaderboard relies on random board generation with no seed, so leaderboard runs aren't comparable.
