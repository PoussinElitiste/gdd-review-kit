# Systems Designer — Round 1

Scope: core loop, progression, pacing, difficulty curve, economy math. Source: `gdd.txt` (GDD v0.2 plus linked docs Combo_system, Schemas_deplacement, Evenements_visuels_et_sonores, Themes_niveaux, Budget_tokens).

The move/switch/median-line rules are well specified and consistent with each other. The trouble starts where those rules connect to the counter, the attacks and progression. Those links are either missing or work against the stated design intent.

---

## Finding 1 — There is no difficulty curve to review: every tuning variable is deferred, and the run lengths go past the content

**Severity: BLOCKING**

**Problem.** Each lever that should shape difficulty across a playthrough is named but never given a value or a direction:
- Solo: the time limit, the gauge size per level ("la quantité de jauge à remplir ... augmente à chaque niveau"), and how a released counter turns into gauge fill.
- All modes: the spawn rate for continuous refill ("une fréquence qui augmente d'un niveau à l'autre").
- Arcade: AI aggression ("enchaîne les combos et envoie ses billes plus vite").
- Zen: the range of the "stress level" option.

The document that should hold this information, `Themes_niveaux.md`, is an empty table where every cell reads "À définir". It also says outright: "Reporté à la version suivante du GDD : Comment l'ordre des thèmes suit la montée de la difficulté (taille de jauge, fréquence d'apparition des billes, agressivité de l'IA)."

The structure also doesn't add up. Solo offers runs of "5, 10 ou 15 niveaux" and Arcade offers "5, 10 ou 15 adversaires", but the MVP has "5 niveaux avec leur thème". Nothing explains how levels 6-15 differ from levels 1-5: repeated themes, scaling parameters, or new AI profiles. A 15-level run with 5 themes and no stated way to scale past level 5 has no defined curve for two-thirds of its length.

**Source.** GDD "Mode de jeu", "Progression et thèmes", "Remplissage continu"; Themes_niveaux.md in full; MVP scope list.

**Why blocking.** The loop's only failure pressure is spawn rate (plus time in Solo). Without even relative values, nobody can tell whether a Solo run is winnable, whether Arcade 15 is just Arcade 5 repeated three times, or what the Agent Test sessions are supposed to check.

---

## Finding 2 — The Puyo-style counter-attack choice can't really be made under the stated timing and release rules

**Severity: MAJOR**

**Problem.** The Combo doc presents counter-play as the strategic heart of the game: "Le joueur a donc un vrai choix : libérer son compteur tout de suite pour se défendre, ou le laisser grossir." The math doesn't support this:
- **The defense window depends on the attacker's counter, not on anything the defender controls.** Waves arrive every 1.5 s, and no grace delay before the first wave is mentioned. A ×1 release lands its only wave in 1.5 s. A ×4 release is fully landed in about 6 s. Only waves still pending can be cancelled.
- **Releasing is never available on demand.** A release requires a full 5-ball median line in a color *other than* the counter's. Moves shift one cell at a time and switch only goes right, so building a specific 5-line takes many inputs. "Release now to defend" is therefore not a button. It is a full line-construction task that has to be finished in seconds.
- As a result, small and medium attacks can't be countered, and the "defend now vs. grow" dilemma only exists when a player happens to have a different-color 5-line almost finished. In Puyo Puyo, garbage waits until the defender's chain resolves. Here it runs on a clock, which removes the mechanic the GDD says it is borrowing.

**Source.** Combo_system.md "Attaque" (1.5 s delay, 3 balls per unit), "Contre-attaque", "Libération du compteur"; GDD "Règle de déplacement".

**Needed.** An initial grace period scaled to incoming size, or a pending queue that waits on the defender's next release. Also a decision on whether the 5-line release trigger is meant to be this expensive.

---

## Finding 3 — Combos and the "stack" tactic barely connect to the attack economy

**Severity: MAJOR**

**Problem.** The Combo doc calls stacking "La tactique à adopter", and the pitch is "faites monter vos combos et ensevelissez votre adversaire". But by the rules:
- The counter only gains from alignments **in the counter's color** (+1/+2/+3). "Un alignement d'une autre couleur n'a aucun effet sur le compteur."
- Attack size is purely counter value × 3, capped at 30 balls.
- The combo counter's only stated output is score ("les combos font monter le score"). "Le compteur de billes se remplit en conséquence" is ambiguous: no combo multiplier on the counter is defined anywhere.

