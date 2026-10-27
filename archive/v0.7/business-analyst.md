# Production/Business Analyst — Round 1

Document reviewed: *The Marble Effect*, GDD v0.2 (22 Oct 2026), with linked documents.

## Finding 1: The selling point is the part this plan doesn't build
**Severity: BLOCKING**

**Problem.** The document sells the game on its audiovisual "wow". The Executive Summary calls it "un voyage audiovisuel inspiré de *Tetris Effect*", and Positioning (l.63) bets the market case on "une réalisation modernisée, avec un environnement « wow »". The production plan never builds that "wow":
- Scope (l.171) says "Les assets restent des placeholders ; un ou une artiste pourra définir l'identité visuelle". The artist is hypothetical.
- Technical constraint 1 (l.207) and Budget_tokens "Limites" (l.747) say textures, models and music come "de l'artiste, de placeholders ou de bibliothèques d'assets, hors budget tokens". No budget line covers them.
- In Evenements_visuels_et_sonores, "Environnement graphique par niveau" is only *Important*. "Reflets lumineux dans les billes en verre", the product's namesake material, is *Optionnel*. The environment reaction to player actions is also *Optionnel*, and it is the first item in the cut list after online play.

Without the art, what ships is a 1997 mechanic (the Lose Your Marbles reference) with placeholder visuals. That is exactly the "visuels datés" the Inspirations section says it is escaping.

**Why it matters.** It is sold as a premium Steam early-access release (l.59), and players judge early-access store pages on screenshots and trailers. The only budget in the document is ~$300–450 of tokens. There is no figure for the artist, the music licensing, asset-library purchases or the Steam Direct fee. The positioning and the scope describe two different products.

**Fix direction.** Either (a) budget and schedule the art and music as a named workstream, or (b) reposition early access as a mechanics-first build and drop "wow" as the launch pitch.

## Finding 2: Too many games at once for 7 weeks and one person
**Severity: MAJOR**

**Problem.** The MVP (l.173) is four modes (Solo, Arcade with 5 AI opponents, local VS, Zen) plus 5 themed levels, each with its own music, skin, environment and intro sequence (Agent Intro, l.189). That is 7 weeks for one person, and the 7 weeks include writing the GDD (l.171). On top of that, the document carries features that are in scope but not designed:
- **Bonus/malus marbles**: "peuvent apparaître dans tous les modes" (l.33). They are listed in every mode's progression (Themes_niveaux) and their appearance event is *Important*. Their design doc is "à rédiger" (l.218).
- **Leaderboard**: it is part of the Solo mode definition (l.18, l.28) and the doc is "à rédiger".
- **The five per-mode docs** (Solo, Arcade, VS, Multi, Zen): all "à rédiger".
- **Post-EA promises**: 10- and 15-level runs, 10/15-opponent series, console, mobile and online play (l.33, l.57).

Neither bonus/malus nor the leaderboard appears in the cut list (l.177). By omission they are protected, yet nothing describes what they are.

**Why it matters.** Each mode is a separate promise to a separate player (see Finding 3). The cut list only trims at the edges and never forces a choice of which game this is. Undesigned features with no place in the cut list are the classic source of schedule slip.

**Fix direction.** Either design bonus/malus and the leaderboard, or explicitly move them post-EA. Add them, and Agent Intro's per-level sequences, to the cut list.

## Finding 3: Audience is "everyone" and the protected features don't match the platform
**Severity: MAJOR**

**Problem.** "Public visé : très large" (l.61). That one line names three or four audiences:
- casual players ("ne demande pas une grande maîtrise des commandes");
- party/couch players ("le VS en local (party game) [est] mis en avant");
- competitive Tetris/Puyo Puyo players;
- relaxation/ASMR players (Zen, "une ambiance ASMR", the "contemplation" fantasy).

The protected core does not fit the launch platform:
- **Local VS is protected while online is cut first.** Local VS is "jamais coupé" (l.177), but local couch multiplayer is a weak selling point on PC/Steam. Online multiplayer is the first cut (l.177, and the automatic cut in constraint 4). The Puyo/Tetris competitive audience the document courts expects online play.
- **The casual "party" audience is mostly elsewhere.** It lives on console and mobile, which are deferred until after early access (l.57).

**Why it matters.** Early access exists "pour recueillir les retours des joueurs et créer une communauté" (l.59). A community needs a defined core audience and something to gather around, such as online play or a leaderboard. The document cuts the first and hasn't designed the second.

