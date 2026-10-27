# Player Psychologist — Round 1

Lens: what a real first-time player experiences in the first hour, where they get confused or frustrated, and whether the reward structure keeps them playing.

---

## 1. Onboarding is too thin for how unintuitive the rules are, and it disappears after one game — BLOCKING

**Problem.** All onboarding consists of two hint systems that only run in the player's first game: an idle hint after 5 seconds, and highlighting of possible 3-alignments. Neither one teaches the rules that decide whether a player progresses: the counter's color, the "full line of a *different* color releases the counter" rule, stacking for combos, column travel limits, or the blocked-board loss condition. The hints only point at 3-alignments, which are the least important action for progress. After that single game, every aid is gone for good.

The first game can also happen in any mode, including timed Solo, Arcade against an AI, or local VS. So a new player's only lesson may take place under time pressure or attack, or while two people share the screen and it's unclear whose "first game" it is. Zen is called "mode d'entraînement et tutoriel", but nothing in it is structured. It is only the normal game with no timer.

**Source.** GDD "Contrôles et onboarding": "Un onboarding passif ... uniquement lors de la toute première partie du joueur, quel que soit le mode ... si le joueur ne fait aucune action pendant 5 secondes ..." and "cette aide n'est active que pendant la première partie du joueur, quel que soit le mode." Mode Zen: "sert aussi de mode d'entraînement (training mode) et de tutoriel". There is no tutorial sequence anywhere, and the hints never mention the counter, release, or stacking. The "Mode Zen" linked doc is listed but missing.

**Severity: BLOCKING.** The game is pitched to a "très large" audience. If the core progression rule is never taught, players will match 3s, see the gauge stay still, and not know why.

---

## 2. The core reward rule works against intuition, and the first payoff is hard to reach — MAJOR

**Problem.** Players naturally expect that building up the thing on screen (the counter) leads to the payoff. Here the payoff comes from a full 5-in-a-row of a color that is *not* the counter's color. A full line of the counter's own color, the hardest and most impressive play, does *not* release it. Each release also re-randomizes what "my color" means, because the counter takes the color of the line. The counter's first color is chosen at random.

The release itself needs 5 contiguous same-color marbles across the entire median row, built with a right-only switch. The generator only guarantees that 5 marbles of one releasable color exist somewhere on the board. For a beginner this is a hard multi-step puzzle. In Solo, the only route to filling the progression gauge is a release, so a new player may spend a whole timed level popping 3s while the main goal bar never moves. That is the moment people quit.

**Source.** GDD "Boucle d'une partie" step 4 and "Alignement et élimination": "Une ligne complète de la couleur du compteur ne le libère pas : elle le remplit avec un bonus." Combo_system "Libération du compteur": the counter "prend ensuite la couleur de la ligne et est réinitialisé à 1"; "couleur, tirée au hasard au début de la partie". "Garanties de génération": only "au moins 5 billes de même couleur sur le tableau". Combo_system: "un alignement d'une autre couleur n'a aucun effet sur le compteur". The events table gives off-color matches a "éclat atténué", which is a smaller reward for exactly what beginners will do most.

**Severity: MAJOR.** It is learnable, but without teaching (see Finding 1) and without an easier first release, the first-hour reward loop may never close.

---

## 3. The control scheme adds friction to every action — MAJOR

**Problem.**
- **Switch only goes right and can't be undone.** To move a marble one cell left, the player has to think "4 switches right". The doc says this restriction "ne limite pas le joueur". That's true mechanically, but it adds a mental-arithmetic step to every repositioning while the player is under a timer or attack. The no-undo rule is justified as making players "anticiper", which is a mastery goal pushed onto novices.
- **The PC mouse scheme needs both hands.** Selecting is done with the mouse and moving with the wheel, but switching is only on Space or right Enter. Mouse players must keep one hand on the keyboard, and nothing is said about mouse-click switching.
- **There's no feedback for a refused input.** A diagonal swipe on touch "ne déclenche aucune action" and has no sound or visual, so a mis-swipe looks like the game ignoring the player.
- **The audience claim doesn't fit.** The doc states "le jeu ne demande pas une grande maîtrise des commandes". Columns that can move only within limits set by both column length and board height, which shrink as the board fills, are not low-mastery controls.

**Source.** GDD "Règle de déplacement" (switch to the right; "4 switchs vers la droite équivalent à 1 vers la gauche"; "il n'est pas possible d'annuler un switch"), "Contrôles" PC section, "Geste en diagonale"; Événements table: "Geste en diagonale (tactile) | aucun | aucun"; "Public visé".

**Severity: MAJOR.** Each item alone is small, but they all hit the most frequent action in the game.

---

## 4. Loss is hard to read, and defeat feels the same as victory — MAJOR

**Problem.** The loss condition is abstract. The player loses when every column is stuck in both directions *and* no switch can form a median-line alignment. A beginner won't see that coming or understand it afterwards. "Approche de la défaite" panic feedback is marked only "Important" (cuttable), so in the MVP a player can lose with no warning.

Game over and victory also share the same feedback: "explosion" sound and "explosion visuelle, puis dissipation de la scène". Losing and winning will feel alike, which removes the emotional contrast that makes a win satisfying and a loss instructive. Neither the end screen nor any feedback explains *why* the player lost.

The draw rule, where a draw counts as a won round "pour chacun des deux joueurs", may also feel strange to players at the end of a match.

**Source.** GDD "Plateau bloqué"; Événements table rows "Game over (plateau bloqué)" and "Victoire" (identical sound/visual, both "Important", not MVP); "Approche de la défaite ... | Important"; GDD "Boucle de jeu" end screen (continue / restart / menu only); "Égalité".

**Severity: MAJOR.** Losses that feel unexplained or unfair are a top cause of early quitting in puzzle games.

---

## 5. Not enough motivation past the first session, and the "calm" promise clashes with the pressure systems — MAJOR

**Problem.**
- **There's little to keep players going.** The launch build has 5 levels per mode. All theme, order and difficulty details are "À définir". There are no unlocks, collectibles or meta-progression, and the Leaderboard doc is listed but doesn't exist. Solo's time limit and gauge size are unspecified. Players will finish in one sitting with no reason to return beyond high scores, whose formula is only sketched ("basé sur le compteur de combo et l'utilisation du compteur de billes").
- **The difficulty curve may scare off casual players.** The designated "wow" moment is "le challenge corsé" of the Arcade AI, which gets "plus agressive" each level. No difficulty selection or assist is mentioned for Arcade, even though the game targets a "très large" casual audience.
- **The fantasy contradicts the pressure.** The fantasy promises "persévérance et contemplation", calm lounge music and an ASMR mood. Yet every mode except Zen has a timer or constant refill plus "effet de panique". Players who come for the calm, contemplative experience get stress by default. The only escape is Zen, which has no scoring, no stakes and no progression.

**Source.** Themes_niveaux.md (whole table "À définir"; difficulty link "Reporté"); GDD "Mode solo"/"Mode arcade" ("des parcours de 10 et 15 niveaux viendront en mise à jour"); "Moment « wow » ... le challenge corsé"; "Progression par mode — Arcade"; "Fantasy"; "Remplissage continu"; Documents liés (Leaderboard, Mode Solo, Mode Arcade unwritten).

**Severity: MAJOR.** In early access, retention and reviews come from the first sessions. A short run with an undefined curve, plus a mismatch between promise and experience, invites "too short" and "not what I expected" reviews.
