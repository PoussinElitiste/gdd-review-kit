# Review Board — Moderator Synthesis (v0.2)

Document reviewed: `gdd.txt` — "The Marble Effect", GDD v0.2 plus linked documents (Combo system, Schémas du déplacement, Événements visuels et sonores, Thèmes des niveaux, Budget tokens)
Board: systems-designer, narrative-critic, player-psychologist, feasibility-lead, adversarial-qa, business-analyst
Inputs: Round 1 (30 findings, 5 per reviewer) and Round 2 cross-examination in `reviews/*.md`. The v0.1 review is archived in `archive/v0.1/`.
Moderator note: everything below traces to the review files; no new critique has been added.

---

## 1. TOP 5 ISSUES (severity × confidence)

### #1 — The counter-release rule, the only progress action in every mode, is contradictory, untaught and almost unreachable
- **Problem:** The GDD says any full 5-line releases the counter, while Combo system says only a 5-line of a *different* colour does; a ×0 release is undefined (a free colour reroll), the colour count is never stated, and because any 3-run on the median line clears at once, a 5-line can only come from an `X X _ X X` row plus a move, or from a cascade.
- **Flagged by:** systems-designer F1 (BLOCKING), adversarial-qa F2 (MAJOR → BLOCKING), player-psychologist F2 (MAJOR → BLOCKING), feasibility-lead F2 (MAJOR → BLOCKING, partly on this basis); narrative-critic Round 2 adds that the same-colour line plays a success-sounding "note ascendante".
- **Cross-examination:** **STRENGTHENED.** Four lenses converged in Round 2: no probability model (systems), players will think the rule is broken and may never release on purpose in Solo (psychologist X2), an implementing agent will pick one of two readings (feasibility), and the rule is contradictory and exploitable at ×0 (QA). Systems and QA agree the release depends on the stack/cascade route, which they now both treat as the main path to progress.

### #2 — The MVP does not fit one person and about 5 build weeks, and the cut list saves almost nothing
- **Problem:** The MVP keeps four modes, five fully themed levels, a scripted AI, leaderboards and local two-player "sur toutes les plateformes"; the four platforms (PC, console, mobile, VR) are never scoped, VR has no control scheme, there are no milestones, and the only cuts are online multiplayer and one or two "Optionnel" effects.
- **Flagged by:** feasibility-lead F1 (BLOCKING), business-analyst #1 (BLOCKING); feasibility-lead F5 and business-analyst X6 add about 40 agent sessions per week for one person to review, a labour cost the plan never counts.
- **Cross-examination:** **STRENGTHENED.** Both reviewers reached it independently and kept BLOCKING. Round 2 added the uncounted human hours, and feasibility-lead noted that Solo/Zen leaderboards imply an online backend unless they are local or Steam-native. The two disagree on whether a competitive (online) branch is possible at this scope (see Disagreement B).

### #3 — Win and loss conditions are undetermined: columns can soft-lock, and an uncapped counter can one-shot a board
- **Problem:** Moves are limited only by the top and bottom of the board, so a column can leave the median line and become permanently unselectable while "Plateau bloqué" never fires; meanwhile the counter has no cap (about ×17 fills any board), the wave delay and wave distribution are deferred, and overflow into a full column, simultaneous lockout and Solo time-out outcomes are undefined.
- **Flagged by:** adversarial-qa F1 (BLOCKING) and F3 (MAJOR), systems-designer F4 (MAJOR → BLOCKING), feasibility-lead F2 (wave rules deferred); player-psychologist X1 shows novices are the players most likely to strand a column.
- **Cross-examination:** **STRENGTHENED.** QA re-read the Switch rule and found it can carry an empty median cell into a neighbouring column, splitting it; the movement diagrams confirm the soft-lock state is reachable. Systems and QA derived the ×17 one-shot independently, and systems concluded the loss rule "misfires in both directions". QA asks that F1 be weighed as single-source but document-verified; feasibility-lead adds that Agent Test cannot catch it without a headless, deterministic C++ simulation.

### #4 — The audiovisual "voyage", the game's stated "wow", has no content, owner, budget or difficulty curve
- **Problem:** The themes table is "À définir" in every cell, the emotional arc is deferred, no artist, composer or sound designer is confirmed and the budget excludes assets; Solo and Arcade offer 5/10/15-step runs over 5 levels with no rule for levels 6-15 and no difficulty values; and in Arcade/VS the scenery builds up as the player's board fills.
- **Flagged by:** narrative-critic #1 (BLOCKING) and #3, business-analyst #2 (BLOCKING), systems-designer F2 (BLOCKING), feasibility-lead F4 (MAJOR), adversarial-qa F5 (MAJOR), player-psychologist F5 and X3.
- **Cross-examination:** **STRENGTHENED.** Narrative-critic counts four colleagues reaching the same empty table and 5-vs-15 gap ("undefined, unowned and unfunded"). Business-analyst X1 adds that the token budget funds the riskiest "wow" (the NPC, feasibility F3) while the "wow" sold by the pitch and title gets nothing, and X3 that it mostly shows while the player is losing. Severity of the funding gap stays split (see Disagreement C).