**Fix direction.** Pick a primary early-access audience (for example, solo puzzle and score-chasers on Steam). Protect the features that serve them, and treat the others as secondary.

## Finding 4: The cut order removes load-bearing features before decorative ones
**Severity: MAJOR**

**Problem.** Cut order (l.177): online, Optional events, levels 4–5, **Zen**, **adversary 4**. These are the wrong cuts:
- **Zen is the tutorial.** It "sert d'entraînement et de tutoriel" (l.31) and is billed as "relax et tutoriel" (l.18). The only other onboarding is passive hints during the player's very first game (l.113–115). Cutting Zen leaves a "très large" audience with almost no way to learn.
- **Adversary 4 teaches the stack**, which Combo_system names as *the* core tactic ("La tactique à adopter consiste à « stacker »"). The AI table says what players learn from it is "la tactique du stack, en la voyant chez l'adversaire". Adversary 5 is protected as the "wow" and "demande de maîtriser toutes les tactiques que les précédents ont montrées". Cutting 4 while keeping 5 removes the lesson but keeps the exam.
- **Decorative items are never at risk.** Per-level intro sequences (Agent Intro), the Agent Narratif progression layer and the per-theme music/skins never appear in the cut list.
- **The protection is internally inconsistent.** Adversary 5 is protected but the Arcade mode it belongs to is not. Levels 4–5 are cuttable, yet adversaries 4 and 5 are tied to levels 4 and 5 in the difficulty table (Themes_niveaux).

**Why it matters.** Under schedule pressure this list keeps spectacle and drops comprehension. That is the reverse of what a mechanics-dense puzzle game with a broad target audience needs.

**Fix direction.** Rank features by whether the core loop and onboarding depend on them. Protect a minimal Zen/tutorial and adversary 4 (or fold the stack lesson into Solo). Put intro sequences high on the cut list.

## Finding 5: Market positioning rests on one unsupported claim, with no pricing or early-access plan
**Severity: MINOR**

**Problem.** The whole market case is one sentence: "aucun jeu reposant sur ce type de mécanique n'est sorti depuis 1997" (l.63). Several things are missing:
- **No comparables.** The document does not test whether that gap is an opportunity or a sign of low demand. It does not position against the 2026 puzzle-VS field it names (Tetris Effect: Connected, Puyo Puyo Tetris) or against modern match-3 and mobile puzzle games.
- **No early-access plan.** "Jeu premium" has no price point. There is no EA duration, no content plan for EA versus 1.0, and no success metric (wishlists, reviews, retention) to decide when to move on to console and mobile.

**Why it matters.** For a premium early-access launch these are go/no-go inputs. As written, there is no way to tell whether the post-EA roadmap (more levels, console, mobile, online) is funded by anything.

**Fix direction.** Add a short market section: 3–5 comparables with price and reception, a target price, the EA scope versus 1.0, and the metrics that trigger the post-EA roadmap.

## Round 2 — Cross-examination

### Conflicts

**Feasibility Lead F3 (assets have no route to production, rated MAJOR) vs. my Finding 1 (BLOCKING).** We agree on the facts and disagree on the severity. Feasibility rates it MAJOR because "the core loop can be built." From a production standpoint that is correct. From a business standpoint it is the wrong test. The document does not pitch a core loop; it pitches "un voyage audiovisuel inspiré de *Tetris Effect*" and an "environnement « wow »" as the reason to buy a premium Early Access title. Feasibility also adds evidence that strengthens my case: per-level music is rated **MVP**, yet no agent, composer or license budget can produce it, and adaptive layering needs stems that library tracks rarely provide. An MVP item with no source is a blocker by the document's own priority scheme. I am keeping BLOCKING. The fix is cheap, though, because repositioning the EA pitch is a document change, so this blocker should be quick to clear.

**Narrative Critic F1, F3 and F4 vs. my Findings 2 and 4 (scope).** The Narrative Critic's fixes add content: distinct win and loss beats, between-level transitions, an end-of-voyage outro, contemplative interludes, and a fiction for five adversaries. My Round 1 position is that this plan is already over-scoped and that decorative layers (per-level intros, Agent Narratif) should go to the top of the cut list. I accept the narrative diagnosis, but each fix has to be paid for by a cut elsewhere:
- Distinct win/loss beats (Narrative F3) are load-bearing, because they are core feedback. That matches Player Psychologist F4. I would fund them by removing four of the five per-level intro sequences.
- The Narrative Critic's own "compressed finale" option is the right production answer to its F1: one finale level that hosts both the Solo ending and adversary 5, in place of separate levels 4 and 5.
- On adversary fiction (Narrative F4), I would go the other way: drop the "wow" label from Arcade instead of commissioning characters. Adding a cast to a 7-week, one-person game in order to justify a marketing line is the wrong trade.

