# Production/Business Analyst — Round 1

Document reviewed: *The Marble Effect*, GDD v0.5 (plus linked documents bundled in gdd.txt).

## Finding 1: The selling point is the one part nobody is building or paying for
**Severity: BLOCKING**

**Problem.** The pitch and the positioning both rest on the audiovisual journey. The Executive Summary calls it "un voyage audiovisuel inspiré de *Tetris Effect*". The Positioning section says "Une réalisation modernisée, avec un environnement « wow », serait l'occasion de faire découvrir ce type de jeu au grand public." The production plan then strips that out:
- Scope: "Les assets restent des placeholders ; un ou une artiste **pourra** définir l'identité visuelle". The artist is optional, unnamed, and has no budget.
- Technical constraint 1 and Budget_tokens "Limites": textures, models and music come "de l'artiste, de placeholders ou de bibliothèques d'assets, hors budget tokens", and their cost "n'est pas compté ici".
- The MVP is defined as "5 niveaux **avec leur thème**". Themes_niveaux specifies those themes in detail: resin marbles with leaves inside, a neon city whose districts light up with combos, a galaxy inside each marble. Evenements lists "Musique propre à chaque niveau" as **MVP** priority.

**Why it matters.** As specified, the MVP is a premium paid early-access game whose main differentiator ships as placeholders. The token budget is the only budget in the document, so the most expensive and market-critical work has no line item, no owner and no milestone. Either the art and music budget and source are defined, or the positioning has to move away from "wow audiovisual journey" toward something the plan can actually deliver, such as mechanics-first competitive puzzle.

## Finding 2: Four modes and three game identities in a 7-week, one-person project
**Severity: MAJOR**

**Problem.** The document is trying to be three games:
1. **A contemplative audiovisual journey** (Solo, Fantasy: "initiation à la persévérance et à la contemplation", ASMR sound design), which is the Tetris Effect game.
2. **A competitive versus puzzler** (Arcade with 5 escalating AIs, plus VS), which is the Puyo Puyo game.
3. **A party/couch game** ("VS en local (party game)" is promoted in the audience section and can never be cut).

Zen adds a fourth, relax/tutorial identity. Each identity pulls toward different success criteria. The journey needs art, music and pacing. The versus game needs balance, tie rules and round length, which the document still lists as "restent à définir". The party game needs local-multiplayer UX and drop-in play. Production order step 2 puts local VS ahead of Arcade, even though the document names Arcade's adversary 5 as "le moment « wow »".

**Why it matters.** The document never states which identity is the core vision, so it cannot judge which features are load-bearing. The cut list shows this: it protects Solo, VS local **and** adversary 5 at once, which preserves all three identities instead of choosing one.

## Finding 3: The cut list removes load-bearing features and keeps decorative ones
**Severity: MAJOR**

**Problem.** The section "Ce qui peut être coupé, dans cet ordre" contains several inversions:
- **Zen is cut before adversary 4**, but Zen is the declared "entraînement et tutoriel" (Concept, Thèmes). Cutting it removes the only structured learning space and leaves just the first-game-only passive hints.
- **Adversary 4** is the only place the stack tactic is taught (AI table: "la tactique du stack, en la voyant chez l'adversaire"). Combo_system calls the stack "la façon la plus rentable de remplir la jauge", which makes it the core mastery skill. Yet adversary 4 is cuttable and the "wow" adversary 5, which "demande de maîtriser toutes les tactiques", is protected. Adversary 5 is then the payoff of a lesson that may never have been taught.
- **Decorative features never appear in the cut list**:
  - per-level intro sequences (Agent Intro, 10 sessions);
  - bonus/malus marbles, which appear "dans tous les modes" with Important-priority events but whose design document is "(à rédiger)";
  - the leaderboard ("classement"), whose document is also "(à rédiger)". An itch.io leaderboard implies a backend that no agent and no budget line covers.

**Why it matters.** Under schedule pressure the plan as written cuts teaching and mastery content first and keeps unscoped extras that have no design. Bonus/malus and the leaderboard are feature creep: they are in every mode's description but in no production step, no agent assignment and no budget.

## Finding 4: Audience and market claims are asserted, not evidenced
**Severity: MAJOR**

