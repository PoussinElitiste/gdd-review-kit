# Review Board — Moderator Synthesis

Document reviewed: `gdd.txt` — "The marbles effect"
Board: systems-designer, narrative-critic, player-psychologist, feasibility-lead, adversarial-qa, business-analyst
Inputs: Round 1 (30 findings, 5 per reviewer) and Round 2 cross-examination in `reviews/*.md`.
Moderator note: everything below traces to the review files; no new critique has been added.

---

## 1. TOP 5 ISSUES (severity × confidence)

### #1 — The required AI "wow" moment, technical strategy, Agents Roles and Token Cost budget are all missing
- **Problem:** The document's own brief (lines 3-6) requires the "Un agent, un effet wow" moment and a Technical Strategy built around it; no AI moment is named, there is no "Stratégie technique" section, and "Agents Roles" / "Token Cost budget" are empty headers.
- **Flagged by:** all six — narrative-critic F1, feasibility-lead F1, adversarial-qa F1, business-analyst F1 (BLOCKING); systems-designer F5, player-psychologist 4 (MAJOR).
- **Cross-examination:** **STRENGTHENED.** The issue's existence is unanimous. Its scope widened: adversarial-qa X1 showed that free, unlimited moves plus a runtime agent plus an empty token budget means uncapped spend; feasibility-lead C2 showed that runtime vs. dev-time agent produces two different codebases. Severity remains split 4 BLOCKING / 2 MAJOR (see Disagreement A).

### #2 — No session goal, outcome or board-resolution rules: the loop stops after "act"
- **Problem:** Nothing defines what a match produces (removal, refill/gravity, cascades), what wins or ends a level, scoring, timers, move limits, or deadlock handling.
- **Flagged by:** systems-designer F1, player-psychologist 2, adversarial-qa F3 (all BLOCKING); business-analyst F4 and feasibility-lead F3 (as missing structure/rules).
- **Cross-examination:** **STRENGTHENED, reframed.** systems-designer (Connection A) names it the root issue that the tone, mode and exploit findings all trace back to. player-psychologist (R2) and adversarial-qa (F3 revision) narrowed it: the blocking gap is "no defined session goal" and "board resolution", not "no fail state" — a deliberate no-fail zen mode would satisfy it. business-analyst tied the fail-state choice to the business model.

### #3 — The core move rule contradicts itself ("slide a column horizontally") and leaves edges and matching undefined
- **Problem:** Line 18 has a *column* sliding both vertically and horizontally, never states wrap/push/blocked edges, step vs. free movement, or undo, and counts only horizontal matches without saying why.
- **Flagged by:** systems-designer F2, player-psychologist 1, adversarial-qa F2 (BLOCKING); adversarial-qa F4 (horizontal-only, MAJOR); feasibility-lead F3 (MAJOR); business-analyst F5 (pitch clarity, MINOR).
- **Cross-examination:** **STRENGTHENED.** New dimensions added: horizontal-only matching halves the matchable lines and raises deadlock frequency (systems-designer B); vertical lines that don't fire will read as bugs to match-3 veterans (player-psychologist X3); diagonal swipes on touch will mis-slide (adversarial-qa X4); deadlock/solvable-board generation is real implementation work (feasibility-lead X5). Severity split remains (see Disagreement B).

### #4 — The product shape is undecided: no game mode, audience, platform, business model or scope
- **Problem:** The Scope section is empty (no team, timeline, platform, engine, level count), and the document never chooses between the products its inspirations and mood imply — a versus puzzler (Lose Your Marbles), a synesthetic journey (Tetris Effect), or a calm ASMR relaxer.
- **Flagged by:** feasibility-lead F2 (BLOCKING); business-analyst F2 (MAJOR → BLOCKING in Round 2) and F3; player-psychologist 3; narrative-critic 4 (tone side of the same split).
- **Cross-examination:** **STRENGTHENED.** business-analyst upgraded F2 to BLOCKING because three colleagues' findings depend on it; feasibility-lead X1 added possible online netcode to the scope range; every reviewer independently concluded in Round 2 that the mode / pressure-source decision must come first (systems-designer Connection A, player-psychologist X4, feasibility-lead X1, adversarial-qa X2, business-analyst on Narrative F5, narrative-critic C4).

### #5 — Colour is the only match key, yet marbles are re-themed every level under glass reflections and VFX
- **Problem:** Matching depends on colour alone, the art direction adds reflections and environmental effects, there is no colourblind fallback, and line 24 re-skins the marbles themselves at each theme — so players must relearn the match key at every transition.
- **Flagged by:** adversarial-qa F5 (MAJOR); player-psychologist 5 (MINOR → MAJOR); connected in Round 2 by systems-designer C, narrative-critic X1, feasibility-lead X3, business-analyst (accessibility as platform cost).
- **Cross-examination:** **STRENGTHENED.** Started as a MINOR accessibility note; Round 2 turned it into a board-wide connection: an uncontrolled difficulty spike at every theme boundary (systems-designer), a correctness problem rather than an add-on (adversarial-qa C4), and a shader constraint that must exist from the first marble prototype or every themed set gets redone (feasibility-lead X3).

