# Systems Designer — Round 1

Document reviewed: The Marble Effect, GDD v0.2 (22 Oct 2026) plus linked documents (Combo system, Schémas du déplacement, Alignement et élimination, AI Architecture, Thèmes des niveaux, Événements, Budget).

Lens: whether the loop, economy, progression and difficulty systems actually interlock.

---

## 1. The attack system cannot reliably produce the only loss condition. "Plateau bloqué" in practice means a 100% full board, and the defender can make themselves immune to attacks — BLOCKING

**Problem.** VS and Arcade are built on "bury your opponent" (Concept: "ensevelissez votre adversaire"). The math of the stated rules says attacks can't do that.

- **Blocked means full.** A column is "blocked in both directions" only when it can't go up (top at L1, or last ball at the median L8) *and* can't go down (bottom at L15, or first ball at L8). With a minimum of 5 balls per column, the only case that satisfies both is top = L1 and bottom = L15, which means 15 balls. So the blocked-board rule (GDD, Fin de partie) needs all 5 columns at 15 balls: 75/75 cells filled. On top of that, no switch can be able to form an alignment.
- **Attacks can't fill the last cells.** Combo system, "Colonne sans place": an attack ball "ne tombe jamais dans une colonne pleine (15 billes) ni dans une colonne qui touche déjà le haut du tableau (L1)... Si aucune colonne n'a de place, la bille est perdue." A column can only reach 15 balls while it touches L1. So attacks alone can never complete a column. A defender who keeps every column pushed up against L1 (one Move each) makes every incoming attack ball "perdue".
- **Attack balls help the defender.** They are "des billes ordinaires... soumises aux garanties de génération" and land in the defender's *shortest* columns. That evens out column length and keeps balls reachable (the k ≤ 8 / n − 7 rule). The guarantee-restoration rule in Alignement et élimination says generated balls take "la couleur utile" when the board falls below the release threshold. If attack balls follow that rule, an attack can hand the victim exactly what they need to release their counter and counter-attack.
- **The backstop is undefined.** The only thing that can top off a column touching L1 is continuous spawn. No document says where a spawned ball goes when its column touches L1 ("apparaissent par le haut" is physically impossible there). So "Une manche se termine donc toujours" (sudden death) rests on a rule that isn't written.

**Source.** GDD "Fin de partie"; Schémas du déplacement "Règle détaillée" and "Limites du move"; Combo system "Attaque", "Couleur des billes d'attaque", "Répartition d'une vague", "Colonne sans place"; Alignement et élimination "Rétablir la garantie".

**Why it is blocking.** The core competitive loop (fill counter → release → attack → opponent blocked) doesn't close. A max attack of 30 balls is minor against a 50-cell buffer that it can't even fully use. Either the loss condition (e.g., any column reaching L1 with no move, or a height threshold) or the attack placement rule has to change before the AI opponents can be tuned.

---

## 2. The counter/gauge economy has no throughput model, and the Solo gauge targets are unvalidated against the action budget — MAJOR

**Problem.** The Solo gauge table (10/15/20/25/30 units in a fixed 3 min) is presented as "valeurs de départ" with no link to how many alignments or releases a player can realistically make per minute. Here is what the rules imply:

- Gauge units only arrive through releases, and **each release needs a full 5-ball median row of a non-counter color**. That is the hardest pattern in the game, and the guarantee only promises 5 such balls are *reachable*, not that they're assembled. Each release resets the counter to ×1.
- Level 5 asks for 30 units in 180 s. Releasing at an average of ×5 means 6 five-of-a-kind rows plus about 24 counter-color three-alignments, on a board receiving a ball every 2 s (90 extra balls against a 50-cell buffer above the 25-ball minimum).
- Eliminations don't lower board pressure when columns are at 5 balls, because they're refilled straight back to 5 (Ordre de résolution, step 4). Only clears from columns above 5 balls count against the 0.5 balls/s spawn.
- Growing the counter and preparing a release compete for the **same 5 median cells** ("Un seul alignement peut se former à la fois sur la ligne médiane"). Holding a staged release row blocks growth, and the reverse is true too. The "vrai choix" between releasing now and growing (Combo system, Contre-attaque) is therefore limited by whether a release row can be built at all within the 3 s pre-wave window. The document never addresses that.

**Source.** Thèmes des niveaux "Lien entre thèmes et difficulté" table; Combo system "Compteur de billes", "Libération du compteur", "Un alignement à la fois", "Contre-attaque"; Alignement et élimination "Ordre de résolution".

**Note.** The team already has a rules simulator (Alignement et élimination cites "Une simulation des règles a montré..."). The gauge, spawn and counter-attack window values should come from it, not from linear placeholders.

---

## 3. The teaching ladder and the cut list work against each other. Core mechanics risk never being taught — MAJOR

