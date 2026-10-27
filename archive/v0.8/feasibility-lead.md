# Technical Feasibility Lead — Round 1

Document reviewed: *The Marble Effect*, GDD v0.5 (26 Oct 2026), plus linked docs. Context: one developer, Unreal, 7 weeks *including writing the GDD*, 10 Claude production agents connected via MCP, token budget of about $300 with a $450 cap. I judged it against what a single person working with 2026-era LLM agents can actually ship.

---

## Finding 1: Most deliverables are Unreal binary assets, and the plan to edit them through MCP has never been tested and has no fallback

**Severity: BLOCKING**

**Problem.** Technical constraint 2 admits the core problem: "un agent ne peut pas modifier un fichier .uasset comme du texte. La logique du jeu est donc écrite autant que possible en C++, et les assets sont modifiés par la connexion MCP vers l'éditeur." But look at what the agents are supposed to produce. Nearly all of it is .uasset content, not C++:
- **Agent NPC**: "un profil scripté (behaviour tree)". Unreal Behavior Trees and Blackboards are editor assets.
- **Agent Menu**: menus, which means UMG widgets (assets).
- **Agent Feedback**: "modèle + animation + VFX", which means Niagara systems, materials and animation assets.
- **Agent Skin**: skins, which means materials and material instances (assets).
- **Agent Intro**: "une séquence d'introduction différente pour chaque niveau", which means Level Sequences (assets).

So 5 of the 9 MVP agents depend on MCP authoring those assets reliably. The document treats this as already solved. Nothing in the production order checks that it works, no session budget covers MCP failures, and there is no fallback (for example, "build the AI as a C++ utility scorer instead of a BT asset" or "the developer builds menus by hand"). Step 1 of the production order already depends on it: Menu (minimal version) and Feedback (MVP effects) are both "Indispensable" for the first playable Solo level.

**Why it blocks.** If MCP editing of BT, UMG, Niagara or Sequencer assets turns out to be partial or fragile, that work goes back to the one developer by hand. The schedule and the 190-session budget don't account for that. The GDD needs a week-1 pipeline spike (one asset of each type created through MCP) and a fallback stated for each agent before anyone can trust the production order.

---

## Finding 2: The AI cannot produce the never-cut "wow" opponent as described, and the cut order doesn't save the effort it claims to

**Severity: MAJOR**

**Problem.** AI_Architecture.md describes the decision algorithm: "elle évalue les coups possibles à une ou deux actions (move ou switch) et choisit celui qui forme l'alignement le plus lourd, ou qui prépare un stack." To stack, you have to build a 3-of-a-kind alignment *off* the median line, above a line you'll clear later. In this cylinder-wrapping move/switch system that takes many coordinated actions. A 1–2 ply greedy search can't plan it. It will just grab the heaviest alignment it can reach right now. Opponents 3, 4 and 5 ("combos de 2 lignes", "stack et combos de 3 lignes et plus", "tous ces comportements") need multi-step planning that the document never specifies.

The scope plan also contradicts itself here:
- Production order: "Arcade, avec les adversaires 1 à 3 et 5, puis 4".
- Cut list: "...puis l'adversaire 4. Le Solo, le VS en local et l'adversaire 5 (le moment « wow ») ne sont jamais coupés."
- Opponent 5 "combine tous ces comportements", stacking included.

Cutting opponent 4 saves almost nothing, because opponent 5 still needs the hardest behaviour (stacking). The one AI feature marked as never cut is also the riskiest, and it rests on an algorithm that can't do what it promises. The 25 Opus sessions for Agent NPC are budgeted as if the problem were already understood.

**Needed.** Either specify a planning approach (a stack-template search, scoring board states several moves ahead, or scripted stack recipes), or redefine opponent 5 so it doesn't need deliberate stacking.

---

## Finding 3: Agent Test is "Indispensable" at step 1 but has no way to do its job, and every tuning value relies on playtesting that has no plan or time

**Severity: MAJOR**

