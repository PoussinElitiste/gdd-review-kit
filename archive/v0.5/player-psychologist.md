# Player Psychologist — Round 1

Lens: what a real player experiences in the first session and the first hour. Where they get confused, frustrated, or stop caring.

---

## 1. Onboarding teaches the match-3, but not the game. The real goals are never explained.
**Severity: BLOCKING**

**Problem.** The only onboarding is passive, runs only in the player's very first game, and teaches only one thing: how to line up 3 marbles. Everything that makes the game work is left for the player to work out alone:
- the marble counter and its color,
- the rule that a 5-marble line of a *different* color releases the counter,
- the stack tactic that makes combos happen,
- incoming waves and how to cancel them.

On top of that, the document guarantees that **nothing lines up by chance** ("Pas d'alignement dû au hasard"). So a new player will never stumble into a cascade and think "whoa, how did that happen?" The game's most exciting moment, the combo chain, is locked behind a tactic nobody shows them.

The document also says onboarding fires "quel que soit le mode par lequel il commence". That means a player can have their first-ever game in Arcade against an AI, or in local VS against a friend. Their one tutorial then happens while they're being buried in marbles, and the hints are gone forever after that.

**Source.**
- "Contrôles et onboarding": "si le joueur ne fait aucune action pendant 5 secondes, le jeu met en surbrillance une bille et l'action à effectuer (switch ou move) pour aligner 3 billes". The highlighting of possible alignments is also "uniquement ... pendant la première partie".
- "Cas limites": "ne crée jamais d'alignement tout fait".
- Zen is said to be the tutorial ("sert aussi de mode d'entraînement ... et de tutoriel"), but nothing describes any tutorial content in it. It is just Zen with a stress slider.
- The stack tactic ("La tactique à adopter consiste à « stacker »...", Combo_system) is described only for the reader of the document, never for the player.

**Likely result.** Players clear 3-marble lines, watch the gauge or their attacks barely move, and can't tell why. That is the classic "I don't get the point" quit in the first 15 minutes.

---

## 2. The counter's rule fights the player's instincts.
**Severity: MAJOR**

**Problem.** Every match-3 player expects "make the biggest line of the highlighted color and get the big payoff." This game flips that:
- A full 5-marble line **in the counter's color** does not release the counter. It only adds +3.
- To cash in, the player needs a full 5-marble line in **another** color.
- Cashing in then resets the counter to x1 and switches it to that new color. The color the player was building toward is gone.

So the most impressive move a player can make, a full line of "their" color, feels like it did nothing. Meanwhile the move that pays off works against the mental model they just learned. The counter's color is also random at the start and changes after every release, so the player has to keep re-reading it under time pressure.

**Source.**
- "Alignement et élimination": "Une ligne complète de la couleur du compteur ne le libère pas : elle le remplit avec un bonus."
- Combo_system, "Libération du compteur": "Il prend ensuite la couleur de la ligne et est réinitialisé à 1."
- Combo_system, "Compteur de billes": the color is "tirée au hasard au début de la partie".

**What's missing.** No feedback event explains *why* a line didn't release. The event table has "Compteur rempli (+1, +2, +3)" but no "line of the counter's color, not released" cue. Gains above the x10 cap are also "perdus" with no feedback event listed.

---

## 3. A right-only switch with no undo contradicts "easy to pick up."
**Severity: MAJOR**

**Problem.** The document targets a "très large" audience because the game "ne demande pas une grande maîtrise des commandes". But the core input asks a lot:
- The switch only goes right, and it rotates the whole middle row.
- Moving a marble one step left takes **4** switches.
- A switch can't be undone.

The design rationale ("pousser le joueur à anticiper") is aimed at experts. For a beginner, an accidental switch scrambles the row they were building, with no way back, under a timer. This is the kind of frustration that reads to the player as "the controls are bad," not "I made a mistake."

The PC mouse scheme adds to this. Selection and move are on the mouse (click and wheel), but switch is only on Space or right Enter. A mouse-first player has to keep one hand on the keyboard for the most common action, and the document doesn't offer a mouse-only switch at all.

**Source.**
- "Règle de déplacement": "Switch : faire glisser la ligne de la bille vers la droite"; "il n'est pas possible d'annuler un switch"; "4 switchs vers la droite équivalent à 1 vers la gauche".
- "Audience": "le jeu ne demande pas une grande maîtrise des commandes".
- PC controls: "Switch : une touche du clavier (barre d'espace ou Entrée droite)".

---

## 4. Solo progress is all-or-nothing, and failure isn't defined.
**Severity: MAJOR**

**Problem.** In Solo, the progress gauge fills **only** when the counter is released, and that takes a 5-marble line in a non-counter color. Every 3- and 4-marble line, every combo, and every counter gain shows no movement on the main goal until that one big event. A struggling player can spend a long stretch of a timed level seeing the gauge sit at zero, with no sign of whether they're getting closer.

The document also never says what happens when the Solo timer runs out with the gauge unfilled: defeat, a partial score, or a retry. The audio phases tie Solo intensity to "la jauge de progression ou le temps", but the player's actual end state is undefined.

The fantasy promises "une ambiance très calme en début de partie" and "contemplation". Yet Solo, the mode the document pushes ("Le mode solo ... mis en avant"), runs on a clock from the first second. A player who came for the calm "voyage" gets a timer-pressure puzzle.

**Source.**
- "Mode solo": "remplir une jauge de progression en temps limité".
- "Boucle d'une partie", step 4: the counter is released only by a full 5-marble line of a different color.
- "Jeu au hasard": "pénalisé par le temps limité en solo".
- Fantasy: "initiation à la persévérence et la comtemplation".
- There is no defined Solo fail state anywhere.

---

## 5. Nothing pulls the player into a second session: no difficulty curve, no meta-reward.
**Severity: MAJOR**

**Problem.** After the first hour, the reasons to keep playing are a leaderboard and 5 levels. Those levels' themes, order, and difficulty curve are entirely "À définir". The "Thèmes des niveaux" table is empty in every cell, and the theme-to-difficulty link and the emotional arc are deferred to the next GDD version.

Nothing describes unlocks, rewards for finishing a series, or how the "voyage" is gated. "Agent Narratif : génère la progression du joueur" is the only mention, and nothing defines what it produces.

The Arcade AI is billed as the "wow" moment ("le challenge corsé"), but with no tuning targets. A "tough" AI with no ramp, facing players who were never taught counter-attacks (Finding 1), will feel unfair rather than exciting.

**Source.**
- Themes_niveaux.md: every cell "À définir"; "Reporté à la version suivante du GDD : Arc émotionnel du voyage".
- "Moment « wow » rendu possible par l'IA : le challenge corsé que propose le NPC".
- Related documents "Leaderboard", "Mode Solo", "Mode Arcade" are listed with no content.

**Minor note in the same area.** Early-access buyers get one 5-level route, with 10- and 15-level routes "en mise à jour". That's a thin first impression for a premium title unless each level holds up on replay.
