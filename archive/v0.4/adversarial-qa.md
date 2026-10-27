# Adversarial QA — Round 1

Reviewer lens: contradictions, unhandled edge cases, sequence breaks, exploits, unplanned fail states.
Source: `gdd.txt` (GDD_V_0_2.md + Combo_system.md + Schemas_deplacement.md + Evenements_visuels_et_sonores.md + Themes_niveaux.md + Budget_tokens.md).

---

## Finding 1 — Post-elimination resolution order is undefined, and one possible order breaks the "no random alignment" rule

**Severity: BLOCKING**

**Problem.** After an elimination on the median line, four rules all fire, and the document never says in what order:
1. the marbles above fall into the empty cells ("les billes situées au-dessus tombent"),
2. a new random marble appears at the top of any column with fewer than 5 marbles,
3. a column with no marble left on the median line moves back up until its first marble is on the median line,
4. a chain check runs (combo cascade).

The order changes the result. Take the document's own example: a column in L8–L12 whose L8 marble is eliminated (GDD, "Colonne toujours sur la ligne médiane").
- **Refill first:** the column is L9–L12 (4 marbles), and the new marble spawns at the top, L8, which is directly **on the median line**. If this happens in all 5 columns after a full-line elimination, the median line is filled with 5 randomly generated marbles.
- **Auto-rise first:** the column moves to L8–L11, then the new marble spawns at L7, above the median line, and it can come down later as part of a cascade.

These give different boards, different combos and different counter gains. The "Pas d'alignement dû au hasard" guarantee also has no clear scope. It covers "la génération", but it doesn't say whether that includes (a) refill marbles that land on the median line, (b) incoming attack marbles in VS/arcade, or (c) stored alignments that fall onto the median line. If (c) counts, the guarantee cancels the stack tactic, which Combo_system.md calls the core skill. If (a) or (b) don't count, free alignments can happen. The document also doesn't say whether player input is locked during a cascade, or whether incoming waves and continuous fill can land in the middle of one.

**Source.** GDD "Alignement et élimination" (fall + refill + "apparaissent toujours par le haut"), "Colonne toujours sur la ligne médiane", "Pas d'alignement dû au hasard"; Combo_system.md "Tactique : le stack", "Un alignement à la fois".

**Why it blocks.** This is the core resolution step that runs on every elimination. Agent Gameplay (40 Opus sessions) and Agent NPC both need one fixed, deterministic order. Without it, VS fairness, leaderboard scores and AI behaviour all depend on whatever order the implementation happens to use.

---

## Finding 2 — The "release is always possible" guarantee is false once columns get long, and nothing handles the soft-lock it creates

**Severity: MAJOR**

**Problem.** The generation guarantee promises at least 5 marbles of one non-counter colour on the board "pour qu'une libération du compteur reste toujours possible". Having the marbles on the board doesn't mean the player can reach them. A column of length n can only bring some of its marbles onto the median line (L8). A full column (15 marbles, L1–L15) can't move, so only its L8 marble can ever be used (Schemas, "une colonne de 15 billes ne bouge plus"). A 14-marble column has two positions. In mid-to-late VS/arcade, attacks push columns towards 15, and the 5 "guaranteed" marbles can sit in rows that will never reach the median line. In that state, release is impossible, but the game isn't over: game over only happens when **every** column is fully blocked (= all 75 cells full). The player then has to wait out a match they can't win or defend, because counter-attack depends on releasing.

Related gaps:
- Attack marbles are added to the target's board, and the document doesn't say whether they are generated under the same two guarantees (colour, no median alignment) or are purely random.
- When the board is close to full, the generator may have no free cell to place a marble that restores the colour guarantee. The document doesn't say what happens then.
- Attack marbles "s'empilent en haut de ses colonnes". If a column already touches L1 (allowed: "une colonne de 10 billes qui touche le haut (L1)"), there is no cell above it. The document doesn't say whether the column is pushed down, whether that push is limited by the median-line rule, or whether the marble goes to the next shortest column.

**Source.** GDD "Garanties de génération", "Plateau bloqué"; Schemas_deplacement.md section 4; Combo_system.md "Attaque", "Colonne pleine".

---

## Finding 3 — Courses of 10/15 levels against a 5-level game; difficulty scaling past level 5 is undefined

**Severity: MAJOR**

**Problem.** Solo offers "parcours de 5, 10 ou 15 niveaux", and arcade offers "5, 10 ou 15 adversaires ... à travers différents niveaux". But the MVP and the Progression section fix exactly **5 levels/themes**, and Themes_niveaux.md is all "À définir", including how difficulty maps to themes. Questions the document doesn't answer:
- What do levels 6–15 look like? Do themes repeat? If they do, does "la quantité de jauge à remplir ... augmente à chaque niveau" keep scaling with no limit, so that a 15-level run eventually becomes impossible to finish within the time limit?
- Does "Agent Intro : une séquence d'introduction différente pour chaque niveau" mean 5 intros or 15?
- "Une introduction se joue" before each game: can it be skipped on repeated runs, or in level 11 when the same theme comes back?
- Spawn frequency rises every level in every mode "y compris VS", but a VS match is a best-of on one level. The document doesn't say what "level" means in VS, or who picks it.

