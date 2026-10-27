# Review Board — Moderator Synthesis (v0.4)

Document reviewed: `gdd.txt` — "The Marble Effect", GDD v0.2 (fourth revision) plus linked documents (Combo system, Schémas du déplacement, Événements visuels et sonores, Thèmes des niveaux, Budget tokens)
Board: systems-designer, narrative-critic, player-psychologist, feasibility-lead, adversarial-qa, business-analyst
Inputs: Round 1 (30 findings) and Round 2 cross-examination in `reviews/*.md`. Earlier reviews are archived in `archive/v0.1/`, `archive/v0.2/` and `archive/v0.3/` (Round 1 only).
Moderator note: everything below traces to the review files; no new critique has been added.

---

## 1. TOP 5 ISSUES (severity × confidence)

### #1 — Matches have no reliable end: the only terminal rule fires too late and possibly too early
- **Problem:** "Plateau bloqué" needs all 75 cells full, so a player who can no longer release (long columns hide the guaranteed colour from the median) can neither attack, counter, nor lose; VS has no round timer, spawn rate is fixed within a match, and drawn rounds have no rule in a best-of. Under a no-wrap match rule, a single switch on a "blocked" board could still clear a line, yet the GDD declares game over "même si un switch reste possible".
- **Flagged by:** systems-designer F3 (MAJOR → BLOCKING), adversarial-qa F2 (MAJOR → BLOCKING), player-psychologist F3 (reframed as a "zombie state"), narrative-critic X2 (four end states, none authored), feasibility-lead N6 (automated tests have no guaranteed end).
- **Cross-examination:** **STRENGTHENED.** Systems proved a block requires 75/75; QA showed the soft-lock and the switch case and concluded the rule "is wrong in both directions"; the psychologist withdrew "game over feels unfair" and replaced it with the stalled, unloseable match. Systems' minimum fix: a round timer with an in-round spawn ramp, and draws that replay with a cap.

### #2 — The board is not deterministic: resolution order and the match rule are undefined, and release frequency is unmodelled
- **Problem:** After an elimination, the order of fall, top refill, column auto-rise and chain check is never stated (one order spawns five random marbles onto the median); whether matches wrap from c5 to c1 or must be contiguous is undefined; and nobody has estimated how often a non-counter 5-line can be built, though every payoff (gauge, attack, counter) depends on it.
- **Flagged by:** adversarial-qa F1 (BLOCKING), systems-designer F4 (MAJOR, "a precondition, not a parallel task"), feasibility-lead N1/N2, player-psychologist X3.
- **Cross-examination:** **STRENGTHENED.** Feasibility called the QA/AI combination "the most important item": a planning AI, leaderboard integrity and any future online play all need a fixed order. Systems linked the refill-first order to random 5-lines and QA to random releases ("RNG decides attacks"). QA widened its finding to include the wrap rule.

### #3 — The MVP is still the whole game for one person in 7 weeks, and human review time is uncounted
- **Problem:** The MVP keeps four modes, five themed levels, an arcade AI, a leaderboard, bonus/malus marbles and per-level intros; the cut list removes only already-optional online play and two Optionnel effects; specs for half the features (bonus/malus, leaderboard, modes) do not exist; Steam release logistics and about 5-6 agent sessions per day of human review are not planned.
- **Flagged by:** feasibility-lead F1 (BLOCKING) and F5 (MINOR → MAJOR for human time), business-analyst F1 (BLOCKING) and F5 (budget half MINOR → MAJOR).
- **Cross-examination:** **STRENGTHENED.** Feasibility noted that the rule work found by systems and QA eats into the same 7 weeks. Both reviewers agree the MVP must shrink but disagree on which mode survives (see Disagreement A).

### #4 — The game's identity (the audiovisual "voyage") is undefined, unowned and first on the cut list
- **Problem:** The theme table is "À définir" in every cell and the arc is deferred; agents are said to "generate" models, skins and VFX that the budget admits they cannot make; no artist or composer is committed while per-level music is MVP; the two effects closest to the Tetris Effect promise (environment reacting to play, glass reflections) are the Optionnel rows cut first; and the declared "wow" is the arcade AI, not the voyage.
- **Flagged by:** narrative-critic F1 (BLOCKING) and F2, business-analyst F3 (MAJOR → BLOCKING) and F2, feasibility-lead F2 (MAJOR), player-psychologist F4/F5.
- **Cross-examination:** **STRENGTHENED.** Business raised F3 to BLOCKING because four reviewers reached it independently; three lenses agree on the two contradictory "wow" moments. Narrative-critic clarified F1 as a writing task to finish before scope cuts; feasibility-lead noted that the arc must be carried by what the pipeline can produce (music layers, colour, pacing, difficulty order).

