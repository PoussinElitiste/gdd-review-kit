# Player Psychologist — Round 1

Lens: what a real player experiences in the first hour. Where they get confused, frustrated, or demotivated, or quit.

---

## 1. Onboarding teaches the wrong thing, only once, and the real tutorial may be cut

**Severity: BLOCKING**

**Problem.** The only onboarding that is actually specified teaches the player to make an alignment of 3. That is the easy part. The thing that decides whether you win or lose is the marble counter loop, and nothing teaches it. To progress, the player has to work out four rules on their own:

- Alignments in the counter's color fill the counter.
- Alignments in any other color do nothing for it.
- A full 5-line in the counter's color does *not* release it.
- A full 5-line in a *different* color releases it, and the counter then takes that new color.

That is a lot of hidden logic, and it runs against intuition. A new player will make 3-alignments, see the marbles pop, and have no idea why the solo gauge isn't filling.

**What the document says.**
- GDD, Contrôles et onboarding: "si le joueur ne fait aucune action pendant 5 secondes, le jeu met en surbrillance une bille et l'action à effectuer (switch ou move) pour aligner 3 billes."
- The alignment-highlight aid is active "uniquement lors de la toute première partie du joueur, quel que soit le mode." A player who opens with Arcade or local VS gets their only tutorial while under attack. In VS, it's unclear which of the two players the "first game" refers to.
- Zen is described as the place for "entraînement et tutoriel". But "Mode Zen (à rédiger)" has no content, and the cut list includes "le mode Zen" (4th in cut order). If Zen goes, the game has no tutorial for the counter, the release, the attack waves, the counter-attack or the stack.

---

## 2. The core reward loop is hard to read and asks for numbers the game says it won't show

**Severity: MAJOR**

**Problem.** The player has to keep two goals in mind at once, and they pull against each other:

- Feed the counter with alignments in color X.
- Build a full 5-wide line in some other color Y to cash it in.

Cashing in then flips the counter to Y, so the color they've been feeding changes every time they score. Meanwhile, the release condition (5 of one color across all 5 columns) is the hardest board state to build:

- The switch only goes right.
- The generator never creates a ready-made alignment on the middle line.
- Marbles deep in a long column can't reach the middle line.

So the payoff moment is rare and the setup is invisible. Players won't feel progress between releases. The +1/+2/+3 counter gains are small, abstract numbers on the side, not the "wow".

**What the document says.**
- Combo_system: "Lorsque le joueur forme une ligne complète de 5 billes de même couleur, et que cette couleur est différente de celle du compteur, le compteur est libéré… Il prend ensuite la couleur de la ligne." Also: "Si la ligne complète a la même couleur que le compteur, celui-ci n'est pas libéré."
- AI_Architecture (Agent Feedback): the player should "lire la partie sans lire de chiffres". This contradicts a loop that depends on knowing the counter's color and value (×1 to ×10) and the gauge measured in "unités de compteur".
- Themes_niveaux: Level 1 requires a gauge of 10 counter units in 3 minutes. A novice releasing at ×2 needs five full 5-lines in three minutes on their first level.
- No feedback event is listed for "you are close to a release" or "this line could release". Feedback only fires *after* something happens.

---

## 3. "Sans pression" first level is actually a timed failure state with panic music

**Severity: MAJOR**

**Problem.** The emotional pitch and the mechanics of the first hour contradict each other. The first level promises calm discovery. In practice, a first-time player gets:

- a 3-minute timer,
- continuous marble spawns,
- a quota they likely can't hit (see Finding 2),
- a "panic" audiovisual escalation as time runs out.

So the most likely first-session memory is failing level 1 while the music signals danger. In solo, "approche de la fin du temps" fires in *every* run, including ones the player is winning. That teaches players to read the panic cue as noise, or as constant stress.

**What the document says.**
- Themes_niveaux, Arc émotionnel: "La Source, l'éveil : le joueur découvre le jeu dans le calme, sans pression."
- Themes_niveaux table: Level 1 is "3 min", "toutes les 4 s", jauge 10.
- GDD Concept: "Si le temps s'écoule avant, le niveau est perdu et peut être rejoué."
- GDD Fantasy: "Effet de panique à l'approche de la défaite ou de la fin du temps."
- No mention of a gentler first solo run, of an untimed level 1, or of failure being softened for new players.

---

## 4. Game over is invisible until it happens, and the irreversible switch makes it feel unfair

**Severity: MAJOR**

**Problem.** The loss condition is a computed state the player can't see coming: no move possible in any column, *and* no switch can form an alignment. The player may still be able to press switch, and the game simply ends. Several things make this worse:

