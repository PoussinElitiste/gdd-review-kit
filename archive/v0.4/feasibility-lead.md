# Technical Feasibility Lead — Round 1

Document reviewed: *The Marble Effect*, GDD v0.2 and its linked documents (Combo system, Schémas du déplacement, Événements visuels et sonores, Thèmes des niveaux, Budget tokens). It is judged against its own stated context: 2026, Unreal Engine, one developer using AI agents over MCP, and Steam Early Access as the first release.

---

## Finding 1 — The MVP cannot fit in 7 weeks with one person, and the cut list does not reflect that
**Severity: BLOCKING**

**Problem.** The MVP includes four modes (Solo, Arcade, local VS, Zen), five fully themed levels, an arcade AI that the document calls its "wow" moment, a leaderboard, bonus/malus marbles, a separate intro sequence for each level, and a Steam Early Access release. All of this has to be done by one person in 7 weeks, and those 7 weeks include writing the GDD ("Calendrier : 7 semaines, rédaction du GDD comprise"). The cut list has only two items: online multiplayer, which is already marked optional, and events tagged "Optionnel". Only two rows in the events table carry that tag: the environment accent on elimination/attack and the glass reflections. Cutting both saves almost no time.

**Evidence / omissions.**
- `Thèmes des niveaux` is all placeholders. All 25 cells read "À définir", and the link between themes and difficulty is "Reporté à la version suivante". Yet "5 niveaux avec leur thème" is an MVP requirement.
- The "Documents liés" section lists Menu et options, Leaderboard, Billes bonus/malus, Mode Solo, Mode Arcade, Mode VS and Mode Zen with no links, because those documents don't exist. Bonus/malus marbles show up in Solo, Arcade, VS and the Zen options, but no bonus or malus is ever defined. A feature with no spec can't be estimated or built.
- There are no milestones and no ordering of work across the 7 weeks.
- Steam release logistics are missing. Steam Direct requires the app fee, store-page and build review, and a "coming soon" page that is public for a minimum period before launch. For a first-time developer there is also a waiting period after paying the fee. Together these take a meaningful share of a 7-week window, and the document never mentions them.
- The menu selects 5, 10 or 15 levels or opponents, but only 5 levels exist. How runs of 10 or 15 are put together (repeats? harder variants?) is not stated, so that work is hidden.

**What should happen.** Either extend the calendar or shrink the MVP to something like Solo + local VS + Zen with one or two themes. Add Arcade and its AI, intros, the leaderboard and bonus/malus to the cut list. Add a week-by-week plan.

---

## Finding 2 — The asset pipeline contradicts itself: agents are said to "generate" what the budget admits they cannot
**Severity: MAJOR**

**Problem.** The Agents Roles section says the Agent Visuel "génère les événements visuels (modèle + animation + VFX)", the Agent Skin "génère les skins de chaque thème", and the Agent Intro "génère une séquence d'introduction différente pour chaque niveau". The budget's own "Limites de l'estimation" section says the opposite: "Claude génère du texte et du code, pas d'images ni de musique… les textures, modèles 3D et pistes musicales viendront de l'artiste, de placeholders ou d'autres outils, dont le coût n'est pas compté ici."

**Evidence / omissions.**
- Nobody is confirmed to make the art: "Un ou une artiste **pourra** prendre en charge la partie graphique" ("may", not "will"). No artist time, cost or schedule appears anywhere.
- "Musique propre à chaque niveau" is marked **MVP** priority. That means 5 tracks, each built in layers so the music can "gain a layer" at each milestone. Nobody is assigned to compose them and they have no budget. The Agent Ambiance only *integrates* music.
- The whole fantasy depends on the audiovisual payoff: the Tetris Effect reference, the "effet waw", and the "wow" environment named as the market position. Shipping Early Access with placeholders undercuts the stated reason the product exists.
- In Unreal, Blueprints, Niagara systems, materials and level assets are binary `.uasset` files. How much an agent can author, diff and debug them through an MCP bridge is a real technical risk. The document does not assess it, and it has no fallback (for example, logic in C++ with a human building the assets).

**What should happen.** Separate code agents from asset production. Name who makes the textures, models, VFX and music, with a budget and dates. Cut the level count until that plan exists.

