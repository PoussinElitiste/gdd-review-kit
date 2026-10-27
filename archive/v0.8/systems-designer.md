# Systems Designer — Round 1

Scope: core loop, progression, pacing, difficulty curve, economy math. Reviewed `gdd.txt` (GDD v0.5 plus linked docs). Overall, the combo/gauge economy is well specified on its own, but it does not interlock with the two systems that are supposed to consume it: the Arcade/VS loss condition and the Solo score.

---

## 1. Arcade/VS has no working pressure source, so the loss state may never be reached — BLOCKING

**Problem.** In Arcade and VS the only thing that adds marbles to a board is the opponent's attack. The numbers make defending much cheaper than attacking:

- A board starts with 25 marbles (5x5) and is lost at 75 (5 columns x 15). The defender has a buffer of 50 marbles, or about 17 full attack gauges (1,700 points).
- An attack sends **3 marbles per 100 points**. Any rank-1 alignment removes **3 marbles** from the defender's own board. When the columns are above the 5-marble floor, a 10-point blue triple cancels a full 100-point attack.
- The doc's own showcase combo (260 points, 3 lines) sends 6 marbles. The defender undoes that with two ordinary alignments.
- Playing offense also clears your own board. So in a symmetric match both boards drift down toward the 25-marble floor, not up toward 75.

The document admits the gap without fixing it: "La durée des manches et les cas d'égalité restent à définir." There is no timer, no garbage escalation, no sudden death and no autofill ("Il n'y a pas de remplissage automatique : la pression vient des billes que l'adversaire envoie"). This affects VS and Arcade, which the cut list says are never cut. It also undermines Agent Test's stated job: "vérifier qu'il peut gagner et perdre" is unverifiable for adversary 1 ("joue lentement… petites attaques").

**Source.** Fin de partie (Arcade et VS); Combo_system.md "Arcade et VS : billes envoyées" (3 billes par jauge, seuil 100); Alignement_et_elimination.md (floor of 5 per column, 5x15 board); AI_Architecture.md adversary 1 row.

**Why blocking.** The win condition of two never-cut modes depends on a rate (marbles in vs. marbles out) that the document never balances. It needs a designed escalation (rising attack ratio, a round timer that turns on autofill, attack marbles that cannot be cleared on arrival, etc.) before the AI ladder can be tuned at all.

---

## 2. The Solo leaderboard score is capped by the win condition, so it barely differentiates players — MAJOR

**Problem.** "Le score affiché dans le classement est le total des points de la partie." But a Solo level ends as soon as the progression gauge is full, and that gauge is counted in 100-point attack gauges (10 / 15 / 20 / 25 / 30 per level). So every player who clears level 1 scores about 1,000 points plus whatever their last sequence overshoots (and up to 99 carried points). Combos and heavy colours make you finish *faster*, but time is not scored ("sans limite de temps"). Skilful play therefore barely shows up in the score. The leaderboard mostly records "completed or not" plus overshoot noise. That is a strange fit for a mode marketed "avec un classement des scores", and it takes away Solo's reason to stack and chase heavy colours. The weight table and multiplier are both built to reward those things.

**Source.** Combo_system.md "Solo : jauge de progression" (score = total points; level won when gauge full); Themes_niveaux.md progression table; Concept "sans limite de temps, avec un classement". The Leaderboard doc is "à rédiger".

**Fix direction.** Score on time-to-clear, on points per marble placed, or on board height at completion. Alternatively, keep the level running past the gauge for score.

---

## 3. The cylindrical wrap (v0.5) breaks gravity, refill and attack placement, which are all defined in terms of "top" and "bottom" — MAJOR

**Problem.** v0.5 makes each column a 15-cell cylinder: "la case L15 est voisine de la case L1, et une colonne reste continue en passant d'un bord à l'autre." Every resolution rule still assumes a linear column with a real top and bottom:

- "les billes du dessus tombent". On a column that wraps L13-L15-L1-L2, which marbles count as "above" a cleared middle-line cell, and where do they stop?
- Refill: "les nouvelles billes apparaissent toujours par le haut de la colonne". The top of a wrapped column is undefined.
- Received attacks land "à une extrémité libre de la colonne : en haut s'il y a de la place, sinon en bas". On a cylinder, any column under 15 marbles has free space at both ends, so the rule cannot pick one.
- "Une colonne de 15 billes tourne sur elle-même" while the loss rule is "toutes les colonnes sont pleines (15 billes)". A full column is still movable, so the endgame state is unclear: can a nearly lost player keep rotating full columns forever?

The resolution order is claimed to be "fixe, pour que le plateau soit déterministe". Determinism is not established while step 1 (Chute) is undefined for wrapped columns. This also hits the stack tactic directly, since the stack depends on stored lines falling predictably onto the middle line. The Move-bloqué event (MVP) also still exists, so the feedback list has not been reconciled with the new rule set either.

**Source.** Schemas_deplacement.md "renvoi au bord du tableau" and "Limites du move b)"; Alignement_et_elimination.md "Chute et remplissage" and "Ordre de résolution"; Combo_system.md attack placement rule.

---

## 4. The skill ladder for the core tactic (stack) is fragile: taught only by a cuttable opponent, built last, and beyond the AI's stated search depth — MAJOR

**Problem.** The doc calls the stack "la façon la plus rentable de remplir la jauge" and the reason combos exist. Where is it taught?

- Onboarding only covers making a 3-match, and only during the first game.
- Solo never introduces it.
- Zen is "entraînement et tutoriel", but it is on the cut list.
- The only explicit teacher is **adversary 4** ("la tactique du stack, en la voyant chez l'adversaire").

The production plan builds adversary 4 *after* adversary 5 ("adversaires 1 à 3 et 5, puis 4"), and adversary 4 is cuttable. Adversary 5, which is never cut, "demande de maîtriser toutes les tactiques que les précédents ont montrées". Under the documented cut order, the player meets the final exam without the lesson. The difficulty curve then jumps from "combos de 2 lignes" (3) straight to "everything at max speed" (5).

There is also a gap in the AI itself. It "évalue les coups possibles à une ou deux actions… et choisit celui qui forme l'alignement le plus lourd, ou qui prépare un stack." Building a stored line usually takes several moves across three columns, and moving any of them breaks lines already stored. A 1-2 ply search with an undefined "prépare un stack" heuristic is unlikely to produce the visible 3+ line combos that opponents 4 and 5 are specified to show. The "wow" moment depends on behaviour the described architecture does not obviously support.

**Source.** Technical Strategy "Ordre de production" and "Ce qui peut être coupé"; AI_Architecture.md "Ce que fait l'IA" and progression table; Contrôles et onboarding; Combo_system.md "Tactique du stack".

---

## 5. Bonus/malus marbles are live in every mode but entirely undesigned — MAJOR

**Problem.** "Des billes bonus/malus peuvent apparaître dans tous les modes". Per Themes_niveaux.md they can appear "à chaque élimination de billes" in Solo, Arcade and VS. They also have an Important-priority VFX/SFX event. Yet their spec is "Billes bonus/malus (à rédiger)". They are not on the cut list, so as written they ship. A random modifier that can fire on every clear interacts directly with every balance lever in the doc:

- Solo gauge size vs autofill rate (one marble every 4 s down to every 2 s, or one alignment every 12 s down to every 6 s just to hold the board level)
- the attack-to-defense ratio in finding 1
- the "no free alignments" generation guarantee

None of those starting values can be playtested meaningfully until this system exists or is explicitly removed from the MVP.

**Source.** Concept "Mode de jeu"; Themes_niveaux.md "Progression par mode"; Evenements_visuels_et_sonores.md (bonus/malus row, Important); Documents liés "(à rédiger)"; absence from "Ce qui peut être coupé".

---

