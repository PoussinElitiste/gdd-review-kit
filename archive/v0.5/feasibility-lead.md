# Technical Feasibility Lead — Round 1

Document reviewed: *The Marble Effect* GDD v0.2 plus linked docs (Combo system, Schémas du déplacement, Événements visuels et sonores, Thèmes des niveaux, Budget tokens). It is a 2026-era project: Unreal, a solo developer, and Claude agents connected over MCP. I judged it against that setup.

---

## F1. The MVP does not fit 7 weeks for one person, and the cut list removes almost nothing — BLOCKING

**Problem.** The calendar is "7 semaines, rédaction du GDD comprise", and the developer is one person ("Une seule personne réalisera... tout ce qui touche à l'intégration des modes de jeu, aux menus et aux mécaniques"). In that time the MVP must ship:
- 4 full modes (Solo, Arcade, local VS, Zen). Zen also has to work as the tutorial.
- 5 themed levels, each with its own environment, music, sound effects, board and marble skin.
- A different intro sequence for each level (Agent Intro).
- An arcade AI that gets more aggressive across 5 opponents.
- Bonus/malus marbles in every mode.
- A score leaderboard, a passive onboarding layer, and the attack/counter-attack wave system.
- A public Steam Early Access release.

**Passage / omission.**
- "Ce qui peut être coupé" lists only online multiplayer and the effects marked Optionnel. Online multiplayer was already outside the MVP. The events table marks only 2 items Optionnel: the environment accent and the glass reflections.
- Everything else is MVP or "Important".
- No feature is ranked behind another. There is no fallback such as 3 levels instead of 5, or dropping Arcade before Steam.
- There are no milestones and no week-by-week plan.
- Steam release overhead is not mentioned at all: the store page, the build review, the required "coming soon" period and Steamworks leaderboard integration.

**Severity: BLOCKING.** As written, nothing gets cut before the deadline is missed. The document needs a ranked cut list and a milestone plan.

---

## F2. Content production has no owner and no budget, and the themes don't exist yet — BLOCKING

**Problem.** The Fantasy and the MVP depend on content the plan cannot produce:
- The events table marks music per level as **MVP**.
- It marks environments per level, the panic effects and the win/lose explosions as Important.

**Passage / omission.**
- Budget_tokens says plainly: "Claude génère du texte et du code, pas d'images ni de musique... les textures, modèles 3D et pistes musicales viendront de l'artiste, de placeholders ou d'autres outils, dont le coût n'est pas compté ici."
- The artist is not committed: "Un ou une artiste **pourra** prendre en charge la partie graphique".
- There is no composer or sound designer at all, yet the plan needs 5 music tracks with layered intensity phases and several variants of every frequent sound effect.
- Themes_niveaux.md is entirely "À définir": no theme, mood, music direction or difficulty mapping for any of the 5 levels.
- The Agent Skin, Visuel, Ambiance and Intro budgets (about 80 sessions) assume content they cannot create.

**Severity: BLOCKING.** The "wow" audiovisual journey is the product's positioning ("Une réalisation modernisée, avec un environnement « wow »"). Shipping it on placeholders defeats the Early Access goal. Asset sourcing, ownership and cost have to be decided now.

---

## F3. The Unreal + MCP agent pipeline is the riskiest technology in the project, and nobody has tested it — MAJOR

**Problem.** The production plan assumes 11 specialised agents can do the following through "une connexion MCP vers un agent IA":
- Generate Unreal menus.
- Build Niagara-style VFX ("modèle + animation + VFX").
- Write behaviour trees.
- Build networking.
- Test gameplay.

Much of Unreal's content is binary assets: Blueprints, behaviour trees, materials, UMG widgets and Niagara systems. Agents can't easily read, diff or check these. The document doesn't say:
- whether the code will be C++ or Blueprint;
- which MCP server will be used or what it can do;
- how one person will review and merge the output of 11 agents that touch overlapping systems. For example, Gameplay, Visuel, Sonore and NPC all hook into the same elimination and resolution events.