**Source.** GDD "Mode de jeu et source de pression"; "Progression et thèmes"; MVP "5 niveaux avec leur thème"; Themes_niveaux.md (entirely undefined); Agents Roles (Agent Intro).

---

## Finding 4 — Bonus/malus marbles are active in every competitive mode but completely unspecified, and the sections contradict each other on where they appear

**Severity: MAJOR**

**Problem.** The concept section lists bonus/malus marbles for **Arcade and VS only**. The Progression section adds them to **Solo** as well ("des billes bonus/malus peuvent apparaître à chaque élimination"), and Zen has options to choose which ones are active. The design doc for them ("Billes bonus/malus") appears only as an unlinked title in "Documents liés", and it is neither in the MVP nor in the cut list. That leaves the following open:
- Solo is the **ranked leaderboard** mode. A random bonus spawn "à chaque élimination" makes leaderboard scores depend on luck, and players can farm them (for example, by triggering many cheap 3-marble eliminations to roll for bonuses).
- It's unclear whether a bonus/malus marble has a colour. Can it complete a 3- or 5-line? Does it count towards the "5 marbles of a non-counter colour" guarantee? Can it sit on the median line without breaking "no random alignment"?
- A malus can presumably lock or remove things, so it can trigger exactly the soft-lock in Finding 2.
- The event table already has an "Important" event "Bille bonus ou malus qui apparaît", so the Sound and Visual agents are being asked to build feedback for a system that doesn't exist.

**Source.** GDD "Mode de jeu" (Arcade, VS) vs "Progression et thèmes" (Solo, Arcade, VS, Zen); "Documents liés"; Evenements_visuels_et_sonores.md.

---

## Finding 5 — Local VS (the headline party mode) has input, assist and draw edge cases nobody has planned for

**Severity: MAJOR**

**Problem.** Local VS is the mode the document pushes hardest ("mis en avant"), and it is in the PC MVP. Three problems:

1. **Input collision on PC.** The PC controls are written for one player. Space **or** right Enter are alternatives for the same Switch, and arrows **or** A/D are alternatives for the same cursor. The mouse (selection + wheel for Move) is a single shared device, and the document doesn't say which board it controls. It also doesn't say whether controllers are supported on PC; the controller scheme is listed only under "Sur console". Nothing defines how two players share one PC, which is the MVP platform for a party game. Cursor behaviour at the edges (pressing right on c5: wrap around or stop?) is also missing.
2. **First-game assists in VS.** The passive onboarding and the "aligner 3" highlight run during "la toute première partie du joueur, quel que soit le mode". In local VS, it's unclear whose first game counts, given there's no profile per player. If only one board gets highlights, one player gets a live hint system in a competitive match. The same rule means a new player whose first game is **ranked Solo** posts a leaderboard score with the auto-hints on.
3. **Draws.** "Blocage simultané en VS : manche déclarée égale." This rule is written only for VS, not arcade (AI vs player can also lock at the same time). The document doesn't say whether a drawn round counts towards "1, 3 ou 5 manches gagnantes". If it doesn't, a best-of-1 can repeat forever with no tiebreak. It also doesn't define "au même moment" (same frame? same wave window?).

Linked flow gap: the end screen offers "recommencer en cas de défaite", but it doesn't say whether a loss at level 9 of a 15-level Solo course, or opponent 9 of 15 in Arcade, restarts the level or the whole course. It also doesn't say how the cumulative leaderboard score is handled on a retry. If only the level restarts and the score carries over, a player can retry the same level for a better run, which exploits the leaderboard.

**Source.** GDD "Contrôles et onboarding" (PC block, "Note : le jeu peut se jouer en local à deux sur toutes les plateformes", passive onboarding, "Règle d'alignement horizontal"); "Blocage simultané en VS"; "Boucle de jeu" end-screen options; "Public visé".

---

## Round 2 — Cross-examination

### Conflicts

**C1 — vs. Systems Designer F2 ("attacks make the victim's board more flexible").** I agree with the low-volume half and disagree with how it is framed. The mobility table assumes a column can occupy any legal window around L8 after receiving marbles. The document does not support that. Attack marbles "s'empilent en haut de ses colonnes", and my Round 1 F2 shows there is no rule for a column whose top already touches L1. A 10-marble column at L1–L10 that receives one more marble has to go somewhere: shift down, overflow to another column, or be dropped. Each option gives a different mobility count. So "more marbles = more positions" up to length 8 is true only under one placement rule that the document never chose. The finding is directionally right, but its numbers depend on an edge case nobody has specified. I would present it as "the penalty curve is undefined", not "the penalty curve is inverted".

