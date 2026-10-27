# Player Psychologist — Round 1

Lens: what a real player experiences in the first hour, where they get confused or frustrated, and whether the rewards motivate. Findings are ordered by severity.

---

## 1. Nothing reliably teaches the stack, and the stack is the whole game — BLOCKING

**Problem.** The combo system states that stacking alignments above the median line is "la façon la plus rentable de remplir la jauge" and "tout l'intérêt du stack": 260 points instead of 125. The only onboarding is passive and limited to the first game. It shows one 3-ball match after 5 seconds of inactivity and highlights alignable balls. Neither of these teaches storing an alignment off the median line. The highlight works against it: it trains the player to fire every match the moment it appears, which is the opposite of holding a stack. Players will build a habit of clearing immediately, find their progress slow, and never discover the system that makes the game rewarding.

The fallback teachers can disappear:
- Zen is pitched as "relax et tutoriel", but its doc is "Mode Zen (à rédiger)" and Zen is the 4th item on the cut list.
- The only explicit stack lesson is Arcade opponent 4 ("la tactique du stack, en la voyant chez l'adversaire"). Opponent 4 is built last in step 3 and is the 5th cut item. Opponent 5, which demands "toutes les tactiques", is never cut. If the cuts happen, the player goes from opponent 3 straight to a final boss that uses a technique nobody has shown them.
- Solo players never meet an opponent who demonstrates anything.

**Source.** "Contrôles et onboarding" (passive onboarding, alignment highlight, first game only). Combo_system "Tactique du stack". AI_Architecture progression table (opponent 4). Technical Strategy cut order. "Mode Zen (à rédiger)".

**Severity.** BLOCKING. The core reward loop has no guaranteed teaching path in the shipped scope.

---

## 2. Solo level 1 promises "sans pression" but makes a novice fight a timer and a reward curve that undervalues them — MAJOR

**Problem.** The emotional arc says La Source is where "le joueur découvre le jeu dans le calme, sans pression". But autofill starts at one ball every 4 s from the first second. Level 1 also asks for 10 full attack gauges, which is 1,000 points. A beginner making plain rank-1 matches scores 10 (blue) to 50 (red), about 30 on average. That works out to roughly 30-plus clean matches. Meanwhile the player needs about one 3-match every 12 s just to break even against autofill. The weighting makes this worse: when the board is filling, the player clears whatever is available, often light colors. A 3-blue clear (10 points) saves the board but barely moves progress. Survival actions feel unrewarded, and players will read that as "I'm working and nothing happens."

This is also probably the player's first game. If they launch Solo before Zen, the novel 1997-style mechanic, the cylinder wrap, and the move limits all arrive under time pressure at once. With no time limit and no estimated level duration, a struggling player has no sense of how far away the end is beyond 5 milestones.

**Source.** Themes_niveaux "Arc émotionnel" (sans pression) vs. the difficulty table (10 jauges, 1 ball / 4 s). Combo_system "Poids des billes". Concept: Zen is a separate mode the player has to choose.

**Severity.** MAJOR.

---

## 3. Victory and defeat feel the same, and the key reward and danger signals are not MVP — MAJOR

**Problem.** In the events table, Game over and Victory have identical feedback: "explosion / explosion visuelle, puis dissipation de la scène", both at intensity "maximale". The emotional peak of the journey ("sentiment d'évasion et d'accomplissement après chaque épreuve") cannot be told apart from failure, at least on audio and visuals. Both are also only "Important", not MVP. The player-motivation signals that matter most sit below MVP:
- "Combo × 2, × 3" escalation, which is what makes the stack feel good: Important.
- "Approche de la défaite" panic, the only warning before a loss: Important.
- "Jalon atteint" music layer, the only mid-level progress reward in Solo: Important.

The MVP build, which is also the first build early-access players will see, can be a game where a 3-chain looks like three separate pops, losing comes without warning, and winning looks like losing. The Agent Feedback promise "lire la partie sans lire de chiffres" doesn't hold under those priorities.

**Source.** Evenements_visuels_et_sonores: rows "Game over", "Victoire", "Combo × 2, × 3…", "Approche de la défaite", "Jalon atteint". Fantasy section ("effet wow", accomplissement).

**Severity.** MAJOR.

---

## 4. Too many directional controls, with risky mappings — MAJOR

**Problem.** The Audience section says the game "ne demande pas une grande maîtrise des commandes", but the scheme has three directional verbs:
- moving the cursor left/right
- move up/down
- switch left/right

On keyboard, A/D moves the cursor and Q/E shifts the row. Both are left/right on adjacent keys with very different results, so expect frequent mis-switches that scramble a planned stack. On gamepad, the left stick handles both cursor movement (horizontal) and move (vertical). Diagonal ambiguity is resolved only for touch ("non interprété"), not for the stick, and a mistaken move under time pressure is a costly error. The model is also unusual: the player selects a ball but moves its whole column or row. With the cylinder wrap and the move limits (a blocked move only gives a "choc sourd" and a column wobble), early players will regularly do something they didn't intend and not know why. None of this is covered by onboarding (finding 1).

