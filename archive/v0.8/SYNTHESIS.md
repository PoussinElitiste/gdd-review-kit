# Round 3 — Moderator Synthesis

Document reviewed: *The Marble Effect*, GDD v0.5. Source: the six review files in `reviews/` (Round 1 and Round 2). Reviewer shorthand: SD = systems-designer, NC = narrative-critic, PP = player-psychologist, FL = feasibility-lead, QA = adversarial-qa, BA = business-analyst. "F" numbers are the reviewers' own Round 1 finding numbers; C, X/N and R are their Round 2 conflicts, connections and revisions.

---

## 1. TOP 5 ISSUES (ranked by severity × confidence)

### 1. Arcade and VS have no forcing function: rounds may never end, and the specified AI cannot force a loss either
- **Flagged by:** systems-designer (F1), adversarial-qa (F1), player-psychologist (F5, reframed in Round 2 as R3). Round 2 connections that build on it: feasibility-lead, narrative-critic (Connection C), business-analyst.
- **Severity after Round 2:** BLOCKING (systems-designer, adversarial-qa). player-psychologist upgraded its reframed finding from MINOR to MAJOR.
- **Outcome: STRENGTHENED.** systems-designer and adversarial-qa reached the same arithmetic independently (about 50 net marbles needed to lose; 3 marbles sent per 100 points; every rank-1 clear removes 3). Systems-designer X1 and adversarial-qa N3 add that a 1-2 ply greedy AI probably cannot beat a defender who keeps clearing. player-psychologist conceded its "quick stomp" premise.
- **Problem:** The only pressure source in Arcade/VS is the opponent's attack, and defending is cheaper than attacking, so boards drift toward the 25-marble floor and the "tableau plein" loss may never be reached. Round length and ties are still "à définir" (adversarial-qa F1 also raises the simultaneous-overflow tie case).
- **Merged:** systems-designer F1 + adversarial-qa F1 + player-psychologist F5 (as reframed: attacks have no felt impact and rounds have no climax).
- **Downstream effects named by reviewers:** Agent Test's "peut gagner et perdre" check cannot pass (systems-designer X1, feasibility-lead Connection 1, adversarial-qa N2); the step-3 milestone has no achievable acceptance test (feasibility-lead); two "never cut" items (local VS, opponent 5) sit on a rule that does not terminate (business-analyst Connection).

### 2. The cut list is internally invalid: every step damages something marked "jamais coupé", and the stack is left untaught
- **Flagged by:** systems-designer (F4), narrative-critic (F1), player-psychologist (F1), feasibility-lead (F2), adversarial-qa (F4), business-analyst (F3).
- **Severity after Round 2:** split, and I am reporting the split. BLOCKING: adversarial-qa (upgraded from MAJOR), business-analyst (upgraded from MAJOR), narrative-critic (F1, levels 4-5 / no ending), player-psychologist (F1). MAJOR: systems-designer (F4, argued in C2 that nothing in the economy requires the stack), feasibility-lead (F2).
- **Outcome: STRENGTHENED.** Six reviewers reached the same chain independently, and Round 2 added that cutting opponent 4 saves almost no work because opponent 5 still needs stacking (feasibility-lead F2, confirmed by business-analyst).
- **Problem:** Cutting Zen removes the declared tutorial, cutting opponent 4 removes the only stack lesson, and cutting levels 4-5 removes the journey's ending (narrative-critic F1) and the stage of the never-cut opponent 5 (adversarial-qa C3). The onboarding only covers the first game (player-psychologist F1, adversarial-qa F4.4).
- **Merged:** the stack-teaching finding (player-psychologist F1, systems-designer F4), the levels 4-5 / no-ending finding (narrative-critic F1), the opponent-4 cut-cost finding (feasibility-lead F2, cut-list part) and the cut-list inversions (business-analyst F3, adversarial-qa F4).