**Problem.** Agent Test is listed as "Indispensable : vérifie que le niveau peut être terminé et qu'aucune règle ne bloque la partie", and it runs on Haiku at 40 sessions. Verifying that a random-refill level *can be completed* needs an automated player plus a headless or fast-forward simulation harness: something that plays thousands of games against the refill timer ("une bille toutes les 4 s"... "toutes les 2 s") and reports win rates. The GDD never mentions such a harness. The only automated player it describes is the arcade AI, and that only arrives in step 3. That's a hidden dependency: the step-1 milestone needs a bot that the plan doesn't build until two steps later. AI_Architecture.md adds "L'Agent Test fait jouer chaque adversaire pour vérifier qu'il peut gagner et perdre", which again assumes a player-side bot exists.

The same gap sits under balancing. Marble weights, the 100-point gauge threshold, progression gauge sizes (10–30 gauges), refill rates and AI speeds are all "valeurs de départ, à régler en playtest". The document gives no playtest schedule, no telemetry and no simulation-based tuning, and the timeline has no slot for it. One person will be tuning 5 solo levels and 5 AI opponents by hand in whatever is left of the 7 weeks.

**Needed.** Make a deterministic headless board simulator plus a baseline bot an explicit step-1 deliverable owned by Agent Gameplay. Agent Test and Agent NPC both depend on it, and it would turn "à régler en playtest" into something that can be done in batches.

---

## Finding 4: The audiovisual "wow" and several open features sit inside MVP scope, but none of them are scheduled or in the cut list

**Severity: MAJOR**

**Problem.** The positioning is "Tetris Effect"-style: "un voyage audiovisuel", "environnement « wow »", 5 strongly themed levels. Yet:
- Technical Strategy: "Les assets restent des placeholders ; un ou une artiste **pourra** définir l'identité visuelle". The artist is optional and has no schedule.
- Constraint 1: textures, models and music come "de l'artiste, de placeholders ou de bibliothèques d'assets, hors budget tokens". No time or money is budgeted for any of it.
- Événements visuels et sonores: "Musique propre à chaque niveau" is marked **MVP**. Five distinct tracks (85–115 BPM, specific instrumentation, and layers added at each milestone as adaptive music) have no source.
- The Ambiance agent gets 5 sessions to integrate layered adaptive music across 5 levels.

The MVP also includes features that are still undefined, and none of them appear in the production order or the cut list:
- **Bonus/malus marbles**: "peuvent apparaître dans tous les modes", with a Zen selection option, and a VFX event at "Important" priority. Their spec doc is "(à rédiger)".
- **Leaderboard**: Solo is "avec un classement". Spec "(à rédiger)", and it's unclear whether it's local or online (online would mean a backend on itch.io).
- **Arcade/VS rules**: "La durée des manches et les cas d'égalité restent à définir."
- Menu/options and all five mode docs are "(à rédiger)".

Since the 7 weeks include writing the GDD and this is v0.5 with about 8 linked docs still unwritten, design time is eating into a schedule that's already very tight. The cut list only addresses features that are already specified. It is silent on exactly the items most likely to slip.

**Needed.** Either put bonus/malus, the leaderboard and themed environments into the cut list explicitly, or schedule and source them. Downgrade per-level music from "MVP" to library tracks, or name where it comes from.

---

## Finding 5: The token budget miscalculates Haiku costs, and the overspend fallback doesn't match the model plan

**Severity: MINOR**

**Problem.** Budget_tokens.md says Haiku's $0.10/$0.50 rate "s'applique aux requêtes de moins de 100 000 tokens (0,50 \$ / 2,50 \$ au-delà)". But the same document assumes every session reads about 1.5M input tokens, and Haiku runs "sans cache". At the over-100k rate, a Haiku session costs about 1.5 × 0.50 + 0.06 × 2.50 ≈ **$0.90**, not $0.18. Agent Test then costs about **$36**, not $7. The MVP total becomes about $329. That's still under the cap but uses up a fifth of the margin. More generally, it's doubtful that 1.5M tokens of "projet relu à chaque étape" fits a small model's context per request at all. The cost model and the context model need to be reconciled.