**Source.** Controles.md keyboard and gamepad mappings, diagonal rule touch-only. Audience: "ne demande pas une grande maîtrise des commandes". Schemas_deplacement move limits. Event "Move bloqué".

**Severity.** MAJOR.

---

## 5. Local VS is pitched as a party game but has no comeback or handicap, and its rules are unfinished — MINOR

**Problem.** VS is a headline mode ("party game", "mis en avant"), but the combo system explicitly removes counter-attack and attack cancelling: "Il n'y a plus de contre-attaque ni d'annulation des attaques". A player who knows the stack sends multiplied garbage that the other player cannot answer. In a living-room match between an experienced player and a newcomer, that means a quick, one-sided loss, which kills party-game sessions. No handicap, stress setting, or catch-up rule is mentioned for VS. Several things that shape how the mode feels are also still undefined: round duration and ties ("restent à définir"), and the bonus/malus balls, which appear in every mode but are "à rédiger". These are the usual levers for evening out a party game. Arcade has the same gap from the other side: Arcade losses are meant to teach ("Chaque défaite montre une tactique"), but nothing says what happens after losing to an opponent (retry? back to opponent 1?).

**Source.** Combo_system "Arcade et VS : billes envoyées" (no counter-attack). Audience/positioning (party game). Fin de partie ("La durée des manches et les cas d'égalité restent à définir"). Documents liés ("Billes bonus/malus (à rédiger)", "Mode VS (à rédiger)").

**Severity.** MINOR as a design gap today. It becomes MAJOR if local VS stays the lead marketing pillar.

---

## Round 2 — Cross-examination

### CONFLICTS

**C1. Systems Designer F1 / Adversarial QA F1 vs. my F5 (VS is a one-sided stomp).** This is a direct conflict, and on the mechanics they are mostly right. My F5 assumed that removing counter-attacks lets an expert bury a newcomer. Their numbers show the opposite: 100 points sends only 3 marbles, any rank-1 clear removes 3 of your own, garbage lands in the shortest columns, and the defender has about 50 marbles of buffer. A newcomer making slow blue clears can probably survive an expert indefinitely. I concede the "quick one-sided loss" claim. It was asserted without the attack/defense arithmetic, and theirs is more careful.

I do not concede that this makes VS fine for players. It moves the failure from "the newcomer gets crushed" to "nobody's actions matter". From the couch, an expert fires the 260-point showcase combo, 6 marbles drop on the friend's board, the friend clears two blue lines, and the board looks the same as before. The attacker gets no felt payoff, the defender gets no tension, and the round never resolves. A party game needs a visible swing and a clear ending more than it needs balance. So the mechanical BLOCKING they raised is also a reward-structure failure: attacking is the main verb of VS and it feels pointless. See revision R3.

**C2. Systems Designer F2 fix direction ("score on time-to-clear") vs. my F2 and Narrative Critic F3.** Systems is right that the Solo score barely tells players apart. But time-to-clear is the wrong fix for this game's players. Solo is the "sans pression", contemplative journey (Narrative F3 says the structure already works against contemplation), and autofill already adds time pressure from the first second (my F2). A clock-based score tells every player who looks at the leaderboard that the calm journey is really a race, and it hurts the novice most. Of the options Systems lists, "points per marble placed" or "keep the level running past the gauge for score" keep the leaderboard opt-in and leave the default experience calm. I would push the board toward those.