### Minor note (not counted)
Alignment length is nearly worthless in the economy. A 4-line adds only +5 points (red 50→55, about +10%) while spending an extra marble. Colour (x5 spread) and rank (xn) dominate. The 4- and 5-line VFX get Important priority for a choice the scoring barely rewards.

---

## Round 2 — Cross-examination

### CONFLICTS

**C1. Player Psychologist F5 (VS is a stomp: "a quick, one-sided loss") vs. my F1 (VS rounds may never end).** These cannot both be true, and the document's numbers support F1. The psychologist reasons from "Il n'y a plus de contre-attaque ni d'annulation des attaques" to "unanswerable garbage". But no counter-attack does not mean no answer. Received marbles are ordinary marbles on the defender's board, and any rank-1 alignment removes 3, which is exactly one full 100-point attack's worth. The showcase 260-point stack sends 6 marbles, and the defender clears that with two triples. To lose, the newcomer has to fall roughly 50 net marbles behind (Adversarial QA F1 reaches the same number independently). Against a weak defender the stomp is plausible, but it is slow, not quick. In the competent-vs-competent case it does not happen at all. So the party-game risk is a long, flat match with no end, not a short humiliating one. That changes the remedy. A handicap or catch-up rule, which the psychologist proposes, would make a non-terminating mode even less likely to terminate. The fix has to come first on the escalation side (timer-triggered autofill, rising send ratio). Only after that can handicaps be tuned. I keep F1 at BLOCKING and ask the moderator to record this as a disagreement on the *direction* of the VS failure, because it determines which fix comes first.

**C2. Player Psychologist F1 (stack teaching is BLOCKING) vs. my F4 (MAJOR).** I agree the teaching path is broken. Three reviewers found the same cut-list chain (psychologist F1, QA F4, business F3), so the finding is solid. I disagree about severity, for a systems reason that is actually worse news for the design. Nothing in the economy *requires* the stack. Solo has no time limit, so a player who only fires single triples still finishes every level, just more slowly, and (per my F2) gets the same leaderboard score. In Arcade/VS, per F1, a non-stacking defender is very hard to kill. A player who never learns the stack loses almost nothing mechanically. The real defect is that the core tactic is optional and unrewarded, not that it goes untaught. Teaching it better would only show players a technique the score and the loss condition then ignore. I keep F4 at MAJOR and point the board toward F1 and F2 as the load-bearing fixes. Once the stack pays off in score and in kill pressure, the teaching gap becomes BLOCKING, and I would support the psychologist's rating at that point.

**C3. Business Analyst F3 (bonus/malus is "decorative" feature creep) vs. my F5.** We agree on the action: cut it from MVP or design it. We disagree on the reason, and the reason matters for the cut list. Bonus/malus is not decorative. Themes_niveaux has it firing "à chaque élimination de billes" in all three scored modes, so it modifies every economy rate in the document. Listing it next to intro sequences as a cosmetic cut understates the risk. Leaving it in unscoped blocks balancing, while leaving intros in unscoped costs only polish. The psychologist (F5) also names bonus/malus as a likely party-game equalizer. That is a second reason not to let it ship undesigned: an undesigned random equalizer layered on a non-terminating VS economy cannot be tuned at all.

### CONNECTIONS

**X1. Feasibility F2 + my F1 + my F4: the AI cannot generate kill pressure, so Arcade cannot be lost by the player either.** Feasibility and I independently concluded that a 1-2 ply greedy search will not build stacks. Put that next to the F1 rate math. An AI that only fires immediate alignments sends at best about 3 marbles per 2-10 alignments (10-50 points each against a 100-point threshold). The player removes 3 marbles with each of their own alignments. Opponent 5 is "tous ces comportements, au rythme le plus rapide", but faster single lines raise the AI's defense more than its offense. As specified, the never-cut "wow" opponent is mathematically close to unable to beat a player who keeps clearing. That means Agent Test's "peut gagner et perdre" check fails for the *win* side of the AI too, not only for adversary 1. The AI design question and the pressure-economy question have to be solved together.

