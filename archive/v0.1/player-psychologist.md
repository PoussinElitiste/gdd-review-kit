# Player Psychologist — Round 1

Lens: what a real player experiences in the first minutes and the first hour: understanding, motivation, frustration, reward.

Context: the GDD is very thin. It has a title, a one-sentence concept, a Fantasy list and two inspirations. The Scope, Agents Roles and Token Cost budget sections are empty headings. Most of what follows comes from things the document leaves out, and each finding says which passage or gap it comes from.

---

## 1. The core verb is hard to picture, and nothing says how a player learns it
**Severity: BLOCKING**

**Source:** Concept: "glisser une colonne de billes verticalement et horizontalement afin d'aligner au minimum 3 bille de même couleur horizontalement". There is no onboarding or tutorial section, and nothing on input or platform.

**Problem:** A player can't form a clear picture of the one action the whole game rests on. Sliding a *column* vertically makes sense. Sliding a *column* horizontally is unclear: does the whole column move one slot sideways and push its neighbors? Does it wrap around? Is it really a row? Matches count only horizontally, but the main move is vertical, so the player has to work out an indirect cause and effect (move things up and down to line them up sideways) with no help. Most players who quit match-3 games do it in the first 60 seconds, when they can't predict what a move will do. The doc doesn't say what the "curseur" is (mouse, touch, gamepad), how a selected column looks, or how the first level teaches the rule. That leaves no basis for designing the first session.

**Ask:** Define the move exactly, with a diagram. Say whether vertical matches are deliberately excluded and why. Add a first-session onboarding beat that teaches the match by having the player do it, not by reading text.

---

## 2. No goal, no fail state, no progression, so nothing motivates the player
**Severity: BLOCKING**

**Source:** None of these appear anywhere: a win condition, a loss condition, score, timer, move limit, level objectives, unlocks, or progression. "Niveau" only appears inside the Fantasy list ("Une thématique très prononcée entre niveau").

**Problem:** The player has no answer to "what am I trying to do, and why keep going?" The doc says levels exist and have different themes, but not what clears a level, what happens if you fail, or what you earn. Without that loop, the visual and sound effects have nothing to reward. Juice reinforces a goal; it can't stand in for one. The first hour has nothing pulling the player forward: no next level to unlock, no score to beat, no mastery curve.

**Ask:** Define the session loop (objective, then play, then outcome, then reward, then next level) and what drives the player over the long run (new themes, new marble types, scoring).

---

## 3. The ASMR fantasy and the inspirations promise opposite feelings
**Severity: MAJOR**

**Source:** Fantasy: "Une ambiance très ASMR … pour surprendre le joueur", "ambiance très lounge electro", "Bruit de verre cassé lors" (the sentence stops there). Inspirations: "SEGASOFT Lose Your Marbles" and "Tetris Effect".

**Problem:** ASMR and lounge music promise calm, low pressure and predictable soothing feedback. "Surprendre le joueur" and a breaking-glass sound suggest jolts. Lose Your Marbles is a competitive, pressure-driven versus puzzle game, and Tetris Effect works by building tension through speed. Players will arrive expecting one emotional contract, and if the game delivers the other they will feel misled. That mismatch causes early quits and bad first reviews. The breaking-glass line is cut off, so we don't know whether it plays on a successful match (reward) or a mistake (punishment). Those two choices create very different emotional loops. Breaking glass as a failure sound in an "ASMR" game would hurt the relaxing mood the game is selling.

**Ask:** Pick the emotional target (relaxing zen mode, rising tension, or a deliberate mix) and say when each feedback sound plays. Finish the glass-breaking sentence.

---

## 4. The doc doesn't name the "wow" moment, so the player has no reason to choose this over other match-3 games
**Severity: MAJOR**

**Source:** Lines 3-6 of the doc's own brief require "le moment, rendu possible par l'IA, qui donne envie d'y jouer" ("Un agent, un effet wow"). No such moment is described anywhere. "Agents Roles" is an empty heading.

**Problem:** From the player's side, this is a missing hook, not a missing technical detail. Match-3 is one of the most crowded genres there is. Without a defined standout moment, the player sees a polished but familiar puzzle game, and the first-hour experience has no high point to remember or tell others about. Building the experience around that moment is the doc's own stated design rule, and the doc doesn't follow it yet.

**Ask:** Describe, from the player's point of view, the moment that is only possible because of the AI agent (for example, music or visuals that react to how the player plays), when it first happens in the first session, and how often it comes back.

---

## 5. Color is the only way to tell marbles apart
**Severity: MINOR**

**Source:** Concept: "aligner au minimum 3 bille de même couleur". Fantasy: "reflet lumineux dans les billes en verre". Nothing about accessibility.

**Problem:** The only matching rule is color, and the art direction adds glass reflections and environmental effects, which make colors harder to read. Players with color-vision deficiency (roughly 8% of men) will make mismatches they can't understand and will feel the game is unfair. Heavy visual effects in the style of Tetris Effect can also hide the board at the moment the player needs to read it.

**Ask:** Add a second cue for each color (pattern, symbol or shape inside the marble) and a rule that effects never hide the readability of the board.

---

## Round 2 — Cross-examination

### CONFLICTS

