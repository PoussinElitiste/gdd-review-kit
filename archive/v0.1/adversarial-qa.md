# Adversarial QA — Round 1

Document reviewed: `gdd.txt` ("The marbles effect")

## Finding 1: The required AI "wow moment" and the Technical Strategy it is supposed to drive are missing
- **Problem:** The document's own brief says the whole "Stratégie technique" must be designed around the AI-enabled "Un agent, un effet wow" moment, and that this comes before anything else. The GDD never names that moment, never says what an agent does in play, and leaves both `# Agents Roles` and `## Token Cost budget` empty. Nothing can be tested against the document's own success criterion. It is also impossible to check what happens when the agent fails, responds slowly, runs out of token budget, or gives output that breaks the board, because no agent behaviour is defined.
- **Source:** Lines 4-6 (the requirement), lines 38-40 (empty `Agents Roles` and `Token Cost budget` headings). The document has no "Stratégie technique" section at all.
- **Severity:** BLOCKING

## Finding 2: The core input is self-contradictory ("slide a column horizontally")
- **Problem:** The cursor lets the player "glisser une colonne de billes verticalement et horizontalement". A column slides vertically. It is undefined what sliding a column horizontally means: moving the whole column left or right, and if so, what happens to the column it moves into? It could also be a mistake for "row". On top of that, none of these are specified:
  - edge behaviour: wrap-around, push-out, or blocked?
  - whether a slide is free movement or a one-cell step
  - whether you can slide back after a move that produces no match
  
  The exploit risk depends entirely on these answers. For example, if wrap-around is allowed and slides are free, every row configuration can be reached.
- **Source:** Line 18, the only mechanical sentence in the document.
- **Severity:** BLOCKING

## Finding 3: No fail state, no cost per move, and no board resolution, so the game can be brute-forced and has no ending
- **Problem:** The document gives a match condition (at least 3 of the same colour, horizontally) and nothing else. It does not say:
  - what happens to matched marbles
  - whether gravity or refill applies, and where new marbles come from
  - whether cascades or chain reactions exist
  - whether a timer, move limit, or score exists
  - what counts as winning or losing a level
  
  With free slides and no move cost, a player can shuffle at random until a match appears, with zero risk. Also undefined: whether matches present at spawn resolve automatically (free points) and what happens on a deadlocked board where no slide can make a match. The pointer to "a separate document" for deeper mechanics doesn't help, because that document isn't referenced, and these are core-loop rules, not deep mechanics.
- **Source:** Line 18 (match rule only); line 8 (deferral to a separate document); line 36 (`## Scope` heading is empty).
- **Severity:** BLOCKING

## Finding 4: Matching is horizontal only, so vertical alignments are left undefined
- **Problem:** Only horizontal lines of 3 count. But vertical slides are the main way to change rows, so vertical lines of 3 of the same colour will form all the time. The document doesn't say whether they are ignored, block anything, or are a missed opportunity. Players coming from match-3 games will expect them to count. With no feedback rule for them, this will be reported as a bug. Also undefined: L/T shapes, lines of 4+ (worth more? a special marble?), and two matches resolving at once.
- **Source:** Line 18, "aligner au minimum 3 bille de même couleur horizontalement".
- **Severity:** MAJOR

## Finding 5: Colour is the only match key, but the visual spec works against telling colours apart; the breaking-glass trigger is cut off mid-sentence
- **Problem:** Colour is the only property that decides a match, and the Fantasy section pushes heavy glass reflections ("reflet lumineux dans les billes en verre") and environmental visual effects. Nothing ensures readability under those effects, and there is no colourblind fallback such as patterns or symbols. Players will fail matches because of the art, not because of a wrong decision. Separately, "Bruit de verre cassé lors" is a truncated sentence, so the event that breaks glass is never stated. It is the only hint that matched marbles are destroyed, which ties back to Finding 3. It also conflicts in tone with the "très ASMR" / lounge direction unless someone defines when it fires and how often.
- **Source:** Lines 22-29 (Fantasy), especially lines 26 and 28; line 18 (colour-only match rule).
- **Severity:** MAJOR

## Round 2 — Cross-examination

### Conflicts

**C1. vs. Feasibility Lead F3 (core mechanic ambiguity, rated MAJOR) and Systems Designer F5 (AI moment, rated MAJOR).**
Feasibility F3 says the ambiguity "blocks the first prototype" and that "two programmers would build two different games", then rates it MAJOR because it is "a one-day fix". How cheap a fix is has nothing to do with whether something blocks. If an issue stops the first prototype, it is BLOCKING until someone resolves it. Systems Designer F2, Player Psychologist 1 and my F2 all rate this BLOCKING, and Feasibility's own text supports that rating. In the same way, Systems Designer F5 rates the missing AI moment MAJOR. Four other reviewers rate it BLOCKING, and the brief (lines 4-6) puts it "avant de développer quoi que ce soit d'autre". The document's own success criterion can't be tested, so I am keeping BLOCKING on both.

