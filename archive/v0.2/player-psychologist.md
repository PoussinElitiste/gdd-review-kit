# Player Psychologist — Round 1

Lens: what a real player experiences in the first hour. Who gets confused, who gets frustrated, and where the rewards stop motivating them. Source: `gdd.txt` (GDD_V_0_2 plus linked docs).

---

## 1. Onboarding teaches the least important action, once, and never covers the systems that decide wins and losses
**Severity: BLOCKING**

**Problem.** The only teaching in the game is a passive hint. It fires after 5 seconds of inactivity, it shows how to "align 3 marbles", and it runs only during the player's very first game, "quel que soit le mode par lequel il commence". Nothing teaches the systems that actually decide wins and losses:
- the counter color, and the fact that alignments in any other color do nothing for the counter
- the release rule (see Finding 2)
- the stack and combo tactic
- pending waves and counter-attack cancellation

The hint even points the player toward a weak move. A highlighted 3-alignment in a non-counter color "disparaît sans effet sur le compteur". So the tutorial can show a move that makes no progress.

The document also says no game in this genre has shipped since 1997. Most players will know none of these mechanics. If someone starts in Arcade or VS, their first and only tutored game happens while an AI is burying them. After that one game, the highlight help disappears for good. A player who loses without understanding why, with no way to replay the tutorial, is a classic first-session quit.

**Source.** "Contrôles et onboarding": "Un onboarding passif ... uniquement lors de la toute première partie du joueur, quel que soit le mode ... si le joueur ne fait aucune action pendant 5 secondes". Also "Règle d'alignement horizontal ... n'est active que pendant la première partie". Positioning: "aucun jeu reposant sur ce type de mécanique n'est sorti depuis 1997". There is no onboarding for anything in Combo_system.md.

---

## 2. The counter release rule works against intuition, and the core-loop text states it wrong
**Severity: MAJOR**

**Problem.** Core loop step 4 tells the player: "former une ligne complète de 5 billes pour libérer le compteur". Combo_system.md says something different. A 5-line in the counter's own color does NOT release the counter. It only adds +3. To release, the player must complete a 5-line in a DIFFERENT color. That line's color then becomes the new counter color.

Most players will reason "I've been collecting red, so I'll finish a red line to cash in." That exact move fails to fire the attack. The design also asks the player to track several things in their head while under time pressure:
- the current counter color, which is random at the start
- which colors are worth aligning
- a different color to build a full line of
- the upcoming counter color, which that line silently picks

None of this is taught (Finding 1). The feedback for "same-color line, no release" is just a "note ascendante" (rising note), which looks like a success sound. Expect players to feel that the game "didn't do what I told it", and expect them to blame the game.

**Source.** GDD "Boucle d'une partie" step 4, compared with Combo_system.md "Libération du compteur": "Si la ligne complète a la même couleur que le compteur, celui-ci n'est pas libéré". Combo_system.md "Compteur de billes": the color is "tirée au hasard au début de la partie".

---

## 3. Right-only switch, no undo and single-step moves create input friction that clashes with the claim of a "very broad" audience
**Severity: MAJOR**

**Problem.** The audience is justified by saying the game "ne demande pas une grande maîtrise des commandes". But the movement rules ask the player to plan ahead, which is a high mental load:
- Switch only goes right. A one-cell move to the left takes 4 switches.
- A switch cannot be undone, "afin de pousser le joueur à anticiper".
- A move is always one cell, "sur toutes les plateformes", on a board 15 cells tall.

On mobile, a left swipe is the natural gesture, and the document does not define what it does. Diagonal swipes are explicitly ignored with no feedback at all. A novice will make a mistake, find they can't undo it, and then have to pay 3 more switches to fix it while the clock runs. For a newcomer, that reads as punishment. They don't experience it as depth. The design treats "no undo" as a motivator. For experienced players it may be one. For the broad audience the game targets, it is a frustration loop.

**Source.** "Règle de déplacement" (Switch right only; "4 switchs vers la droite équivalent à 1 vers la gauche"; no undo; one cell per move on all platforms). "Public visé: très large". Mobile controls define only up, down and right swipes. "Geste en diagonale ... ne déclenche aucune action", and the events table confirms no sound and no visual for it.

---

## 4. Accessibility patterns change with every theme, so colorblind players must relearn them each level
**Severity: MAJOR**