---

## Finding 3 — The arcade AI, the main risk, has no spec and sits at the end of a dependency chain
**Severity: MAJOR**

**Problem.** The document says the main AI-enabled "wow" moment is "le challenge corsé que propose le NPC en mode arcade, piloté par le profil scripté généré par l'Agent NPC" (a behaviour tree). In a puzzle game, a strong opponent needs a board evaluator and lookahead search: planning stacks for combos, deciding between releasing the counter now or letting it grow, and responding to incoming waves. A behaviour tree on its own picks actions; it doesn't search the board. None of this is specified.

**Evidence / omissions.**
- The only difficulty spec is "l'IA devient plus agressive : elle enchaîne les combos et envoie ses billes plus vite". There are no parameters for reaction time, actions per second, search depth or how often the AI makes mistakes. The difficulty curve is supposed to live in `Thèmes des niveaux`, which is entirely "À définir".
- Arcade runs can be 5, 10 or 15 opponents, but nothing says how many distinct AI profiles are needed or how they differ.
- Dependencies: the AI cannot be tuned until the gameplay core (move/switch/elimination/cascade/attack/counter-attack) is final, and the gameplay core is itself a 40-session Opus job. The visual, sound and test agents depend on the same core. With no milestones, the riskiest feature lands last in an already overfull calendar.

**What should happen.** Write a short AI spec (evaluation heuristic, search approach, difficulty knobs). Schedule the AI to start as soon as the core rules freeze. Decide now whether Arcade can be cut, given that it is also the main selling point.

---

## Finding 4 — Local two-player is the featured mode, but there are no controls for a second player
**Severity: MAJOR**

**Problem.** Local VS is part of the MVP and is named as a lead mode ("Le mode solo et le VS en local (party game) sont mis en avant"). The document also says "le jeu peut se jouer en local à deux sur toutes les plateformes". But the PC controls describe only one player: mouse **or** arrows/WASD to select, wheel **or** arrows/WASD to move, Space **or** Right Enter to switch. These read as alternative schemes for one person, not as a split between two people.

**Evidence / omissions.**
- Nothing says which player gets which keys on a shared keyboard. The mouse can only serve one player, and the wheel-based move can't be split between two. Gamepad support on PC is not mentioned.
- The cursor must be visibly owned by each player, but player assignment, device mapping, and how two input sources are handled in Unreal's input system are not covered.
- "Toutes les plateformes" includes mobile. Two players on one touchscreen with two 5x15 boards is a separate design and engineering problem that the document never acknowledges.

**What should happen.** Define the PC two-player input map, probably two gamepads plus a split keyboard fallback. Remove mobile local VS from the claims, or mark it as deferred.

---

## Finding 5 — The token budget tracks the wrong limit and underprices one model
**Severity: MINOR**

**Problem.** The budget is careful about dollars, but cost is not what will stop this project. The plan is 190 agent sessions in 7 weeks, and part of that time goes to writing the GDD. That is roughly 5-6 sessions per working day, and one person has to review, integrate and test each one in Unreal. Human review time is the real limit, and the document doesn't estimate it at all.

**Evidence / omissions.**
- Haiku 5.5 is priced at 0.10/0.50 $, with a note that this rate only applies to requests "de moins de 100 000 tokens (0,50 $ / 2,50 $ au-delà)". The same document assumes about 1.5M input tokens per session with "le projet relu à chaque étape", so many test requests will go over 100k tokens. At the higher rate a Haiku session costs about 0.90 $ instead of 0.18 $, and the Agent Test line rises from about 7 $ to about 36 $. That is small in dollars, but it shows the assumptions weren't checked against each other.
- The budget leaves out every non-token cost that affects whether the game ships: art and music (explicitly "pas compté ici"), the Steam app fee, and any asset-store purchases used as placeholders.
- "Reporté à la version suivante du GDD : Mesurer le coût réel des premières sessions" means the one feedback loop the budget relies on is also postponed.

**What should happen.** Add a human-hours estimate per agent session (review + integration + playtest). Correct the Haiku line. Add a short section on non-token costs.

---