**Problem.**
- **Onboarding is narrow.** It is passive, runs only in the player's first game, and only highlights 3-ball alignments (Contrôles et onboarding). Nothing teaches the counter-inversion rule (same color grows it; a full line of a *different* color releases it), counter-attack cancellation, or the stack.
- **Teaching is pushed onto content that can be cut.** Zen is "entraînement et tutoriel" but is 4th on the cut list. The stack, which the Combo system calls "La tactique à adopter", is taught only by watching Arcade opponent 4. Opponent 4 is built last ("adversaires 1 à 3 et 5, puis 4") and is the last thing cut.
- **The final boss isn't protected by its prerequisites.** Opponent 5 is "never cut" and is described as demanding "de maîtriser toutes les tactiques que les précédents ont montrées". In the most likely slip scenario, the player meets the final test with no tutorial and no exposure to the stack.
- **Opponent 5 loses its level.** The difficulty table binds opponent N to level N's spawn rate. Levels 4 and 5 are cut *before* opponent 5 can be, so opponent 5 would have no level and no defined spawn rate.

**Source.** GDD "Contrôles et onboarding"; Technical Strategy "Ordre de production" and "Ce qui peut être coupé"; AI Architecture progression table; Thèmes des niveaux difficulty table.

---

## 4. The difficulty curve numbers don't produce the stated difficulty arc — MINOR

**Problem.** The emotional arc puts the "premier vrai mur" at Les Sables (level 3) and a calm-then-ablaze finale at level 5. The levers don't support that shape:

- The gauge rises linearly (+5 per level).
- Spawn rate goes 0.25 → 0.29 → 0.33 → 0.40 → 0.50 balls/s, so the steepest step is 4→5, not 2→3.
- Time is fixed at 3 min on every level.
- Required throughput triples (10→30) while board pressure doubles, with no rest level.

Nothing makes level 3 a wall. And Arcade moves two levers together (opponent behavior and spawn rate), so it's impossible to tell whether a loss came from the new AI behavior the level is meant to teach or from the faster spawn.

**Source.** Thèmes des niveaux "Liste et ordre des thèmes" (Difficulté column), "Lien entre thèmes et difficulté" table, "Arc émotionnel du voyage".

---

## 5. Systems that are named as progression levers or ranking criteria are undefined — MINOR

**Problem.**
- **Bonus/malus balls.** They appear in all four modes' progression lists ("peuvent apparaître à chaque élimination") and as a Zen option, and they have an MVP-tier audio/visual event. Their document is "à rédiger", so nobody can check how they interact with the counter, the generation guarantees, or the attack math.
- **Score.** Solo has a "classement des scores (combos et utilisation du compteur)", but no score formula exists and the Leaderboard doc is "à rédiger". So there is no way to know whether the score rewards the same behavior the gauge does (e.g., big releases vs. many small ones).
- **Unsupported content claim.** A full Solo run is 5 × 3 min = 15 min of content. The doc never says what brings a player back other than an unspecified score.

**Source.** Concept ("Des billes bonus/malus peuvent apparaître dans tous les modes"); Thèmes des niveaux "Progression par mode"; Événements "Bille bonus ou malus qui apparaît"; Documents liés ("Billes bonus/malus (à rédiger)", "Leaderboard (à rédiger)").

---

## Round 2 — Cross-examination

### CONFLICTS

**vs. Player Psychologist #4 ("Game over is invisible until it happens", MAJOR).** I accept the irreversible-switch point. I disagree with where the finding puts the danger. Under the rules as written, "plateau bloqué" needs all 75 cells filled (my #1, which Adversarial QA #1 reached independently). A surprise blocked-board game over is therefore close to the *least* likely way a player loses. The real invisible loss in Solo is the one in Adversarial QA #2: the board grows past the release threshold, the gauge can no longer fill, and the player runs out the clock on a level that is already lost. No cue and no readable state warn them. Psychologist #4 is right that players will feel cheated. It points at the wrong mechanism, though, and making the blocked-board state more visible would not fix the losses players will actually suffer. The readability work should target "release is no longer possible", not "no move is possible".

**vs. Narrative Critic #1 (cut list should protect the arc's ending rather than the mechanical boss) and Business Analyst #4 (protect Zen and adversary 4).** These two fixes compete for the same slip budget, and both treat the problem as content priority. From the systems side, a level is not a theme. It is a set of tuning values (spawn rate, gauge target, opponent pairing). Opponent 5 can technically be re-hosted on level 3's setting. If it is, its "rythme le plus rapide" becomes 0.33 balls/s, the same as opponent 3, and its identity as the hardest fight rests entirely on AI behavior. That runs into Adversarial QA #5: the 1-2 action lookahead may not be able to produce that behavior. So I disagree with both framings. The cut that matters is not "which content survives". It is whether the difficulty table still has a top tier after the cut. The document needs a cut-scenario difficulty table, not just a reordered cut list.

