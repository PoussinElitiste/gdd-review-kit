# Technical Feasibility Lead — Round 1

Document reviewed: `gdd.txt`, "The marbles effect" (41 lines; a French-language GDD template that is only partly filled in). Era: contemporary (2020s). The template's own brief makes an AI agent "wow" moment the center of the technical strategy, so I judge it against current engines and current LLM/agent tooling.

---

## F1. The required technical strategy and AI "wow" moment are missing. BLOCKING

**Problem.** The document's own brief says the Technical Strategy section should be built around one AI-enabled moment "avant de développer quoi que ce soit d'autre." No such moment is identified anywhere. There is no "Stratégie technique" section. The two headings that would hold the AI architecture, "# Agents Roles" and "## Token Cost budget" (lines 38-40), are empty.

**Passage / omission.** Lines 4-6: "identifiez le moment, rendu possible par l'IA, qui donne envie d'y jouer… Concevez toute la section « Stratégie technique » autour de la réussite de ce moment." Lines 38-40: empty headings.

**Why it blocks.** The project's main technical risk is undefined. I can't tell what the agent does, whether it runs at runtime or only during development, what latency a real-time puzzle loop can tolerate, whether it calls the network on every move, or what each session costs. As written, the concept is a conventional match-3 with no AI component. The two possible readings are the AI feature was cut silently, or it hasn't been designed yet. Either way production can't start until it is resolved, and there is no token or cost ceiling to design against.

---

## F2. The Scope section is empty: no team, timeline, platform or content count. BLOCKING

**Problem.** "## Scope" (line 36) has no content. The document doesn't give team size, schedule, target platform, engine, number of levels/boards, or what counts as done.

**Passage / omission.** Line 36: `## Scope` followed directly by `# Agents Roles`.

**Why it blocks.** Feasibility is scope measured against resources, and both sides are blank. This matters more than usual here because the cost of the art and audio direction (F4) depends almost entirely on platform (real-time glass reflections on mobile vs. PC/console) and on how many distinctly themed levels are promised. Nothing can be estimated, scheduled or cut.

---

## F3. The core mechanic is too ambiguous to prototype, and the follow-up spec is not referenced. MAJOR

**Problem.** The single-sentence concept contradicts itself and leaves out the rules a programmer needs. The player slides "une colonne de billes verticalement et horizontalement," but a column slides vertically by definition. Horizontal movement implies rows, so is the cursor selecting a row, a column, or either? Matches count only horizontally. That asymmetry matters for implementation: vertical slides only produce matches incidentally, which changes board-evaluation and cascade logic. The document doesn't specify grid dimensions, whether slides wrap around (Lose Your Marbles–style shifting) or push marbles off the edge, refill or gravity rules, cascades/chains, fail state, or move/timer pressure.

**Passage / omission.** Line 18: "glisser une colonne de billes verticalement et horizontalement afin d'aligner au minimum 3 bille de même couleur horizontalement." Line 8 says deeper mechanics "must be define in a separate document," but no such document is named or linked.

**Why it matters.** Match-3 logic is low-risk technically, but only once the rules are fixed. With these ambiguities, two programmers would build two different games. Every downstream system (VFX triggers, audio sync, level design, and any AI involvement) is keyed to match and cascade events that aren't defined yet. This should be a one-day fix, but it blocks the first prototype.

---

## F4. The art and audio target is the real cost driver, and nothing is budgeted or marked as cuttable. MAJOR

**Problem.** The "Fantasy" section commits to a Tetris Effect–class audiovisual identity: ASMR-grade sound, "lounge electro" music, light reflections in glass marbles, collision sounds during manipulation, environmental visual effects, and "une thématique très prononcée entre niveau." Tetris Effect relied on audio-reactive, music-synced VFX across many fully themed stages and was built by an experienced, specialized studio. Here that is listed as ambiance bullets with no priorities. Per-level themes make content cost grow with level count, and level count isn't given (F2). Convincing glass refraction and reflection is a real rendering cost on low-end or mobile hardware. Platform isn't given either.

**Passage / omission.** Lines 22-29 (Fantasy list) and line 34 ("Tetris Effect" as an inspiration). Nowhere does the document say which of these effects are core and which can be cut.

