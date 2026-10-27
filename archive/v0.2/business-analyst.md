# Production/Business Analyst — Round 1

Document reviewed: *The Marble Effect* — GDD_V_0_2.md plus linked docs (Combo system, Schémas du déplacement, Événements visuels et sonores, Thèmes des niveaux, Budget tokens).

---

## 1. The scope is several games' worth of work for one person in 7 weeks, and the MVP cuts almost nothing — BLOCKING

**Problem.** The plan has one developer working with AI agents, an artist who is only described as possible ("Un ou une artiste *pourra*…"), and 7 weeks that include writing the GDD. Against that, the document commits to four platforms (PC, console, mobile and VR), four modes, five fully themed levels (environment, music, sound effects, board and marble skins), a different intro sequence for each level, a scripted AI opponent with rising difficulty, leaderboards, bonus/malus marbles, and online play. The MVP keeps all four modes and all five themed levels. The cut list has only two items: online multiplayer and the effects marked "Optionnel". Only one of the 30+ events in the events table is marked Optionnel, so that cut saves almost nothing.

**Source.** "Plateformes : PC, console, mobile et VR." / "Le jeu peut se jouer en local à deux sur toutes les plateformes." / MVP: "Modes Solo, Arcade, VS en local et Zen score / 5 niveaux avec leur thème" / "Calendrier : 7 semaines, rédaction du GDD comprise." / Events table: one Optionnel event (plus one Optionnel décor item).

**What the document leaves out.** VR has no control scheme, no design notes and no agent. It appears only in the platform line. Console certification, mobile store submission and the work to port to each platform are not mentioned anywhere. There is no milestone plan inside the 7 weeks, so it is unclear what must be playable by which week.

