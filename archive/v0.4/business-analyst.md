# Production/Business Analyst — Round 1

Document reviewed: `gdd.txt` (GDD_V_0_2 "The Marble Effect" plus linked docs: Combo system, Schémas du déplacement, Événements visuels et sonores, Thèmes des niveaux, Budget tokens).

---

## Finding 1 — The "MVP" is not minimal: four modes, five themed levels, per-level intros and a narrative layer in 7 weeks, for one person, with the GDD included in that time
**Severity: BLOCKING**

**Problem.** The cut list removes almost nothing. Everything that makes up the game is in the MVP. One developer, working with AI agents, is expected to ship four separate modes, five fully themed levels, a leaderboard, bonus/malus marbles, an arcade AI with difficulty progression, per-level intro sequences and a "progression narrative" in 7 weeks. Those 7 weeks include writing the GDD. Each mode has its own win condition, its own settings (5/10/15 courses, best of 1/3/5 rounds) and its own progression rules. No feature is ranked by value, so if the schedule slips there is no planned order for cutting things.

**Source.**
- Scope, MVP: "Modes Solo, Arcade, VS en local et Zen", "5 niveaux avec leur thème".
- "Ce qui peut être coupé": only online multiplayer (which was already "optionnel") and the Optionnel rows of the events table.
- "Calendrier : 7 semaines, rédaction du GDD comprise."
- Agents Roles: an Agent Intro ("une séquence d'introduction différente pour chaque niveau") and an Agent Narratif ("progression du joueur en modes solo et arcade"). Both are funded in the budget (10 sessions each).
- "Documents liés": Menu et options, Leaderboard, Billes bonus/malus, Mode Solo, Mode Arcade, Mode VS, Mode Multi and Mode Zen are listed with no links. The specs for half of the shipped features don't exist yet.

**Recommendation direction.** Define a real MVP: one competitive mode (Arcade *or* local VS), Zen as the tutorial, 1-2 finished themes, and no per-level intros. Treat the other modes and themes as early access content updates. That is what early access exists for.

---

## Finding 2 — The document pitches at least three different games to three different audiences, and its own "wow" moment points away from its own fantasy
**Severity: MAJOR**

**Problem.** The vision pulls in three directions at once:
1. **A contemplative audiovisual journey for a very broad audience** (Tetris Effect lineage).
2. **A couch party game**.
3. **A demanding competitive versus game / score attack** (Puyo Puyo lineage, leaderboards, "challenge corsé").

These audiences want different things: low friction and no failure, social chaos, or mastery and fairness. The document never picks one to optimize for. The clearest sign is the declared "wow" moment. The Fantasy section says the "effet waw recherché" is the feeling of escape and accomplishment of the journey. The Agents section says the wow moment is "le challenge corsé que propose le NPC en mode arcade", which serves the competitive audience, not the broad contemplative one. A store page, a trailer and an early access community all need one primary hook. This document offers two hooks that contradict each other.

**Source.**
- "Public visé : très large, car le jeu ne demande pas une grande maîtrise des commandes".
- "Le mode solo et le VS en local (party game) sont mis en avant".
- "Le jeu peut aussi attirer les joueurs de Tetris et de Puyo Puyo".
- Fantasy: "une forma d'initiation à la persévérence et la comtemplation … c'est leffet waw recherché".
- Agents Roles: "Moment « wow » rendu possible par l'IA : le challenge corsé que propose le NPC en mode arcade".

**Recommendation direction.** Name one primary audience and one primary mode for the early access launch. Make the declared wow moment match the Fantasy, or rewrite the Fantasy.

---

## Finding 3 — The load-bearing fantasy is unfunded and undefined, while decorative features get dedicated agents
**Severity: MAJOR**

**Problem.** The title, the Tetris Effect comparison and the positioning ("environnement « wow »") all depend on themed audiovisual levels and on glass marbles that react to play. Yet:
- The themes document is entirely "À définir". All 5 rows are empty: theme, mood, music, difficulty.
- The artist is conditional ("Un ou une artiste **pourra**…"). Assets are placeholders.
- The budget says outright that Claude cannot produce textures, models or music, and that their cost "n'est pas compté ici".
- Meanwhile the events table marks the two things that most directly *are* the Tetris Effect promise as **Optionnel**, which means they will be cut first:
  - "Élimination ou attaque réussie → effet gratifiant dans l'environnement"
  - "Reflets lumineux dans les billes en verre"

  Under the document's own rules, the first cuts remove the product's identity. Per-level intro sequences and a narrative progression agent stay funded.

That priority order is backwards. Without the audiovisual layer, what's left is a mechanical remake of a 1997 puzzle game with placeholder art. That product cannot carry the "wow" positioning on a Steam store page.

**Source.**
- Themes_niveaux.md table (all cells "À définir").
- Scope: "Des placeholders seront utilisés pour les assets"; "Un ou une artiste pourra prendre en charge la partie graphique".
- Budget_tokens.md, "Limites de l'estimation": art and music come from the artist, placeholders or other tools, "dont le coût n'est pas compté ici".
- Événements visuels et sonores: the "Optionnel" rows listed above.
- Positioning: "Une réalisation modernisée, avec un environnement « wow »".