Separately, Constraint 4's overspend lever ("les tâches courantes passent d'Opus à Sonnet") contradicts the cost table, where routine tasks already run on Sonnet. Only Gameplay and NPC use Opus, and those are exactly the systems flagged above as the hardest. The real fallback is either downgrading the two riskiest agents or cutting online multiplayer, which isn't in the MVP anyway. In practice, the plan has no working cost lever.

---

### Summary of severity
| # | Finding | Severity |
|---|---|---|
| 1 | Asset-heavy agents depend on untested MCP authoring of Unreal assets; no spike, no fallback | BLOCKING |
| 2 | 1–2 ply AI can't stack; never-cut opponent 5 needs it; cutting opponent 4 saves nothing | MAJOR |
| 3 | Agent Test has no sim harness or bot at step 1; all balance values "à régler en playtest" with no plan | MAJOR |
| 4 | Audiovisual "wow", per-level music (MVP), bonus/malus, leaderboard and round rules are unscheduled and missing from the cut list | MAJOR |
| 5 | Haiku pricing tier ignored (~$36 vs $7); overspend lever doesn't match the model plan | MINOR |

---

## Round 2 — Cross-examination

### 1. CONFLICTS

**vs. Business Analyst F2 (pick one identity; suggested pivot toward "mechanics-first competitive puzzle").** I agree the document has to choose a core. I disagree with the direction the analyst floats. On feasibility grounds, the competitive half is where the highest technical risk sits: Arcade needs an AI that can stack, which the stated 1–2 ply search can't do (my F2). Arcade and VS also have no termination rule (Systems F1, QA F1), and Arcade alone needs 25 Opus sessions of NPC work. Solo already has a forcing function (autofill), needs no AI, and can be tested by a simple bot. If the board picks one identity to protect, the version this one-person, 7-week plan can actually deliver is Solo-first, with VS as a thin add-on that reuses the Solo board and an autofill timer. A competitive-first pivot puts the riskiest system at the centre of the product.

**vs. Narrative Critic F1 (cutting levels 4–5 leaves no ending; needs a planned fallback) and F4 (give opponents characters and framing).** I accept the diagnosis but not the cost of the implied fixes. Re-staging Les Sables as a finale, adding an outro, and giving opponents dramatic setup all route through Agent Intro and Agent Feedback, which produce Level Sequences, Niagara and UMG assets. Those assets sit behind the MCP pipeline that has never been tested (my F1, BLOCKING). My counter-position: any narrative fallback has to be text, UI cards or audio (a name card per opponent, a closing text screen plus a music sting). New sequences should not be added until the MCP spike has passed. I also question the premise of F1 from the cost side. Cutting levels 4–5 saves very little *engineering*, because a level here is mostly data (gauge count, refill rate) plus theme art and music that are unbudgeted anyway (my F4). The cut costs the narrative a lot and saves production almost nothing, which supports the critic's case that the cut is the wrong lever. I just get there for a different reason.

**vs. Player Psychologist F3 (promote combo escalation, defeat warning and milestone music from Important to MVP).** Every one of those events is a Feedback/Ambiance deliverable, and Feedback is already "Indispensable" at step 1 on top of an unproven asset pipeline. Promoting three more VFX/music events to MVP adds to the most fragile part of the plan. My position: promote them only in their cheap form, with no new Niagara systems. That means an audio sting and a UI multiplier label for combos, a colour or tint change on the board for approaching defeat, and a single layer swap for milestones. Victory versus defeat (Psych F3, Narrative F5) is the exception: it's a near-zero-cost asset swap and should simply be done.

**vs. Player Psychologist F4 (controls).** The gamepad diagonal ambiguity is real but cheap to fix technically: an axis-lock or dominant-axis rule in the input layer, a few lines of C++ that sit outside the .uasset problem. I would rate the engineering risk here as low. The keyboard A/D vs Q/E collision is a mapping decision, not a feasibility risk.

### 2. CONNECTIONS

