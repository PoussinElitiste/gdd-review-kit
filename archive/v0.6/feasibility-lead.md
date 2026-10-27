# Technical Feasibility Lead — Round 1

Document reviewed: *The Marble Effect* GDD v0.2 plus linked docs (Combo system, Schémas du déplacement, Événements visuels et sonores, Thèmes des niveaux, Budget tokens). It's a 2026-era project: one developer, AI agents connected to Unreal through MCP, and a Steam Early Access target. I judged it against what that setup can deliver today.

---

## Finding 1 — Seven weeks, with the GDD included, is not enough for the MVP as defined, and the art pipeline has no owner

**Problem.** The MVP is four modes (Solo, Arcade, VS local, Zen) and 5 fully themed levels. Each level needs its own environment, music, sound effects, board and marble skin, and intro sequence. On top of that come an arcade AI, leaderboards, bonus/malus marbles, a first-game onboarding, and a Steam Early Access release. All of it must ship in **7 weeks including writing the GDD**, built by **one person**. The art side is unstaffed. The text says an artist "pourra" handle it, which means "may", not "will". The budget doc admits that Claude "génère du texte et du code, pas d'images ni de musique", so it can't make the textures, 3D models or music. Those assets "viendront de l'artiste, de placeholders ou d'autres outils, dont le coût n'est pas compté ici." Five original music tracks appear in the events table as **MVP** ("Musique propre à chaque niveau ... MVP"). No one is assigned to make them.

**Passages.** Scope: "Calendrier : 7 semaines, rédaction du GDD comprise"; "Un ou une artiste pourra prendre en charge la partie graphique"; "Des placeholders seront utilisés pour les assets". Themes_niveaux.md: every cell of the theme table is "À définir", and the theme/difficulty link is "Reporté à la version suivante du GDD". The pitch's central promise is "Une réalisation modernisée, avec un environnement « wow »", yet the plan ships placeholders, and the themes that would deliver the "wow" are not even named. Budget_tokens.md: asset cost "n'est pas compté".

**Why it matters.** The art and audio work is the part the GDD calls the product's differentiator, and it has no owner, no budget and no schedule. The 7 weeks also include design time, and the theme design hasn't started. Realistically, the code side might reach a playable Early Access build in 7 weeks with placeholder art. The "5 niveaux avec leur thème" MVP can't.

**Severity: BLOCKING**

---

## Finding 2 — The 11-agent plan has no integration owner and no test harness, and every agent depends on Agent Gameplay

**Problem.** The agent list splits the work into 11 parallel roles: Menu, Gameplay, Skin, Visuel, Sonore, Ambiance, Narratif, Intro, NPC, Multi and Test. Almost all of them depend on one core: the board state, the resolution order (Chute → Remontée → Cascade → Remplissage) and the event hooks that come out of it.
- Agent Visuel and Agent Sonore need event hooks for roughly 30 events.
- Agent NPC needs to be able to read and simulate the board.
- Agent Test needs a testable game loop.
- Agent Multi needs a serializable, deterministic game state.

Nothing in the document gives the order these agents run in. It doesn't name an architecture owner, and it defines no interface contract (event bus, board-state API) that the other agents code against. The integration work falls on the one human, and the schedule and budget give it no time.

**Unreal and MCP.** The document doesn't say whether the core is C++ or Blueprints. Blueprints, levels, materials, Niagara VFX and sequences are binary `.uasset` files. Agents can't read or diff them reliably as text. Agent Visuel ("modèle + animation + VFX") and Agent Intro (a different intro sequence per level) have to work almost entirely in that binary space through MCP. That is the least proven part of this toolchain, and the plan treats it like ordinary code generation.

**Agent Test.** It runs on Haiku and "teste la boucle de jeu de chaque niveau". The document never says what it tests against. There's no mention of a headless simulation, Unreal Automation tests or a scripted input harness. Without one, "testing" a real-time Unreal game means a human playing it.

**Passages.** "Agents Roles" (11 agents, with no dependencies or order listed). "Le jeu sera développé sous Unreal, avec une connexion MCP vers un agent IA." Budget_tokens.md assumes ~1.5 M input tokens per session ("le projet relu à chaque étape"), which is only realistic if the project is mostly text.

**Severity: MAJOR**

---

## Finding 3 — The arcade AI, billed as the AI-enabled "wow moment", is under-specified and the plan picks the wrong technique