**Player Psychologist (whole review) vs. my Finding 3 (audience).** The Player Psychologist treats the "très large" audience as given and scores against a novice in the first hour. If the team narrows the EA audience as I recommend, some severities shift:
- The irreversible switch and the hidden counter rules (PP F2, F4) cost less with puzzle veterans.
- The missing reason to return (PP F5) costs more, because score-chasers churn fastest without a defined score and leaderboard.

Who the audience is changes how severe these findings are. The board should not rank player-experience findings until the audience is chosen.

### Connections

**The "never cut" core is the part with the broken win condition (Systems F1, Adversarial QA F1 + my Findings 3 and 4).** The scope list protects three things: Solo, local VS and adversary 5. Two independent reviewers show that the attack loop cannot produce the loss condition (defenders push columns to L1 and attack balls are "perdue"), so VS and Arcade have no reliable way to end. The plan therefore protects, as its non-negotiable core, the two competitive features whose central mechanic doesn't close. Business consequence: until that is fixed, local VS cannot be the "mis en avant" party-game pitch, and online multiplayer is not worth scheduling at all.

**The protected "wow" boss is undeliverable twice over (Adversarial QA F5 + Feasibility F2 + my Finding 4).** Adversary 5 is the one Arcade item the document will never cut. QA shows that a 1–2 action lookahead cannot stack or release on purpose, so opponents 4 and 5 cannot show their advertised behaviour. Feasibility and QA both show that adversary 5 loses its level, music and spawn tier when levels 4–5 are cut. That makes the marketing centrepiece the riskiest line in the plan, and it is also one of the two Opus-priced agent lines in the budget. Protecting it costs the most and is the least likely to pay off.

**My recommended audience rests on systems that don't exist yet (Systems F5, Adversarial QA F2 + my Finding 3).** In Round 1 I suggested solo puzzle players and score-chasers as the EA audience. Systems F5 shows there is no score formula, and QA F2 shows that a late Solo level can soft-lock once every column passes 7 balls. A score-chasing audience needs a score that exists and a Solo mode that is always winnable. That makes the leaderboard and the release-guarantee fallback launch-critical, not nice-to-have.

**There are no working financial controls at all (Feasibility F5 + my Findings 1 and 5).** Feasibility shows that the only budget safeguard (switch Opus to Sonnet, cut online) saves almost nothing, because both are already reflected in the MVP figure. Combined with my findings (no art, music, store or price figures), the document has no functioning budget control. The token line is the only thing tracked, and it is the cheapest input.

**Unscoped work keeps growing (Feasibility F4 + Adversarial QA F4 + my Finding 2).** Bonus/malus marbles came up independently in four reviews (Systems F5, QA F4, Feasibility F2, mine). Feasibility adds an automated test harness that nobody is scoped to build. QA shows that bonus/malus would invalidate every rule that harness is supposed to check. These are real deliverables that sit outside both the production order and the cut list.

### Revisions

- **Finding 2: upgrade MAJOR → BLOCKING.** Feasibility F1 supplies the arithmetic I lacked: about 32 agent sessions a week, each reviewed by one person; no dated milestones, so the cut list never triggers; and Steam store-page and review lead times inside a window that ends in mid-December. Combined with the unscoped features above, the scope cannot be delivered as written. This is the same problem as Feasibility F1, seen from the scope side. I recommend the moderator merge the two.
- **Finding 3: partially withdraw and re-aim.** I withdraw the implication that cutting online first is a mistake. With the attack loop broken (Systems F1, QA F1), online is correctly the first cut. What survives is that local VS should not be "jamais coupé" on a PC launch, and the audience still has to be chosen. The fix now depends on the Solo score and soft-lock issues above. Severity stays MAJOR.
- **Finding 4: hold at MAJOR, with sharper evidence.** Four reviewers reached the same cut-order contradiction independently (Systems F3, Player Psychologist F1/F5, Feasibility F2, QA F3). That is the strongest consensus on the board. I am not upgrading it, because it is a document fix that costs no production time. The fix direction now adds: name the level that hosts adversary 5 in the cut scenario (a single compressed finale, per Narrative F1).
- **Finding 1: hold BLOCKING** (see Conflicts).
- **Finding 5: hold MINOR.** Nothing in the other reviews changes it. It only becomes go/no-go once Finding 1's repositioning decision is made.