**X2. Feasibility F3 (no headless simulator) is the cheapest way to settle F1, F2 and F5.** Every disputed number on this board (VS drift toward the floor, Solo level-1 load per the psychologist's F2, the leaderboard overshoot exploit in QA F3) is a rate question that a deterministic board simulator plus a greedy bot would answer in an afternoon of batch runs. My F3 (cylinder breaks gravity and determinism) is also a *prerequisite* for that simulator, since you cannot write a deterministic sim for an undefined "Chute" step. That gives the board a clear dependency chain: define wrapped gravity, refill and attack placement → build the sim → measure the pressure ratio → then tune the AI and the gauges. Feasibility's step-1 harness recommendation should name F3 as its blocking input.

**X3. Narrative Critic F3 + Psychologist F2: the difficulty curve is a single monotonic ramp with no intra-level shape, and the milestone system that could shape it is wired only to audio.** Narrative wants a calm opening for La Nébuleuse. The psychologist shows La Source is not "sans pression" at 1 marble / 4 s (one alignment every 12 s just to hold level, against ~33 average triples to reach 1,000 points). Both point to the same systems gap. Refill rate is fixed per level, while the 5 "jalons" per level only add music layers. Tying refill rate to milestones (slow at jalon 0, nominal by jalon 2, peak at jalon 5) would give each level an arc, deliver "sans pression" for level 1 and the "calme puis embrasement" script for level 5, and cost almost nothing to implement. That is a real interlock the document already has the pieces for.

**X4. Psychologist F2 + my F1: the same action is under-rewarded in one mode and over-powered in another.** In Solo, a light-colour defensive triple barely moves progress (10 / 100 of a gauge, out of 1,000+). In Arcade/VS, the same triple fully cancels an incoming 100-point attack. The game teaches opposite lessons about defensive clearing depending on the mode, and Solo, where the player learns, teaches the lesson that is wrong for Arcade. Weights should be reviewed per mode, or the attack conversion should be weight-aware.

**X5. Narrative F1 / QA F4.2 + progression curve.** If levels 4 and 5 are cut, the Solo curve ends at level 3, which the arc itself labels "premier vrai mur". From a pacing standpoint that is a run ending on its difficulty spike with no consolidation level after it. The Arcade ladder is mapped row-for-row to levels 1-5, and its top rung (never cut) would sit above a Solo curve that stops two steps lower. The two ladders are meant to rise in parallel. The cut list breaks that alignment without saying so.

### REVISIONS

- **F1 (BLOCKING): upgraded in confidence.** QA reached the same rate math independently. X1 shows the problem also stops the AI from winning, not just the player from losing.
- **F2 (MAJOR): revised in substance, severity unchanged.** Adversarial QA F3 is right that I understated the overshoot. Progression only resolves "à la fin de la séquence" and the rank multiplier is unbounded, so the overshoot is not noise. It is the *only* variable, and it is exploitable: stall at N−1 gauges, build one huge chain, fire. The leaderboard therefore does not fail to differentiate. It differentiates on a single degenerate tactic, with autofill as the only check. That is a worse failure than I described. My fix list stays the same, plus one addition: cap overshoot or score per marble placed so a stall-and-dump is not optimal.
- **F3 (MAJOR): unchanged in severity, scope widened.** QA F2 adds the median-line invariant contradiction (empty median cells during a cascade) and the "toujours un move" vs. MVP "Move bloqué" contradiction. Both come from the same unreconciled v0.5 edit. Per X2, F3 is also a dependency for any simulation-based tuning, which makes it effectively a precondition of the F1 fix.
- **F4 (MAJOR): unchanged; AI sub-point handed off.** Feasibility F2 covers the search-depth issue more fully. I defer to it there and keep F4 focused on the progression-ladder angle and the argument in C2.
- **F5 (MAJOR): unchanged.** Business and Feasibility both confirm it is absent from the cut list and from production. My recommendation is to explicitly remove it from MVP until it is designed.