### #5 — Onboarding teaches only the 3-match; the hardest event in the game is never taught
- **Problem:** The only aid is a 5-second idle hint in the player's first game; it never covers the counter colour, the counter-intuitive release rule (a same-colour 5-line does not release), stacking, combos or wave cancelling; Zen is labelled a tutorial with no tutorial content; and the hints leak into ranked Solo and one side of local VS.
- **Flagged by:** player-psychologist F1 (BLOCKING), adversarial-qa F5.2; business-analyst concedes in Round 2 that a real tutorial is a missing cost line.
- **Cross-examination:** **STRENGTHENED, with the fix contested.** The psychologist narrowed the fix to a separate, unscored tutorial with assists off in ranked Solo and VS. Systems-designer argues it is mainly a rate problem: a 5-line cannot be built incrementally on the median, so the fix is to make release attainable or give the player an explicit release action (see Disagreement D).

---

## 2. UNRESOLVED DISAGREEMENTS

### A. Which modes ship at Early Access launch?
- **VS + Zen (+ Solo), Arcade later** — feasibility-lead: local VS needs only an input map and a match-length backstop, while Arcade carries the only unspecified AI; adversarial-qa sides with this.
- **Solo + Zen, then VS, then Arcade** — business-analyst (revised R1): VS has no two-player input, no guaranteed end and repeating draws, and leaves no online trace for the community goal.
- **Cutting modes does not remove the core risk** — systems-designer: every mode is paid for by the same unmodelled 5-line release rate, so release frequency must be simulated before modes are picked; if forced, keep VS over Arcade.
- **Theme count** — narrative-critic accepts 1-2 themes only if the full 5-theme arc is written now and EA is framed as the first leg of the journey; business-analyst wants the arc shipped as updates.
- **Escalated decision:** Which mode is the EA front door, and is release frequency validated before choosing?

### B. Should environmental reactivity and glass reflections move up to MVP?
- **Yes** — business-analyst, narrative-critic, player-psychologist: they are the product's identity and the only trailer-worthy moment.
- **Only if paid for by an explicit cut** — feasibility-lead: they are the only items the cut list removes, and they depend on the unowned art pipeline (binary `.uasset` work agents handle worst); candidates are Agent Intro, Agent Narratif and Arcade.
- **Escalated decision:** What is cut to fund the audiovisual identity?

### C. How is the difficulty curve produced?
- **Write the values** — systems-designer (BLOCKING): every lever is named but has no number, and runs exceed the 5 levels.
- **Build a tuning tool first** — feasibility-lead: numbers on paper are false precision; the blocker is "no curve and no means of producing one", fixed by a headless rules simulator.
- **Cut the long runs** — business-analyst: removing 10- and 15-level runs resolves part of systems F1, all of QA F3, narrative F3 and psychologist F5.
- **Escalated decision:** Are 10/15 runs cut, and does a simulator come before any balancing numbers?

### D. Is the onboarding gap a teaching problem or a rule problem — and which comes first?
- **Teaching first** — player-psychologist: a scripted, unscored tutorial belongs inside any reduced MVP, and "the player can learn the win condition" should rank above "the voyage has an arc" in production order.
- **Rule first** — systems-designer: a tutorial can explain the counter rule but cannot make a 5-line reachable for a new player; make release attainable or explicit.
- **Arc first** — narrative-critic keeps the voyage BLOCKING and asks for it to be written before scope cuts.
- **Escalated decision:** Does the release mechanic change (e.g. an explicit release action), and is the tutorial scheduled before the theme arc?

### E. Does receiving an attack hurt?
- **Inverted** — systems-designer F2: up to 8 marbles per column a column gains mobility, so small attacks help the victim and an AI that "sends faster" gets weaker; the music still escalates (narrative X1, psychologist X1).
- **Undefined, not inverted** — adversarial-qa C1: the numbers depend on where marbles land when a column already touches L1, a rule the document never chose.
- **Escalated decision:** What is the attack placement rule, and are attack marbles inert (Puyo-style garbage) or normal colours?

---

## 3. QUICK WINS

1. **Write the resolution order and the wrap rule.** One fixed order for fall, refill, auto-rise and chain check, and one sentence saying whether matches wrap from c5 to c1. This also settles whether a switch can rescue a "blocked" board. (adversarial-qa F1 and C2; systems-designer F4.1.)
2. **Add a round timer with an in-round spawn ramp, and say how draws count in a best-of.** (systems-designer F3 revision; adversarial-qa F5.3; feasibility-lead N6.)
3. **Remove the 10- and 15-level runs from the MVP.** (business-analyst Round 2, resolving parts of systems-designer F1, adversarial-qa F3, narrative-critic F3 and player-psychologist F5.)

---

## 4. VERDICT

This document is not yet ready to drive production, but the rules have moved a long way: the v0.2 blockers — the contradictory release rule, columns stranded off the median by a move, the uncapped counter — no longer appear. What remains on the rules side is narrower and more structural: the board is not yet deterministic (resolution order, wrap rule) and the end-of-match logic can leave a VS round unending or end it while a move could still save the player. Around that, the scope is still the whole game for one person in seven weeks, and the audiovisual identity that justifies the product is undefined and the first thing to be cut. The single change that matters most, per feasibility-lead's Round 2 proposal and systems-designer's request to treat the release rate as a precondition, is to fix the resolution order and match rule and make the first deliverable a headless rules simulator: it answers whether matches end, whether attacks hurt and how often releases happen, gives the arcade AI its forward model and Agent Test something to test, and is the evidence needed to decide which modes ship at Early Access.
