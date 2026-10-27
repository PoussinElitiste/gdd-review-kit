# Technical Feasibility Lead — Round 1

Document reviewed: *The Marble Effect* GDD v0.2 plus linked docs (Combo_system, Schemas_deplacement, Evenements_visuels_et_sonores, Themes_niveaux, Budget_tokens). Judged against 2026 constraints: Unreal Engine, agentic AI coding via MCP, one developer, 7 weeks.

---

## Finding 1: The MVP is still too big for one person in 7 weeks, and the cut list leaves out the biggest costs
**Severity: BLOCKING**

**Problem.** The schedule is "7 semaines, rédaction du GDD comprise", with "Une seule personne" handling modes, menus and mechanics, plus an artist who "pourra" (may) join. That leaves about 5 build weeks. The MVP still includes four modes (Solo, Arcade, VS local, Zen score), 5 fully themed levels, a scripted AI opponent, leaderboards ("enregistrer ses records dans un classement") and local two-player "sur toutes les plateformes". The cut list has only two items: "Mode multijoueur" and the "Optionnel" events. In the events table only two rows are marked Optionnel (the environment reward accent and glass reflections), so the second cut saves almost nothing.

**The omission that matters most.** "Plateformes : PC, console, mobile et VR" is never scoped. It is not in the MVP, not in the cut list, and the schedule doesn't mention it. Each platform needs its own control scheme (the doc already specifies three; VR has none), its own UI layout, and its own performance budget. Console also needs devkits, publisher or platform-holder access, and certification. Local two-player on mobile on one screen is not addressed at all. The Steam early-access line suggests PC is the real target, but the document never says the other three platforms are out of scope for this production.

**Also missing:** no milestones or week-by-week plan, no vertical slice, and no build order. Because of that, nothing says what still ships if week 5 slips.

---

## Finding 2: Core rules the code depends on are still marked "Reporté" or "À définir"
**Severity: MAJOR**

**Problem.** The Agent Gameplay (40 Opus sessions, the largest single cost) is supposed to build the board, movement, elimination and combo system. Several rules it needs are explicitly unfinished:
- Delay between attack waves: "Reporté à la version suivante". The counter-attack/cancel window depends entirely on this value, so the Puyo-style VS system can't be built or tuned without it.
- How each wave's 3 marbles are spread across the opponent's columns: "Reporté". The doc admits "le schéma les place arbitrairement".
- Simultaneous alignments from one move: "Reporté". This decides whether chains are counted at all, so it affects score, the combo counter and attack size.
- Number of marble colours: never stated. Both the no-pre-made-alignment generator and the "au minimum 5 billes de même couleur" guarantee depend on it.
- Spawn rate for continuous refill, and how fast it ramps per level: not given. Themes_niveaux, where the difficulty curve should be, is an empty table ("À définir" in every cell).
- Bonus/malus marbles: listed in every mode's progression and in Zen options, but the linked document "Billes bonus/malus" has no content or link at all.
- The generation rule is ambiguous. "Une nouvelle bille est générée aléatoirement pour garantir au minimum 5 billes de même couleur" contradicts or duplicates the "au moins 5 billes par colonne" rule a few lines earlier. An agent implementing literally will pick one reading.

**Risk.** With agentic code generation, unspecified rules don't stay empty. The agent fills them with arbitrary choices that then have to be found and reworked, which eats the session budget.

---

## Finding 3: The AI opponent, the "wow moment", is underspecified and probably the wrong technique
**Severity: MAJOR**

**Passage.** "Agent NPC : génère un profil scripté (behaviour tree) pour piloter les tableaux en mode arcade" and "Moment « wow » rendu possible par l'IA : le challenge corsé que propose le NPC".

**Problem.** A behaviour tree chooses between behaviours. It can't find good moves in this game by itself. An opponent that "enchaîne les combos" has to search sequences of moves and switches on a 5x15 board with column-length limits, judge stacked alignments, and decide when to release its counter or hold it to counter-attack. That means a board evaluator and a search or planner, which is a separate system the document doesn't mention. The difficulty ramp for 5/10/15 opponents is described only as "plus agressive", with no tunable parameters (move rate, search depth, deliberate errors). The NPC also depends on the attack-wave timing from Finding 2, which isn't defined. The feature the document calls its showcase sits on the least-specified and most technically risky system, and it gets 25 sessions with no fallback plan.

---

## Finding 4: The asset pipeline has no owner. The GDD promises "wow" audiovisuals, but its own budget says the agents can't make them
**Severity: MAJOR**

**Passage.** Budget_tokens, "Limites": "Claude génère du texte et du code, pas d'images ni de musique ... les textures, modèles 3D et pistes musicales viendront de l'artiste, de placeholders ou d'autres outils, dont le coût n'est pas compté ici."

