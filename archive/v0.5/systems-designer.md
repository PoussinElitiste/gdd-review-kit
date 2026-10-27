# Systems Designer — Round 1

Scope: core loop, progression, pacing, difficulty curve, resource/economy math. Every finding below traces to a passage in `gdd.txt` (GDD_V_0_2 plus linked docs) or to something it conspicuously leaves out.

---

## 1. The difficulty curve has no numbers anywhere, and the only file meant to hold them is empty — BLOCKING

**Problem.** Every difficulty lever the game relies on has no value: continuous fill rate, solo time limit, solo gauge size per level, AI aggression, how often bonus/malus marbles appear, and the base rate that sudden death doubles. The continuous fill is described as the thing that guarantees every game ends ("une partie ne peut pas durer indéfiniment, même en VS si aucun joueur n'attaque"). It is also the only penalty for random play outside solo. Despite that, its base value and its increase per level are never stated. Sudden death ("la vitesse de remplissage double toutes les 30 secondes") doubles an unknown number. Solo has two fail states, the time limit and a blocked board, and the document never says which is supposed to be the main threat at which level.

**Source.**
- "Remplissage continu ... avec une fréquence qui augmente d'un niveau à l'autre (voir Progression et thèmes)". Progression et thèmes only says "les billes apparaissent plus fréquemment" and then points to Themes_niveaux.md.
- Themes_niveaux.md: "Document en attente de rédaction". Every cell in the table, including the *Difficulté* column, is "À définir". The theme-to-difficulty link is "Reporté à la version suivante du GDD".
- Solo: "remplir une jauge de progression en temps limité". Neither the time nor the gauge size is given.
- Arcade: "l'IA devient plus agressive : elle enchaîne les combos et envoie ses billes plus vite". There is no parameter, and Agent NPC (25 Opus sessions, the "moment wow") has nothing to tune against.

**Why it matters.** With a 7-week schedule that includes writing the GDD, this is the curve that turns a 5-level course into a progression. As written, the cross-references go GDD → Progression → Themes → empty table. Nobody can build or test a level pacing pass (Agent Test "teste la boucle de jeu de chaque niveau") without a target.

---

## 2. The board math contradicts the attack fantasy: early attacks make the opponent's columns *more* mobile — MAJOR

**Problem.** The documents say more marbles means less movement ("Plus une colonne contient de billes, moins elle peut bouger"; attacked columns "s'allongent et bougent de moins en moins"). The movement rules say otherwise. A column must keep at least one marble on the median line (L8) and stay inside L1–L15, so its number of legal positions is:

| Column length | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Legal positions | 5 | 6 | 7 | **8** | 7 | 6 | 5 | 4 | 3 | 2 | 1 |

Mobility goes **up** from 5 to 8 marbles and only falls after that. A column of 5 to 8 also keeps every one of its marbles reachable on the median line, so an attack that adds 1 to 3 marbles per column gives the defender more colors to work with. Attack waves fill "les colonnes les plus courtes" first and spread evenly. That means a typical early or mid release, such as ×4 = 12 marbles across 5 columns, makes the target's position *better*. Attacks only start to hurt once columns pass 8, which is where marbles at the ends become permanently unreachable.

The same rules mean a column only stops moving at 15 marbles. "Plateau bloqué" therefore effectively requires a completely full 75-cell board. Combined with item 1, the time it takes to reach a loss is unknown.

A smaller related error: "au plus 30 billes, ce qui empêche de remplir le tableau adverse d'un seul coup" is only true while the opponent has 45 marbles or fewer. Free capacity runs from 50 cells down to 0, so a ×10 release can finish a board that already holds more than 45.

**Source.** Schemas_deplacement.md §4 and the line "Plus une colonne contient de billes, moins elle peut bouger". Combo_system.md, *Attaque*. GDD *Règle de déplacement* (column-length and board-height limits). Combo_system.md, *Colonne pleine* and the ×10 cap.

---

## 3. The release action, which is the hinge of the loop, can be completed in essentially one way, and nobody has modeled how often it happens — MAJOR