**Problem.** The arcade AI is presented as the showpiece: "Moment « wow » rendu possible par l'IA : le challenge corsé que propose le NPC en mode arcade, piloté par le profil scripté généré par l'Agent NPC". The planned approach is a behaviour tree. Behaviour trees decide when to do what, but they don't evaluate board positions. A competent opponent for this game has to search move/switch sequences to find 3-, 4- and 5-alignments, plan stacks above the median line for combos, and decide when to release its counter to cancel incoming waves versus letting it grow. That takes a board simulator plus a search with a scoring function, and the result then needs tuning across 5 difficulty steps.

The difficulty curve itself is deferred: "Reporté ... Comment l'ordre des thèmes suit la montée de la difficulté (... agressivité de l'IA)". There are no targets (moves per second, reaction delay, combo depth per opponent). Agent NPC is budgeted at 25 Opus sessions, but no time is budgeted for human playtesting, and that tuning is what makes a puzzle AI feel "corsé" rather than either unbeatable or obviously stupid. An AI that can see the whole board and act at machine speed will be unbeatable by default. Making it fair is the hard part, and the document never mentions it.

**Passages.** "Agent NPC : génère un profil scripté (behaviour tree)"; Progression → Arcade: "l'IA devient plus agressive : elle enchaîne les combos" (no figures given); Themes_niveaux.md defers the difficulty curve.

**Severity: MAJOR**

---

## Finding 4 — The cut list is too thin: unstarted MVP dependencies and side features aren't in it

**Problem.** "Ce qui peut être coupé" names only online multiplayer and the events marked Optionnel. Those events amount to just two rows: "Élimination ou attaque réussie" and "Reflets lumineux". Several MVP features depend on documents that are only listed under "Documents liés" and appear never to have been written. The cut list doesn't treat any of them as at risk:
- **Billes bonus/malus.** They appear in every mode's progression, and Zen even offers "options pour choisir les bonus/malus actifs". No linked document exists and no effects are defined. This is an entire unwritten system inside the MVP.
- **Leaderboard.** Required for Solo ("enregistrer ses records dans un classement"). No linked document exists, and Steam leaderboard integration isn't mentioned.
- **Menu et options, Mode Solo, Mode Arcade, Mode VS, Mode Zen.** All listed with no link.

Feature-level items that a 7-week schedule should list as cuttable aren't marked either: 5 distinct per-level intros (Agent Intro), the "Narratif" progression, music layers that react to milestones, and Zen's per-bonus toggles. Agent Multi is listed in the roles and budgeted at 20 Opus sessions even though online play is "optionnel". The cut list shows no awareness of where the schedule will actually slip.

**Passages.** Scope → "Ce qui peut être coupé" (2 items); "Documents liés" (Billes bonus/malus, Leaderboard, Menu et options and the mode docs have no link); Progression section (bonus/malus in every mode).

**Severity: MAJOR**

---

## Finding 5 — Local VS on PC has no two-player input scheme, and the "deterministic" board isn't specified for netcode

**Problem (input).** Local VS is an MVP feature, and the document says "le jeu peut se jouer en local à deux sur toutes les plateformes". The only PC controls listed are a single-player scheme:
- **Cursor:** arrows **or** A/D, or the mouse.
- **Move:** up/down arrows **or** W/S, or the mouse wheel.
- **Switch:** space **or** right Enter.

Both key sets drive the same actions, and nothing splits them between two players. Nor does the document say whether gamepads are supported on PC. Two people on one keyboard also run into keyboard ghosting/rollover limits. The input-mapping and device-assignment layer for two players is missing, though it's cheap to fix now.

**Problem (determinism).** "Cet ordre est fixe, pour que le plateau soit déterministe (même situation, même résultat)." Yet new marbles, ties between equally short columns when attack waves land ("le choix entre elles se fait au hasard") and the starting colour of the counter are all random, and the document never mentions a seeded or shared RNG. That doesn't matter for local play. But if online stays on the table (Agent Multi, "lobby + game state"), the netcode model is left open: lockstep with shared seeds versus server-authoritative state. That choice affects how Agent Gameplay must write the core from day one. Adding online after Early Access on a core that wasn't built for it usually means a rewrite.

**Passages.** "Contrôles et onboarding" → Sur PC; "Note : le jeu peut se jouer en local à deux sur toutes les plateformes"; "Ordre de résolution ... déterministe"; Combo_system.md "le choix entre elles se fait au hasard"; "Agent Multi : met en place la structure réseau (lobby + game state)".

**Severity: MINOR** (input mapping: cheap to fix now). It becomes MAJOR if online multiplayer stays planned.
