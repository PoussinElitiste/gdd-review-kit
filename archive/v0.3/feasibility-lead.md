# Technical Feasibility Lead — Round 1

Document reviewed: *The Marble Effect* GDD v0.2 plus linked documents (Combo system, Schémas du déplacement, Événements visuels et sonores, Thèmes des niveaux, Budget tokens). Judged against 2026 conditions: one developer orchestrating AI coding agents in Unreal through MCP.

---

## Finding 1: The MVP is far too big for 7 weeks, and the cut list removes nothing that is in the MVP

**Severity: BLOCKING**

**Problem.** The schedule and the MVP don't fit together. The cut list doesn't help, because the only real feature on it was never part of the MVP.

**Evidence.**
- "**Calendrier** : 7 semaines, rédaction du GDD comprise." The GDD is still at v0.2, so the production window is less than 7 weeks.
- MVP = "Modes Solo, Arcade, VS en local et Zen" + "5 niveaux avec leur thème." Each theme has its own environment, music, SFX, and board/marble skin. On top of that, "Agent Intro : génère une séquence d'introduction différente pour chaque niveau", an arcade AI opponent across 5/10/15-opponent ladders, a leaderboard ("enregistrer ses records dans un classement"), bonus/malus marbles in every mode, a first-game onboarding/hint system, and a Steam Early Access release.
- "**Ce qui peut être coupé**": online multiplayer and the "Optionnel" events. Online play is already outside the MVP ("Le jeu en ligne est optionnel"), so cutting it changes nothing. Only two rows in the event table are marked Optionnel: the environmental reward accent and the glass reflections. Every other piece of MVP scope is protected.
- The plan relies on one person: "Une seule personne réalisera ... tout ce qui touche à l'intégration des modes de jeu, aux menus et aux mécaniques de jeu." The budget assumes 190 agent sessions. Over roughly 5–6 working weeks, that is 6–8 sessions per working day for one human to review, integrate and playtest.
- Nothing covers Steam lead time. The document never mentions the store page, the "coming soon" visibility period, build review, achievements, or leaderboard integration through Steamworks.

**What gets cut in practice, and the document doesn't say so:** per-level intro sequences, 3 or 4 of the 5 themes (likely shipping as placeholders), Arcade ladder depth, and bonus/malus. The GDD should rank the MVP internally, for example: core board + VS local + Solo first, then Zen, then Arcade AI, then themes 2–5.

---

## Finding 2: The content pipeline has no owner. Art and music are MVP items with no confirmed source

**Severity: BLOCKING**

**Problem.** The event table makes audio-visual content an MVP requirement. The production plan provides nothing that can produce it.

**Evidence.**
- "Un ou une artiste **pourra** prendre en charge la partie graphique". The artist is a possibility, not a commitment. There is no named composer or sound designer at all.
- The Budget document admits: "Claude génère du texte et du code, pas d'images ni de musique ... les textures, modèles 3D et pistes musicales viendront de l'artiste, de placeholders ou d'autres outils, dont le coût n'est pas compté ici."
- Even so, "Musique propre à chaque niveau — une piste par thème" is marked **MVP**. The audio fantasy is a lounge electro score that builds in intensity through 3 phases ("la musique gagne une couche"). That needs layered or stemmed adaptive music for 5 levels, which is a specialist job.
- The per-sound variation rule ("chaque son fréquent ... existe en plusieurs versions") multiplies the SFX asset count. The number of versions is deferred ("Reporté ... Nombre de versions par son fréquent").
- `Themes_niveaux.md` is entirely "À définir". The 5 themes, which are the main MVP content and the selling point ("environnement « wow »"), have zero design. The Agent Skin, Visuel, Ambiance and Intro agents cannot start until they exist.

**Consequence.** Four of the eleven agents are blocked by inputs nobody is assigned to produce. The "wow" positioning against *Tetris Effect* depends on exactly this unfunded, unstaffed work.

---

## Finding 3: Eleven agents writing into one Unreal project through MCP, with no integration, version-control or test-harness plan

**Severity: MAJOR**

**Problem.** The whole production model is "Le jeu sera développé sous Unreal, avec une connexion MCP vers un agent IA". It is described in a single sentence and has the highest technical risk in the document.

