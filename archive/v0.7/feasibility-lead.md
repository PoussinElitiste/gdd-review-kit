# Technical Feasibility Lead — Round 1

Document reviewed: *The Marble Effect*, GDD v0.2 (22 Oct 2026) plus its linked documents. I judged it against 2026 conditions: one developer using Unreal and Claude agents connected to the editor over MCP.

---

## F1. The 7-week schedule has no milestones, and the Steam release steps are missing — BLOCKING

**Problem.** The scope is 4 modes, 5 themed levels (each with its own environment, music, skin and intro), 5 AI opponents, local VS and a Steam Early Access launch. One person builds all of it in 7 weeks, and those weeks include writing the GDD (Technical Strategy > Scope: "en 7 semaines (rédaction du GDD comprise)"). The budget plans about 190 agent sessions (Budget_tokens.md). Even if GDD writing takes only one week, that is about 32 sessions a week, or 4-5 a day with no days off. Each session's output has to be reviewed, integrated in the editor and playtested by the same single person. That human review is the real bottleneck, and the document never sets a budget for it.

**Passage / omission.**
- The "Ordre de production" gives a sequence but no dates or week numbers. The token-budget fallback (Contrainte 4: "Si la moitié du plafond est dépensée avant la moitié du projet") depends on a midpoint that the plan never defines in terms of deliverables.
- Nothing covers what an Early Access launch needs besides the game itself: the Steamworks setup, the store page and its required visibility period before release, build review, capsule art and trailer. The GDD is dated 22 October 2026 and 7 weeks takes it to mid-December. That window is tight even before counting the platform's own lead times.
- Most mode specs are still unwritten: "Mode Solo / Arcade / VS / Zen (à rédiger)", "Menu et options (à rédiger)", "Leaderboard (à rédiger)". Specification work is still ahead of a production clock that has already started.

**Severity: BLOCKING.** Without a dated milestone plan, the cut list has no trigger points. The project will discover it is late only when it is already too late to cut cleanly.

---

## F2. The cut list conflicts with the opponent-to-level mapping, and several features are in neither the production order nor the cut list — MAJOR

**Problem.** The cut order (Scope: "le multijoueur en ligne, les événements Optionnel, les niveaux 4 et 5, le mode Zen, puis l'adversaire 4") was written without checking how features depend on each other.
- **Opponent 5 is "never cut", but level 5 can be.** Themes_niveaux.md assigns one opponent to each level ("5 La Nébuleuse … n°5"). The Arcade fill rates also come from that level table. If levels 4 and 5 are cut, opponent 5 has no level, no music and no fill-rate tier, and the "wow" finale happens in a level-3 setting. The document does not say which assets or tuning opponent 5 would use.
- **Cutting levels 4 and 5 also shrinks Solo**, which is "never cut", from a 5-level course to 3 levels. The sentence "Le Solo … ne [sont] jamais coupés" hides that.
- **Unscoped features that appear in every mode.** Bonus/malus marbles "peuvent apparaître dans tous les modes" and have an audio/visual event rated "Important", but their spec is "à rédiger". The Solo leaderboard ("classement") also has an unwritten spec, and a Steam leaderboard means platform integration work. Neither feature appears in the production order or the cut list, so nobody has decided whether they ship.

**Severity: MAJOR.** The fallback plan, which the schedule depends on, would produce a build that contradicts its own "never cut" promises.

---

## F3. The game's main selling point depends on assets with no source, schedule or budget — MAJOR

**Problem.** The positioning is an audiovisual journey in the style of *Tetris Effect* ("environnement « wow »", 5 themed levels with distinct music at 85-115 BPM, music layers that build with intensity, intro sequences). The production plan says "Les assets restent des placeholders" and that textures, models and music come "de l'artiste, de placeholders ou de bibliothèques d'assets, hors budget tokens" (Contrainte 1).

**Passage / omission.**
- Events_visuels_et_sonores.md rates "Musique propre à chaque niveau" as **MVP**. Yet no agent can produce music, no artist or composer is on the schedule, and no money is set aside for licenses. The layered, adaptive music (the "Palier atteint" and "Tableau qui se remplit" events, rated Important) needs stems written for that purpose. Off-the-shelf library tracks rarely come as stems.
- The Agent Intro is supposed to generate "une séquence d'introduction différente pour chaque niveau". Cinematic sequences with only placeholder assets will not deliver the stated effect.
- No production step integrates final assets. Step (5), "niveaux complets", assumes they exist.

**Severity: MAJOR.** The core loop can be built. The thing that sets the game apart has no route to production, and an Early Access page with placeholder art undercuts the "wow" pitch on day one.

---

## F4. Key systems depend on Unreal editor assets and an automated test harness that nobody is scoped to build — MAJOR