**Why it matters.** This is the part most likely to blow the schedule, and it is the part most likely to be cut silently late in production. Because the document claims the game's appeal *is* the ASMR feel, cutting it late would take the pitch with it. The document needs a ranked list of required vs. optional effects, a minimum art/audio set (e.g., one fully realized themed board), and a statement of whether music/VFX are synced to gameplay events. That sync is a separate engineering task beyond plain layered audio.

---

## F5. Audio and VFX event specs are incomplete. MINOR

**Problem.** The sensory feedback is the core feature, yet at least one key event is left unfinished. "Bruit de verre cassé lors" (line 28) never says what triggers it: a match, a clear, a failure, a combo? The document also never ties manipulation sounds or environmental effects to specific gameplay states. Audio and VFX implementation needs an event table (event → sound → visual → intensity scaling), and the hooks have to exist in the match logic from the first prototype.

**Passage / omission.** Line 28 (sentence cut off). Lines 27-29 are not linked to any rule in line 18.

**Why it matters.** It's cheap to fix now and expensive to retrofit. If feedback hooks are added after the board logic is written, the cascade code usually has to be reworked to expose timing and intensity data.

---

**Overall feasibility read:** The gameplay core (a sliding match-3) is low-risk and well precedented (SegaSoft's *Lose Your Marbles*, 1997). But the document leaves blank every section where feasibility would be decided: scope, technical strategy, agent roles and token budget. Its most expensive commitment, the audiovisual layer, has no budget or cut plan. As written it is not buildable to a plan.

---

## Round 2 — Cross-examination

### CONFLICTS

**C1. Systems Designer F1/F2, Adversarial QA F2/F3, Player Psychologist F1: the core-mechanic ambiguity rated BLOCKING. I hold at MAJOR.**
Four colleagues rate the slide/match ambiguity as BLOCKING; I rated it MAJOR (my F3). I'm keeping my rating, and the reason is practical. A blocker is something you can't get past by deciding. The row/column contradiction, wrap vs. push, and horizontal-only matching are design decisions a lead can settle in an afternoon with a whiteboard, and a grey-box prototype in any modern engine follows within days. The Scope and AI-moment gaps (my F1/F2) are different: nobody on the team can close them without resources, budget and an architecture choice. If everything is BLOCKING, the board can no longer say which gaps are cheap and which are expensive, and production will treat a one-day fix like a project risk. My concession is that I'm not saying the gap is acceptable, only that it's cheap. It has to be closed before any other work starts, so it is first on the critical path even though it's MAJOR.

**C2. Systems Designer F5 and Player Psychologist F4: the missing AI moment rated MAJOR. I hold at BLOCKING.**
Both treat the absent AI moment as a missing hook or a missing system. From a build perspective it decides the architecture. A runtime LLM agent in a real-time puzzle loop needs answers before any code is written: is it online-only, what latency budget fits between moves (an agent call can take hundreds of milliseconds to seconds, which a slide-and-match loop can't absorb synchronously), what runs as fallback when the call fails, and what each session costs. A dev-time-only agent (for example, content generation) needs none of that but changes the asset pipeline instead. Those two branches produce different codebases. The document's own brief (lines 4-6) puts this decision ahead of everything else, and the Token Cost budget heading (line 40) shows the authors expected a runtime cost. It stays BLOCKING.

**C3. Player Psychologist F4's example AI moment ("music or visuals that react to how the player plays").**
I disagree that this is a good illustration. Adaptive music and reactive VFX are deterministic systems; Tetris Effect shipped them without any AI agent. Building that with an LLM agent would add latency, network dependency and token cost for an effect a rules-based audio director does better and more cheaply. If the team picks this as the "wow", feasibility will push back: either the moment really needs an agent (open-ended generation, natural-language interaction, per-player content), or the brief is being met in name only. The board shouldn't pass along an example that would make the build cost more without benefit.

### CONNECTIONS

**X1. Business Analyst F3 (Lose Your Marbles is a versus game) + my F2 (no scope): a possible netcode commitment no one has costed.**
I cited Lose Your Marbles only as proof that the mechanic is low-risk. The Business Analyst points out that it is a head-to-head game. If "versus" is in the product, the scope picks up attack/garbage systems, competitive balance, and either local split-screen or online netcode with matchmaking. Online play is the most expensive item that could be added to this concept, and the document doesn't say single-player or multiplayer. That pushes my F2 even further from estimable: the scope range now runs from a solo puzzler to an online competitive game. Game mode must be the first line of the Scope section.

**X2. Narrative Critic F4 / Player Psychologist F3 (breaking glass on match vs. fail) + my F4: performance cost depends on the answer.**
The tone question is also a performance question. If glass shatters on every match, and cascades exist (unspecified, see my F3), one slide can set off several simultaneous shatter effects, with particles, debris and refraction shaders, on top of environmental VFX. On mobile, that's the frame-rate worst case, and it lands exactly at the reward moment. If shattering only happens on failure, the cost is rare and easy to absorb. Same for audio: ASMR-grade clink and shatter sounds that fire hundreds of times an hour need many recorded variations and pitch/velocity layering to avoid repetition fatigue. That's a real sound-design content cost the document doesn't count. Closing the line 28 sentence therefore settles tone, performance budget and audio content scope together.

**X3. Player Psychologist F5 / Adversarial QA F5 (colour-only matching, readability) + my F4: accessibility is a shader constraint that has to exist from the start.**
Adding symbols or patterns to the marbles isn't a late UI patch in this game. The marbles are refractive glass with reflections, so a symbol inside them has to stay readable through the refraction shader under every theme's lighting and environmental VFX. That constraint needs to be in the marble material and in the per-theme art brief from the first shader prototype. Retrofitting it after N themed marble sets exist means redoing every set. That makes per-level theming (Business Analyst F4, my F4) more expensive than either finding states alone.

**X4. Business Analyst F4 (per-level theme content is the costliest commitment) + my F1 (no AI role): the only place an agent obviously cuts cost.**
I'm not proposing a design, but the board should note one point: per-theme art and audio is the largest content cost in this document, and a dev-time generative pipeline is the agent role most likely to reduce cost rather than add it. Whether or not the team goes that way, the Agents Roles section has to say if the agent is a runtime feature (adds cost, adds risk) or a production tool (may reduce content cost, adds pipeline and quality-control work). Both readings are currently possible.

**X5. Adversarial QA F3 (deadlocks, spawn-matches) adds engineering scope to my F3.**
I called match-3 logic "low-risk." QA's finding lists items that aren't free: detecting deadlocked boards, generating boards with no pre-existing matches that still have at least one solution, and resolving simultaneous matches deterministically. These are well-known solved problems, but each one is a task and a source of bugs, and solvability guarantees under a wrap-around slide rule depend on rules that aren't specified yet. I still rate F3 MAJOR (see C1), but the "one-day fix" covers only the rule decision, not the implementation.

### REVISIONS

- **F1 (AI moment / technical strategy): BLOCKING, unchanged.** Five of six reviewers flagged it. My rating is higher than Systems Designer and Player Psychologist for the reasons in C2.
- **F2 (empty Scope): BLOCKING, strengthened.** The Business Analyst's open single-player vs. versus question (X1) and the missing platform (BA F2) widen the possible scope range to include netcode.
- **F3 (ambiguous mechanic): MAJOR, held against four BLOCKING ratings (C1).** I've added implementation scope (X5) and withdrawn the implied claim that the whole fix costs one day. Only the decision does.
- **F4 (uncosted art/audio): MAJOR, strengthened.** Three colleague findings add cost drivers I didn't count: possible per-match shatter VFX (X2), audio variation volume for ASMR (X2), and accessibility-aware marble shaders across every theme (X3). I'd rate it the top MAJOR for schedule risk. I'm not raising it to BLOCKING because a ranked cut list can contain it once scope exists.
- **F5 (audio/VFX event table): MINOR, unchanged in severity, partially folded.** Systems Designer F4 makes the same point from the loop side and rates it MAJOR. I keep MINOR for the engineering task itself (the hook table is cheap). The line 28 decision it depends on carries more weight, and I've moved that to F4 via X2.