---

## 2. UNRESOLVED DISAGREEMENTS

### A. The AI moment: BLOCKING, and does it come before or after the core loop?
- **Agent first / BLOCKING** — feasibility-lead, business-analyst, adversarial-qa, narrative-critic: the brief makes it the acceptance criterion "avant de développer quoi que ce soit d'autre"; whether the agent is runtime or dev-time decides architecture, latency budget and cost before any code exists (feasibility-lead C2); without it there is no product case in the most crowded casual genre (business-analyst).
- **Loop first / MAJOR** — systems-designer, player-psychologist: every plausible AI hook needs a working loop to attach to (a difficulty director needs a difficulty variable; reactive music needs combo events), so an agent designed first can only decorate (systems-designer); players quit over an unreadable verb and missing goal before any AI moment appears (player-psychologist C1).
- **Escalated decision:** Does the team define the core loop first and fit the agent to it, or define the agent first as the brief demands? Related sub-dispute: feasibility-lead (C3) rejects player-psychologist's example ("music or visuals that react to how the player plays") because it needs no AI agent.

### B. The core-mechanic ambiguity: BLOCKING or MAJOR?
- **BLOCKING** — systems-designer, adversarial-qa, player-psychologist: it blocks the first prototype and sets the reachable state space, deadlock rate and need for a move limit; "how cheap a fix is has nothing to do with whether something blocks" (adversarial-qa C1).
- **MAJOR** — feasibility-lead: it is a decision a lead can settle in an afternoon; if everything is BLOCKING the board can no longer separate cheap gaps from expensive ones. Feasibility agrees it is first on the critical path.
- **Escalated decision:** Should severity measure *whether work is blocked* or *how costly the gap is to close*? Both sides agree the rule must be fixed before any other work.

### C. What does the breaking glass (line 28) mean, and who decides?
- **narrative-critic:** completing "Bruit de verre cassé lors" is a creative-direction decision; reading it by default as "matches shatter the marbles" settles the tone by accident, since the reward becomes destroying the objects the document presents as precious.
- **player-psychologist:** shatter-as-reward is the psychologically safe reading (satisfying-shatter is an ASMR subgenre); the real risk is sound fatigue over long sessions, fixed by sound design.
- **adversarial-qa:** it is a mechanical gap — the only line suggesting matched marbles leave the board — and the "failure sound" reading assumes a fail state that does not exist.
- **feasibility-lead:** the event table is a MINOR engineering task, but shatter-on-every-match is the frame-rate worst case at the reward moment and needs many audio variations.
- **Escalated decision:** Does glass breaking signal reward, failure, or nothing — and is it decided by creative direction, by the board-resolution rules, or by the performance budget?

---

## 3. QUICK WINS

1. **Rewrite line 18 with a diagram:** row vs. column, edge behaviour (wrap / push / blocked), step vs. free slide, undo, and an explicit statement on vertical matches. (player-psychologist 1 ask; feasibility-lead C1 "an afternoon with a whiteboard"; systems-designer "the board's cheapest quick win"; adversarial-qa F2/F4.)
2. **Finish the sentence "Bruit de verre cassé lors"** and add an event table: event → sound → visual → intensity. (systems-designer F4; feasibility-lead F5; flagged by all six reviewers.) The meaning remains escalated in Disagreement C.
3. **Give every marble colour a second cue** (symbol or pattern) that stays fixed across all themes and is built into the marble material from the first shader prototype. (player-psychologist 5; narrative-critic X1; feasibility-lead X3.) Fix the spelling "loose your marble" → "Lose Your Marbles" in the same pass (narrative-critic F5).

---

## 4. VERDICT

This document is not ready to drive production. Every reviewer found it unbuildable as written: the AI moment its own brief requires is absent, the only mechanical sentence contradicts itself, there is no session goal or board-resolution rule, Scope is empty, and the most expensive commitment (per-level themed art and audio) has no budget or cut list. The board disagrees on whether the AI moment or the core loop must be designed first, but in Round 2 all six reviewers independently converged on one upstream decision: choose the game mode and its pressure source — solo zen, solo rising tension, or versus — and write it as the first line of Scope. That single change settles the tone conflict, sets the fail-state and business model, bounds the scope (including whether netcode exists), and gives both the loop and any AI agent something to be designed around.