So a 4-step cascade of non-counter colors adds nothing to an attack, and a cascade only helps if every stacked row is the counter color. With 5 random colors (about 20% each), that is the hardest possible setup. In VS and Arcade, the headline skill expression (chains) and the win condition (burying the opponent) run on separate tracks. Combos look nice but don't affect who wins. Filling the counter from 1 to 10 takes 9 gain points, which is three counter-color 5-lines or nine 3-alignments, and chains only help this in a narrow case.

There is also an unhandled interaction: a cascade can drop a full 5-line of another color onto the median and **force** a release at a bad moment. That cuts against the "let it grow" strategy, and the document never addresses it.

**Source.** GDD "Boucle d'une partie" steps 2-4, "Alignement et élimination"; Combo_system.md "Compteur de billes", "Barème", "Combos", "Tactique : le stack".

**Needed.** Decide what a combo is worth in versus play, for example a combo multiplier on counter gain or extra waves per chain step, and write the formula down.

---

## Finding 4 — The lose state and board saturation are defined incompletely and conflict with the switch action

**Severity: MAJOR**

**Problem.**
- **Game over ignores switch.** "Plateau bloqué : si le joueur ne peut plus faire aucun move ... c'est le game over." Switch is always possible on the median row, even when all five columns hold 15 balls. A switch can still create a 3-alignment, which removes balls and makes moves possible again. Read literally, the rule ends the game while a legal, possibly saving action is still available. The doc needs to say whether a full board is an instant loss or whether switch-only play continues.
- **Overflow is undefined.** Attack balls "s'empilent en haut de ses colonnes", and refill balls "apparaissent toujours par le haut de la colonne". No rule covers what happens when the target column's top ball is already at L1 (Schemas 4b explicitly allows a column to sit against L1). Does the column get pushed down, does the ball get rejected, or is it lost? The same gap exists for a wave whose shortest target column already holds 15 balls.
- **Time to loss is very long relative to the attack cap.** The board holds 75 cells and the minimum is 25 balls, so about 50 balls have to arrive before anyone is blocked. That is roughly 17 counter units, or two maxed ×10 releases with nothing cancelled. Combined with Finding 2, matches will be decided mostly by continuous refill, not by attacks. That may be acceptable, but it is the opposite of the "ensevelissez votre adversaire" fantasy, and nothing shows it was intended.

**Source.** GDD "Cas limites" ("Plateau bloqué", "Remplissage continu"), "Règle de déplacement"; Combo_system.md "Attaque", "Répartition d'une vague"; Schemas_deplacement.md section 4.

---

## Finding 5 — Systems referenced in every mode have no specification: bonus/malus marbles, Solo scoring, the spawn color guarantee

**Severity: MAJOR**

**Problem.** Several pieces of the economy are cited as active but never defined:
- **Bonus/malus marbles** appear in Solo, Arcade and VS progression ("peuvent apparaître à chaque élimination de billes"), Zen lets players toggle them, and the event table gives them a spawn effect. Yet "Billes bonus/malus" appears in "Documents liés" with no link and no content. No effects, frequency or scaling are given. A malus in VS directly changes the attack economy above, so it can't be tuned separately.
- **Solo score** is "basé sur le compteur de combo et l'utilisation du compteur de billes", but there is no formula, even though a leaderboard depends on it.
- **The spawn rule contradicts itself.** "Lorsqu'une bille disparaît, une nouvelle bille est générée aléatoirement pour garantir au minimum 5 billes de même couleur" sits next to "ne crée jamais d'alignement tout fait sur la ligne médiane." The doc doesn't say whether the 5-of-a-color guarantee is board-wide or per color, and doesn't say whether it is biased toward the counter's color. That bias decides how quickly the counter can fill, so this is economy-critical, not just an implementation detail.

**Source.** GDD "Mode de jeu", "Progression et thèmes", "Cas limites", "Documents liés"; Evenements_visuels_et_sonores.md (bonus/malus row).