**Evidence and omissions.**
- Unreal content is largely binary (`.uasset`, `.umap`, Blueprints). Agents cannot diff or merge it as text, and cannot read it into context. The budget assumption "1,5 million de tokens en entrée (le projet relu à chaque étape)" assumes a text-readable project. That is only true if everything lives in C++ or in data files, and the GDD never decides this.
- Eleven agents (Menu, Gameplay, Skin, Visuel, Sonore, Ambiance, Narratif, Intro, NPC, Multi, Test) touch overlapping systems. Visuel/Sonore hook into Gameplay events, and NPC drives the Gameplay board. The document gives no interface contract (for example an event bus or data assets), no branching/locking strategy, and no ownership boundaries.
- "Agent Test : teste la boucle de jeu de chaque niveau" runs on Haiku. Automatically testing a real-time Unreal game needs a headless or deterministic simulation of the board, plus seedable RNG and functional-test scaffolding. None of this is specified. Without it, the "tests" are the single human playing.
- The Haiku cost line looks understated. The doc says the Haiku rate applies "aux requêtes de moins de 100 000 tokens (0,50 $ / 2,50 $ au-delà)". If requests re-read the project, as the assumption says, they will go over that threshold and cost 5x. The Test agent would then cost about 36 $ instead of 7 $. That is small in money terms, but it shows the per-request model hasn't been worked through.

**Recommendation.** Write the core board simulation as pure, deterministic, engine-independent C++ (or as a text-defined module). Use Unreal for presentation only, and give agents explicit file ownership.

---

## Finding 4: The highest-risk systems depend on specs that don't exist

**Severity: MAJOR**

**Problem.** Several systems in the MVP feed directly into other MVP systems, yet their design is missing or ambiguous. That turns them into schedule risk rather than implementation work.

**Evidence.**
- **Bonus/malus marbles** appear in Solo, Arcade, VS and Zen, including Zen "options pour choisir les bonus/malus actifs" and an MVP-adjacent event row. The linked document "Billes bonus/malus" is listed under Documents liés but doesn't exist. The same goes for "Menu et options", "Leaderboard", and all five "Mode ..." documents.
- **Arcade AI ("moment « wow »")**: the challenge rests on "un profil scripté (behaviour tree)". A competent opponent has to plan multi-step move/switch sequences, stack aligned rows above the median line for cascades, and decide when to release versus hold the counter against incoming waves. That is a search/planning problem, not a behaviour-tree script. No difficulty parameters, reaction-time model or fairness constraints are given. The agent gets 25 sessions, and the theme-to-difficulty curve is deferred ("Reporté ... agressivité de l'IA").
- **Marble generation constraints conflict.** There are three of them. First, generation never creates an alignment on the median line. Second, every column keeps at least 5 marbles. Third, generation guarantees "au minimum 5 billes de même couleur" ("Lorsqu'une bille disparaît, une nouvelle bille est générée aléatoirement pour garantir au minimum 5 billes de même couleur"). That last clause is ambiguous: 5 of one colour on the board? Per colour? Meanwhile, the "Remplissage continu" rule spawns marbles that make columns longer. The document doesn't say what happens when a spawn or a fall moves a ready-made row onto the median line. Falls after an elimination are exactly the stack mechanic, so the no-free-alignment rule needs an exact definition. Otherwise it will be implemented, reimplemented and argued over during week 5.
- **Win condition in VS/Arcade**: "Plateau bloqué" means no column can move in either direction. A column can only be fully locked at 15 marbles, and 5 columns × 15 = 75 cells. Under attack (max 30 marbles per release) and continuous refill, how long a round lasts is unknown. Match length drives tuning for 1/3/5-round formats and 5/10/15-opponent ladders, and nobody has estimated it.

---

## Finding 5: The multi-platform and local-multiplayer promises go past what the document plans to build

**Severity: MINOR**

**Problem.** The scope statements commit to more platforms and input setups than the plan accounts for.

**Evidence.**
- "le jeu peut se jouer en local à deux sur toutes les plateformes". That includes mobile, which means two players on one phone screen with two touch-gesture zones, and nothing in the document designs this.
- PC controls give the mouse ("à la souris", "molette") as a selection and move method. In local VS, only one player can use the mouse. Player 2 key bindings are not explicitly assigned (arrows+Right Enter versus WASD+Space is implied but never stated), and gamepad support on PC is not mentioned. Steam players will expect it for a local party game.
- Console and mobile are post-EA ("viendront après l'accès anticipé"), which is fine. But the event table already specifies a touch diagonal-gesture rule as **MVP** for a PC-only MVP. That signals priorities are not being filtered by platform.
- Optional online play ("Agent Multi : lobby + game state", 20 sessions) is under-scoped. A real-time competitive puzzle game with random spawns and timed waves needs deterministic lockstep or server-authoritative state, plus seed sync and latency handling. "Lobby + game state" doesn't cover that. It is correctly marked cuttable, and it should be cut explicitly from the EA roadmap rather than kept as "optionnel".

---

### Summary

The core marble mechanic itself is small and buildable. The real problems are around it:
- **Schedule:** 7 weeks with one person is not enough for an MVP of 4 modes, 5 themes and an AI opponent, and the cut list cuts nothing that is in the MVP.
- **Assets:** art and music are MVP requirements with no committed source.
- **Workflow:** running 11 agents on an Unreal-through-MCP project has no integration plan.