**Recommendation.** Cut the MVP down to PC (Steam EA), Solo + VS local, and 1–2 themed levels. Move console, mobile and VR to "post-EA, if traction". Decide what Zen and Arcade are needed for (see #4).

---

## 2. The feature that sets the game apart is the least specified and is not funded — BLOCKING

**Problem.** The market position rests on the modern audiovisual experience: "Une réalisation modernisée, avec un environnement « wow »". The Fantasy section and the Tetris Effect inspiration say the same thing, and so does the title, which echoes Tetris Effect. Without that layer, the pitch is a remake of a 1997 mechanic. Yet:
- The themes document is completely empty. Every cell in the theme table reads "À définir", and the link between themes and difficulty is postponed.
- The artist is not confirmed.
- The token budget explicitly says it does not cover textures, 3D models or music ("dont le coût n'est pas compté ici").
- The production plan uses placeholders for marbles and levels.

So the one element that justifies the product has no content, no owner and no budget. Meanwhile the parts that are fully specified (movement, combo, counter-attack) are the parts the document itself says were already in a 1997 game.

**Source.** Positioning paragraph; Fantasy; Themes_niveaux.md table ("À définir" ×25); Scope ("Des placeholders seront utilisés…", "Un ou une artiste pourra…"); Budget_tokens.md "Limites de l'estimation".

**Recommendation.** Before production, fully define and cost at least one theme, including its art and music source. Treat it as a vertical slice that proves the "wow" claim.

---

## 3. The two target audiences pull in opposite directions, and the competitive one depends on the first feature to be cut — MAJOR

**Problem.** The audience line names two groups. One is a very broad, low-skill audience: "très large, car le jeu ne demande pas une grande maîtrise des commandes". The other is "la scène compétitive" of Tetris and Puyo Puyo players. A competitive scene needs:
- online play
- matchmaking or ranking
- netcode that holds up under high-skill play
- balance work

Online multiplayer is the first item on the cut list, and its design document ("Mode Multi") is only a title in the related-documents list. The leaderboards used in Solo and Zen also need an online backend, and the "Leaderboard" document is also just a title. If the cut happens, the competitive audience is gone and the leaderboards may go too, yet the positioning still claims both.

The broad-audience claim also sits uneasily with the core mechanic. The document itself admits players have to learn workarounds, such as one-way switching ("4 switchs vers la droite équivalent à 1 vers la gauche") and stacking alignments above the line for combos.

**Source.** "Public visé…"; "Ce qui peut être coupé : Mode multijoueur"; "Documents liés: Leaderboard, Mode Multi" (no content).

**Recommendation.** Choose one primary audience for Early Access. If it is competitive, online VS belongs in the MVP and something else must be cut. If it is broad or casual, drop the competitive claim and say whether leaderboards are online or local.

---

## 4. Several features and agents are decorative: they cost budget but do not serve the core vision — MAJOR

**Problem.** Several items cost scope or tokens without supporting the core loop or the audiovisual promise:
- **Agent Narratif** ("génère la progression du joueur en modes solo et arcade", 10 sessions). The game has no story. Solo and Arcade progression is a series of 5/10/15 levels or opponents, which is a configuration table, not narrative work.
- **Agent Intro** (10 sessions): a different intro sequence for each level, while the levels themselves are undefined.
- **Zen score mode** is Solo without a timer plus option sliders. As a separate MVP mode it adds a menu, a leaderboard and a balancing pass, when it could be a Solo option.
- **Two competing "wow" moments.** The Fantasy says the wow is the audiovisual journey ("c'est leffet waw recherché"). The Agents section says the wow is "le challenge corsé que propose le NPC". The document never decides which one the product is built around, and the budget gives Opus sessions to the NPC, not to the art direction.

**Source.** Agents Roles list; Budget_tokens.md per-agent table; Mode Zen score; Fantasy vs "Moment « wow » rendu possible par l'IA".

**Recommendation.** Merge Narratif into Menu or Gameplay as configuration. Defer the per-level intros. Make Zen a Solo setting. Name one "wow" and point the budget at it.

---

## 5. The only budget in the document is the token budget; real costs and the commercial plan are missing — MAJOR

**Problem.** The document estimates AI token costs carefully (≈300–530 $) but leaves out every other cost and the whole commercial side:
- artist fees
- music licensing or composition
- console dev kits and certification
- VR hardware
- the Steam Direct fee
- engine royalty terms

It names a premium model with Steam Early Access first, but gives:
- no price point
- no scope or duration for Early Access
- no plan for what changes between Early Access and 1.0
- no stance on mobile monetization, even though mobile is a listed platform and premium-only pricing there is a deliberate choice that needs justifying
- no success metric

The market claim "aucun jeu reposant sur ce type de mécanique n'est sorti depuis 1997" has no competitor analysis behind it, so it cannot be checked. "Ordres de grandeur, pas des devis" is fair for tokens, but nothing equivalent exists for the rest of the project.

**Source.** "Modèle économique : jeu premium, avec un accès anticipé (early access) sur Steam en priorité."; Positioning paragraph; Budget_tokens.md (scope limited to tokens, assets excluded).

**Recommendation.** Add a one-page business section: total cost (people, assets, platforms), EA price and content plan, platform order, a short list of comparable titles that supports or corrects the "since 1997" claim, and one measurable EA success target (e.g., wishlists or reviews).

---

## Round 2 — Cross-examination

### CONFLICTS

**C1. Narrative Critic #5 (faceless Arcade opponents) versus my #4 (cut Agent Narratif and the per-level intros).**
The Narrative Critic asks the document to decide whether opponents are characters, and leans toward giving them identity, citing Puyo Puyo's cast. In production terms, a character cast is a new content line: names, portraits, at least one idle and win/lose state each, and some way of presenting them. That needs the artist who is not confirmed, and the Narratif and Intro agents that I flagged as decorative. My position is that the document should make the decision the critic asks for, and that the answer for Early Access is **faceless by design**: abstract opponents tied to the theme, which is consistent with the "sans la cours de récré" break from Lose Your Marbles that the GDD already chose. The critic's MINOR rating is right for this scope. It should only become MAJOR if Arcade is shown to be the primary product, and nothing in the positioning says it is.

**C2. Narrative Critic #1 (the voyage has no arc) — I agree on severity and disagree on the fix.**
We both rate the empty theme work BLOCKING. The critic's fix points toward writing the whole journey: a five-theme arc, a destination, and an answer for how 15-step runs move through it. That is a lot of definition and content for a 7-week, one-person project with no confirmed artist. My position is that for production the minimum bar is **one fully realized theme as a vertical slice** that proves the "wow". The journey-level arc can be described in a paragraph and built after Early Access. If the board makes a full five-stop arc a precondition for production, that pushes the schedule problem (my #1, Feasibility F1) further past what the document can deliver.

**C3. Player Psychologist #5 and Narrative Critic #2 (Zen should be the protected contemplative space) versus my #4 (fold Zen into Solo).**
These colleagues treat Zen as the only place the "contemplation" pillar can live, and as the only safe practice space. That is a fair challenge to my recommendation to remove it as a separate mode. I keep the core of my position: as written, Zen is not load-bearing for contemplation, because it is a leaderboard grind ("battre le record de combo du classement") on a board that still fills. That is exactly what the two colleagues complain about. Adversarial QA F4 adds that the Zen leaderboard can be exploited through its own settings. So the version of Zen in the document costs a mode, a leaderboard, a balancing pass and an exploit surface, and still does not deliver contemplation. **Revised recommendation (see R2):** keep the contemplative purpose, drop the mode overhead. Make it an untimed, no-fail, no-leaderboard Solo setting. That is cheaper than what the document specifies, closes QA F4, and gives the Psychologist the practice space they want.

**C4. Feasibility Lead F4 rates the unowned asset pipeline MAJOR; I rated the same gap BLOCKING (my #2).**
The Feasibility Lead frames this as a pipeline problem: who makes the assets. I argue it is a positioning problem. The document's only stated reason to exist next to a 1997 game is "réalisation modernisée, avec un environnement « wow »". A placeholder build is not a weaker version of this product. It is a different product with no market claim. The Feasibility Lead says this too in their last sentence ("On a placeholder build, that pitch disappears"), which in my view argues for BLOCKING. I keep BLOCKING.

### CONNECTIONS

**X1. The "wow" the budget funds is the riskiest one, and the "wow" the pitch sells is unfunded (my #4 + Feasibility F3 + Narrative #5).**
In my #4 I noted the document names two different wow moments. Feasibility F3 shows the funded one, the NPC with 25 sessions, rests on the wrong technique: a behaviour tree with no board evaluator or search. Narrative #5 shows the same NPC has no identity, so even if it works it shows up as "this got harder". Taken together: the token budget is going to the most technically risky and least marketable wow, and the wow that appears in the pitch, title and trailer gets nothing. This is a budget-allocation error, not only a spec gap.

**X2. Neither target audience is served by the current design (my #3 + Systems F3 + Adversarial QA F3 + Player Psychologist #1/#3).**
For the competitive audience, my #3 said online play is cut. Systems F3 adds that combos do not feed the attack, so the depth that sustains a Puyo-style scene is missing. QA F3 adds that an uncapped counter allows one-shot kills, which a competitive scene would find right away and reject. For the broad audience, Psychologist #1 shows onboarding teaches only the least important action, once, and Psychologist #3 shows right-only switching with no undo is friction for new players. So the document claims two audiences and the design delivers to neither. This changes my #3 from "pick one" to "the pick decides what gets fixed first", which is why I upgrade it (R1).

**X3. The differentiator is shown mostly while the player is losing (my #2 + Narrative #3).**
Narrative #3 shows that in Arcade and VS, the environment and music build up as the player's board fills. Commercially, this means the content that sells the game (store screenshots, trailer capture, streamer clips) happens in failure states. A skilled player or streamer who stays ahead sees the minimal opening. For a product whose whole market claim is the audiovisual journey, that hurts marketing as well as narrative.

**X4. Per-theme accessibility patterns multiply asset cost as well as cognitive load (Player Psychologist #4 + my #1/#2).**
The Psychologist flags per-theme patterns as a relearning burden. From a production view, the same rule turns one accessibility pattern set into five, each needing design, readability testing and skinning on an unfunded art pipeline. A single global pattern set fixes the player problem and also removes four asset sets. That makes it a quick win under both lenses.

**X5. Bonus/malus marbles are an unscoped feature in a scope that already does not fit (Systems F5 + my #1).**
Systems F5 notes bonus/malus marbles are in neither the MVP list nor the cut list. That is a scope-governance failure: a feature listed in all four modes' progression and in Zen's options, with no design document, that nobody has decided to ship or cut. It should go on the cut list explicitly.

**X6. The real budget gap includes the developer's own hours (my #5 + Feasibility F5).**
Feasibility F5 estimates about 40 agent sessions per week for one person to review and integrate. My #5 listed the missing money costs. The missing labor cost is larger: the plan has no estimate of human hours, and token cost is not the binding constraint on this project. The business section I recommended should state the developer's time as a cost line, not only dollars.

### REVISIONS

**R1. Upgrade #3 (audience) from MAJOR to BLOCKING.**
Reason: X2. With the evidence from Systems F3, QA F3 and Psychologist #1/#3, the audience decision is not just positioning wording. It decides which cluster of colleague findings must be fixed before production: competitive depth and balance (attack economy, counter cap, online play) or accessibility of play (onboarding, input friction). The MVP cannot be scoped (my #1) until this is chosen, so it gates other work.

**R2. Revise the recommendation in #4 for Zen.**
Old: "Make Zen a Solo setting." New: make Zen an **untimed, no-fail, no-leaderboard** Solo setting that serves as the practice and contemplation space (see C3). The finding stays MAJOR. Zen as specified is still decorative overhead, but its purpose is load-bearing for the Fantasy and for onboarding, so I am no longer recommending removing that purpose.

**R3. Concede that onboarding is load-bearing, not decorative.**
My #1 recommendation was pure cutting. Psychologist #1 (BLOCKING), combined with the document's own "since 1997" claim, shows a real tutorial is required work that is not currently scoped. The capacity freed by the cuts in #1 and #4 (VR/console/mobile, Narratif, Intro, separate Zen mode) should be put explicitly toward onboarding for the counter and release rules. The cut list and the add list should be presented together.

**R4. Trim #5 slightly; severity unchanged (MAJOR).**
I withdraw "engine royalty terms" and "Steam Direct fee" as meaningful items. Unreal royalties apply only above a revenue threshold this project will not reach in Early Access, and the Steam fee is negligible. The core of the finding stands: no price, no Early Access content plan, no success metric, no competitor check, and (per X6) no human-time cost.