**Problem.** The color-plus-pattern pairing is the only accessibility measure for color perception. The document makes the patterns theme-specific: "il change d'un thème à l'autre". A colorblind player builds the mapping "cross = the color I need" in level 1. In level 2, the mapping is gone. Over the 5-, 10- and 15-level runs, they have to relearn it repeatedly, under a timer, while also tracking a counter color that changes. The same player also has to read the counter color correctly to understand Finding 2.

On top of that, the panic effects and the "explosion visuelle" are not covered by any options: no reduced-motion or flash settings, and no audio-only cue for players who can't see well. The accessibility section stops at three bullets.

**Source.** "Accessibilité": "Le motif de chaque couleur est propre à la thématique du niveau : il change d'un thème à l'autre". Themes_niveaux.md: patterns are "propre à la thématique". No other accessibility options are listed.

---

## 5. Zen, the only safe practice space, still ends in a forced game over, and the "contemplation" fantasy is undercut in every mode
**Severity: MAJOR**

**Problem.** Given how weak onboarding is, Zen is where a confused player would go to learn. Zen is pitched as "l'équivalent d'un mode entraînement, sans limite de temps". The edge-case rules contradict this: continuous filling applies "dans tous les modes", and "une partie ne peut pas durer indéfiniment". The blocked-board game over "s'applique à tous les modes de jeu". Zen does offer a spawn-frequency option, but the document never says it can be turned off. So a practice session still ends in failure, and Zen's score is the longest chain, which is a pressure metric.

The fantasy promises "persévérance et contemplation" and a calm lounge opening. Yet every mode adds a timer or a filling board plus a "panic effect". The player is promised serenity and given constant threat, with no actually stress-free place to learn or relax. The reward side is also thin. The document lists no meta-progression, unlocks or between-run rewards. The leaderboard doc is unlinked. The 5 themes that make up the "voyage" are all "À définir". That leaves the "wow" payoff, the main reason to keep playing, with nothing behind it yet.

**Source.** "Mode Zen score" in Concept. "Cas limites": "Remplissage continu : dans tous les modes ...", "Plateau bloqué ... s'applique à tous les modes de jeu". "Fantasy" section. Themes_niveaux.md table is entirely "À définir". "Documents liés": Leaderboard is listed without a link.

---

## Round 2 — Cross-examination

### CONFLICTS

**C1. Business Analyst #4 ("Make Zen a Solo setting") and #1 (MVP = "Solo + VS local").** I disagree with how these two recommendations combine. BA treats Zen as decorative and proposes cutting the MVP to Solo and VS local. From the player's side, that leaves only two modes. Solo is timed, and its own text says random play is "pénalisé par le temps limité". VS local puts a human opponent on top of a newcomer who has had one passive hint (my F1). The MVP would then have no low-pressure place to learn a mechanic the document itself says nobody has played since 1997. I'm not defending Zen's menu entry, its leaderboard or its name. I'm defending its function: a timer-off, fill-off, no-fail practice space. If Zen becomes a Solo setting, that's fine, but the setting has to switch off both the timer and continuous fill. Otherwise BA's cut quietly deletes the only fallback for my BLOCKING onboarding finding. Escalate as: "Does the MVP keep a no-fail practice space, in any form?"

**C2. Adversarial QA F4 (Zen should have a minimum spawn frequency to keep the game finite and the leaderboard fair).** QA's fix and mine pull in opposite directions. QA wants Zen bounded so its combo leaderboard can't be farmed. I want a space where the board never forces a loss. Both can't hold in one mode. My position: the leaderboard is the part to sacrifice. A practice space that is also ranked will be optimized, which is QA's point, and it will feel stressful, which is mine. Split it into two things: an unranked practice mode where frequency can be zero, and a ranked Zen with fixed settings. QA's exploit only exists because the document puts both jobs in one mode.

**C3. Narrative Critic #3 (the scenery escalates as the player gets buried, "the scenery reports failure").** I partly disagree. From a feedback point of view, layering music and visuals as the board fills is a good danger signal. It's peripheral, readable without looking away from the cursor, and it rises before the loss. Players learn "the music is getting dense, I'm in trouble" quickly, and I would not want that cue removed. Where I agree with Narrative is that this is the ONLY reveal channel in Arcade/VS. A player who is doing well gets no escalating reward. So the fix isn't to flip the trigger. It's to add a separate success-driven reveal, for example tied to releases or cancelled waves, so danger and accomplishment each have their own signal.

### CONNECTIONS