**C3. Adversarial QA F3 (leaderboard "rewards stalling") vs. Systems Designer F2 (score doesn't reward skill), seen through my F1.** QA calls the "hold at N−1, build one giant stack" pattern an exploit. From the player's side, it is the only place in Solo where the game rewards its core mastery skill, and that skill is the stack. My objection is narrower: it is not an exploit to remove but a reward that only players who already found the stack on their own will ever see (my F1). The leaderboard will separate "discovered the stack" from "didn't", not good from bad, and nobody is ever shown how to get from one group to the other. Fix the teaching before deciding whether the incentive is degenerate.

**C4. Business Analyst F1/F2 (pick one identity, maybe "mechanics-first competitive puzzle") vs. my F1/F2.** If the board recommends a pivot toward the competitive identity, the onboarding problem gets worse, not better. The competitive modes are where teaching is weakest: you learn the stack by watching an AI that may not be able to stack (Feasibility F2), VS has no ending (Systems/QA F1), and there are no comeback rules. Solo is the only mode where the first hour can be paced and controlled. On player-experience grounds, the protected core should be Solo, with a teaching level, before the competitive identity. This is a disagreement in recommendation, not in diagnosis, since BA and I both find that the identities pull against each other.

**C5. Narrative Critic F5 (victory = defeat, rated MINOR) vs. my F3 (rated MAJOR).** We cite the same rows and agree on the problem, so the disagreement is about severity. Narrative calls it MINOR because the fix is cheap. Severity should measure impact on the player, not cost to fix. My F3 is also wider: the only pre-loss warning ("Approche de la défaite") and the combo escalation are below MVP as well. In the MVP build, a player can lose without warning and then get the same explosion they'd get for winning. That breaks the basic loop of "know where you stand, then feel the result", so I hold MAJOR. I agree with Narrative that the fix is cheap, which makes it a strong quick-win candidate.

### CONNECTIONS

**X1. Feasibility F2 + my F1: the only stack teacher may not be able to stack.** I treated opponent 4 as a teacher that could be cut. Feasibility shows that even if it ships, a 1–2 ply greedy AI probably can't build the multi-move stored lines that "la tactique du stack, en la voyant chez l'adversaire" relies on. Add a learning-design point: watching what happens on the opponent's board while you manage your own under incoming attacks is a weak way to learn, because attention is on your own board. With combo escalation feedback also not MVP (my F3), an AI stack on the other side of the screen will probably look like a few unrelated pops. So all three supposed channels (Zen, opponent 4, onboarding) can fail, for different reasons: cut, technically infeasible, and too narrow in scope.

**X2. Feasibility F3 + my F2: nobody tests the first hour.** Every value behind my F2 complaint (10 gauges, one marble every 4 s, color weights) is a "valeur de départ, à régler en playtest", and Feasibility finds no playtest plan. Agent Test's job is to verify "que le niveau peut être terminé", which a bot can do on a level a human novice finds miserable. Verifying with a bot answers "can it be completed?", not "will a first-time player stay?". The plan needs at least one session with a novice playing level 1 cold, and the planned headless simulator should also report time-to-clear at a novice clear rate (about one match every 10–15 s), not only win rates.

**X3. Feasibility F1 + my F3: the feedback I flagged is the most likely to slip.** The Feedback agent's output is Niagara/material/animation assets that rely on the untested MCP path. If that path is fragile, Feedback work falls back to the solo developer, and "Important" items are the first casualties. So the MVP-vs-Important line in the events table is not a theoretical priority call. It decides what early-access players actually see. Danger warning and victory/defeat distinction should move to MVP precisely because they will be the only feedback that survives.

**X4. Systems F3 / QA F2 (cylinder wrap breaks "up/down") + my F4 (controls confusion).** The rule ambiguities they found are also perception problems. If the developer can't say where marbles fall in a wrapped column, a player can't predict it, and the stack depends on predicting where stored lines land. QA's contradiction also bears on my F4: the GDD says "le joueur peut donc toujours faire un move", yet "Move bloqué" ships in MVP with just a "choc sourd". The player is told nothing about a rule the document itself contradicts, so the blocked move will feel like a bug.

**X5. QA F4 + my F1: onboarding in a VS first game.** QA notes the onboarding fires "quel que soit le mode", including a first game in local VS, with no rule for which player gets the hint. Beyond fairness, this means that in the advertised party scenario (a friend picks up the second pad), the friend gets no onboarding at all, because it is tied to the machine's first game, not to each player. The newcomer who most needs it never gets it.

**X6. Business Analyst F4 + my F4.** BA calls "ne demande pas une grande maîtrise des commandes" an unevidenced audience claim. My F4 shows it is also contradicted inside the document by three directional verbs on adjacent keys. Together: the widest-audience promise rests on a control scheme that is harder than the competitors BA names.

### REVISIONS

- **R1. F1 (stack untaught): hold BLOCKING, higher confidence.** Systems F4, QA F4 and BA F3 reached the same cut-list problem independently, and Feasibility F2 adds that the remaining teacher may not work even if it ships (X1). This is the most corroborated player-facing issue on the board.
- **R2. F3 (feedback): hold MAJOR** against Narrative's MINOR (C5). Recommend the board list it as a quick win as well, since the fix is cheap and the impact is large.
- **R3. F5 (VS party game): withdraw the "one-sided stomp" mechanism, reframe, and upgrade to MAJOR.** The premise was wrong (C1). The corrected finding: VS attacks have almost no felt impact and rounds have no climax or end, which kills the party pitch. This is the player-experience side of the BLOCKING mechanical issue Systems and QA raised. I upgrade it because the mode is never cut and its failure is now established by arithmetic, not guesswork. The Arcade "what happens after a loss" gap remains.
- **R4. F2 (Solo level 1 pressure): hold MAJOR, with a caveat on fixes.** Narrative F3 independently found the same pressure-vs-calm mismatch at level 5. I add that a time-based leaderboard fix (C2) would make it worse. I support Narrative's suggestion of a protected slow-refill opening and would apply it to level 1 first.
- **R5. F4 (controls): hold MAJOR.** It is strengthened by QA's "Move bloqué" contradiction (X4) and BA's audience critique (X6).