**Recommendation direction.** Promote environmental reactivity and the glass material to MVP. Commit an art and music budget and owner. Demote or cut Agent Intro and Agent Narratif.

---

## Finding 4 — Market positioning rests on the absence of competitors rather than evidence of demand, and the early access plan has no community mechanism
**Severity: MAJOR**

**Problem.**
- **The positioning argument.** The whole positioning case is "no game using this mechanic has shipped since 1997". A gap that has lasted almost 30 years can also mean there is little demand, and the document doesn't address that. There is no competitive analysis of the games actually fighting for the same players on Steam: Tetris Effect itself, Puyo Puyo Tetris, and the wider premium match-3 and versus-puzzle field. The document says it can attract those players but never says why they would switch.
- **Price.** No price point or price tier is given for a "premium" title.
- **Early access.** Early access is justified as the way to "recueillir les retours et créer une communauté". But the multiplayer features are local VS, which leaves no online trace and has no network effect, and online play is the first feature on the cut list. The leaderboard is the only remaining community hook, and its document is an unlinked title.
- **Content and post-launch.** Nothing is planned for early access cadence, content updates, or the conditions for leaving early access. Console and mobile ports are promised "après l'accès anticipé", with no criteria.

**Source.**
- "Positionnement : aucun jeu reposant sur ce type de mécanique n'est sorti depuis 1997".
- "Modèle économique : jeu premium, avec un accès anticipé … sert à recueillir les retours des joueurs et à créer une communauté".
- "le multijoueur en ligne est optionnel".
- "Documents liés: Leaderboard" (no link).
- "Console et mobile viendront après l'accès anticipé".

**Recommendation direction.**
- Add a short competitor table and a price hypothesis.
- Define what the early access version contains, what each update adds, and the leave-EA criteria.
- Either commit to online leaderboards and asynchronous features as the community engine, or drop the community claim.

---

## Finding 5 — The budget section measures the cheapest input precisely and leaves the real costs out, and future-platform work leaks into the PC MVP
**Severity: MINOR**

**Problem.**
- **Budget.** The only budget in the document is AI tokens (~$300-530), itemized per agent, per model and per session. It leaves out:
  - art and music, which are explicitly excluded;
  - the developer's own time;
  - the Steam listing fee and capsule art;
  - QA on real hardware;
  - any marketing for the early access launch;
  - online infrastructure if Multi is kept.

  The precision looks like financial planning, but it covers a minor cost line.
- **Platform leak.** Effort for platforms that are out of scope is already spread through the MVP:
  - The full mobile and console control schemes are specified.
  - "le jeu peut se jouer en local à deux sur toutes les plateformes" commits to two-player local play on mobile, which is an unusual and expensive design problem.
  - The events table gives the tactile diagonal-gesture event **MVP** priority, although mobile is not in the MVP.
  - Agent Multi appears in the production roster even though online play is out of the MVP.

**Source.**
- Budget_tokens.md: whole document, especially "Limites de l'estimation".
- Contrôles et onboarding: the console and mobile sections; "Note : le jeu peut se jouer en local à deux sur toutes les plateformes".
- Événements table: "Geste en diagonale (tactile) | … | MVP".
- Agents Roles: "Agent Multi".

**Recommendation direction.**
- Add a one-line total-cost-of-production view that includes non-token costs.
- Mark the mobile, console and Multi content as post-EA so it doesn't get scheduled into the 7 weeks.

---

## Round 2 — Cross-examination

### 1. CONFLICTS

**vs. Feasibility Lead F1 (proposed MVP = Solo + local VS + Zen, cut Arcade).** We agree the MVP must shrink, but I disagree with keeping local VS. Read alongside the other reviews, local VS is one of the most expensive modes to ship and one of the least valuable for an early access launch:
- Feasibility's own F4 and Adversarial QA F5 both show there is no two-player input scheme on PC. Two-player input is new engineering, not just cutting something.
- Systems F3 shows a VS match has no guaranteed end (no round timer, spawn rate constant within a match).
- QA F5 shows that draws in a best-of-1 can repeat forever.
- In business terms (my F4), couch VS leaves no online trace. It produces no leaderboard entries and no network effect, so it does nothing for the stated EA goal of "créer une communauté".

I am revising my own Round 1 recommendation as well (see Revisions). I now argue for **Solo + Zen** as the EA launch core, with VS as the first content update once input mapping and a round timer exist. Solo's open problems are scoring formula, bonus/malus and the retry rule (Systems F5, QA F4/F5). These are spec gaps that can be closed on paper. VS's open problems are engineering and termination problems.

**vs. Narrative Critic F1 and F3 (name all 5 themes with an arc, or define an opponent/framing layer).** The diagnosis is right: the voyage has no content. The fix as written adds scope to a plan that is already over capacity. Narrative's ask is "theme 5 resolves theme 1", which commits production to all five themes before launch. Production should go the other way: ship 1-2 finished themes in EA and release the rest of the voyage as updates. This also gives EA a content cadence, which my F4 found missing. On F3, I would take the cheap branch of Narrative's own fork: state that there are no characters and cut the Agent Narratif. Building an opponent and framing layer for 15 arcade opponents is the kind of decorative scope my F1/F3 warn against.