**C1. Against the board consensus that the missing AI "wow" moment is BLOCKING (Narrative F1, Feasibility F1, Adversarial QA F1, Business F1).** Four colleagues rate it BLOCKING, and from their angles they are right: the brief requires it, and production, cost and testing can't be planned without it. I keep it at MAJOR from the player's side, and I think the distinction matters for the synthesis. A player never sees "an AI moment". They see a hook or they don't. A sliding match-3 with excellent feel can hold a player for an hour with no agent at all. Without a defined goal and a readable verb (my F1 and F2), no AI moment can hold them, because they quit before it shows up. So the order of fixes matters: if the team answers the board by first designing an agent feature on top of an undefined loop, players get a showpiece on a game they don't understand. My position is that the AI gap blocks the *document* (it fails its own brief), but for the *player* the loop and the verb come first. The moderator should not merge these into a single "everything is blocking" item.

**C2. Against Adversarial QA F3's framing that a game with no fail state or move cost is broken ("brute-forced with zero risk").** I agree the rules are missing; that's my F2. I don't agree that risk-free shuffling is a defect in itself. The Fantasy section pitches "très ASMR" and "lounge electro". For that audience a no-fail, free-shuffle mode is a legitimate and proven design (zen modes, relaxation puzzlers), and adding move limits or timers could break the emotional contract the doc is selling. The real defect is that the doc never chooses. A fail state should follow from the emotional target (my F3), not be added by default to close an exploit. I'm revising my own F2 to match (see R2 below).

**C3. Against Narrative Critic F4's reading that shattering the marbles as a reward is a "real tension" because it destroys precious objects.** In practice, players experience destruction-as-reward as deeply satisfying. Satisfying-shatter and crush content is a whole ASMR subgenre, and in puzzle games, breaking the thing you carefully lined up is the payoff, not a loss. So the "reward" reading of "Bruit de verre cassé lors" is psychologically the *safe* one. The risk I see is different: loudness, sharp high frequencies, and frequency over a long session. Glass breaking on every match with cascades can turn from soothing to grating in 20 minutes. The fix is sound design (soft, crystalline, varied, scaled to combo), not a rethink of the fiction. I still agree with Narrative that breaking glass as a *failure* sound would be harmful, which my F3 already says.

**C4. Partial tension with Narrative Critic F5 (use "losing your marbles" as composure vs. unraveling).** It's an elegant theme, but it pushes the game toward the tension side of my F3. A player who picks this game for its ASMR promise and then gets a "mental unraveling" arc is exactly the broken emotional contract I warned about. It can work only if the doc commits to it openly and the store pitch and first level set that expectation. It doesn't fix F3 by itself; it's one of the two choices F3 asks the team to make.

### CONNECTIONS

**X1. Themed marbles per level + color-only matching = relearning the color vocabulary every level (my F5 + Narrative F3 / Business F4 / Systems F3).** "Une thématique très prononcée entre niveau (niveau, tableau, billes)" means the *marbles themselves* change look per theme. If the colors and materials change at each theme change, the player has to relearn which marbles count as "the same color" at the exact moment the doc wants to impress them. Pair that with glass reflections and environmental VFX, and theme transitions become spikes of mismatch errors. That error will feel unfair, not like a skill failure. Nobody saw this on their own: Systems sees themes as cosmetic, Narrative sees them as undefined, and I only saw static color readability. Together they make a concrete first-hour frustration point.

**X2. Feasibility F4 (the art and audio is the likeliest late cut) + my F2 (no loop) = a game with nothing left to motivate the player.** Right now the only player-facing reason to keep playing is sensory (Business F4 makes the same count: five of six Fantasy bullets are polish). If that layer gets cut or reduced late, as Feasibility predicts, and no goal or progression loop has been designed, the player has no motivator at all. This turns my F2 into a production risk, not just a design gap: the loop is the insurance policy for the polish.

**X3. Adversarial QA F4 (vertical lines will be reported as bugs) strengthens my F1 as a negative-transfer problem.** Match-3 veterans, which is the likely audience, already have "3 in a line in any direction" as an automatic habit. Each vertical line that sits there without clearing doesn't just confuse them; it tells them "the game is broken" in the first minute. That's worse than an unfamiliar rule, because it contradicts a learned one. The onboarding beat I asked for in F1 must teach the horizontal-only rule *explicitly*, ideally with a vertical line on screen that visibly doesn't fire, or the rule should be reconsidered.

**X4. Business F2 / F3 (no platform, no mode) is the root of my F1 and F3.** I couldn't design onboarding because the "curseur" input was undefined. Business shows why: touch vs. gamepad changes the verb completely. And whether the game is versus (Lose Your Marbles) or solo (Tetris Effect) *is* the emotional contract decision in my F3. A versus game can't be ASMR-calm. Mode choice should be the first decision, because it settles both.

### REVISIONS

**R1. Upgrade F5 (color-only matching) from MINOR to MAJOR.** Because of X1, this isn't only an accessibility edge case for color-blind players. It's a recurring readability failure for *all* players at every theme change, made worse by the doc's own art direction. Adversarial QA independently rated the same issue MAJOR.

**R2. Keep F2 BLOCKING, but restate it.** Following C2, the blocking problem is "no defined goal or outcome for a session", not "no fail state". A no-fail zen design would satisfy this finding if it's chosen deliberately and gives the player a clear objective and sense of completion (for example, clearing the board or finishing a themed track).

**R3. Keep F4 at MAJOR** despite four colleagues at BLOCKING, for the reasons in C1. I'm stating this so the moderator records it as a lens disagreement and not an oversight.

**R4. Narrow F3.** I withdraw the implied suggestion that the breaking-glass reward reading is risky (see C3). F3 stays MAJOR, now resting on the calm-vs-surprise contradiction, the competitive-vs-zen inspiration split (backed by Business F3), and the unknown trigger for the glass sound.