### 3. There is no headless simulator or bot, so Agent Test cannot do its job and every tuning value has no plan behind it
- **Flagged by:** feasibility-lead (F3). Supported in Round 2 by systems-designer (X2), player-psychologist (X2), adversarial-qa (N2), business-analyst (connection with F5).
- **Severity after Round 2:** BLOCKING (feasibility-lead upgraded from MAJOR).
- **Outcome: STRENGTHENED.** feasibility-lead upgraded it after combining it with issue 1 (an unverifiable acceptance test). Four colleagues independently pointed to the simulator as the cheapest way to settle their rate disputes (systems-designer X2: F1, F2 and F5).
- **Problem:** Agent Test is "Indispensable" at step 1 but needs an automated player and fast simulation the document never builds. All marble weights, gauge sizes, refill rates and AI speeds are "à régler en playtest", with no playtest slot in the 7 weeks (feasibility-lead F3, player-psychologist X2). Per business-analyst, paid early access then becomes the playtest with no feedback channel.
- **Dependency chain stated by the board:** define the round-end rule (issue 1) and wrapped gravity (issue 5), then build the simulator, then tune the AI, then run Agent Test (systems-designer X2, feasibility-lead Connections 1-2, adversarial-qa N2).

### 4. The plan relies on MCP authoring of Unreal binary assets that has never been tested and has no fallback
- **Flagged by:** feasibility-lead (F1). Round 2 reinforcement from business-analyst (intros and skins depend on the least proven pipeline) and narrative-critic (Connection A).
- **Severity after Round 2:** BLOCKING.
- **Outcome: INTACT.** No reviewer challenged it, and feasibility-lead added a sub-point: undefined rules get silently resolved by the implementing agent. It is a single-lens finding, so confidence rests on feasibility-lead's reasoning plus the Round 2 dependencies other reviewers drew from it. Feasibility-lead also used it to reject the cost of narrative-critic's proposed fixes (new Level Sequences and Niagara assets).
- **Problem:** Five of nine MVP agents (NPC behaviour trees, Menu/UMG, Feedback/Niagara, Skin materials, Intro/Level Sequences) produce .uasset content, with no spike, session budget or fallback. feasibility-lead asks for a week-1 spike with one asset of each type created through MCP, and a stated fallback per agent.

### 5. The v0.5 cylindrical wrap leaves gravity, refill and attack placement undefined, and contradicts two board invariants
- **Flagged by:** systems-designer (F3), adversarial-qa (F2). Round 2 reinforcement from feasibility-lead (Connection 2), player-psychologist (X4).
- **Severity after Round 2:** MAJOR (both reviewers kept it there).
- **Outcome: INTACT, scope widened.** systems-designer widened it with adversarial-qa's two added contradictions (a column emptied during a cascade vs "la ligne médiane ne doit jamais avoir de case vide"; "toujours un move" vs the MVP "Move bloqué" event). adversarial-qa R3 says the fix is cheap (one paragraph) but must be made before step 1.
- **Problem:** "Chute", refill from "le haut" and received-marble placement ("en haut s'il y a de la place, sinon en bas") are all defined for a linear column. The "même situation, même résultat" determinism claim cannot hold until they are specified. It is a prerequisite for the simulator in issue 3 (systems-designer X2, adversarial-qa N2).