**Passage / omission.**
- The "Agents Roles" section gives each agent a one-line job.
- The Agent Test role is "teste la boucle de jeu de chaque niveau", running on Haiku. Nothing says how an LLM can test a real-time Unreal game loop: no headless simulation, no automated test harness, no replay system.
- The resolution order is said to be "déterministe (même situation, même résultat)". That determinism would allow a headless rules simulation that is testable outside Unreal, but the plan never uses it.
- "Mesurer le coût réel des premières sessions" is postponed to the next GDD version. There is no week-1 spike to prove the pipeline works.

**Severity: MAJOR.** If the MCP pipeline underperforms, all 7 weeks slip and there is no fallback. Add a week-1 technical spike, choose C++ or Blueprint for the rules core, and define a headless test plan.

---

## F4. The declared "wow" feature, the arcade AI, is the hardest system to build and has no spec — MAJOR

**Problem.** The document's one AI-enabled "wow" moment is "le challenge corsé que propose le NPC en mode arcade, piloté par le profil scripté (behaviour tree)".

A strong opponent in this game has to plan multi-step move/switch sequences, stack alignments above the median line for cascades, and choose between releasing the counter early to defend and holding it for a bigger counter-attack. That is search and board evaluation. A behaviour tree is a poor fit for search unless a planner sits underneath it, and none is mentioned.

**Passage / omission.**
- The "Mode Arcade" linked doc is listed but not written.
- Difficulty is described only as "l'IA devient plus agressive : elle enchaîne les combos et envoie ses billes plus vite".
- There is no board evaluator, search depth, or action-rate limit for a beatable/fair AI.
- There is no answer to whether the AI sees the same random refill.
- The AI behaviour is also bound up with wave timing (1.5 s per wave), the sudden-death rule and draw rules. All of these must be stable before AI tuning can start, so the AI cannot be finished until late in the 7 weeks.

**Severity: MAJOR.** Tuning across 5 opponents is the most likely thing to slip, and it is the headline feature. Specify the AI architecture, or scale the "wow" claim down.

---

## F5. Unwritten linked specs and unaddressed generation constraints leave core systems unbuildable — MAJOR

**Problem.** Several systems in the MVP have no written spec, but agents would need one to implement them:

- **Bonus/malus marbles** appear in every mode's progression and in Zen options. Their doc ("Billes bonus/malus") is only listed and does not exist.
- **Leaderboard**, **Menu et options**, and all **per-mode docs** are listed but not linked or written.
- The solo score formula is only "basé sur le compteur de combo et l'utilisation du compteur de billes". There are no numbers.
- **Constrained generation**: every refill and spawn must, at the same time:
  - never create a ready-made alignment on the median line;
  - keep at least 5 marbles per column;
  - keep at least 5 marbles of one non-counter colour on the board.

  The document doesn't say what happens when these constraints conflict, for example on a nearly full board. It also doesn't say whether attack marbles from the opponent (Combo system, "Attaque") follow the same rules, or what colour they are.
- **Sudden death** doubles the fill rate every 30 s with no ceiling.
- **Local 2-player on PC** has one player on the mouse wheel and both sharing one keyboard. The document doesn't consider this, or gamepad support on PC.

**Passage / omission.** The "Documents liés" list (Leaderboard, Billes bonus/malus, Mode Solo/Arcade/VS/Multi/Zen, Menu et options has no link). Also the "Garanties de génération" and "Pas d'alignement dû au hasard" sections.

**Severity: MAJOR.** Agents that implement from incomplete specs will each invent behaviour, and one person will have to reconcile it. These specs must be written before production starts. Under a 7-week calendar that includes writing the GDD, that cuts into build time again.

---

### Minor note (not counted)
The token budget prices Haiku sessions at the under-100k rate. Each session reads about 1.5 M input tokens with no cache ("le projet relu à chaque étape"), so individual requests likely exceed 100k. At the higher tier, the test agent's cost rises from about \$7 to about \$36. This doesn't threaten the overall ceiling.
