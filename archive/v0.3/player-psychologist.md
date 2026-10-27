# Player Psychologist — Round 1

Lens: what a real player experiences in the first hour, where they get confused or frustrated, and whether the reward structure motivates them.

---

## 1. Onboarding teaches only the easy 10% of the game, then disappears for good — BLOCKING

**Problem.** The only onboarding is passive. If the player does nothing for 5 seconds, the game highlights a move that makes a 3-alignment, and it also highlights 3-alignments that are available. Both aids run "uniquement lors de la toute première partie du joueur, quel que soit le mode". Nothing teaches the systems that decide whether you win:
- the counter's color, and why most alignments "do nothing" (a muted burst, no effect on the counter);
- the release rule: a full line of 5, in a color *different* from the counter;
- the stack tactic, which the Combo doc calls "La tactique à adopter";
- counter-attack and wave cancellation;
- what makes columns lock up, and why that ends the game.

A player who picks Arcade or VS first gets the aids only while also facing an AI or a friend. After that one game, all help is gone. The doc says Zen "sert aussi de mode d'entraînement et de tutoriel", but nothing in it actually teaches. It is just the normal game with a slower fill rate and no game over.

**Source.** GDD "Contrôles et onboarding" (passive onboarding, alignment highlighting). GDD "Mode Zen". Combo_system "Tactique : le stack" and "Contre-attaque". No structured tutorial is described anywhere.

**Likely player experience.** They make 3-alignments, nothing visibly good happens, they never find out how to "release", and they lose in VS without understanding why. That is a churn point inside the first session, and early access is exactly where first-hour reviews decide the game's fate.

---

## 2. The core reward rule works against the player's intuition — MAJOR

**Problem.** A full line of 5 is the hardest and most satisfying thing to build. If it matches the counter's color, it does *not* release the counter: it only adds +3. To cash out, the player has to build a full line in a *different* color. On top of that:
- the counter's color is random at the start;
- alignments in any other color have no effect on the counter at all;
- once the counter releases, it resets to ×1 and takes a new color.

So the player has to track two goals at once ("feed color X" and "finish a line in not-X") while dodging a third trap: finishing a 5-line in another color by accident. That would dump the counter early, and the doc gives no way to hold it back. In VS this undercuts the "vrai choix" the Combo doc promises (release now, or let it grow). A player who completes a line at the wrong moment will feel punished for playing well.

**Source.** GDD "Boucle de jeu" step 4. GDD "Alignement et élimination" ("Une ligne complète de la couleur du compteur ne le libère pas"). Combo_system "Libération du compteur" and "Contre-attaque". The events table lists "Alignement d'une autre couleur que le compteur → éclat atténué, sans effet".

**Likely player experience.** "I made a perfect line of five and nothing happened" alternates with "I made a line and lost my ×8". Both feel arbitrary unless the UI keeps showing which color feeds the counter and which colors would trigger a release. The doc specifies no such indicator beyond the counter marble changing color.

---

## 3. "Doesn't need much control skill" is contradicted by an indirect, one-way, no-undo control model — MAJOR

**Problem.** The audience is described as "très large, car le jeu ne demande pas une grande maîtrise des commandes". But the controls are indirect:
- the player only ever touches the middle row;
- a switch rotates the whole row, and only to the right;
- moving a marble one cell left takes 4 switches;
- a switch can't be undone ("afin de pousser le joueur à anticiper").

Moving one marble to a target cell takes a move, then up to 4 switches, then another move on a different column. The player has to plan all of that under a time limit (Solo) or a continuously filling board (every other mode). Each switch also shifts all 5 middle-row marbles, so it can break an alignment the player was building on that row. Expert puzzle fans will enjoy this kind of planning. For casual and party-game players, the stated target, it is the steepest part of the learning curve, and none of it is taught (see Finding 1). On PC, a mouse player picks with the mouse and moves with the wheel, but has to switch with Space or right Enter, so they need both hands for a three-action game.

**Source.** GDD "Public visé". "Règle de déplacement" (switch, no undo, "4 switchs vers la droite équivalent à 1 vers la gauche"). "Contrôles et onboarding", PC bindings.

**Likely player experience.** Lots of overshoot ("I switched one too many and now I have to go all the way around"), plus feeling that the time pressure is about input count rather than thinking. In local VS, the novice in the pair gets overwhelmed and stops wanting to play, and that kills the party-game pitch.

---

## 4. The loss condition is hard to read, and the feedback that would explain it is not MVP — MAJOR

**Problem.** Losing isn't "the board is full". It's "no column can move in either direction", whether because of board height or because of column length. A player is unlikely to see that coming. Columns lose mobility gradually as incoming or spawned marbles stack up, and game over arrives when the *last* legal move disappears, which may not match how full the board looks. The doc gives each failed move a "choc sourd / la colonne tressaute" (MVP). But the cues that would show danger building up are all non-MVP:
- the escalating "approche de la défaite" panic effect is "Important";
- "Tableau qui se remplit" layering is "Important";
- the Game over explosion itself is "Important".

That means a cut-down build could end a match with no clear signal of why. Zen has a related problem. It is sold as relaxing ("pas de game over"), but when the board fills, the level restarts automatically, and the doc doesn't say whether the player is told or loses their progress silently.

**Source.** GDD "Cas limites", "Plateau bloqué". Schemas_deplacement 4 (limits of move). Evenements_visuels_et_sonores, "Partie et ambiance" priorities. GDD "Mode Zen".

**Likely player experience.** "Why did I lose? I could still see empty space." Losses that feel unfair are the main driver of rage-quits in competitive puzzle games.

---

## 5. The reward feedback for skilled play can be cut, and there is no long-term motivation layer — MAJOR

**Problem.** The game's skill expression is the stack and the combo cascade. Yet the feedback that makes that skill feel good is ranked below MVP:
- "Combo × 2, × 3…" (rising pitch, growing light) is "Important";
- 4- and 5-alignment bursts are "Important";
- "Élimination ou attaque réussie → effet gratifiant dans l'environnement" is "Optionnel", even though the Fantasy section names it as the core "effet waw".

In an MVP build, a 4-chain combo could look and sound almost the same as a basic 3-alignment. Beyond the single match:
- the only progression hook is a Solo leaderboard;
- every level theme, and the theme/difficulty curve, is "À définir";
- the doc names no unlocks, mastery goals or arcade rewards;
- the bonus/malus marbles appear "à chaque élimination" in every competitive mode, but their own document isn't written, so the doc can't yet show they'll feel fair rather than random.

**Source.** Evenements_visuels_et_sonores, priority column. GDD "Fantasy". Themes_niveaux (every row "À définir"; difficulty link "Reporté"). GDD "Documents liés" (Billes bonus/malus, Leaderboard: no content). GDD "Progression et thèmes".

**Likely player experience.** A player who learns to chain combos doesn't feel a payoff in proportion to the skill. Once they've seen the 5 levels, nothing gives them a reason to come back. That hurts the early-access goal of "créer une communauté".