**Systems F1 + QA F1 (Arcade/VS never terminate) with my F2 and F3 means the step-3 milestone has no achievable acceptance test.** AI_Architecture defines Agent Test's job as checking that each opponent "peut gagner et perdre". With no forcing function, rounds against a slow opponent 1 may not end, so the test cannot pass. With no headless simulator (my F3), nobody finds that out until a human plays it late in the schedule. That would be after 25 Opus sessions have already been spent tuning AI behaviours on a ruleset that doesn't converge. The dependency chain is: round-end rule, then simulator, then AI tuning, then Agent Test. The production order runs it backwards. A fix is technically cheap, because Solo autofill already exists in code, so a round timer that switches autofill on is a reuse, not a new system. It still has to be designed before step 3 starts.

**Systems F3 + QA F2 (cylinder wrap breaks gravity, refill and attack placement) with my F3: a deterministic simulator can't be built against an undefined resolution step.** The GDD claims "même situation, même résultat", but "Chute" on a wrapped column is unspecified. In an agent-driven pipeline, that gap does not stay open. Agent Gameplay (Opus) will fill it with whatever behaviour it infers, and that inferred behaviour then becomes the spec silently. This is a feasibility risk that only shows up because the builders are LLM agents: holes in the spec are resolved by the implementer without review. The v0.5 rule change also lands late, which means re-reading the docs and possibly rewriting code that was written against v0.4.

**QA F3 + Systems F2 (degenerate Solo leaderboard) with Business F3 and my F4 (online vs. local leaderboard unspecified).** Both designers' fixes (time-to-clear, points per marble) are local calculations. If the leaderboard is declared local-only (a SaveGame table), it costs roughly one session. If it stays ambiguous, "classement" on an itch.io release could be read as online, which needs a backend that no agent, budget line or production step covers. The design fix and the scope fix should be made together: a local leaderboard with a time-based score.

**Business F1 (art and music unfunded) with my F1.** "Placeholders" understates the problem. Even library assets have to be imported, set up as materials and wired into Niagara and Sequencer, which is exactly the MCP authoring path that hasn't been validated. So the visual "wow" depends on two unproven inputs at once: an asset source and an asset-authoring pipeline.

**QA F4 / Systems F4 / Psych F1 (opponent 4 is the only stack teacher, cuttable, built last) with my F2.** These compound. Opponent 4 is the last AI task in step 3, so it is the item most likely to be cut by schedule default, not by choice. Cutting it saves almost no AI work, because opponent 5 still needs stacking. It also removes the only lesson for the skill opponent 5 tests. Under that cut order, the plan keeps all of the technical risk and loses the pedagogy.

### 3. REVISIONS

- **F3 upgraded MAJOR → BLOCKING.** With Systems F1 and QA F1, it's no longer just "tuning without tools". Agent Test's stated acceptance criteria are unverifiable for Arcade and Solo at the milestones where they're marked "Indispensable", and the missing termination rule would only be found by manual play late in the schedule. The simulator and the round-end rule are prerequisites for step 3.
- **F2 strengthened (stays MAJOR).** Three colleagues independently reached the same AI and cut-order problem from design, QA and psychology angles. My addition is that the cut doesn't even buy schedule relief. I'm keeping it at MAJOR, not BLOCKING, because "redefine opponent 5 without deliberate stacking" is a viable cheap way out. But it should be decided now, not discovered later.
- **F4 kept at MAJOR, with the music line narrowed.** Business F1 rates the unfunded audiovisual identity BLOCKING on market grounds, and I don't contest that from their lens. Technically, a build with placeholders still ships, so it stays MAJOR for me. Separately, the narrative cut-list point above changes my recommendation: before cutting levels 4–5, cut per-level bespoke music (switch to library tracks) and per-level intro sequences. Those save real production effort, while level count saves almost none.
- **F5 unchanged (MINOR).** QA F5 independently reproduced the ~$0.90/session Haiku figure and the dead overspend lever, so the finding is confirmed. It's still minor because the corrected total (~$329) stays under the $450 cap.
- **New sub-point under F1:** the cylinder-wrap ambiguity (Systems F3, QA F2) makes F1 worse in an agent pipeline. Undefined rules get decided by the implementing agent with no review. The week-1 spike should include a short "rule freeze" check: no implementation of resolution order until Chute and Refill on a wrapped column are written down.
