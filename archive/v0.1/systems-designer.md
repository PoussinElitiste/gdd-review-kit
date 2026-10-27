# Systems Designer — Round 1

Document reviewed: `gdd.txt` ("The marbles effect"). Overall: the document has one core verb and no systems built around it. Most of my lane (progression, pacing, difficulty, economy) has nothing written to review. That gap is the main finding, and I have split it below into the parts a designer could actually act on.

## Finding 1 — No loop exists beyond a single action; there is no win, fail, or end-of-level condition
- **Problem:** The Concept describes one action (slide a column, align 3+ same-colour marbles) but never says what a match produces or what ends a level. Nothing covers scoring, clearing, refill or gravity after a match, timers, move limits, board-fill fail states, or level objectives. Without these, the core loop "act -> feedback -> reward -> next goal" stops after "act". Nobody can prototype or tune this.
- **Source:** Concept (line 18) is the only gameplay statement in the document. Line 8 says "More in depth mechanic must be define in a separate document", but no such document is referenced or provided.
- **Severity:** BLOCKING

## Finding 2 — The core manipulation rule is self-contradictory or underspecified
- **Problem:** The cursor lets the player "glisser une colonne de billes verticalement et horizontalement". You slide a column vertically. Sliding a *column* horizontally is undefined: does the whole column move one slot over, do the columns swap, does it wrap around? Matches also count only horizontally ("aligner ... horizontalement"), so vertical alignments are deliberately excluded with no reason given. Whether rows/columns wrap at the edges, and whether one slide can trigger cascades, decides the whole possibility space and difficulty ceiling of the puzzle. None of it is stated.
- **Source:** Concept, line 18.
- **Severity:** BLOCKING

## Finding 3 — No progression structure, difficulty curve, or pacing across a playthrough
- **Problem:** "Niveau" is mentioned, but nothing says how levels differ mechanically. There is no level count, no new marble types, no board-size growth, no obstacles, no rising speed or density, and no unlock structure. The only between-level variation given is a strong *theme* change (level, board, marbles), which is purely cosmetic. With Tetris Effect named as an inspiration, the gap is clear: that game's audiovisual escalation is tied to rising gameplay intensity. Here, theme changes and difficulty are not connected at all.
- **Source:** Fantasy, line 24 ("Une thématique très prononcée entre niveau"). Inspirations, line 34. The "Scope" section (line 36) is empty.
- **Severity:** MAJOR

## Finding 4 — The ASMR/sensory layer is not hooked to any gameplay event, and one hook is literally unfinished
- **Problem:** The fantasy pillar is sensory feedback, but the document never maps feedback to system events. Which events (match, combo, cascade, level clear, failure) trigger which escalation? The broken-glass sound is cut off mid-sentence ("Bruit de verre cassé lors"), so the one feedback cue that probably signals a successful match has no defined trigger. Without combo/cascade rules (Finding 1), there is no intensity gradient for the audio/visual layer to follow. The "wow" and the loop sit side by side without being connected.
- **Source:** Fantasy, lines 22-29, especially line 28.
- **Severity:** MAJOR

## Finding 5 — The required AI "wow moment" is missing, so no AI-driven system exists to integrate with the loop
- **Problem:** The brief says to identify the AI-enabled moment ("Un agent, un effet wow") and build the Technical Strategy around it. The document names no AI moment, and the "Agents Roles" and "Token Cost budget" sections are empty headers. From a systems view, I cannot judge whether an agent would change the loop (e.g., adaptive board generation, a difficulty director, generative theming) or just decorate it. There is also no token budget to set against per-level or per-session call frequency.
- **Source:** Brief, lines 3-6. Empty sections "Agents Roles" (line 38) and "Token Cost budget" (line 40).
- **Severity:** MAJOR

## Round 2 — Cross-examination

### 1. CONFLICTS

**vs. Feasibility Lead F3 (core mechanic ambiguity rated MAJOR, "a one-day fix") and my Finding 2 (BLOCKING).**
I agree it takes a day to *write*. I don't agree that the *decision* is small, and severity should track the decision. Wrap-around versus push-out versus blocked edges isn't an implementation detail. It sets the reachable state space. Adversarial QA F2 makes the same point from the exploit side: free wrapping slides can reach every row configuration. Reachability then decides how often deadlocks happen, whether a move limit is needed to create any challenge at all, and so whether Finding 1's fail state is a timer, a move budget, or nothing. Every downstream system Feasibility lists in F3 (VFX triggers, audio sync, level design) depends on this one rule. A short fix that everything else depends on is still blocking until someone makes it. I keep BLOCKING. I accept it as the board's cheapest quick win.