- The warning cue for approaching defeat is only "Important", not MVP.
- Column reachability rules ("n − 7 ≤ k ≤ 8") are never surfaced to the player.
- A switch can't be undone and only goes right ("4 switchs vers la droite équivalent à 1 vers la gauche").

One mis-press can't be corrected, and a board can become dead without any readable warning. Losses that feel arbitrary are a classic quit trigger, especially for the "très large" audience the document targets.

**What the document says.**
- GDD Fin de partie: "si le joueur ne peut plus faire aucun move… et qu'aucun switch ne peut plus former d'alignement sur la ligne médiane, c'est le game over."
- Evenements_visuels_et_sonores: "Approche de la défaite ou de la fin du temps | … effet de panique | forte | **Important**". The Game over event itself is also only "Important".
- Schemas_deplacement: "Il n'est pas possible d'annuler un switch, afin de pousser le joueur à anticiper." This justification assumes expert planning, but the audience section says "le jeu ne demande pas une grande maîtrise des commandes".

---

## 5. Arcade teaches by defeat and spectating, the "stack" teacher can be cut, and there is no long-term motivation layer

**Severity: MAJOR**

**Problem.**
- **Learning by spectating.** The arcade ladder expects the player to learn counter-attack and stacking by watching the AI's board next to their own, while managing their own board under pressure. Players under load don't watch the other board. They learn by losing, which the document openly states.
- **The cut list breaks the ladder.** Opponent 4 is the only one that demonstrates the stack. It is on the cut list, while opponent 5, which "demande de maîtriser toutes les tactiques", is never cut. If 4 goes, the final boss tests a skill nothing taught.
- **No reason to come back.** Nothing gives the player a reason to return after the 5 levels and 5 opponents:
  - The leaderboard is unwritten.
  - Bonus/malus marbles are unwritten.
  - "ce qui se débloque à chaque étape" is never specified.
  - Longer runs are post-early-access.

**What the document says.**
- AI_Architecture, opponent 4: "la tactique du stack, en la voyant chez l'adversaire". Also: "Chaque défaite montre une tactique que le joueur peut ensuite reproduire."
- GDD Scope: "Ce qui peut être coupé, dans cet ordre : … le mode Zen, puis l'adversaire 4. Le Solo, le VS en local et l'adversaire 5 (le moment « wow ») ne sont jamais coupés."
- Documents liés: "Leaderboard (à rédiger)", "Billes bonus/malus (à rédiger)", "Menu et options (à rédiger)". Agent Narratif: "et ce qui se débloque à chaque étape". No unlocks are defined anywhere.

---

## Round 2 — Cross-examination

### Conflicts

**With Systems Designer #1 and Adversarial QA #1 (game over needs a 75/75 full board; defenders can become immune to attacks).** These findings undercut part of my Finding 4, and I concede that part. I argued that a board can "become dead without any readable warning" after one mis-pressed switch. Their math shows that a blocked board in practice means all five columns at 15 marbles. A completely full board is about the most visible state a puzzle game has, so "invisible game over" is mostly wrong as stated. But I don't accept that the player-experience problem goes away. It moves somewhere else, in two ways:
- **VS and Arcade.** If attacks can be nullified by pushing columns to L1, the player's experience of a match is a stall. Nothing they do seems to hurt the opponent, and the round doesn't end. Their finding is a rules bug. Seen from my lens, it is a reward bug: the release, the game's single big payoff, visibly does nothing.
- **Solo.** The invisible failure is real, but it is the one Adversarial QA #2 describes. Once every column passes 7 marbles, a release can become impossible while the timer is still running. The player keeps trying for minutes with no signal that the level is already lost. That is worse than the arbitrary game over I described, because it wastes the player's effort as well as ending the run.

**With Business Analyst #3 (pick a primary audience, e.g. solo score-chasers; local VS is a weak sell on Steam).** I agree the audience needs narrowing. I push back on the implied direction, though. The document's own onboarding is weakest exactly for that audience:
- Score-chasers need a legible score, and Systems Designer #5 shows no score formula exists. Pointing early access at score-chasers turns my Finding 5 (no reason to come back) from a retention gap into a launch blocker.
- Local VS is also where a lot of new players first meet the game, because a friend hands them a controller. Adversarial QA's "also noted" item confirms that the first-game highlight is undefined and asymmetric in VS. Whatever audience is chosen, the first-session path for that audience has to be designed. The document does this for none of them.