**C2. vs. Systems Designer F2 ("vertical alignments are deliberately excluded with no reason given").**
The document does not show that the exclusion is deliberate. Line 18 already confuses "colonne" with what is probably "row" in the same sentence. So "horizontalement" at the end could be a second slip just as easily as a design choice. If we treat it as deliberate, we build exclusion logic on what may be a typo. Player Psychologist 1 frames it correctly as something to "say whether ... deliberately excluded". That is why my F4 asks for the behaviour of vertical lines to be defined explicitly, not assumed.

**C3. vs. Feasibility Lead F5 (glass trigger/event table, rated MINOR), and the tone readings in Narrative Critic 4 and Player Psychologist 3.**
Three reviewers read the cut-off "Bruit de verre cassé lors" as a tone or audio-hook problem. My concern is mechanical. It is the *only* line in the document that suggests matched marbles leave the board. Without it, we don't know whether a match removes marbles at all (see my F3). Narrative 4 and Psychologist 3 also raise "breaking glass as a failure sound". That reading assumes a fail state the document never defines. The sentence can't be read as signalling failure unless failure exists, and nothing in the document says it does. It isn't MINOR. It is part of the missing resolution rule.

**C4. vs. Player Psychologist 5 (colour-only distinction, rated MINOR).**
The Psychologist treats this as an accessibility add-on. I argue it is a correctness problem. Colour is the *sole* match key in line 18, and the Fantasy lines 26 and 29 deliberately add reflections and environmental VFX over that key. A player with typical vision can still misread a match under heavy effects, and that turns into reported "match didn't register" bugs. I am keeping MAJOR. See C/X3 below for why the theme system makes it worse.

### Connections

**X1. Token cost × free moves (my F1 + F3, Feasibility F1, Systems Designer F5).**
Feasibility F1 asks whether the agent "calls the network on every move". My F3 shows that moves cost nothing and are unlimited, and that random shuffling is the dominant low-effort strategy. Put these together: if the agent responds to player actions at runtime, which is the obvious reading of an in-play "effet wow", then a player who spams slides drives unlimited agent calls. `## Token Cost budget` is empty, so nothing caps it. A missing move-cost rule therefore becomes a missing cost ceiling. Neither problem is visible from one review alone.

**X2. Versus mode × undefined fail/deadlock rules (Business Analyst 3 + my F3).**
Business Analyst 3 notes that *Lose Your Marbles* implies a competitive versus product. In versus play, every gap in my F3 becomes an exploit. With no move limit or timer, a player can stall forever. With no deadlock rule, a dead board has no defined outcome. With undefined spawn-match resolution, one player can get free points from the RNG. A single-player zen mode can live with some of these gaps. A versus mode cannot. Choosing the mode is therefore a precondition for rating how serious my F3 is.

**X3. Per-level themed marbles × colour-only matching (Narrative Critic 3, Business Analyst 4, Systems Designer 3 + my F5).**
Line 24 promises a pronounced theme change across "niveau, tableau, billes", so the marbles themselves are reskinned per level. If colour is the only match key and the marble art changes per theme, then each theme either redefines the colour palette, which forces the player to relearn the match key every level, or locks the palette, which limits the "very pronounced" theming. Any colourblind fallback also has to survive every theme. The colleagues treated theming as content cost and narrative logic. In fact it also attacks the integrity of the only match rule.

**X4. Input device × ambiguous slide axis (Player Psychologist 1, Business Analyst 2 + my F2).**
Both colleagues note that "curseur" is not tied to any platform. On touch, a single-gesture slide that can go "verticalement et horizontalement" needs direction disambiguation, because diagonal swipes are common. Without a rule, the game will mis-slide. Combined with my F2's unknown undo rule, a mis-slide may be irreversible. The input contradiction is worse on the platform Business Analyst 2 considers most likely, which is mobile.

### Revisions

- **F3: severity unchanged (BLOCKING), framing revised.** Psychologist 3 raises the possibility of a deliberate zen/ASMR target. If that is the goal, "brute-forceable with zero risk" is not automatically a flaw, because a relaxing game can have no fail state. The exploit only matters if score, progression gates or versus exist. The BLOCKING part that remains is board resolution: removal, refill, cascades and deadlock handling are needed in any mode. I am narrowing the finding to that part.
- **F5: split.** The readability/colour-key part stays MAJOR (see C4 and X3). I am moving the truncated glass sentence into F3 as evidence for the missing resolution rule (C3). As a standalone text defect it would only be MINOR.
- **F1: upgraded in scope, not severity.** I am adding X1. The empty token budget is an exploitable cost surface once it is combined with free moves, not just a missing section.
- **F4: unchanged (MAJOR).** C2 confirms that the problem is the undefined behaviour, whether or not the exclusion is intentional.
