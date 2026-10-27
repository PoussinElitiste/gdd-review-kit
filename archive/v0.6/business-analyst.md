# Production/Business Analyst — Round 1

Document reviewed: *The Marble Effect* — GDD_V_0_2.md plus linked documents (Combo_system, Schemas_deplacement, Evenements_visuels_et_sonores, Themes_niveaux, Budget_tokens).

---

## 1. The MVP is several products, and the 7-week schedule does not fit them — BLOCKING

**Problem.** The MVP commits to four modes (Solo, Arcade, local VS, Zen), five fully themed levels (each with its own environment, music, sound effects, board skin and marble skin), a different intro sequence for each level, a scripted AI opponent with rising aggression, and bonus/malus marbles in every mode. All of this has to ship in "7 semaines, rédaction du GDD comprise", built by one person with AI agents. The art is not secured either: an artist "pourra" (may) take on the visuals, and the budget says outright that textures, 3D models and music tracks "viendront de l'artiste, de placeholders ou d'autres outils". The only real cut list is online multiplayer, which was already optional, plus the "Optionnel" VFX rows. In the event table only 2 entries are marked Optionnel, and most of the audiovisual "wow" content is marked MVP or Important.

**Source.** Scope / MVP / "Ce qui peut être coupé" / Calendrier; Progression et thèmes; Agents Roles (Agent Intro, Agent Skin, Agent Ambiance); Budget_tokens "Limites de l'estimation".

**Why it matters.** The core rules are well specified, but the content that sells the game is not planned at all. A realistic cut ladder is missing. For example: ship Solo + Zen + local VS with 1 to 2 polished themes, and treat Arcade AI and per-level intros as stretch goals. Without that, the project is likely to reach week 7 with five placeholder levels, which is the one outcome its own positioning cannot survive.

---

## 2. The differentiator is the audiovisual "journey", and it is entirely undefined — BLOCKING

**Problem.** The positioning rests on "une réalisation modernisée, avec un environnement « wow »", and *Tetris Effect* is named as the main inspiration for the "voyage audiovisuel". The Fantasy section describes each level as a journey "à travers le temps, l'espace et la nature". But Themes_niveaux.md is a table where every cell says "À définir": theme, mood, music, difficulty. The emotional arc and the link between themes and difficulty are both "Reporté à la version suivante". The sound and visual table is also "à valider".

**Source.** Positionnement; Fantasy; Inspirations (Tetris Effect); Themes_niveaux.md (all rows "À définir"); Evenements_visuels_et_sonores "Les sons et visuels proposés sont à valider".

**Why it matters.** Without the journey, the product is a modern remake of a 1997 mechanic with placeholder art. The audiovisual journey is what justifies a premium price and the Tetris Effect comparison, yet it is the least-defined part of the document and has no costed owner. The document cannot drive production on its main selling point.

---

## 3. Audience and platform strategy pull in opposite directions — MAJOR

**Problem.** Within one section the target audience is all of the following:
- "très large" (casual, low-skill controls);
- party gamers (local VS is "mis en avant");
- competitive puzzle players ("joueurs de Tetris et de Puyo Puyo", leaderboards);
- relaxation/ASMR players (Zen mode, "contemplation", "ambiance ASMR").

The launch platform is PC Steam Early Access only. That is the weakest platform for couch party play and for a "grand public" casual audience. The platforms where those audiences actually live (console for party play, mobile for casual) come "après l'accès anticipé". Online multiplayer, which competitive Puyo/Tetris players expect, is listed as cuttable.

**Source.** Public visé; Plateformes; Modèle économique; Mode VS ("Le jeu en ligne est optionnel"); Mode Zen; Variations sonores ("ambiance ASMR").

**Why it matters.** Each audience implies a different headline mode, store pitch and feature priority. The launch combination (PC, local-only VS, no online) serves none of them fully. Early Access is meant to "créer une communauté", but the competitive community needs online play and the casual and party audiences are not on the launch platform. The document needs to pick one primary audience for the Early Access launch and rank the modes to match.

---

## 4. Decorative features take scarce production budget and blur the core pillar — MAJOR

**Problem.** Several planned items do not support the core loop or the audiovisual fantasy:
- **Agent Narratif** (10 sessions) "génère la progression du joueur en modes solo et arcade". The game has no story, and progression is already defined numerically in Progression et thèmes.
- **Agent Intro**: "une séquence d'introduction différente pour chaque niveau". These are five bespoke cinematics with no defined content, since the themes themselves are undefined.
- **Bonus/malus marbles** appear in Solo, Arcade, VS and Zen. Zen even has options to toggle them. Yet the "Billes bonus/malus" document is listed without a link and does not exist. A cross-mode feature has no spec.
- **"Mode Multi"** has its own planned document and agent while also being "optionnel".
- **The declared "wow" moment** is "le challenge corsé que propose le NPC en mode arcade". That contradicts the Fantasy, Positioning and Tetris Effect framing, which locate the wow in the audiovisual journey. The AI opponent is also the second most expensive agent line (66 $), so budget is following the wrong pillar.

**Source.** Agents Roles; "Moment « wow » rendu possible par l'IA"; Documents liés; Progression et thèmes; Budget_tokens per-agent table.

**Why it matters.** The project has a 7-week, one-person budget. Every session spent on narrative scaffolding, per-level intros or an unspecified bonus system is a session not spent on the five themes that the positioning depends on. The document should state one wow pillar and cut or defer whatever does not serve it.

---

## 5. Premium Early Access offer and business model are under-specified — MAJOR

**Problem.**
- The game is "premium", but no price point, no Early Access duration and no exit criteria for leaving Early Access are given.
- The paid launch content is 5 levels. Longer runs (10 and 15 levels, and 10/15-opponent arcade series) are promised as post-Early Access updates. The document does not say whether those runs need new themed levels, which multiplies the content problem from finding 1, or simply repeat the 5. If they repeat, the update promise is thin.
- The budget covers only AI tokens. It leaves out the artist, music (commissioned or licensed), sound effects, the Steam Direct fee, marketing and community management for an Early Access launch, and console certification and porting for the planned console follow-up.
- The positioning claim that "aucun jeu reposant sur ce type de mécanique n'est sorti depuis 1997" is asserted without any competitive scan. For a market-gap argument, that is the evidence that should be on the page.

**Source.** Modèle économique; Mode solo / Mode arcade (updates after Early Access); Positionnement; Budget_tokens "Limites de l'estimation" (assets "dont le coût n'est pas compté ici").

**Why it matters.** "Under 530 $" understates what the project costs to ship. The unknown costs (art, music, marketing) are exactly the ones the differentiator depends on. A premium Early Access launch with 5 placeholder-risk levels and no stated price or roadmap commitments will have trouble earning the community the model relies on.