**Problem.** Releasing the counter needs a full 5-of-a-kind line on the median line in a non-counter color. But any 3 contiguous same-color marbles on the median line are removed at once (*Cascade*: "le jeu vérifie s'il y a un nouvel alignement sur la ligne médiane. Si oui, il est éliminé"). So a 5-line can't be built up gradually. It has to appear all at once, and with these rules that means the median line reads `X X ? X X` and a single Move on c3 completes it. Every intermediate state with 3 in a row eliminates itself. The Switch makes this worse: one right-rotation of `X X Y X X` gives `X X X Y X`, which fires a 3-alignment and destroys the setup. The stack route has its own limit. A stored 5-line only drops intact if all five columns below it lose a marble together, which means a 5-line elimination must already have happened underneath it. A 3-line elimination drops only 3 columns and breaks the stored line apart.

The same constraint applies to 4-lines (`X X ? X` / `X ? X X`) and to the +3 "ligne complète" bonus on the counter.

**Source.** GDD *Boucle d'une partie* step 4 ("former une ligne complète de 5 billes ... pour libérer le compteur"). *Ordre de résolution*. Combo_system.md *Libération du compteur* and *Un seul alignement à la fois*. The generation guarantee only promises that 5 marbles of a non-counter color exist *somewhere on the board*. That is far from a reachable `XX?XX` setup.

**Why it matters.** The release rate sets the attack frequency in VS and arcade and the speed of gauge progress in solo. It is the economy's main throughput variable, and it is never estimated. The onboarding only teaches 3-alignments ("met en surbrillance ... pour aligner 3 billes"), so a new player is never shown how the main objective is actually completed.

---

## 4. Combos, the advertised skill expression, have no effect on the attack or gauge economy — MAJOR

**Problem.** The stack-and-cascade tactic is presented as "la tactique à adopter", and the Puyo Puyo inspiration is quoted. The rules never connect it to what wins a match. The counter scale is +1 / +2 / +3 by alignment size, only for the counter's color, with no combo term. Alignments in other colors do nothing ("un alignement d'une autre couleur n'a aucun effet sur le compteur"). The loop says combos raise the **score**, but VS and arcade have no score and no leaderboard. As a result, a 4-chain cascade in VS sends exactly as many marbles as four separate counter-color alignments, and if the stacked lines aren't the counter's color it sends zero.

The documents contradict each other on this. Combo_system.md says "Le compteur de combo augmente alors, et le compteur de billes se remplit en conséquence", which implies a combo multiplier, and the scale table has none. The arcade AI's escalation is defined as "elle enchaîne les combos", a behavior that by the written rules gives it no extra attack power.

**Source.** GDD *Boucle d'une partie* step 3 ("les combos font monter le score"). Combo_system.md *Barème*, *Combos*, *Tactique : le stack*. GDD *Progression*, Arcade bullet.

**Why it matters.** In Puyo Puyo, chains matter because the chain length multiplies the garbage sent. Without that link, VS rewards collecting colors, not building chains. The "vrai choix" between defending now and building a bigger counter-attack exists only in terms of counter value, and combos play no part in it.

---

## 5. Bonus/malus marbles are a progression lever in every mode but have no spec, and they would break the board's guarantees — MAJOR

**Problem.** Bonus/malus marbles "peuvent apparaître à chaque élimination de billes" in Solo, Arcade and VS. They are listed as part of each mode's progression, Zen exposes "options pour choisir les bonus/malus actifs", and their appearance is a tracked feedback event. Yet the effects, spawn rate, counterplay, and how they interact with the counter all go undefined. The linked "Billes bonus/malus" document isn't provided, and it isn't in the MVP or the cut list either. They can spawn on every elimination, so they will directly affect two guarantees: the determinism promise ("même situation, même résultat") and the generation guarantees (no free alignments on the median line, and 5 marbles of a non-counter color always present). Nothing says whether a bonus/malus marble counts as a color, can be part of an alignment, or can block a release.

**Source.** GDD *Mode de jeu* (Arcade, VS). *Progression et thèmes* (all four mode lists). *Cas limites* (generation guarantees, determinism). *Documents liés*: "Billes bonus/malus" has no link. Evenements_visuels_et_sonores.md: "Bille bonus ou malus qui apparaît", priority Important.

**Why it matters.** This is an unspecified system in every competitive mode, and it sits on top of a resolution order that is otherwise carefully defined. Either cut it from the MVP explicitly or specify it before Agent Gameplay builds the resolution pipeline. Retrofitting it later means reworking the cascade/refill ordering.