**Problem.** These items are rated MVP or Important: "Musique propre à chaque niveau" (MVP, 5 tracks), 5 themed environments, 5 board and marble skins with a different accessibility pattern set per theme, an intro sequence per level (Agent Intro), several variants of every frequent sound, and end-of-game explosion and dissipation scenes. The roles list still says "Agent Ambiance : intègre les musiques" and "Agent Skin : génère les skins", as if the content will just be there. The only possible content source is an artist who "pourra" take part. No composer or sound designer is named, there is no asset list, and there is no money budgeted for them. The positioning depends on a "réalisation modernisée, avec un environnement « wow »". On a placeholder build, that pitch disappears.

---

## Finding 5: The agent workflow assumes Unreal can be driven by text agents, and automated testing is unrealistic
**Severity: MAJOR**

**Passage.** "Le jeu sera développé sous Unreal, avec une connexion MCP vers un agent IA." "Agent Test : teste la boucle de jeu de chaque niveau" (Haiku, 40 sessions). Each session is assumed to use about "1,5 million de tokens en entrée (le projet relu à chaque étape)".

**Problems.**
- **Unreal content is mostly binary.** Blueprints, materials, Niagara VFX and UMG menus are stored as binary .uasset files. An MCP bridge can automate parts of the editor, but the document doesn't say whether gameplay will be C++ (which agents can edit) or Blueprint (which they mostly can't). It also doesn't check how reliable the MCP bridge is. This is the main technical risk of the whole production model, and it isn't acknowledged.
- **Agent Test can't really test a game.** Checking that a real-time game "loop" works needs either deterministic, headless simulation of the board logic or someone playing it. The document plans neither. Running the cheapest model on 1.5M-token sessions also clashes with its own note that Haiku pricing applies "aux requêtes de moins de 100 000 tokens".
- **Human review time is the real bottleneck, and it isn't counted.** 190 to 210 agent sessions in about 5 build weeks is roughly 40 per week for one person to review, integrate and debug. The budget counts dollars and leaves out these hours. The 50% margin covers token cost, not schedule.
- **The dependency chain is serial but not planned that way.** Visuel, Sonore, Ambiance and Test all depend on the Gameplay events, and NPC depends on Combo/attack. The 11 agents are described as parallel roles, with no integration order or interface contract (for example an event bus shared with the events table).

---

## Round 2 — Cross-examination

### CONFLICTS

**vs. Business Analyst #3 (competitive audience: "If it is competitive, online VS belongs in the MVP").** I disagree with this branch of the recommendation and want the board to close it off. Real-time online VS for a falling/sliding-block puzzle game needs a deterministic simulation, input-delay or rollback netcode, a matchmaking/session backend, and desync testing. That is a multi-month job for a dedicated network programmer, and the document has one generalist and about 5 build weeks (my Finding 1). Moving online into the MVP doesn't "cut something else". It replaces the whole MVP. On feasibility grounds the only workable answer to BA #3 is the casual branch: drop the competitive claim for Early Access, ship local VS only, and keep leaderboards local or Steam-native (Steam Leaderboards needs no custom backend). I agree with BA that the two audiences conflict. I disagree that the competitive option is actually available at this scope.

**vs. Business Analyst #2 (audiovisual "wow" unfunded, BLOCKING) and #4 ("point the budget at" one wow).** We found the same gap (my Finding 4). Two disagreements. (a) Severity: from the build side I keep it MAJOR, not BLOCKING. The mechanics, AI and modes can all be built and tested on placeholders, and the gap is closed by a scope decision (one finished theme), not a technical breakthrough. For the product case I accept BA's BLOCKING: an EA build without the audiovisual layer has no pitch. (b) BA #4 suggests moving Opus sessions from the NPC to art direction. That doesn't work. Budget_tokens itself says the agents produce no images or music, so tokens can't buy the wow. Pointing the budget at the audiovisual promise means money for a human artist and composer, which is a cost line the document doesn't have (BA #5). The agent-side share of the wow is small: shaders, Niagara wiring, the music-layer system. The real cost is content.

**vs. Narrative Critic #5 (opponents need identity) and #4 (distinct victory/defeat endings, run-completion payoff).** Every one of these fixes adds content to a production that already can't fill 5 themes (my Finding 4, Narrative's own #1). Named opponent personas mean portraits, writing, and probably per-opponent intros, all from an artist who is only a "pourra". A run-completion ending sequence is another non-MVP scene. I'd push the board to take Narrative #5's own fallback: declare the opponents deliberately faceless. That costs nothing and fits an abstract journey. For #4 the cheap fix is a parameter, not new content. Victory and defeat share the explosion system but use different music stingers, colour grading and dissipation direction. That should be marked MVP so the "Important" tag doesn't let it get cut entirely.