**Problem.** Contrainte 2 relies on C++ to work around binary .uasset files. However, several MVP pieces are editor assets by nature: UMG menus (Agent Menu), Niagara VFX and materials (Agent Feedback, Agent Skin), level sequences (Agent Intro) and, if the native system is used, **Behavior Tree and Blackboard assets** (Agent NPC). All of these can only be built through the MCP connection to the editor. The document treats that connection as a solved problem and gives no fallback if it is unreliable for a given asset type.

**Testing omission.** The Agent Test runs on Haiku for 40 sessions, at about $7 in total. It is supposed to verify that "chaque niveau peut être terminé" and that each opponent "peut gagner et perdre". For a real-time game, that requires a headless, deterministic simulation of the rules, or Unreal automation that plays matches at faster-than-real-time speed. Neither is listed as a deliverable for any agent or as a production step. Alignement_et_elimination.md mentions that "Une simulation des règles a montré…", so some simulator exists. The plan does not say whether it is the same rules code as the game, so the two could drift apart. The generation guarantees (no ready-made alignments, 5 reachable useful marbles, refill rules that restore the guarantee) are the kind of rules that need this harness to be trusted. The GDD itself records that an earlier version of the guarantee failed in simulation.

**Severity: MAJOR.** These are the riskiest integration points, and they have no owner and no time allocated.

---

## F5. The budget fallback has no real effect, and the cost assumptions contradict each other — MINOR

**Problem.**
- Contrainte 4 says that if spending runs ahead, "les tâches courantes passent d'Opus à Sonnet et le multijoueur en ligne est coupé". In Budget_tokens.md, routine tasks already run on Sonnet (Opus is used only for Gameplay and NPC), and online multiplayer is already outside the MVP figure. If the trigger fires, the MVP budget saves almost nothing.
- Contrainte 3 says "chaque agent ne reçoit que les fichiers de sa partie". The cost model assumes "environ 1,5 million de tokens en entrée (le projet relu à chaque étape)" for every agent, including Haiku. The table itself notes that the Haiku rate applies only to requests under 100,000 tokens, with a higher rate above that. If test requests carry real project context, the Agent Test line could cost several times its estimate of about $7.
- Budget_tokens.md also notes that running agents under a subscription changes the whole model, but the document does not choose between the API and a subscription.

**Severity: MINOR.** The amounts are small in absolute terms, but the only financial safeguard does not actually do anything.

---

## Round 2 — Cross-examination

### CONFLICTS