### Just outside the Top 5 (for the record, not ranked)
- **Solo leaderboard is degenerate** (systems-designer F2, adversarial-qa F3; MAJOR; STRENGTHENED: adversarial-qa showed the overshoot is unbounded and exploitable by stall-then-fire, per systems-designer's R2 revision). Fix direction is disputed, see Disagreement 4.
- **Audiovisual "wow" is unfunded and unscheduled** (business-analyst F1 BLOCKING, feasibility-lead F4 MAJOR; severity disagreement, see Disagreement 5).
- **Victory and defeat share identical feedback; combo, danger and milestone signals are not MVP** (player-psychologist F3 MAJOR; narrative-critic F5 upgraded from MINOR to MAJOR in Round 2).
- **Bonus/malus marbles are live in all modes but undesigned** (systems-designer F5 MAJOR; business-analyst F3 treats it as decorative creep; feasibility-lead F4).
- **Controls: three directional verbs on adjacent keys** (player-psychologist F4 MAJOR; feasibility-lead rates the engineering risk as low).
- **Token budget Haiku miscalculation and inert overspend lever** (feasibility-lead F5, adversarial-qa F5; MINOR; confirmed independently, about $0.90 per Haiku session and about $329 corrected total).

---

## 2. UNRESOLVED DISAGREEMENTS

### Disagreement 1 — What is wrong with VS, and which fix comes first
- **systems-designer (C1):** the failure is a long, flat match with no end, not a quick stomp. A handicap or catch-up rule would make a non-terminating mode less likely to terminate, so escalation (timer-triggered autofill, rising send ratio) must come first.
- **adversarial-qa (N1):** VS has both failure modes at once. Escalation fixes the stalemate but speeds up the stomp, and a handicap fixes the stomp but extends the stalemate. Both levers must be designed together.
- **player-psychologist (C1, R3):** concedes the "quick stomp" claim, but argues the failure is that attacks have no felt payoff and nothing resolves, which kills the party pitch. Handicaps and catch-up rules remain on its list.
- **Escalated decision:** What ends a VS/Arcade round, and are escalation and catch-up rules designed together or in sequence?

### Disagreement 2 — Which identity is the core, and whether local VS stays "never cut"
- **business-analyst (F2, R2):** the document protects three identities at once. Recommends Solo plus Arcade as the core, demoting local VS from "never cut" and not marketing it as a party game until it has rules. The cut list should not protect both emotional peaks.
- **feasibility-lead (C1):** protect Solo-first, with VS as a thin add-on reusing the Solo board and an autofill timer. A competitive-first pivot puts the riskiest system (an AI that can stack) at the centre.
- **player-psychologist (C4):** the protected core should be Solo with a teaching level, since competitive modes are where teaching is weakest.
- **narrative-critic (C1):** the journey should not go; its emotional register is mis-specified. Rebuild it around perseverance, move contemplation to Zen, and keep every mode.
- **Escalated decision:** Choose the core identity (and one funded emotional peak), and decide whether local VS keeps its "jamais coupé" status.

### Disagreement 3 — Levels 4-5, intros and narrative fallbacks
- **narrative-critic (F1, C1, Revisions):** a 3-level journey ends on "premier vrai mur" with no ending. It needs a planned narrative fallback; intros may be cut only if a near-zero-cost fallback (such as a title card) exists; the closing beat should rank above intros 2-4; opponents need at least a name and realm.
- **business-analyst (C1):** design the 3-level version as the shipping arc (re-stage Les Sables as the finale) and treat levels 4-5 as early-access updates, since they are the most expensive and least funded content. Opponent framing is new scope unless the competitive identity is chosen.
- **feasibility-lead (C2, F4 revision):** cutting levels 4-5 saves almost no engineering. Cut bespoke per-level music and intro sequences first; any narrative fallback must be text, UI cards or audio until the MCP spike passes.
- **adversarial-qa (C3):** cutting level 5 also strips the stage from the never-cut opponent 5, so the list is invalid either way.
- **Escalated decision:** Is the shipping arc 3 levels or 5, and what, if anything, replaces the ending, opponent framing and intros if levels or intros are cut?

### Disagreement 4 — How to fix the Solo leaderboard (or whether to keep it)
- **systems-designer (F2, R2):** score on time-to-clear, points per marble placed, or board height at completion, or keep the level running past the gauge; also cap overshoot or score per marble so stall-and-dump is not optimal.
- **player-psychologist (C2, C3):** opposes time-to-clear because it turns the calm journey into a race; prefers points per marble or running past the gauge. Argues stall-then-fire is the one place the core stack skill is rewarded, so teaching should be fixed before the incentive is judged an exploit.
- **adversarial-qa (C2, N6):** board height rewards stalling, "points per marble placed" has no referent in Solo, and a run-past-the-gauge fix only works if autofill keeps accelerating. Time-to-clear is the only fix that resists stalling but contradicts "sans limite de temps". The score formula must be decoupled from completion before the Level 1 and Level 5 pacing fixes.
- **feasibility-lead (Connection 3):** a local leaderboard with a time-based score.
- **business-analyst (Connection):** remove the leaderboard from MVP and from the Solo pitch outright.
- **Escalated decision:** Keep the leaderboard with a chosen score formula (and whether time enters it), or remove it from MVP.

### Disagreement 5 — Severity of the unfunded audiovisual identity
- **business-analyst (F1, R2):** BLOCKING. A premium paid early-access game whose stated differentiator ships as placeholders cannot be sold under the document's own positioning; this blocks the business case.
- **feasibility-lead (F4, R2):** MAJOR. A build with placeholders still ships; the music line should be narrowed to library tracks. feasibility-lead does not contest business-analyst's severity from the market lens.
- **Escalated decision:** Fund or source the art and music, or move the positioning away from "voyage audiovisuel" and "environnement wow".

### Disagreement 6 — Which feedback signals move into MVP
- **player-psychologist (F3, R2):** combo escalation, "Approche de la défaite" and "Jalon atteint" should be MVP, alongside victory vs defeat (it also holds MAJOR against narrative-critic's original MINOR; narrative-critic has since upgraded to MAJOR).
- **feasibility-lead (C3):** promote them only in cheap forms (audio sting plus UI multiplier label, a board tint, a single layer swap), with no new Niagara systems. Victory vs defeat is the exception and should simply be done.
- **Escalated decision:** Accept the cheap-form promotion, or hold the Important/MVP line as written.

(Resolved in Round 2 and therefore not listed above: the "quick stomp" claim was withdrawn by player-psychologist; the Narrative Critic/Player Psychologist severity gap on victory vs defeat was closed when narrative-critic upgraded F5; systems-designer's "forever-rotating full columns" point was rebutted by adversarial-qa C5 as event-based, not a real fail state.)

---

## 3. QUICK WINS (up to 3)

1. **Give victory and defeat distinct feedback.** feasibility-lead (C3): "a near-zero-cost asset swap and should simply be done." player-psychologist (R2) names it a strong quick-win candidate, and narrative-critic (F5, Revisions) calls for a distinct signature and a closing beat.
2. **Remove opponent 4 from the cut list.** narrative-critic (Connection E) records that four lenses converge on this; feasibility-lead (F2) shows the cut saves almost no work because opponent 5 still needs stacking; business-analyst (Connection) calls it a cut that buys no schedule.
3. **Write one paragraph defining "Chute", refill and received-marble placement on a wrapped column.** adversarial-qa (R3) says a designer can define it in one paragraph, before step 1; feasibility-lead (new sub-point under F1) asks for a "rule freeze" check so agents do not decide it silently.

---

## 4. ONE PARAGRAPH VERDICT

The document is not ready to drive production. Several issues are BLOCKING after Round 2: the round-end rule for Arcade and VS (systems-designer, adversarial-qa), a cut list with no valid cut sequence (adversarial-qa, business-analyst, narrative-critic, player-psychologist), the missing simulator and test bot (feasibility-lead), and the untested MCP asset pipeline (feasibility-lead); business-analyst also holds the unfunded audiovisual identity at BLOCKING. The single change that matters most is to design a termination and escalation rule for Arcade and VS rounds, such as a round timer that turns on autofill, a rising send ratio, or another escalation the designer chooses. systems-designer, adversarial-qa and narrative-critic all independently name escalation as the fix, and the board's dependency chain runs through it: feasibility-lead and adversarial-qa show that the simulator, the AI tuning and Agent Test's "peut gagner et perdre" check cannot work until rounds can end, and two of the "jamais coupé" commitments (local VS and opponent 5) rest on it. How that rule interacts with handicaps (Disagreement 1) and with the choice of core identity (Disagreement 2) remains the designer's call.
