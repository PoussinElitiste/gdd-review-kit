# Production/Business Analyst — Round 1

Document reviewed: `gdd.txt` (The Marble Effect, GDD v0.2 plus linked docs: Combo system, Schémas du déplacement, Événements visuels et sonores, Thèmes des niveaux, Budget tokens).

---

## Finding 1 — The "MVP" is the full game, and the calendar can't deliver it

**Problem.** The MVP is not minimal. It contains every mode except online: Solo with leaderboard, Arcade with a scripted AI, local VS, and Zen. It also needs 5 fully themed levels (environment, music, sound set, board/marble skin), a separate intro sequence for each level, bonus/malus marbles in every mode, a combo/counter-attack system, and onboarding. All of this has to fit in **7 weeks, GDD writing included**, built by **one person** with AI agents. The only cuts on the list are online multiplayer (already called "optionnel" everywhere) and the "Optionnel" rows of the event table, and only two rows are marked that way (environment reward accent, glass reflections). So the cut list removes almost nothing. There is no fallback, such as fewer levels, dropping Arcade, or dropping Zen as its own mode, if the schedule slips.

**Source.** Scope section: "Minimum viable product (MVP) : Plateforme PC… Modes Solo, Arcade, VS en local et Zen… 5 niveaux avec leur thème"; "Ce qui peut être coupé : Mode multijoueur en ligne (optionnel), certains événements…"; "Calendrier : 7 semaines, rédaction du GDD comprise." Event table: only 2 of roughly 30 rows are Optionnel.

**Severity: BLOCKING.** Nothing in the document says what ships if week 5 goes badly. The project needs a real priority order, for example Solo + Zen + 1-2 themed levels as a vertical slice, with Arcade/VS and extra themes as later tiers.

---

## Finding 2 — The feature that sells the game has no owner, no content and no budget

**Problem.** The Positionnement section stakes the whole market case on presentation: a modernised take on a 1997 mechanic "avec un environnement « wow »". The Inspirations lean on *Tetris Effect*'s audiovisual journey, and the Fantasy is built around themed levels and reactive electro music. This is the one part of the game that sets it apart. Yet:
- the artist is conditional ("Un ou une artiste **pourra** prendre en charge…") and is not in the calendar or budget;
- assets are explicitly placeholders;
- the budget says Claude cannot produce textures, models or music, and that these costs "ne sont pas comptés ici". No composer is named, even though the design needs 5 tracks with layered intensity stems;
- all 5 themes in Thèmes des niveaux are "À définir", down to the theme names;
- the stated "wow moment made possible by AI" is a hard Arcade NPC, not the audiovisual journey.

Mechanically, the game is close to *Lose Your Marbles* with modern controls. Without the themed audiovisual layer the stated positioning disappears, and that layer is the least defined and least resourced part of the plan.

**Source.** Positionnement; Inspirations (Tetris Effect); Scope ("Des placeholders seront utilisés pour les assets", "pourra"); Budget tokens › Limites ("dont le coût n'est pas compté ici"); Thèmes des niveaux table (all "À définir"); "Moment « wow » rendu possible par l'IA".

**Severity: BLOCKING.** Add a named art and music plan with a cost line, and define at least one complete theme before production starts. Reconsider which feature is the "wow" one.

---

## Finding 3 — Four audiences in one product, and the stated audience doesn't match the flagship feature

**Problem.** The document is aimed at several different players at once:
1. **Contemplative/ASMR players**: Zen mode, "initiation à la persévérance et la contemplation", "ambiance ASMR".
2. **Couch party players**: local VS "mis en avant" as a party game.
3. **Score-attack players**: timed Solo runs with leaderboards.
4. **Competitive puzzle players** (Tetris/Puyo Puyo fans): Puyo-style offset and counter-attack, and an Arcade AI pitched as a "challenge corsé".

The stated target is "très large, car le jeu ne demande pas une grande maîtrise des commandes". However, the AI-enabled "wow moment" is a hard opponent, which pulls toward experts. The Tetris/Puyo players the document hopes to attract expect online versus, and online is the one feature marked optional and cut-first. Local couch VS is also the weakest multiplayer pitch on a PC/Steam early-access launch, which is the only platform in scope. The party-game emphasis fits the console version that comes later better than the PC launch.

**Source.** Public visé; Positionnement; Mode VS ("Le jeu en ligne est optionnel"); Mode Zen; Fantasy; Événements › Variations sonores ("ambiance ASMR"); "Moment « wow »… le challenge corsé".

**Severity: MAJOR.** Pick a primary player for the Steam EA launch and choose the mode emphasis to match. For a PC launch that probably means Solo/Zen audiovisual journey first, with competitive play as a secondary pillar.

---

## Finding 4 — Decorative features eat production time without supporting the core vision

**Problem.** Several features look like they were added for completeness rather than because the core loop needs them:
- **Agent Intro**: a "séquence d'introduction différente pour chaque niveau" in a puzzle game with no story. That is 10 sessions spent on cutscene-like content.
- **Agent Narratif**: "génère la progression du joueur en modes solo et arcade". The game has no narrative, and progression is already defined numerically (gauge size, spawn rate, AI aggression).
- **Bonus/malus marbles**: in all four modes plus Zen toggles. Their design doc ("Billes bonus/malus") is listed but not provided, so they are a whole unspecified system inside the MVP.
- **Run lengths of 5/10/15 levels and 5/10/15 opponents with 1/3/5 rounds**: there are only 5 levels, so 10- and 15-level runs repeat content. These options multiply balancing and test work and add little variety.
- **Console and mobile control schemes** are specified even though those platforms come after early access, while Menu/Options, Leaderboard and every per-mode doc are missing.

Each of these makes it harder to finish the core: the move/switch board, combo/counter, one great themed level, and Solo.

**Source.** Agents Roles (Agent Intro, Agent Narratif); Mode Solo/Arcade (5/10/15 runs); Progression et thèmes (5 levels); Documents liés (Billes bonus/malus, Leaderboard, Menu et options, Mode Solo/Arcade/VS/Multi/Zen listed without content); Contrôles (console/mobile).

**Severity: MAJOR.** Cut or defer Intro, Narratif and bonus/malus from the MVP. Limit run lengths to what 5 levels can support.

---

## Finding 5 — Early access business plan has a cost model for tokens but nothing for the business itself

**Problem.** The model is premium Steam early access, used "pour recueillir les retours des joueurs et à créer une communauté". The document gives a detailed token budget ($300-$530) but leaves out the inputs that decide whether early access works:
- no price point;
- no early-access content/roadmap promise (what is in EA 1.0 versus full release);
- no community or marketing plan, even though building a community is the stated purpose of EA;
- no wishlist or visibility strategy;
- no total budget beyond tokens (no artist, music, Steam fee, or console porting cost, even though console and mobile are planned next).

Mobile also tends to need a different business model than premium, and the document does not address this. The comparables claim ("aucun jeu reposant sur ce type de mécanique n'est sorti depuis 1997") is stated with no market check. It works both ways: it could signal a gap, or show that the mechanic hasn't found an audience in 25+ years.

**Source.** Modèle économique; Plateformes ("Console et mobile viendront après l'accès anticipé"); Positionnement; Budget tokens (the only cost document); Limites de l'estimation (asset costs excluded).

**Severity: MAJOR.** Not needed for the prototype, but the doc presents a Steam EA release as the production target, so a minimum price, EA scope and community plan belong in the GDD.