**vs. Adversarial QA #4 (bonus/malus: "specify them before production" or cut).** I agree on severity (see Revisions) but I disagree on the first fix option. Specifying a modifier layer on top of a base economy that doesn't close yet (my #1 and #2, QA #1 and #2) is the wrong order. Any bonus/malus spec written now would be balanced against rules that are about to change. The only sound option for the MVP is QA's second one: move them out and put them on the cut list.

**vs. Business Analyst #1 and Feasibility F3 (the missing audiovisual "wow" is the top risk).** I don't dispute that the art and music have no route to production. I do dispute the ranking implied by Business's fix (b), "reposition early access as a mechanics-first build". That fallback is only available if the mechanics hold up, and right now the competitive loop has no reachable loss state and the Solo economy can soft-lock. Fallback (b) depends on the systems blockers being fixed first. It is not a cheaper alternative to them.

### CONNECTIONS

**My #1 + Adversarial QA #2 → VS has a two-sided stall equilibrium.** The defensive posture that makes a player immune to attacks (every column long enough to sit at L1) is the same board state in which, according to the document's own simulation, a release becomes "souvent impossible à partir de 11 billes par colonne, et toujours à partir de 13". Two competent players who both turtle cannot attack, cannot be attacked, and cannot be blocked unless the undefined spawn-at-L1 rule fills them to 75/75. That is a draw that never resolves, not an edge case. Fixing either rule alone is not enough: if attacks can land at L1 but release is still impossible, the turtle just loses slowly with no counterplay.

**My #1 + Adversarial QA #5 → Arcade opponents may never attack at all.** Attacks only come from counter releases, and a release needs a full 5-ball line of a non-counter color. QA argues that 1-2 action greedy search can't plan that line. Combined with my #1, the AI's only path to victory is a player filling all 75 cells, which barely happens. Arcade's win condition for the AI may in practice be unreachable, independent of how good the player is.

**My #2 + Adversarial QA #2 → the Solo gauge targets are unreachable, not just untuned.** The restore rule only spawns useful colors into columns of 7 balls or fewer. On level 5 (0.5 balls/s), starting from 5 per column, the board needs only +15 net balls (about 30 s of net accumulation) before every column is above 7. Eliminations from columns at 5 don't reduce pressure (refill step 4). So for most of the 180 s, a level that asks for 30 units, the highest gauge target in the game, runs with its release guarantee switched off.

**My #2 note + Feasibility F4 + Adversarial QA #5 → one missing deliverable serves three needs.** Feasibility F4 says nobody is scoped to build a headless simulation harness. QA #5 says the AI needs a goal/plan-driven planner. My #2 says the gauge and spawn values must come from simulated throughput. These are the same artifact: a rules-exact simulator driven by a planning agent. It would tune the economy, power the Arcade AI, and give Agent Test something real to verify. The document should name it as one production step, owned and scheduled, before levels are tuned.

**My #5 (score) + Business #3 → the community hook has no defined target behavior.** Business Analyst wants the leaderboard to be the community anchor once online play is cut. With no score formula, it is unknown whether the leaderboard rewards the same behavior as the gauge. If it rewards something else, the Steam community will optimize against the design.

### REVISIONS

- **#1 — stays BLOCKING, strengthened.** Adversarial QA #1 reached the same conclusion on its own (L1 immunity, 75-ball game over, missing spawn-at-L1 rule). I am adding the two-sided stall equilibrium above: the fix has to address both the attack placement rule and the release guarantee together.
- **#2 — upgrade MAJOR → BLOCKING.** On its own, my finding was "values are unvalidated". Combined with QA #2, the restore rule provably has no valid column for most of a high-spawn level. Solo is "never cut", and its only win path is the gauge. A Solo mode whose win path can switch itself off cannot ship.
- **#3 — keep MAJOR, but flag it for merging.** Player Psychologist #1/#5, Business #4, QA #3, Feasibility F2 and Narrative #1 all touch the cut list versus the teaching ladder. My distinct contribution is the spawn-rate binding (opponent 5 loses its tuning tier, not only its theme). The moderator should treat this as one consensus issue.
- **#4 — upgrade MINOR → MAJOR.** Narrative #2 and Psychologist #3 show that the curve also fails at the *start*: level 1 is timed, quota-gated and spawning from second one, against a "sans pression" brief. With my point that nothing makes level 3 a wall and the steepest step is 4→5, the curve fails at both ends. A within-level spawn ramp (calm opening, rising rate) would serve the narrative brief and the difficulty shape at once.
- **#5 — split.** The bonus/malus part goes up to MAJOR, in line with QA #4, Business #2 and Feasibility F2: it ships by omission and touches every fragile rule. My recommended fix is to cut it from the MVP, not to specify it now (see Conflicts). The score and replay part stays MINOR.