**C2 — vs. Player Psychologist F3 ("game over while a switch is still possible feels unfair").** This is filed as a perception problem. In one reading of the rules it is a real rules bug. Systems Designer F4.1 points out that it is undefined whether matches wrap from c5 to c1. Suppose wrap does **not** count, which is the reading "5 cases ne peuvent pas contenir deux alignements de 3" implies. Then on a fully blocked board (75/75), a median row such as `X X Y Z X` becomes `X X X Y Z` after a single switch, and that is a legal 3-match that clears marbles and unblocks the board. The document still declares game over "même si un switch reste possible". The player is not just *feeling* that they had a move left; they did have one, and it would have saved them. If wrap **does** count, a switch on a full row only rotates the same circular row and can never create a match, so the rule is sound and only needs to be explained on screen. Psych F3's severity therefore depends on the wrap decision. Under the no-wrap reading it should be MAJOR-to-BLOCKING as a correctness bug, not just as a UX issue.

**C3 — vs. Business Analyst F1 ("one competitive mode: Arcade *or* local VS") and Feasibility Lead F1 ("Solo + local VS + Zen").** From an edge-case standpoint these cuts are not equivalent, and I side with Feasibility. Arcade carries every VS edge case (resolution order, attack placement, soft-lock, stall). On top of that it has an AI with no spec (Feasibility F3) and a simultaneous-block case the document only resolves for VS (my F5.3). Local VS's only unique gaps are input mapping and per-player assists, both of which a short spec can fix. Business also proposes "Zen as the tutorial", but Psych F1 shows that Zen has no tutorial content. And per Systems F3, Zen with continuous fill and no timer has no guaranteed end. A tutorial mode that can stall forever with no fail state teaches the player nothing about the loss condition.

### Connections

**X1 — The end-state rules are broken at both ends (my F2 + Systems F3 + C2 above).** Combining the three gives one systemic defect:
- *Too late:* the game only ends at 75/75 (Systems F3). A player whose reachable colours cannot form a non-counter 5-line (my F2) cannot attack, cannot counter-attack, and cannot lose. In VS, with no round timer, both players can enter this state and the round never ends.
- *Too early:* under the no-wrap reading, game over is declared while a switch could still clear marbles (C2).

The "Plateau bloqué" rule is the only terminal condition in VS and Arcade, and it is wrong in both directions. That is BLOCKING, not two separate MAJORs.

**X2 — Shared audio in local VS (Narrative F5 / Psych F4 + my F5).** Both colleagues note that in VS the music "gagne une couche" when "le tableau du joueur commence à se remplir". Local VS shows two boards on one screen with one audio mix. The document does not say *whose* board drives the layers or the "Effet de panique". Whichever rule is picked, one player gets audio danger cues and the other doesn't, or the cues fire for the wrong person. Either way, the cue becomes an information leak about the opponent's state. This is undefined behaviour in the featured mode, not just a tonal choice.

**X3 — Draws have no ending (Narrative F4 + my F5.3).** Narrative shows that victory and game over use word-for-word the same effect. The VS rule also declares a third outcome, "manche déclarée égale", and the events table has no event for it at all. A drawn round currently has no ending sequence and no defined effect on the best-of count.

**X4 — The leaderboard is unauditable (Systems F5 + my F4 and F5).** Systems F5 shows that the score formula does not exist. That means my exploit vectors can't be ruled in or out: bonus farming via cheap 3-matches, retry-a-level score carry-over, and first-game auto-hints on a ranked run. Business F4 names the leaderboard as the *only* remaining community hook. The one feature carrying the early-access community claim has no formula and three known abuse paths.

**X5 — The colour guarantee vs. automatic release (Systems F4.3 + my F1).** Release fires automatically on any non-counter 5-line. If refill marbles can land on the median (refill-first order, my F1), the generator can complete a 5-line by chance. That fires a random release, which sends a random attack to the opponent in VS. The "pas d'alignement dû au hasard" rule therefore also has to cover random *releases*. Otherwise RNG decides attacks.

### Revisions

- **F2: upgrade MAJOR → BLOCKING**, reframed as "terminal conditions are wrong in both directions" (see X1). With Systems F3's proof that a block requires 75/75 and the switch case from C2, this is no longer a corner case. It governs whether VS rounds end at all and whether they end correctly.
- **F1: stays BLOCKING, scope widened.** The wrap rule (Systems F4.1) is a fifth unspecified input to resolution. It decides whether `X Y Z X X` already sits on the median as a match, so it changes what "no random alignment" has to check.
- **F3: downgrade MAJOR → MINOR** for my lens. Systems F1, Feasibility F1, Narrative F3 and Psych F5 all raise the core 5-vs-15-levels issue as a design and scope problem. What is uniquely mine (intro skipping, what "level" means in VS) is minor next to that.
- **F4: stays MAJOR, but now conditional.** If the board adopts the Feasibility and Business recommendation to cut bonus/malus from the MVP, F4 goes away, *provided* the Solo progression text and the "Important" bonus/malus event row are also removed. Otherwise the contradiction survives the cut.
- **F5: stays MAJOR.** Feasibility F4 independently confirms the input gap. X2 and X3 add two more undefined behaviours to the same featured mode.