## Summary of severities
| # | Finding | Severity |
|---|---|---|
| 1 | 7-week, one-person scope vs MVP; cut list nearly empty; core docs missing | BLOCKING |
| 2 | Asset generation contradiction; no confirmed artist or composer; Unreal binary-asset risk | MAJOR |
| 3 | Arcade AI ("wow" moment) has no spec and comes last in the dependency chain | MAJOR |
| 4 | Local VS featured but no second-player input scheme; mobile local VS claimed | MAJOR |
| 5 | Token budget measures dollars, not human review time; Haiku long-context pricing ignored | MINOR |

---

## Round 2 — Cross-examination

### 1. CONFLICTS

**C1 — Business Analyst F3, Narrative Critic F4, Player Psychologist F4: "promote environmental reactivity (and glass reflections) to MVP."**
Three colleagues want to move the two "Optionnel" audiovisual rows up to MVP or Important. I agree on what matters most to the product, but on feasibility grounds I object to doing it as a simple re-labelling. Those two rows are the only things the document's cut list actually removes (my F1). Promoting them leaves the cut list empty while the MVP is already over capacity. They also depend completely on the asset pipeline that my F2 shows has no owner: an environment that reacts to play needs authored environment assets and VFX, and glass marbles need a tuned translucent material. Both are binary `.uasset` work that agents over MCP are least able to do, and that the budget explicitly does not fund. My position is that promotion is only legitimate if it is paid for by an explicit cut somewhere else. The obvious candidates are Agent Intro (five unique intro sequences) and Agent Narratif, which the BA already suggests demoting, and preferably Arcade as well (see C2). Otherwise "promote to MVP" just means "fail later, with less warning".

**C2 — Business Analyst F1: which mode survives a reduced MVP.**
The BA proposes "one competitive mode (Arcade *or* local VS) + Zen + 1-2 themes". We agree on the shape and differ on the choice. From a feasibility point of view, Arcade is the worse candidate to keep. It carries the only unspecified AI system (my F3). That AI cannot start until the resolution order is fixed (QA F1, see N1), and it needs difficulty numbers that do not exist (Systems F1). Local VS needs no AI, only an input map (my F4) and a match-length backstop (Systems F3), and both are small, well-understood tasks. I hold my recommendation: VS + Zen (+ Solo if the leaderboard survives), with Arcade moved to the first EA content update. The cost is that the document's declared "wow" moment slips out of launch. I flag that as a decision for the owner, not as a reason to keep Arcade.

**C3 — Narrative Critic F3: "define an opponent and framing layer".**
The Narrative Critic gives two options. Under the 7-week constraint, only the first one (state that there are no characters and rescope or remove Agent Narratif) is feasible. A character and framing layer for 5/10/15 opponents adds writing, art for each opponent, and probably more intro content, all running through the same art pipeline that has no owner.

**C4 — Systems Designer F1 (BLOCKING): "no numbers for the difficulty curve".**
I agree with the problem but not fully with the remedy. In a puzzle game, spawn rates, gauge sizes and AI aggressiveness are found by tuning, not by writing them into a GDD. Putting numbers on paper now would give false precision. What actually blocks production is that there is **no tuning tool and no tuning time**. Nothing in the plan lets anyone measure clear rates or release frequency, and the 7-week calendar has no balancing phase. I would reframe Systems F1 as "no curve **and no means of producing one**". That keeps it BLOCKING, but the fix becomes a deliverable (see N2), not a paragraph of text.

**C5 — Player Psychologist F5: arcade difficulty selection or adaptive tuning.**
Adaptive difficulty is a second AI system layered on an AI that has no spec. With the current calendar, I would push back on adding it before a fixed-step difficulty ladder exists and works.

### 2. CONNECTIONS

**N1 — Adversarial QA F1 + my F3: the AI cannot look ahead into a board whose rules are not deterministic.**
QA shows that the order of fall, refill, auto-rise and cascade check is undefined, and that different orders give different boards. A competent puzzle AI (my F3) has to simulate future board states to plan stacks and choose when to release. With no fixed resolution order, the Agent NPC cannot write a forward model that matches the real game, and the Agent Gameplay and Agent NPC (separate agents in separate sessions) will very likely produce two inconsistent implementations. Random refill also makes lookahead stochastic, and the document never says whether the AI can see upcoming spawns. The same determinism gap affects leaderboard integrity and would prevent the deferred Agent Multi from ever doing lockstep online play. **This is the most important item from combining two lenses: QA F1 sits upstream of my F3, Systems F2-F4, and online play.**