**X1. My F3 (no undo, right-only switch) + Adversarial QA F1 (columns can leave the median line and become unselectable).** Neither finding shows the full problem alone. QA found a soft-lock state. I found the input model that will lead novices into it. One-cell moves on a 15-tall board, no undo, and a design that wants the player to "anticiper" mean a newcomer who presses down a few times too many can push a column off the median line. Then they can never select it again, and no game over fires. This is the worst kind of failure for a first-hour player: permanent, silent, and without a clear cause. They don't lose. They just stop being able to play. My F3 was framed as frustration. Combined with QA F1, it is a quit trigger.

**X2. My F2 (release rule is counter-intuitive and mis-stated) + Adversarial QA F2 (a 5-line is nearly unreachable by direct play) + Systems F1 (no probability model for releases).** QA shows that any 3-alignment on the median line is eliminated at once, so a deliberate 5-line needs an `X X _ X X` setup, and otherwise comes from cascades. For a player, that means the release will feel random. It happens "to" them during cascades rather than as the result of a plan. Solo gauge progress depends entirely on releases (Systems F1). So a first-hour Solo player who doesn't know the different-color rule, and can't build the line on purpose anyway, may finish their first level with an empty gauge and no idea what they were supposed to do. That isn't a confusing rule. It's a progress system the player can't see or steer.

**X3. My F5 (thin reward structure) + Feasibility F4 / Business Analyst #2 (the audiovisual "wow" has no asset source or budget).** In Round 1 I noted that there is no meta-progression, so the AV journey is the only long-term motivator. Feasibility and BA show that this one motivator has no owner, no composer, and no money ("dont le coût n'est pas compté ici"). Together: if the build ships on placeholders, the player has no unlocks, no progression and no spectacle. The only remaining reason to come back is a leaderboard whose document doesn't exist. Retention past session one would have nothing to stand on.

**X4. Narrative Critic #4 (victory and defeat get the same "explosion, dissipation" beat) + Adversarial QA F5 (Solo time-out is never stated to be a loss).** From the player's chair, it's possible to reach the end of a game and not know whether you won. The ending effects are identical, and in Solo the time-out outcome isn't even defined. Unclear outcome feedback is a known motivation killer, because the player can't connect what they did to what happened. This adds to my F1: the game doesn't explain the rules up front, and it doesn't clearly report the result afterwards either.

**X5. Feasibility F3 (AI is a behaviour tree with only "plus agressive" as a difficulty lever) + my F1.** Since the only tutorial is one passive hint, the first Arcade opponent effectively *is* the onboarding for anyone who starts there. The document gives that opponent no defined behaviour or difficulty setting. Nothing guarantees opponent 1 is beatable by someone who doesn't know the release rule. That needs to be specified.

### REVISIONS

- **F2: upgrade MAJOR to BLOCKING.** X2 is the reason. In Round 1 I treated this as a rule players would misread. With QA F2 and Systems F1, it's worse: the only way to make progress in Solo and the only way to attack in VS/Arcade is mis-stated in the GDD, not taught, against intuition, and possibly not reachable on purpose. A first-hour player may not knowingly trigger it at all.
- **F3: stays MAJOR, with higher confidence.** The soft-lock outcome belongs to QA F1 and should be ranked under QA's BLOCKING. My finding explains why novices in particular will hit it. I'd also narrow one claim. Business Analyst #3 makes the broad-vs-competitive audience point from the business side. Mine stands as the experiential evidence for it, not as a separate audience argument.
- **F1: stays BLOCKING, strengthened.** QA F5 adds two details I missed. The onboarding is used up even if the first game is quit after a few seconds. And in local VS, it's undefined which player counts as "first-time". Both make the single-shot tutorial even less likely to reach the person who needs it.
- **F5: refocused, not withdrawn.** The "Zen contradicts endless" half is now flagged by Systems F4, QA F4 and Narrative #2, so it's well covered and I defer to them on the rule itself. My distinct contribution is the player consequence: there is no safe place to learn (C1/C2), and the long-term reward layer has nothing behind it (X3). I keep MAJOR.
- **F4: stays MAJOR, with one addition.** Systems F2 and QA F5 show that 15-level runs will cycle through the same 5 themes in an undefined way. A colorblind player then relearns pattern sets in an order the document doesn't specify, possibly mid-run. Systems F1 also notes the color count is unstated. The number of mappings to relearn (colors x themes) can't even be bounded yet.