**vs. Player Psychologist #1 (onboarding must teach release, stack, waves, cancellation).** I agree with the diagnosis but warn against the obvious fix. A scripted multi-step tutorial with forced board states, prompts and validation is one of the most bug-prone and iteration-heavy features in any puzzle game, and it touches every system in Finding 2 that is still undefined. It can't be built until those rules are frozen, so it would land in the last week. The feasible version is cheaper: (1) make the existing hint system replayable and switchable from the options, (2) extend its trigger to highlight a release-capable line and a pending wave, not just a 3-match, and (3) make Zen (spawn rate adjustable, see below) the practice room. That covers most of what the psychologist wants using systems that are already in scope.

### CONNECTIONS

**Adversarial QA F1 (median-line soft-lock) + my Finding 5 (Agent Test can't test).** QA's soft-lock is exactly the kind of bug that proves my Finding 5. It only shows up after a specific sequence of moves on a specific board state. A Haiku agent "testing the loop" through an MCP editor bridge won't reach it. A human playtester might take hours. A headless, deterministic C++ board simulation with a random-move fuzzer would find it in seconds, and would also catch QA F2 (release unreachable) and F3 (overflow at column 15). This turns my Finding 5 recommendation into a concrete requirement: the board rules must live in a pure C++ module, separate from Blueprint/UMG presentation, so they can be fuzzed and so agents can edit them as text.

**Adversarial QA F2 (5-line effectively reachable only via cascades) + Systems F1 (no release probability model) + my Finding 3 (AI technique).** If, as QA argues, a release mostly comes from cascades, then the NPC can't be a behaviour tree choosing between behaviours. It has to look several moves ahead through cascade resolution to find any release at all. The search cost rises a lot. Without a release, the NPC can never attack, which removes the "challenge corsé" the document calls its wow. Combined, these three findings mean the showcase feature may be impossible to make competent at this scope. The feasible fallback is a "cheating" opponent: a heuristic player that fakes attack pressure on a tuned schedule instead of earning it on its board. The document should name that fallback now.

**Systems F3 (combos don't feed attack) + my Finding 3.** If the board accepts Systems' fix and makes chains the attack multiplier, Puyo-style, the NPC's evaluation function has to value chain setups, which is a known-hard AI problem (Puyo AIs are a research topic in their own right). The systems fix is right for design, but it raises the NPC's technical risk again. The two decisions must be made together.

**Player Psychologist #4 (accessibility patterns change per theme) + my Finding 4 (no asset pipeline).** Fixing the accessibility problem is also a scope cut. One global pattern set (round, bar, cross, ring) instead of 5 theme-specific sets removes 4 pattern-set authoring passes from an art pipeline with no confirmed artist. This is the cheapest high-value change on the board.

**Systems F2 / QA F5 / Narrative #1 (15-level runs over 5 levels) + Agent Intro budget.** "Une séquence d'introduction différente pour chaque niveau" over a 15-level run means 15 intros, or 5 intros repeated with no defined rule. The cheap resolution is parametric: levels 6-15 reuse the 5 themes with a difficulty table (gauge size, spawn rate, AI parameters as numbers), and the intro plays only on a theme change. Without that rule, Agent Intro's 10 sessions are sized for an unknown amount of work.

**QA F4 / Psychologist #5 / Narrative #2 (Zen never ends, or always ends).** Three reviewers want Zen's spawn rate to be able to reach zero. Technically that is trivial, but it reopens QA F1: a zero-spawn Zen game with a stranded column never ends and can't be recovered. The fixes must ship together (clamp columns to the median line, then allow zero spawn). Otherwise Zen becomes the game's soft-lock trap.

### REVISIONS

- **Finding 2 → upgraded from MAJOR to BLOCKING.** In Round 1 I treated the missing rules as rework risk. Combined with QA F1 (a movement rule that soft-locks the board), QA F2 (the release rule contradicts itself between GDD and Combo_system) and Systems F1 (colour count missing), the core board rules are not only incomplete but in places self-contradictory. The Gameplay agent's 40 Opus sessions are the foundation that every other agent depends on (my Finding 5, dependency chain). Starting them on contradictory rules wastes the most expensive part of the budget, and the rework spreads to Visuel, Sonore, NPC and Test. Production should not start until the rules are frozen.
- **Finding 3 → stays MAJOR, with higher confidence.** QA F2 and Systems F3 make the NPC harder than I estimated. It remains MAJOR rather than BLOCKING only because a cheating-AI fallback exists and could ship.
- **Finding 1 → stays BLOCKING, strengthened.** The Business Analyst reached the same conclusion independently. BA #3 adds a cost I under-weighted: leaderboards in Solo and Zen imply an online backend unless they are explicitly local or Steam-native. Small correction to BA #1: online play is already on the cut list, so it doesn't add to MVP scope. The platform line, the per-theme content and the leaderboards are the real excess.
- **Finding 4 → stays MAJOR** on build feasibility (argued above). I note BA's BLOCKING rating for the product case.
- **Finding 5 → stays MAJOR,** now with a concrete mitigation from the QA connection: a pure C++ rules module plus a headless fuzzer replaces most of what "Agent Test" was supposed to do.