**vs. Systems Designer F1 (BLOCKING: no difficulty curve, runs longer than content).** I agree it is BLOCKING. I disagree with the implied fix of designing a curve for levels 6-15. The cheapest fix is a product decision, not a balancing task: **remove the 10- and 15-length runs from the MVP.** That one cut resolves part of Systems F1, all of QA F3, the structural mismatch in Narrative F3, and the "padded runs" point in Player Psychologist F5. Five levels, one curve, one intro count.

**vs. Player Psychologist F5 (no unlocks, cosmetics or meta-progression) and F1 (Zen tutorial is empty).** F5 lists missing long-term hooks. I would not add meta-progression in a 7-week MVP. In early access, the content updates are the retention hook, which is why my F4 asks for an update cadence. F1, though, overturns part of my own recommendation. I proposed "Zen as the tutorial" as if it were free. Psychologist F1 shows Zen has no tutorial content, and the counter-release rule (a 5-line of the counter's own colour does not release it) is counterintuitive enough to need teaching. A real tutorial is a cost line my MVP proposal left out.

### 2. CONNECTIONS

- **The declared "wow" is both off-audience and the riskiest feature (my F2 + Feasibility F3 + Psychologist F5).** I flagged that the Arcade AI "challenge corsé" contradicts the contemplative Fantasy. Feasibility shows the same feature has no spec and sits at the end of the dependency chain. Psychologist shows it is likely to drive away the "très large" audience. So the document has made its headline selling point the feature it is least likely to deliver, aimed at the audience it least wants. This makes the case for moving the wow back to the audiovisual layer much stronger.
- **The marketing asset depends on cuttable features (my F3 + Narrative F4/F5 + Psychologist F4 + Feasibility F2).** A Steam page for a Tetris Effect-style game sells on trailer footage of the world reacting to play. Narrative F5 and Psychologist F4 show that in VS/Arcade the crescendo fires when the player is *losing*. The environmental reward for skill is Optionnel (my F3). Feasibility F2 shows nobody is assigned to make the art or music. Put together, there is currently no in-game moment that would make good trailer footage, and no owner to build one.
- **The headline party mode is the least proven loop (my F2 + Systems F2/F3 + QA F2).** The document sells "VS en local (party game)". Systems F2 shows small attacks *help* the victim, Systems F3 shows matches can stall, and QA F2 shows a player can be soft-locked, unable to release but not yet dead. A party game needs short, decisive, readable rounds. This loop can produce long, stalled, confusing ones. The party-game audience pillar has no working mechanic behind it yet.
- **The only community engine has integrity problems (my F4 + QA F4/F5 + Systems F5).** I named the leaderboard as the last remaining community hook. QA shows its scores are luck-dependent (bonus spawns), exploitable (level retry), and polluted (first-game hints active in ranked Solo). Systems shows the score has no formula. The community plan rests on a feature that can't yet produce trustworthy numbers.
- **Human time is the real budget (my F5 + Feasibility F5 + Feasibility F1 Steam logistics).** I listed "the developer's own time" as one omitted cost among several. Feasibility puts a number on it: about 190 sessions, or 5-6 per working day to review and integrate. Feasibility also shows Steam's coming-soon and review periods eat into the same 7 weeks. Together these mean the calendar is unverifiable, not just optimistic.

### 3. REVISIONS

- **F1 — stays BLOCKING; recommendation revised.** I replace "one competitive mode (Arcade *or* local VS)" with **Solo + Zen at EA launch, 5-level runs only, 1-2 themes, a real counter/release tutorial, then VS, then Arcade as updates.** The reasons are the VS findings from Feasibility, QA and Systems summarized above.
- **F2 — stays MAJOR, strengthened.** Narrative F2 found the same two-wow contradiction independently from a tonal angle. Psychologist F5 adds the churn risk. Systems F2 also weakens my framing slightly: I credited the "demanding competitive" pillar as a real third audience, but its core loop is currently mechanically broken. The pull toward that audience is mostly aspiration, not design.
- **F3 — upgraded from MAJOR to BLOCKING.** Four reviewers (myself, Narrative, Psychologist, Feasibility) reached the same point independently: the product's only differentiator is undefined, unowned and first on the cut list. Without it the positioning in my F4 has nothing to stand on. I now see this as the same decision as F1: what gets cut must be the reverse of what the document currently cuts.
- **F4 — stays MAJOR, strengthened** by QA's leaderboard-integrity findings. I keep the "30-year gap may mean low demand" point, but I note it is a risk to test with a competitor table, not a proven flaw.
- **F5 — split.** The budget half is **upgraded to MAJOR**, merged with Feasibility F5: the missing human-hours estimate is what makes the 7-week claim untestable. I also adopt Feasibility's Haiku long-context pricing correction as further evidence the assumptions weren't checked against each other. The platform-leak half stays **MINOR**, confirmed by Feasibility F4 (mobile local VS).
