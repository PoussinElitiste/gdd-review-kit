# Production/Business Analyst — Round 1

Lens: feature creep, load-bearing vs. decorative features, market positioning, audience clarity, scope vs. resources.

---

## 1. The selling point has no owner, no budget, and no content definition — BLOCKING

**Problem.** The document sells this game on its audiovisual "wow" factor. This is the only differentiator from its 1997 inspiration. Yet that is the part of production with the least backing. Gameplay code gets Opus sessions and a cost table. The art, the music, and the five themes, which are what the game is actually selling, get no named owner, no budget, and no definition.

**Source.**
- Positioning (l.49): "Une réalisation modernisée, avec un environnement « wow », serait l'occasion de faire découvrir ce type de jeu au grand public."
- Fantasy (l.26): "c'est leffet waw recherché". The Tetris Effect inspiration (l.38) is cited for "le voyage audiovisuel".
- Scope (l.201): "Un ou une artiste **pourra** prendre en charge la partie graphique". This is conditional, not committed.
- Budget_tokens "Limites": textures, 3D models, and music tracks "viendront de l'artiste, de placeholders ou d'autres outils, dont le coût n'est pas compté ici."
- Themes_niveaux.md: every cell of the 5-theme table reads "À définir". The link between themes and difficulty and the emotional arc are both deferred.
- MVP (l.209) still lists "5 niveaux avec leur thème", and the events doc marks "Musique propre à chaque niveau" as **MVP**.

**Why it matters.** As written, the most likely outcome is a game that plays correctly but ships with placeholder art and music. That is the 1997 game without the "visuels datés", which is exactly the product the positioning says no one needs. Before production starts, the MVP needs a committed art and audio owner, a line in the budget, and at least the theme list.

---

## 2. Too many modes, audiences, and "wow" moments: the document is trying to be three games — MAJOR

**Problem.** The document pitches three products with different audiences and different success criteria:
- **Contemplative ASMR journey**: "initiation à la persévérance et la contemplation" (l.26), "ambiance ASMR" (events doc), Zen mode with no game over.
- **Competitive Puyo-style versus game**: attack/counter-attack, sudden death, and an AI that gets more aggressive at every level.
- **Mass-market party game**: "Public visé : très large" (l.47), with local VS "mis en avant".

The document also names two different "wow" moments that pull in opposite directions:
- The Fantasy's wow is the audiovisual journey (l.26).
- The AI section's wow (l.234) is "le challenge corsé que propose le NPC en mode arcade".

A tough AI opponent is a hook for hardcore players. It works against both the "très large" audience and the contemplation fantasy.

**Source.** Lines 26, 47, 234, and the Fantasy/ASMR framing set against Arcade progression (l.165).

**Why it matters.** With seven weeks and one person, the team cannot polish all three identities. Marketing for Early Access needs one pitch. Pick the primary fantasy and let it decide which "wow" moment gets the Opus budget. Right now the AI challenge does (Agent NPC, 25 Opus sessions), not the audiovisual journey.

---

## 3. The headline social mode is local-only, on a launch platform built for online play, and the online fallback is the first thing cut — MAJOR

**Problem.**
- The document puts local VS ("party game") forward as a lead feature (l.47).
- It launches only on PC/Steam Early Access (l.43), with the stated goal "créer une communauté" (l.45).
- Online play is "optionnel" and is the first entry in "Ce qui peut être coupé" (l.213).
- Couch versus is a weak fit for a PC Early Access launch, and community-building usually depends on online play or shareable competition.
- Couch-friendly platforms (console) are deferred until after Early Access. Steam Remote Play Together, the usual low-cost bridge, is not mentioned.
- PC two-player input is unclear. Controls offer mouse **or** keyboard and arrows **or** WASD (l.125-127), but nothing says how two players split one keyboard and mouse.

**Source.** Lines 43, 45, 47, 121 ("le jeu peut se jouer en local à deux sur toutes les plateformes"), 125-127, 213.

**Why it matters.** The Early Access community goal relies on a feature whose most important part, playing remotely, is marked cuttable. Either commit to online or Remote Play for the PC launch, or stop leading with VS until console ships. Solo with leaderboards is the more realistic Steam hook.

---

## 4. MVP feature creep: decorative systems sit inside the MVP while the cut list protects almost nothing — MAJOR

**Problem.** The "can be cut" list (l.211-214) has only two items: online play and the events marked Optional. Everything below stays in scope by default, even though it is decorative or undefined.

- **Agent Intro**: "une séquence d'introduction différente pour chaque niveau" (l.229). That is five bespoke cinematics for a puzzle game. Pure decoration, and not in the cut list.
- **Agent Narratif**: "génère la progression du joueur en modes solo et arcade" (l.228). No narrative or meta-progression system is described anywhere in the GDD, so this is a budgeted agent with no spec.
- **Bonus/malus marbles**: they appear in every mode (l.15, 17, 164-170) and are configurable in Zen (l.172). The "Billes bonus/malus" document is listed but does not exist (l.251, unlinked). This is a system in MVP scope with no definition.
- **Leaderboard** (l.13, l.250): leaderboards on Steam require online services. The document is otherwise treating online as cuttable, and the leaderboard doc is also missing.
- **Four modes at MVP** (l.208). Zen also has to serve as the tutorial and training mode, and passive onboarding is a separate system on top of it.

**Source.** Lines 13, 172, 208, 211-214, 228-229, 250-251.

**Why it matters.** Seven weeks, including writing the GDD (l.216), for one person is not enough for this list. The load-bearing core is the board, move/switch, the counter, attack and counter-attack, one mode, and one fully themed level. The cut list should be rebuilt in priority order: per-level intros, Narratif, bonus/malus, and the Arcade AI ramp should come before any core-feel item.

---

## 5. The business model is a label, not a model: no price, no Early Access plan, and a post-launch roadmap the MVP cannot support — MINOR

**Problem.** "Jeu premium" with Early Access (l.45) is the whole business section. The document does not give:
- a price point;
- the length of Early Access or the exit criteria for leaving it;
- a revenue or wishlist target;
- any cost line beyond AI tokens (the budget doc explicitly excludes art, music, and the Steam fee).

The roadmap also promises more than the MVP produces:
- 10- and 15-level courses and 10- and 15-opponent series "en mise à jour" (l.13, l.15), but only 5 levels are scoped. Longer courses mean either new content (unbudgeted) or repeating levels, which conflicts with the "voyage" fantasy.
- Console and mobile ports, with mobile as a premium Unreal title. The cost and fit of that port are not discussed.

**Source.** Lines 13, 15, 43, 45; Budget_tokens.md "Limites de l'estimation".

**Why it matters.** Early Access players buy on the roadmap. A roadmap whose content has no owner or budget is a reputational risk on Steam. At minimum, state the price tier, how long Early Access lasts, and whether the 10/15 courses reuse the 5 levels or need new themes.