**vs. Narrative Critic F1, Feasibility Lead F1, Adversarial QA F1, Business Analyst F1 (missing AI "wow" moment rated BLOCKING) and my Finding 5 (MAJOR).**
Four colleagues rate this BLOCKING because the brief says to design the technical strategy around the AI moment "avant de développer quoi que ce soit d'autre". From a systems view, that ordering is backwards, and I'm keeping MAJOR on purpose. Every plausible AI hook needs a working loop to attach to: a difficulty director, adaptive board generation, music that reacts to play (Player Psychologist #4's example). A difficulty director needs a difficulty variable, and none exists (Finding 3). Reactive music needs combo and cascade events, and none exist (Finding 4). An agent designed first, with no loop under it, can only decorate. I agree the document fails its own brief, and that is a real compliance gap. But naming the AI moment does not unblock prototyping, and defining the loop does. I'm escalating this as a sequencing disagreement: which comes first, the loop or the agent?

**vs. Adversarial QA F3 (no move cost means the game "can be brute-forced").**
I flagged the same missing fail state (Finding 1), so the omission isn't in dispute. The framing is. Free, costless shuffling is only an exploit if the game is meant to be a challenge. If the target is the ASMR/lounge experience in lines 22-23, costless experimentation may be *the intended loop*: a zen mode where the reward is the sensory feedback and there is nothing to lose. Calling it a defect assumes a tone decision the document hasn't made (see Connection A). The real BLOCKING issue is that the game hasn't picked between a challenge economy and a zen economy. Brute-forcing is a symptom of that.

### 2. CONNECTIONS

**A. The tone conflict is a loop decision, not a writing decision (Narrative Critic F4 + Player Psychologist #3 + Business Analyst F3 + my Findings 1/3).**
Narrative and Psychology both see a calm-vs-surprise split. Business sees *Lose Your Marbles* (versus, pressure) pulling against *Tetris Effect* (single-player journey). Put next to my lane, these are one unresolved question: **what is the pressure source?** A timer or rising board means tension, which fits Lose Your Marbles. Garbage or attack exchange means versus play and needs a combo-to-attack conversion economy. No fail state means zen, which fits ASMR. Each choice produces a different progression structure (Finding 3): campaign levels with objectives, a versus ladder, or endless or score-attack. Copy or mood boards can't settle the tone. Picking the fail-state model settles it. This makes my Finding 3 depend on a game-mode decision I didn't name in Round 1.

**B. Horizontal-only matching interacts badly with column slides (Adversarial QA F4 + my Finding 2).**
QA notes that vertical same-colour runs will form all the time and players will report them as bugs. There's a structural problem underneath. If a vertical slide is a wrapping rotation, it never changes the order of marbles inside a column, only which row each one sits in. So vertical runs persist through every vertical move and can only be broken by a horizontal move or a clear. Excluding vertical matches also roughly halves the number of lines that can match, which raises deadlock frequency. The rule asymmetry is therefore a match-density lever, not a cosmetic choice. It should be justified, or used as a difficulty knob across levels.

**C. Per-level marble themes are a hidden difficulty spike (Player Psychologist #5 + Adversarial QA F5 + my Finding 3).**
Line 24 changes the *billes* per theme. Colour is the only match key. So every theme change re-skins the match key itself, and players have to relearn which marbles match at each transition. Glass reflections and environmental effects make the colours harder to read. That is an uncontrolled difficulty change at every theme boundary, and it lands on colour-blind players hardest.

**D. Theme cost and progression need to be designed together (Feasibility Lead F4 + Business Analyst F4 + my Finding 3).**
Both colleagues note that per-level themes scale content cost with level count, and no level count is given. A progression structure fixes this cheaply: themes as *worlds* (a few), with several mechanically different levels inside each world. Mechanical variation is cheap to produce and art variation is expensive. That lets the progression curve carry pacing and keeps the art budget fixed. Right now the document does the reverse: all variation is in the expensive layer.

### 3. REVISIONS

- **Finding 3: wording revised, severity held at MAJOR.** I withdraw "purely cosmetic". Connection C shows the theme change has a mechanical side effect (it re-skins the match key). That strengthens the finding: theme transitions already change difficulty, just without control. I'm also adding a dependency. Progression can't be designed until the game mode (Business Analyst F3) and pressure source (Connection A) are chosen.
- **Finding 4: held at MAJOR, against Feasibility F5's MINOR.** Feasibility's own reasoning ("cheap to fix now and expensive to retrofit"; cascade code has to be reworked to expose timing and intensity) argues for higher severity, not lower. The event table is also where Narrative F4's reward-vs-punishment question about breaking glass actually gets decided. Three reviewers' concerns meet in this one table.
- **Finding 2: held at BLOCKING** (see Conflict 1).
- **Finding 5: held at MAJOR.** This is a deliberate disagreement with four colleagues, escalated as a sequencing question (see Conflict 2).
- **Finding 1: strengthened, held at BLOCKING.** With Connection A, Finding 1 is now the root issue that the tone conflicts (Narrative F4, Psychology #3), the mode conflict (Business F3) and the exploit concern (QA F3) all trace back to.