**With Feasibility Lead #1 and Business Analyst #2 (scope is already too large for 7 weeks).** My Finding 1 asks for more teaching. That adds scope to a plan both colleagues say is already over capacity, so I am changing my fix rather than my severity. I no longer ask to protect Zen as a full mode. The cheaper fix is to put the counter, release and color-flip rules into Solo level 1 itself as contextual prompts, with an untimed or extended first attempt. Solo is "never cut", so the lesson becomes safe from the cut list for almost no extra content. This agrees with Business Analyst #4's "fold the stack lesson into Solo". I'd also take the cost from the per-level intro sequences, which that finding already marks as cuttable. It does not take it from the Narrative Critic's outro request (see below).

**With Narrative Critic #2 (level 5 should open with calm).** We agree that "sans pression" is false for level 1. We part ways on how to fix it. Narrative wants calm moments added inside the hardest level. From the player's side, calm at the start of the level with the steepest spawn step (Systems Designer #4: 0.40 to 0.50 marbles/s) is a trap: the player relaxes into the fastest fill rate in the game. The calm/contemplation beat belongs in the transitions between levels and after a victory, not in the opening seconds of a timed level. Inside the levels, the player-safe place for "no pressure" is level 1.

### Connections

1. **Systems Designer #2 + my Finding 2 + Adversarial QA #2: the first level is probably not winnable by a new player.** Systems shows that counter growth and staging a release compete for the same 5 middle-line cells, and that the gauge targets have no throughput model. QA shows the release guarantee lapses once columns pass 7 marbles. Put those together with my claim that novices don't understand the release rule at all. The likely first-hour experience is then a player who doesn't know what to aim for, can't build it in the time given, and may be in a state where it can't be built anymore. Each finding alone is MAJOR. Together they describe the first session ending in confused failure.
2. **Adversarial QA #5 + my Finding 5: there is nothing to spectate.** I argued that players under load don't watch the opponent's board. QA shows that a 1-2 action lookahead AI can't actually stack or reliably release. So even a player who does watch opponent 4 will not see the stack demonstrated. The "learn from each defeat" ladder fails on both ends: the AI doesn't perform the lesson, and the player isn't looking.
3. **Systems Designer #1 + AI_Architecture's "Chaque défaite montre une tactique": defeats don't carry a lesson.** "Learn by losing" only works if a loss can be traced to what the opponent did. If attacks can't bury the player, Arcade losses come from the continuous fill, not from the tactic the opponent is meant to teach. The player can't connect the loss to the lesson. Systems Designer #4 makes the same point from the tuning side: opponent behavior and spawn rate change together.
4. **Narrative Critic #3 + my Finding 3: the player can't tell they won.** Victory and game over are word-for-word the same event ("explosion visuelle, puis dissipation de la scène"), and both are only "Important". For a novice, especially one who doesn't understand the gauge (Finding 1), that means the end screen doesn't say whether the attempt succeeded. That is a readability failure, not only a narrative one. I'd make a clearly distinct win/loss outcome MVP before any intro sequence.
5. **Feasibility Lead #1 and #4: nobody tests the first hour with a new player.** All playtesting falls on one developer, who already knows every hidden rule, and Agent Test only checks that levels "can be completed" and that opponents "can win and lose". So no step in the plan can detect Findings 1-3: a rule the designer understands but a newcomer does not. The plan needs at least one round of playtests with people who have never seen the game, before the gauge values are locked.

### Revisions

- **Finding 1 (onboarding): stays BLOCKING.** I changed the fix: instead of protecting Zen, fold the counter tutorial into Solo level 1. The severity is confirmed independently by Systems Designer #3 and Business Analyst #4.
- **Finding 2 (core reward loop): upgraded from MAJOR to BLOCKING.** Systems Designer #2 and Adversarial QA #2 show that the release, which is already hard to understand, is also mechanically hard to reach and can become impossible. A payoff that players can neither understand nor reliably reach blocks the core loop.
- **Finding 3 ("sans pression" level 1): stays MAJOR.** Narrative Critic #2 makes the same point independently. It is now partly a consequence of Finding 2.
- **Finding 4 (invisible game over): reframed, stays MAJOR.** I withdraw the claim that one mis-pressed switch can create a dead board without visible warning. Given Systems Designer #1, a blocked board is a full board, which is easy to read. I replace it with the experiences their findings imply: unwinnable Solo time with no signal (QA #2), and VS stalls where releases visibly do nothing (QA #1). The irreversible right-only switch is still a friction point for a "très large" audience, but on its own it is MINOR.
- **Finding 5 (Arcade spectating and no long-term motivation): stays MAJOR, strengthened.** Adversarial QA #5 shows the spectated behavior may not exist at all. If early access targets score-chasers (Business Analyst #3), the missing score and leaderboard layer would become BLOCKING.