### #5 — Onboarding teaches only "align 3", once, and none of the systems that decide wins and losses
- **Problem:** The only teaching is a passive hint in the player's very first game, in any mode; it never covers the counter colour, the release rule, stacking, pending waves or cancellation, it is consumed even if that game is quit after a few seconds, and in local VS it helps one player only.
- **Flagged by:** player-psychologist F1 (BLOCKING), adversarial-qa F5 (MAJOR); feasibility-lead, systems-designer and business-analyst engage with it in Round 2.
- **Cross-examination:** **STRENGTHENED, with the fix contested.** Business-analyst withdrew its pure-cutting stance and conceded onboarding is load-bearing (R3). Systems-designer argues the hint toward non-counter matches is not a weak move and that the real gap is teaching the two-colour split. Feasibility-lead warns a scripted tutorial is bug-prone and can only be built once rules freeze, and proposes a replayable, extended hint plus Zen as a practice room.

---

## 2. UNRESOLVED DISAGREEMENTS

### A. What is Zen for: a no-fail practice and contemplation space, or a bounded, ranked mode?
- **Protect a no-fail space** — player-psychologist, narrative-critic: Zen is the only low-pressure place to learn a mechanic no one has played since 1997 (psychologist C1), and every proposed fate for Zen leaves "contemplation" with no home; either name a contemplative space with no fill, score or panic, or remove the word from the Fantasy (narrative C3).
- **Bound it** — adversarial-qa: a zero-spawn Zen has no end condition, a combo leaderboard gamed through settings, and an unrecoverable soft-lock; a calm Zen needs a voluntary end action, segmented leaderboards and the F1 fix.
- **Fold it into Solo** — business-analyst (revised R2): an untimed, no-fail, no-leaderboard Solo setting, cheaper than a separate mode; QA warns this moves the leaderboard exploit into Solo unless modified-setting scores are excluded. Psychologist proposes splitting into an unranked practice mode and a ranked mode with fixed settings.
- **Escalated decision:** Does the MVP keep a no-fail practice space, and if so, does it carry a leaderboard?

### B. Which audience, and how small an MVP?
- **Pick one audience first; competitive means online VS in the MVP** — business-analyst (upgraded #3 to BLOCKING): the design currently serves neither the competitive nor the broad audience, and the MVP cannot be scoped until one is chosen.
- **Only the casual branch is feasible** — feasibility-lead: real-time online VS needs deterministic simulation, netcode, matchmaking and desync testing, a multi-month job that would replace the whole MVP; ship local VS with local or Steam-native leaderboards.
- **A 1-2 theme MVP breaks the Fantasy** — narrative-critic accepts the scope verdict but notes one or two themes cannot be a "voyage à travers le temps, l'espace et la nature"; the Fantasy must be rewritten or the journey moved inside a level, which makes its Finding 3 the main narrative issue. Business-analyst answers that a one-theme vertical slice is the right bar and the arc can be a paragraph until after Early Access.
- **Escalated decision:** Is Early Access aimed at the broad or the competitive audience, and does the MVP shrink to a one- or two-theme slice?

### C. Is the unfunded audiovisual layer a production blocker?
- **BLOCKING** — business-analyst: a placeholder build is a different product with no market claim; the "wow" is the whole pitch.
- **MAJOR** — feasibility-lead: mechanics, AI and modes can be built and tested on placeholders, and the gap is closed by a scope decision (one finished theme); tokens cannot buy the "wow", so moving Opus sessions to art direction does not help — it needs money for a human artist and composer.
- **Escalated decision:** Does production start before one theme is fully defined and its art and music source secured?

### D. Should the Arcade/VS scenery stop escalating as the board fills?
- **It rewards losing** — narrative-critic #3: the journey unfolds as the player is buried, so the scenery reports failure instead of accomplishment.
- **Keep it as a danger signal, add a success channel** — systems-designer and player-psychologist: fill-driven layering is the only continuous danger readout in VS; the fix is a second escalation channel driven by releases and cancelled waves, not removing the existing one.
- **Escalated decision:** Is the fill-driven escalation kept as a danger cue alongside a new success-driven reveal, or replaced?

---

## 3. QUICK WINS

1. **Reconcile the release rule in one sentence and define the ×0 case.** Rewrite GDD loop step 4 to match Combo system ("a 5-line of a different colour releases the counter") and state what a ×0 release does. (adversarial-qa F2; player-psychologist F2; systems-designer F1, which also asks for the colour count to be stated.)
2. **Keep at least one marble of every column on the median line.** Clamp moves (or let the cursor select any column) and define what a Switch does with an empty cell. (adversarial-qa F1 fix direction; feasibility-lead notes zero-spawn Zen must ship with this clamp.)
3. **Use one global accessibility pattern set for all themes.** It fixes the colour-blind relearning problem and removes four pattern-set authoring passes. (player-psychologist F4; feasibility-lead Round 2, "the cheapest high-value change on the board"; business-analyst X4; narrative-critic sides with the psychologist.)

---

## 4. VERDICT

This document is not ready to drive production. Compared with v0.1, the board no longer reports empty sections: the core loop, movement, elimination, combo and attack systems are written, and the v0.1 top issues (missing AI moment, contradictory move rule, no session goal) do not recur. What remains is that the written rules contradict each other at the centre of the game — the counter release that is the only way to progress or attack, a movement rule that can soft-lock the board, and win/loss outcomes that depend on deferred or uncapped values — while the scope (four platforms, four modes, five themes for one person in about five build weeks) and the unfunded audiovisual "voyage" leave the product case open. The single change that matters most, per feasibility-lead's Round 2 revision and the four-reviewer convergence on Issue #1, is to freeze and reconcile the core board rules — starting with the counter-release rule, then the median-line clamp, the counter cap and the wave timing — before the Gameplay agent's sessions begin, because every other agent and every balance decision is built on top of them.