**vs. Business Analyst F1 (selling point not built, BLOCKING) against my F3 (same problem, MAJOR).** We agree on the facts and disagree on severity. My position: from a build standpoint this is not BLOCKING. The core loop, the AI and the modes can all be built with placeholders, and the missing art and music can be solved by a decision that costs no production time: either name and pay an asset workstream, or reposition EA as mechanics-first (BA's option b). A BLOCKING finding should mean "production cannot proceed until this is fixed", and production can proceed here. One part of BA's case I accept and fold into my F1: the Steam store page has to be visible well before launch and needs capsule art and a trailer, so the art question has a deadline at roughly the midpoint of the 7 weeks, not at the end. The decision is urgent. The finding is still MAJOR.

**vs. Business Analyst F4 (protect Zen and adversary 4; cut intros early).** I agree with the reordering, and I support putting Agent Intro's five per-level sequences high on the cut list. Those are editor-only Level Sequence assets (my F4) with placeholder art (my F3), so they are the highest-risk, lowest-return item in the scope. I disagree with "protect a minimal Zen". "Mode Zen (à rédiger)" has no spec, and it carries its own options (a bonus/malus toggle per Adversarial QA F4, which means a second rule set to test). A guided tutorial sequence that reuses the Solo level code with the timer and spawn disabled is cheaper than a mode with its own menu flow and options. What should be protected is the tutorial content, not the Zen mode as written. Similarly, protecting adversary 4 is cheap only if the AI can actually stack, and Adversarial QA F5 says it cannot (see Connections).

**vs. Business Analyst F3 (local VS protected while online is cut first).** The audience argument is reasonable, but the implied remedy (online for the competitive Puyo/Tetris audience) is not buildable in this plan. A real-time competitive puzzle game needs synchronized deterministic simulation or rollback, Steam networking, matchmaking and disconnect handling. One person cannot add that to a 7-week schedule that is already over-full (my F1). Cutting online first is the one cut-list decision that is technically right. If the board takes BA's advice to pick a primary audience, the feasible pick is solo score-chasers, which makes the leaderboard load-bearing. That turns my F2 note ("a Steam leaderboard means platform integration work") from a loose end into a scheduled deliverable that needs a score formula (Systems Designer F5) before anyone can build it.

**vs. Narrative Critic F1 (cut list should protect the arc's ending "even as one compressed finale").** I agree the current cut list is incoherent. I disagree with the remedy if "compressed finale" means new content: a merged level 4/5 finale would be a sixth level design, a new theme blend and new tuning. That is added scope during a slip. The feasible version is a reorder with no new work: cut level 4 alone first (and its paired opponent only if the board accepts Systems Designer F3 / Player Psychologist F5's warning about the stack lesson), and keep level 5 so that opponent 5, the Solo finale and the arc ending all keep the same existing assets. That resolves Narrative F1, my F2 and Adversarial QA F3 with one cut-list edit.

**vs. Narrative Critic F3 and Player Psychologist F3/F4 (distinct win/loss beats, panic cue to MVP, untimed first level).** No conflict on substance. I note these are among the cheapest requests on the board: differentiating win from loss and promoting the "approche de la défaite" cue to MVP are parameter changes to VFX and audio the Feedback agent already builds, and an untimed level 1 is a tuning flag. I would support them as quick wins, unlike the outro and per-level sequences, which are new Level Sequence content.

### CONNECTIONS

**C1. The core rule hole is a schedule blocker, not only a design blocker (Systems Designer F1 + Adversarial QA F1 + my F1/F4).** Two colleagues independently showed that the loss condition cannot be reached reliably: a defender can push columns to L1 and lose every attack ball, and the spawn rule for columns at L1 is unwritten. From my lens, the problem is the dependency chain. Gauge targets, spawn rates, Arcade fill rates, all five opponent behaviours and the Agent Test acceptance criteria depend on the end-of-round rule. The production order puts Gameplay first and NPC after it, so any rule change after week 2 or 3 invalidates AI tuning that the Opus NPC sessions have already paid for. The 7-week clock is already running while the most fundamental rule is still open. This strengthens my F1: the first milestone has to be "rules frozen and proven in simulation", and the plan does not have it.

**C2. Adversarial QA F5 (1-2 action lookahead cannot stack or release) combined with my F4 (no test harness, Behavior Tree assets via MCP).** The opponent table promises behaviours a greedy 2-ply search cannot produce, so the fix is a goal- or plan-driven AI. That is a bigger engineering task than the document budgets for the NPC agent, and a planner needs a fast forward-simulation of the board, which is the same headless rules simulator my F4 says is unscoped. One missing deliverable (a rules core in C++, shared by game, AI and tests) is behind three findings: QA F5's AI, QA F2's soft-lock (which the existing simulator already detected), and Systems Designer F2's unvalidated gauge targets. Also, per QA F3, the production order builds opponent 5 before opponent 4, so the opponent meant to combine every behaviour is built before the hardest behaviour (stacking) exists. The plan should build 4 before 5, or define 5 without stacking.

**C3. Bonus/malus is now a test-surface risk, not just a scope gap (Adversarial QA F4 + Business Analyst F2 + my F2).** Three of us flagged the missing spec. QA's point that bonus/malus effects collide with absolute rules (determinism, the generation guarantees, the AI's "ne triche pas") means that if they ship, they multiply the test matrix that a $7 Haiku test line (my F5) is supposed to cover. The cheapest technical resolution is to move them out of the EA build explicitly.

**C4. Music is the asset the plan ranks highest and can source least (my F3 + Business Analyst F1).** BA notes the per-level environment is only Important and the glass reflections are Optional. My F3 notes that per-level music is MVP. So the plan's own priority table makes music the must-have audiovisual asset, and music is the one asset class no agent can produce and that adaptive layering needs in stems. That makes music licensing or commissioning the single most urgent non-code procurement decision.

### REVISIONS

- **F4: upgrade MAJOR -> BLOCKING.** Systems Designer F1/F2 and Adversarial QA F1/F2/F5 show that the rules, the AI and the tuning cannot be trusted without a shared, headless simulation of the rules. The document already relies on one ("Une simulation des règles a montré…") without making it a deliverable. Without it, Agent Test cannot verify "chaque niveau peut être terminé", and the AI cannot plan. This is now the gating technical deliverable.
- **F1: kept BLOCKING, strengthened.** Added C1: the milestone plan must open with a rules-freeze milestone, and the art/store-page deadline sits mid-schedule (from BA F1).
- **F2: kept MAJOR, scope widened.** Combined with QA F3, BA F4 and Systems Designer F3, the cut list is also out of order with the production order (opponent 5 before 4) and with onboarding dependencies. The single fix I recommend is above: cut level 4 alone first, keep level 5, and move bonus/malus and per-level intros onto the cut list.
- **F3: kept MAJOR** despite Business Analyst's BLOCKING (argued above). Narrowed to music as the critical-path item (C4).
- **F5: kept MINOR.** No colleague engaged with it; it remains true but low-impact. If bonus/malus stays in scope (C3), the Haiku test-line underestimate grows, but it remains small in absolute terms.