**Problem.**
- **Audience**: "très large, car le jeu ne demande pas une grande maîtrise des commandes". Ease of controls is not an audience definition. The same section also targets Tetris and Puyo Puyo players, who are a skill-focused, competitive niche, while the Fantasy targets contemplative/ASMR players. These are three different buyers with different expectations of a premium price.
- **Positioning**: "aucun jeu reposant sur ce type de mécanique n'est sorti depuis 1997". There is no competitive analysis behind this. The document's own inspirations, *Tetris Effect* and *Puyo Puyo*, are active franchises that compete directly for the same versus-puzzle and audiovisual-puzzle player. A 30-year gap since *Lose Your Marbles* is at least as likely to signal weak demand as an open market. The document does not discuss which.
- **Platform fit**: local couch VS is a headline feature on PC via itch.io. That storefront and platform have weak couch-play habits, and the Controls doc requires a shared keyboard or two controllers.

**Why it matters.** Without a primary target player, the team cannot settle Findings 2 and 3 (which identity, which cuts) on market grounds.

## Finding 5: The premium early-access model has no price, goals or feedback loop, but promises a long roadmap
**Severity: MAJOR**

**Problem.** The business model is "jeu premium, avec un accès anticipé... sert à recueillir les retours des joueurs et à créer une communauté". The document does not state:
- a price point;
- a revenue or player-count target;
- any success criterion for leaving early access;
- how feedback is collected (no telemetry, survey, Discord or devlog plan);
- who runs community management after the 7 weeks.

At the same time it commits to a large post-EA roadmap:
- console and mobile ports;
- a full mobile touch scheme, including "le jeu peut se jouer en local à deux sur toutes les plateformes", which implies two-player on one phone;
- 10- and 15-level journeys and 10- and 15-opponent series;
- online multiplayer.

The only budget shown covers 7 weeks of tokens. Nothing covers EA operations or the roadmap.

**Why it matters.** Paid early access on itch.io with placeholder art (Finding 1) and no community plan risks low uptake. That would leave the feedback EA exists to collect too thin to steer the roadmap. The roadmap promises in Controls.md and the Concept section also create player expectations that the plan does not fund.

## Note (MINOR, not counted above)
Much of the GDD is about the production pipeline rather than the game: 10 agents, token costs, model choices. This includes the Executive Summary's opening line and an "Effet des autres agents sur le gameplay" table that lists Agent Test as player-facing. That material belongs in a production plan. In a design document it dilutes what the player is buying, and it presents a production method as if it were a game feature.

## Round 2 — Cross-examination

### CONFLICTS

