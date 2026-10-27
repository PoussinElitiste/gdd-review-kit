# Round 3 — Moderator Synthesis

Document reviewed: *The Marble Effect*, GDD v0.2 (22 Oct 2026). Every claim below traces to the six review files. Finding numbers are the reviewers' own: SD = systems-designer, NC = narrative-critic, PP = player-psychologist, FL = feasibility-lead (F1-F5), QA = adversarial-qa, BA = business-analyst. Severities are as each reviewer last stated them after Round 2.

---

## 1. TOP 5 ISSUES (ranked by severity x confidence)

### 1. The competitive loop does not close: attacks cannot produce the loss condition, and "a round always ends" is unproven

- **Problem:** "Plateau bloqué" requires all 5 columns at 15 balls (75/75), attack balls are never placed in a column touching L1 (they are "perdue"), so a defender can push every column to L1 and become immune, and no document says where a spawned ball goes when its column touches L1.
- **Flagged by:** systems-designer (#1), adversarial-qa (#1, derived independently). Confirmed as a dependency chain by feasibility-lead (Round 2 C1) and as a business consequence by business-analyst (Round 2 Connections). narrative-critic accepts the diagnosis (Round 2 C3).
- **Severity:** BLOCKING (systems-designer #1 and adversarial-qa #1 both).
- **Outcome: STRENGTHENED.** The two independent derivations agree on the 75/75 result and the L1 immunity. systems-designer added a two-sided stall equilibrium (two turtling players can neither attack nor be blocked), and feasibility-lead showed that gauge targets, spawn rates, all five opponents and the Agent Test criteria depend on this rule. player-psychologist conceded its own finding #4 against it.
- **Merged:** systems-designer #1 with adversarial-qa #1, plus the Round 2 connection "Arcade opponents may never attack at all" (systems-designer + adversarial-qa #5).

### 2. The release guarantee lapses under pressure, so Solo can become unwinnable with time on the clock, and the gauge targets are unvalidated

- **Problem:** The restore rule only places useful colors in columns of 7 balls or fewer. Once all columns exceed 7 (normal at 0.5 balls/s), a release becomes "souvent impossible à partir de 11 billes par colonne, et toujours à partir de 13" (adversarial-qa #2). In Solo the gauge fills only through releases, the gauge targets (10 to 30 units) have no throughput model (systems-designer #2), and a novice cannot read or reach the release (player-psychologist #2). No feedback says "release is no longer possible".
- **Flagged by:** adversarial-qa (#2), systems-designer (#2), player-psychologist (#2). Reinforced by narrative-critic (Round 2 X2: the finale can soft-lock).
- **Severity:** BLOCKING. All three upgraded MAJOR to BLOCKING in Round 2: adversarial-qa #2, systems-designer #2, player-psychologist #2.
- **Outcome: STRENGTHENED.** Each reviewer's MAJOR finding became BLOCKING once combined with the others. Solo is "never cut" and its only win path is the gauge.
- **Merged:** adversarial-qa #2, systems-designer #2, player-psychologist #2. player-psychologist #3 ("sans pression" level 1, MAJOR) is partly a consequence of this and is not separately counted here.

### 3. The cut list contradicts the level/opponent mapping and the teaching ladder, and the counter loop is never taught

- **Problem:** Levels 4 and 5 are cut before opponent 5, which is "never cut" but is tied to level 5 (no level, music or spawn tier). Zen (the stated tutorial) and opponent 4 (the only stack teacher) are cut early. Onboarding highlights only 3-alignments in the first game and teaches nothing of the counter, release or colour-flip rules. A cut Solo ends on its "wall" (Les Sables).
- **Flagged by:** systems-designer (#3), narrative-critic (#1), player-psychologist (#1, #5), feasibility-lead (F2), adversarial-qa (#3), business-analyst (#4). All six.
- **Severity: contested.** narrative-critic #1 and player-psychologist #1 hold BLOCKING. systems-designer #3, feasibility-lead F2, adversarial-qa #3 and business-analyst #4 hold MAJOR. adversarial-qa argues explicitly that this is fixable with one paragraph and should not be BLOCKING (Round 2). I report the split rather than choosing.
- **Outcome: STRENGTHENED in confidence, severity unchanged.** business-analyst calls it the strongest consensus on the board (four independent reviewers). It grew in scope: feasibility-lead notes the production order also builds opponent 5 before opponent 4, and adversarial-qa shows protecting opponent 4 is not enough because the AI cannot stack (see issue 5).
- **Merged:** the cut-list findings of all six reviewers with player-psychologist #1 (onboarding), because systems-designer #3 and business-analyst #4 treat teaching and cut order as one problem.

### 4. The 7-week, one-person scope has no milestones, no dated cut triggers and unscoped work

- **Problem:** About 190 agent sessions (about 32 a week), all reviewed and playtested by one person, with no dated milestones, so the cut list has no trigger. The Steam Early Access lead times, bonus/malus balls, the leaderboard and the per-mode documents ("à rédiger") are in neither the production order nor the cut list.
- **Flagged by:** feasibility-lead (F1, F2), business-analyst (#2). Supported by adversarial-qa (#4) and systems-designer (#5) on bonus/malus.
- **Severity:** BLOCKING (feasibility-lead F1; business-analyst #2, upgraded from MAJOR in Round 2).
- **Outcome: STRENGTHENED.** feasibility-lead added the rules-freeze milestone (Round 2 C1), and business-analyst upgraded #2 to BLOCKING using feasibility-lead's arithmetic and asked the moderator to merge the two.
- **Merged:** feasibility-lead F1 with business-analyst #2 (per business-analyst's own request).

### 5. No shared headless rules simulator exists as a deliverable, and the 1-2 action AI cannot perform the opponent behaviours

- **Problem:** The plan relies on "Une simulation des règles a montré..." but no one is scoped to build a deterministic simulator shared with the game. Agent Test criteria ("peut être terminé", "peut gagner et perdre") are existence checks that pass despite the defects above. A 1-2 action lookahead cannot stack or release on purpose, so opponents 4 and 5 cannot show their advertised behaviour.
- **Flagged by:** feasibility-lead (F4), adversarial-qa (#5), systems-designer (#2 note). Connected by narrative-critic (Round 2 X1) and player-psychologist (Round 2 Connection 2, 5).
- **Severity: split.** feasibility-lead F4 is BLOCKING (upgraded in Round 2). adversarial-qa #5 stays MAJOR.
- **Outcome: STRENGTHENED.** systems-designer, feasibility-lead and adversarial-qa independently describe one missing artifact (a rules-exact simulator driving a planning agent) behind three findings. It also breaks the "teacher" premise in narrative-critic's X1 and player-psychologist's Connection 2.
- **Merged:** feasibility-lead F4 with adversarial-qa #5 and the simulator note in systems-designer #2.

---

## 2. UNRESOLVED DISAGREEMENTS

### D1. How to fix the loss condition

- **systems-designer (#1):** change the loss condition, for example any column reaching L1 with no move, or a height threshold, or change the attack placement rule. Its Round 2 adds that both the attack rule and the release guarantee must change together.
- **adversarial-qa (Round 2):** a height or L1 trigger lets players lose by choice (reaching L1 is a legal move, Schemas_deplacement 4b) and the AI will do it unless penalised. It proposes "no legal placement for an incoming ball" as the loss event.
- **narrative-critic (C3):** any harsher global loss rule makes La Source's "sans pression" less true, so the change should apply to VS and Arcade only, or Solo's early levels need a looser rule.
- **Escalated decision:** which loss rule replaces the 75/75 block, whether it applies globally or only to competitive modes, and where spawned and attack balls go at L1.

### D2. The audiovisual "wow": severity and the launch pitch

- **business-analyst (#1):** BLOCKING. The plan sells a "Tetris Effect"-style audiovisual journey that it does not build, and per-level music is rated MVP with no source. Fix: fund an art and music workstream, or reposition early access as mechanics-first.
- **feasibility-lead (F3):** MAJOR. The core loop can be built with placeholders, so production is not blocked. It accepts a mid-schedule deadline because of the Steam store page.
- **systems-designer (Round 2):** the "mechanics-first" fallback only works if the systems blockers (issues 1 and 2) are fixed first.
- **narrative-critic (C2):** repositioning would also require rewriting the Fantasy, arc and "contemplation" claims.
- **Escalated decision:** fund the art and music workstream, or reposition. Either way, name an owner and a date.

### D3. What the cut list protects, and how the tutorial and finale are kept

- **feasibility-lead (Round 2):** cut level 4 alone first and keep level 5, so opponent 5, the Solo finale and the arc ending share assets. Protect the tutorial content (a guided Solo-based sequence), not Zen as written. Cut the per-level intros.
- **business-analyst (Round 2):** a single compressed finale level hosting the Solo ending and opponent 5. Protect a minimal Zen/tutorial and opponent 4, or fold the stack lesson into Solo. Fund distinct win/loss beats by removing four of the five intros.
- **narrative-critic (C1, Finding 1):** protect the arc's ending. Replace the cinematic intros with a cheap narrative spine (one title card per stage and a distinct ending beat) rather than cutting all narrative delivery.
- **player-psychologist (Round 2):** fold the counter, release and colour-flip lessons into Solo level 1 as contextual prompts with an untimed or extended first attempt.
- **systems-designer (Round 2):** the table needs a cut-scenario difficulty table, because opponent 5 would lose its spawn-rate tier. **adversarial-qa (Round 2):** protecting opponent 4 achieves nothing unless the AI spec changes.
- **Escalated decision:** one agreed cut order, which level hosts opponent 5, and whether the stack lesson lives in opponent 4, in Solo, or in a tutorial.

### D4. Where calm and contemplation belong

- **narrative-critic (Finding 2):** level 1 should be genuinely pressure-free and La Nébuleuse's "calme immense au départ" should be real. Calm moments should be named in the structure.
- **player-psychologist (Round 2):** calm at the opening of the fastest-spawning level is a trap. Contemplation belongs between levels and after victory, with "no pressure" confined to an untimed level 1.
- **Escalated decision:** where the contemplative beats go (in-level opening, interludes, or an untimed level 1).

### D5. Primary audience, and which player-experience findings to rank

- **business-analyst (Finding 3, Round 2):** pick a primary early-access audience (solo score-chasers on Steam). The board should not rank player-experience findings until it is chosen. Local VS should not be "jamais coupé" on a PC launch.
- **player-psychologist (Round 2):** the document's onboarding is weakest for that audience. A score-chasing target makes the missing score formula and leaderboard (no reason to come back, #5) a launch blocker, and local VS is where many new players meet the game, so the first-session path must be designed for whichever audience is chosen.
- **feasibility-lead (Round 2):** online play is not buildable in this plan, so solo score-chasers is the only feasible pick, which makes the leaderboard a scheduled deliverable needing a score formula (systems-designer #5).
- **Escalated decision:** the primary audience, and whether local VS keeps its "never cut" status.

---

## 3. QUICK WINS

1. **Give victory and defeat distinct audiovisual beats, and promote them (and the approach-of-defeat cue) to MVP.** narrative-critic (Round 2 Revisions) narrows its ask to this plus one voyage-completion text card. player-psychologist (Connection 4) asks the same, and feasibility-lead calls these among the cheapest requests (parameter changes to VFX and audio).
2. **Move bonus/malus balls out of the MVP and onto the cut list.** systems-designer (Round 2 Conflicts and Revisions) recommends cutting rather than specifying now. adversarial-qa (#4 fix, second option) and feasibility-lead (Connection C3) agree. business-analyst (#2 fix) also asks for them to be added to the cut list.
3. **Reorder the cut list so level 4 is cut alone first and level 5 is kept.** feasibility-lead (Round 2 Conflicts) says this one edit resolves narrative-critic #1, feasibility-lead F2 and adversarial-qa #3 with no new work.

---

## 4. ONE PARAGRAPH VERDICT

The document is not ready to drive production. Two BLOCKING rule defects sit in the "never cut" core: the attack loop cannot reliably produce the loss condition, so VS and Arcade may never end (systems-designer #1, adversarial-qa #1), and the release guarantee can lapse so that Solo becomes unwinnable with no warning (adversarial-qa #2, systems-designer #2, player-psychologist #2). Around them, the schedule has no milestones or cut triggers (feasibility-lead F1, business-analyst #2), the cut list contradicts itself (flagged by all six reviewers), and nothing in the plan can detect these defects because the shared rules simulator is not scoped (feasibility-lead F4). The single change that matters most is to build and schedule the rules-exact headless simulator as the first milestone, freeze the end-of-round and release-guarantee rules against it before any tuning, and let that rules-freeze gate every later step. feasibility-lead (Round 2 C1, C2) shows the simulator is the one artifact behind the broken loss rule, the soft-lock, the unvalidated gauge values and the AI's inability to plan, and systems-designer (Round 2 Connections) reaches the same conclusion independently.