**N2 — Systems Designer F2/F3/F4 + my F1/F5: the core rules are not frozen, and nothing is planned for iterating on them.**
Systems shows that attacks help the victim at low volume, that matches may never end, and that how often releases happen is not modelled. Each of these will force rule changes after implementation. Every rule change spreads to the AI, the VFX/SFX event hooks and the Agent Test scripts. The 40 Opus sessions for gameplay assume a stable spec, and the 7 weeks already include finishing that spec. Feasibility recommendation that comes from combining these: make the **first deliverable a headless rules simulator** (plain C++ or a standalone script, outside Unreal assets), with a fixed resolution order and seeded RNG. That one tool answers Systems F2 (mobility by column length), F3 (stall rates), F4 (5-line frequency), gives the AI its forward model (N1), and gives the Agent Test something it can actually test automatically. It is also text and code, which is exactly what the budget says the agents are good at.

**N3 — Player Psychologist F3/F4 + my F1: the realistic cut line removes loss warnings and skill feedback.**
The document assumes cuts stop at "Optionnel". My F1 argues that the actual cut will go much deeper, into the "Important" tier. Psych F3/F4 shows what that tier holds: the danger/panic warning, the game-over event, combo escalation, and 4/5-match feedback. So in practice the shipped build will most likely lose its fail-state readability and its rewards for skill first. The priority labels are not just mis-weighted from a design point of view (Psych/BA). They also hide where the real schedule cut line is.

**N4 — Adversarial QA F5 + my F4: same finding reached independently, plus a hidden dependency.**
QA reached the two-player input gap separately, which raises confidence. QA's point about "whose first game" adds an engineering dependency: per-player onboarding and hint state needs device-to-player assignment and some kind of local player identity. That is the same input-mapping work my F4 asks for, so the two should be specified together.

**N5 — Narrative Critic F1 + my F2: themes are the upstream input for four agents.**
Agent Skin, Agent Visuel, Agent Ambiance and Agent Intro all need a defined theme as their brief. With Themes_niveaux entirely "À définir" and the GDD-writing time counted inside the 7 weeks, none of the asset-side agents can start. The dependency chain in my F3 therefore applies to the audiovisual side too, not only to the AI.

**N6 — Adversarial QA F2 + Systems F3: unbounded matches break automated testing.**
If a VS/Arcade match can stall or soft-lock indefinitely, AI-vs-AI soak tests and the Agent Test's "teste la boucle de jeu de chaque niveau" have no guaranteed end condition. A round timer or in-match spawn ramp is needed for design reasons and is also a precondition for automated QA.

### 3. REVISIONS

- **F1 (scope) — stays BLOCKING, strengthened.** Systems F2-F4 and QA F1-F2 show that the core rules themselves still need design work. That work counts against the same 7 weeks, so the real production window is shorter than I assumed in Round 1.
- **F3 (Arcade AI) — stays MAJOR, strengthened, now explicitly dependent on QA F1.** If the board adopts the reduced MVP in C2, this finding becomes "deferred, specify before the EA update" rather than a launch risk.
- **F4 (two-player input) — stays MAJOR.** Independently corroborated by QA F5; scope widened by N4.
- **F5 — split.** I **upgrade the human-review-time part to MAJOR**. Systems' findings mean much of the remaining work is tuning and judging game feel, which only the human can do, on top of reviewing about 5-6 agent sessions a day. That makes the developer's time the binding constraint. I **withdraw the Haiku long-context pricing point** as a finding. It is accurate but too small to matter, and the BA's F5 covers the more important non-token cost omissions better.
- **F2 (asset pipeline) — stays MAJOR.** Note for the moderator: as C1 argues, this finding is a precondition for three colleagues' promotion recommendations. If F2 is not resolved, those recommendations cannot be carried out.