**vs. Narrative Critic F1 (protect the journey's ending, BLOCKING) and F4 (give Arcade opponents characters/framing).**
I agree the 3-level fallback has no ending. I disagree with where that points. Levels 4 and 5 are the two most expensive themed environments in the document (a neon city whose districts light up per combo, a galaxy inside each marble), and per my F1 they are exactly the content with no art budget, no artist and no music source. Protecting them protects the most unfunded part of the plan. The cheaper fix, and the one that matches the budget, is to design the 3-level version as the shipping arc (re-stage Les Sables as a finale, or re-sequence so the "espace" beat comes third) and treat levels 4-5 as EA content updates. That also gives early access a roadmap it currently lacks (my F5). On F4: adding named opponents, framing and per-opponent staging is new content scope. I'd only support it if the board first decides the competitive identity is the core one (my F2). Otherwise it adds a fourth content track to a project that can't fund its first.

**vs. Narrative Critic F2 (two "wow" moments; "protect each accordingly").**
We agree on the diagnosis. The remedy "Solo's peak is contemplative and Arcade's is competitive, with each protected accordingly" is the problem I flag in F2, not a solution to it. Protecting both peaks is how the cut list ended up protecting three identities at once. For a single developer on 7 weeks the document has to choose one peak to fund. The other becomes a secondary mode with reduced staging.

**vs. Player Psychologist F5 (local VS has no comeback or handicap; MINOR unless VS stays the lead marketing pillar).**
By the document's own text, it *is* the lead pillar: the audience section "met en avant" VS local (party game), and the cut list says VS local is "jamais coupé". So the psychologist's own condition for MAJOR is already met, and I'd rate it MAJOR. I also disagree on the fix. Adding handicap, catch-up and bonus/malus balancing adds more features to the least-specified mode. Given the market point in my F4 (couch play on a PC itch.io release), the cheaper business answer is to demote local VS from "never cut" and stop marketing it as a party game until it has rules. Arcade can carry the competitive identity alone.

**vs. Feasibility Lead F4 (audiovisual wow unscheduled, rated MAJOR).**
Same evidence as my F1, but the severity differs, and I'm holding mine. From a feasibility view the game can still be built with placeholders, so MAJOR is fair under that lens. From a product view, a premium paid EA whose stated differentiator ("un voyage audiovisuel", "environnement « wow »") ships as placeholders can't be sold under the positioning the document gives. That blocks the business case, not the build.

### CONNECTIONS

**Systems Designer F1 + Adversarial QA F1 (Arcade/VS rounds may never end) x my F2/F3.** Combined, these give the sharpest feature-creep result on the board. The two modes the cut list marks "jamais coupés" (VS local and opponent 5) both sit on a win/loss rule that two reviewers independently show may never terminate. Production order step 2 builds local VS *before* Arcade. So the plan commits early, irreversibly, to the mode with the least working design. A never-cut label should require a working ruleset. VS currently fails that test.

**Systems Designer F2 + Adversarial QA F3 (degenerate Solo leaderboard) x my F3 (leaderboard is unscoped creep).** Viewed alone, my finding said the leaderboard has no design doc, no backend and no budget. Their findings add that, even if built, it would measure "completed or not" plus one stall-and-fire combo. A feature that's unfunded, unscheduled *and* measures nothing meaningful is the clearest MVP cut in the document. I now recommend removing it from MVP and from the Solo pitch ("avec un classement") outright.

**Feasibility Lead F2 (cutting opponent 4 saves almost nothing) x my F3.** I argued that cutting opponent 4 was wrong on teaching grounds. Feasibility shows it's also wrong on cost grounds: opponent 5 still needs the stacking behaviour, so cutting opponent 4 removes the lesson without removing the engineering. That item buys no schedule. A cut list item that saves no schedule isn't functioning as a cut.

**Feasibility Lead F1 (MCP authoring of Unreal assets untested) x my F3 (decorative features absent from the cut list).** Per-level intro sequences (Agent Intro) are Level Sequence assets. They are among the most decorative features in the plan, and they depend on the least proven pipeline (Sequencer via MCP). Skins have the same profile. These should sit at the *top* of the cut list, ahead of anything that teaches or balances the game.

**Feasibility Lead F3 (no playtest plan, no sim harness) x my F5 (EA has no feedback loop).** Every tuning value is "à régler en playtest", but there's no playtest slot in the 7 weeks. So, by default, paid early access *is* the playtest, but with no telemetry or feedback channel to collect results. Paying customers become the balancing harness for an untuned game, and the document has no way to hear what they find.

**Player Psychologist F3 (victory = defeat; combo and danger signals not MVP) x my F5.** The MVP build is the first build EA buyers see. Under the current priorities they get placeholder art, combos that don't feel like combos, no warning before losing, and a win that looks like a loss. That's the opposite of the "créer une communauté" goal. The first impression is the community funnel, and the feedback priorities don't serve it.

**Player Psychologist F4 (control scheme is not simple) x my F4 (audience claim).** The only evidence offered for "audience très large" is "ne demande pas une grande maîtrise des commandes". The psychologist shows the control scheme contradicts that premise (three directional verbs, A/D vs Q/E, stick ambiguity). That leaves the broad-audience claim with no supporting argument at all.

**Feasibility F5 / Adversarial QA F5 (Haiku cost error, no working overspend lever) x my F5.** The token budget is the only financial plan in the document. Two reviewers independently found it's both miscalculated and has no functioning guardrail. That strengthens my point that the document has no financial governance. It doesn't change their MINOR rating, since the dollar amount is small.

### REVISIONS

- **F3 upgraded MAJOR -> BLOCKING.** The cut list is the project's only risk-management tool. Combined with colleagues' findings, it now fails on three independent axes:
  - It cuts teaching before the test it prepares for (Systems F4, Psychologist F1, QA F4 all reached this independently).
  - Its opponent-4 cut saves no engineering effort (Feasibility F2).
  - It is silent on every unspecified or decorative feature that is most likely to slip: bonus/malus, leaderboard, intros (Feasibility F4).

  A broken cut list on a 7-week single-developer schedule blocks reliable production planning.
- **F2 sharpened, stays MAJOR.** The cross-examination gives a concrete answer to "which identity is load-bearing". The competitive identities (VS and Arcade) have a non-terminating core rule, and the journey identity is unfunded (my F1). My recommendation is to make Solo plus Arcade the core and demote local VS from "never cut". The board should escalate that choice to the designer.
- **F4 partially withdrawn.** The "platform fit" bullet (itch.io and PC have weak couch-play habits) is my market judgment, not something the document states or omits. I withdraw it as evidence. The rest of F4 stands on the document's own text, and Psychologist F4 strengthens the audience-claim bullet. F4 stays MAJOR.
- **F1 unchanged, BLOCKING** (see conflict with Feasibility F4 above).
- **F5 unchanged, MAJOR.** It is strengthened by the Feasibility F3, Psychologist F3 and budget-error connections.
