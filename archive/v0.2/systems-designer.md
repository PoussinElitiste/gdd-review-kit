# Systems Designer — Round 1

Lens: core loop, progression, pacing, difficulty curve, resource/economy math. The question for each finding is whether the systems actually interlock or just sit on the same page.

---

## F1 — The cash-out step of the core loop has no probability model, and the design puts it on the hardest action on the board
**Severity: BLOCKING**

**Problem.** In every mode, the only way to turn play into progress is to *release* the marble counter. Solo uses it to fill the gauge; VS and Arcade use it to attack. A release happens only when the player builds a full 5-wide row on the median line in a color **different** from the counter. The board is exactly 5 wide, so that means making the entire median row one color, using a color that adds nothing to the counter. Two things decide how often this can happen, and the document specifies neither:
- **Number of marble colors.** The GDD never states it. The movement schematics mention four patterns ("rond, barre, croix, anneau"), which suggests 4 colors, but this is never set as a rule. The color count is the main lever on how often 3-matches happen (counter fill) and how often 5-matches happen (release). Without it, nobody can estimate the time from release to release, so the solo time limit, the gauge size and the attack tempo can't be tuned.
- **The spawn guarantee is unclear.** "avec au minimum 5 billes de même couleur" (Cas limites, l. 87) and "une nouvelle bille est générée aléatoirement pour garantir au minimum 5 billes de même couleur" (l. 89) can be read as "5 marbles per column" or as "some color always has at least 5 marbles on the board". On the second reading it guarantees that a release is *possible*, but not that it is *reachable*. The document also never says how this guarantee squares with the "Pas d'alignement dû au hasard" rule on the median line.

The loop also creates a tension the document doesn't address. Filling the counter rewards counter-color matches. Releasing it requires a full row of a *different* color, and those 5 marbles score nothing. After a release, the counter switches to the new color and resets to zero. Nothing says how much counter value a typical release should carry, or whether releasing an empty or ×1 counter is a valid (and perhaps optimal) way to switch colors.

**Source.** GDD "Boucle d'une partie" step 4; Combo_system "Libération du compteur"; GDD "Cas limites" l. 87-89; no color count anywhere.

---

## F2 — There is no difficulty curve; run lengths (5/10/15) don't map onto the 5 levels that exist
**Severity: BLOCKING**

**Problem.** The progression section names the difficulty levers but gives none of them a value. These are gauge size per level, spawn frequency per level, AI aggression, and the solo time limit. The table that should hold them (Themes_niveaux, column "Difficulté") reads "À définir" in all 5 rows, and "Lien entre thèmes et difficulté" is deferred. On top of that, Solo is sold as runs of "5, 10 ou 15 niveaux" and Arcade as "5, 10 ou 15 adversaires", but the MVP has only "5 niveaux avec leur thème". The document never says what levels 6-15 are: do themes repeat with higher numbers, does the curve keep climbing, or does it plateau? A 15-level run is three times the length of a 5-level run, yet difficulty is tied to the 5-entry theme list. With no values and no mapping, nothing defines pacing over a full run. A balance pass or the Agent Test has no target to test against, even though Agent Test is described as testing "la boucle de jeu de chaque niveau".

**Source.** GDD "Mode de jeu" (5/10/15); GDD "Progression et thèmes"; Themes_niveaux table; MVP "5 niveaux"; no time-limit value anywhere in the document.

---

## F3 — Combos are cut off from the attack economy, so the stated core tactic has no versus payoff
**Severity: MAJOR**

**Problem.** The design cites Puyo Puyo for attack and counter-attack. In Puyo, though, chains *are* the attack multiplier, and that is where the competitive depth comes from. Here, attack strength is simply "Chaque unité du compteur libéré envoie 3 billes". Combos raise **score** ("les combos font monter le score") and fill the counter "en conséquence", which in practice means the same +1/+2/+3 per match as a non-combo match. As written, a 3-chain stack is worth exactly the same in attack terms as three separate matches, while costing more setup and more risk, since a stored line sits above the median and the board fills on its own. The document calls the stack "La tactique à adopter", but in VS and Arcade nothing rewards it beyond tempo. The solo score formula ("basé sur le compteur de combo et l'utilisation du compteur de billes") is also never given. Simultaneous matches, which a stack setup naturally produces, are deferred ("Alignements simultanés … gains cumulés, combo ou non"). As a result, the combo system and the attack system live in the same document without interlocking.

**Source.** Combo_system "Attaque" (3 balls per unit, flat); "Combos" and "Tactique : le stack"; GDD loop step 3; deferred simultaneous matches.

---

## F4 — The attack/defense timing and the loss threshold aren't specified enough for the "vrai choix" to exist
**Severity: MAJOR**

**Problem.** The design's central VS decision is "libérer son compteur tout de suite pour se défendre, ou le laisser grossir". That choice only exists if the player has a window of known length to answer. But "Délai entre deux vagues" is deferred, and so is the distribution of the 3 marbles per wave across columns. The countdown, cancellation and overflow rules all depend on a timer that has no value. The loss math has problems too:
- **Binary loss threshold.** Game over happens only when "toutes ses colonnes sont bloquées" (l. 95). A column is blocked only at 15 marbles, so the player loses only when all 75 cells are full. Above the 25-marble floor there are 50 free cells. A single release of about ×17 (51 marbles) therefore kills outright, and nothing caps it. Below that, a player with four locked columns and one partly free column is still "alive" but has almost no options. The document doesn't define any intermediate pressure state for this.
- **Landing rule.** Incoming marbles "s'empilent en haut de ses colonnes". The document doesn't say what happens when that column is already against the top limit (L1), as in schematic 4. Does it get pushed down, is the marble rejected, or is it queued?
- **Zen contradiction.** Zen is "sans limite de temps", but "Remplissage continu" applies "dans tous les modes" and guarantees the board will fill up. Zen exposes a spawn-frequency option, but the document doesn't say whether that frequency can be set to zero, so it's unclear whether Zen is really endless.

**Source.** Combo_system "Attaque" (two deferred items) and "Contre-attaque"; GDD "Plateau bloqué", "Remplissage continu"; Schemas_deplacement §4; GDD "Mode Zen score".

---

## F5 — Bonus/malus marbles are a progression lever in every mode but have no definition
**Severity: MAJOR**

**Problem.** Bonus/malus marbles appear in Arcade and VS mode descriptions, in the progression list for all four modes ("peuvent apparaître à chaque élimination de billes"), and as a Zen option ("choisir les bonus/malus actifs"). The visual/audio table gives them a priority of "Important". Yet "Billes bonus/malus" in Documents liés is an unlinked title with no content. The document doesn't define the effects, spawn rate, how the effects interact with the counter/release/attack economy, or whether they escalate by level. Because they spawn "à chaque élimination", they could change the economy math in F1 and F4 more than anything else in the game, for example by forcing a release, freezing a column, or adding marbles. They are also not in the MVP scope list or the "can be cut" list, so it's unclear whether they ship at all.

**Source.** GDD "Mode arcade", "Mode VS", "Progression et thèmes" (all four modes); Documents liés "Billes bonus/malus" (no link); GDD MVP and cut lists.

---

## Round 2 — Cross-examination

I re-checked every disputed claim below against `gdd.txt` before writing it.

### CONFLICTS

**vs. Player Psychologist #1: the hint toward a non-counter 3-match is not a "weak move".**
The psychologist says the onboarding hint "can show a move that makes no progress", because a non-counter alignment "disparaît sans effet sur le compteur". In this economy that is wrong. Non-counter matches are the player's only way to clear the board against continuous fill. The GDD says alignments are eliminated "quelle que soit leur couleur, ce qui permet au joueur de nettoyer le tableau". Non-counter colors are also the *only* colors that can trigger a release (my F1). A player who makes only counter-color matches cannot cash out and will be buried. The real onboarding failure is not the hint pointing at the wrong move. It is that nothing teaches the two-color job split: counter color to build value, other colors to survive and release. I agree with the BLOCKING severity, but the fix is to teach that split, not to steer the hint toward counter-color matches. Doing the latter would train the losing strategy.

**vs. Narrative Critic #3: escalation tied to board fill is correct feedback; the error is that nothing else escalates.**
The critic argues that in Arcade/VS, the music layering up "quand le tableau du joueur commence à se remplir" rewards being buried. From a systems view, a pressure readout tied to board fill is exactly what a versus player needs. It is the only continuous danger signal the document defines, because wave countdowns are shown only while an attack is pending. Removing it to fix the narrative would leave players blind to their own loss state, which is already hard to read (see the soft-lock connection below). I don't accept "move the reveal to success" as a fix. The systems problem underneath the critic's point is that Arcade/VS have **no positive progress metric at all**. Solo has the gauge and its "paliers"; Arcade/VS have only "my board is filling" and "theirs is filling". The resolution is a second, separate escalation channel driven by releases and cancelled waves, with fill-driven panic kept as it is. I side with the critic that the journey can't be keyed only to danger, but not with stripping the danger signal.

**vs. Feasibility Lead #2 (and Adversarial QA F2, third bullet): "simultaneous alignments" is mostly a non-issue, and I overstated it too.**
Feasibility says the deferred simultaneous-alignment rule "decides whether chains are counted at all". QA says the stack "produces exactly these cases". Both are wrong on the geometry, and so was my own F3. Only the median line eliminates, and it is 5 cells wide. Two disjoint runs of 3 or more need at least 6 cells, so **two simultaneous alignments on the median line are impossible**, whether or not the row wraps. Chains are already defined: they are sequential cascades of stacked rows falling onto the median ("Combos", "Tactique : le stack"). The deferred item only needs one sentence ("at most one alignment per resolution step"). It should not be treated as a blocker on the Gameplay agent. See Revisions.

**vs. Business Analyst #4: the Narratif agent is not decorative; it owns the empty difficulty table.**
Business calls Agent Narratif decorative because Solo/Arcade progression "is a configuration table, not narrative work". The configuration framing is right. But the agent's stated job, "génère la progression du joueur en modes solo et arcade", *is* the difficulty curve that my F2 shows has no values. Folding it into Menu or Gameplay is fine. Cutting the work is not, because those numbers (gauge per level, spawn rate, AI parameters, time limit, what levels 6-15 are) are the most under-specified artifact in the game. Rename it; don't delete it.

### CONNECTIONS

**Adversarial QA F2 + my F1 → the release is reachable almost only through the stack, which makes my F1 worse and partly answers my F3.**
QA observes that any 3+ run on the median is eliminated immediately, so a full 5-line cannot be built gradually. I verified the consequence. A **Switch can never produce a 5-line**: it rotates a row that has no pre-existing all-same coloring. A **Move** changes one median cell, so it produces a 5-line only from an `X X _ X X` row. The remaining route is a **cascade**: a median 3-match drops a stacked row into those columns and completes the color of the two cells left in place. So the release, the only way to progress in every mode, depends on either holding four non-counter marbles in a fragile split pattern or running the stack. Both cost time and score nothing. This raises my confidence in F1: release frequency isn't just unmodeled, it is structurally rare. It also means my F3 was too absolute (see Revisions). Combos *do* interlock with the economy, as the main route to a release, but not with attack *magnitude*.

**Adversarial QA F1 + my F4 → the loss condition measures board geometry, not player agency.**
QA shows that a 5-7 marble column can be moved fully off the median line (schematic 4 literally puts c3 at L1-L5), after which it can't be selected or recovered. Combined with my F4: "Plateau bloqué" checks whether any column *could* move, not whether the player *can* act. Two consequences follow. First, one stranded column leaves a gap on the median, so a 5-line is impossible, **releases are impossible**, Solo's gauge can never fill, and VS attack and defense both stop. That is a loss state the game doesn't recognize. Second, my "landing rule" question is the same bug from the other side: incoming waves land "en haut" of a column that may already be at L1. I now consider QA F1 BLOCKING and treat my F4 as the same family (upgrade below).

**Feasibility Lead #3 + my F3 → the named AI difficulty lever doesn't move the attack number.**
Arcade difficulty is described as "l'IA devient plus agressive : elle enchaîne les combos et envoie ses billes plus vite". Under the flat "3 billes par unité" rule, chaining combos raises the AI's *score*, not its attack size. The lever that actually decides VS outcomes is counter size and release timing against the (undefined) wave delay. So the one AI difficulty knob the document names targets a stat that doesn't affect the player in Arcade. A behaviour tree tuned to "chain more combos" would look harder without being harder. This makes Feasibility's "no tunable parameters" point concrete: the parameters that matter are release threshold, hold-versus-cancel policy, and moves per second.

**Player Psychologist #3 + my lens → "4 switches = 1 left" is mechanically false, not just tedious.**
The psychologist frames the right-only switch as friction. It is also a rules bug. A Switch rotates the whole median row, and rotation can bring c5 and c1 next to each other, so an intermediate switch can create a 3-run that resolves at once (e.g. `X X Y Z X` → `X X X Y Z`). The document claims "cela ne limite pas le joueur" and that four rights equal one left. That is only true when none of the intermediate rotations contains an alignment. So a player can't treat the "left move" as one planned action, and on a timer the side effects are unpredictable. This strengthens the psychologist's MAJOR.

**Business Analyst #3 + my F3 + QA F3 → even with online play, the competitive claim has no systemic support.**
Business argues the competitive audience disappears if online play is cut. The problem runs deeper. Attack doesn't scale with skill expression (combos), the counter is uncapped so hoarding can one-shot (QA F3, my F4), and the wave delay that sets the whole defensive window is deferred. A competitive scene would form around a ruleset where the dominant strategy is still unknown and the skill ceiling isn't linked to the attack output. The audience choice Business asks for is also a systems decision.

### REVISIONS

- **F1 — keep BLOCKING, confidence up.** With QA's observation, release is not only unmodeled but structurally confined to `XX_XX` + move, or to cascades. Also add the zero-counter release (QA F2) as a confirmed exploit: a ×0 release is a free counter-color reroll.
- **F3 — keep MAJOR, narrow the claim, withdraw one sub-point.** I withdraw "simultaneous matches, which a stack setup naturally produces, are deferred" as a concern: two simultaneous median alignments are geometrically impossible on a 5-wide row. I also soften "combos are cut off from the economy": cascades are the main route to a release, so they are linked to *tempo*. What stands is that attack *magnitude* is flat and ignores combos, so the competitive depth the Puyo citation promises isn't there, and the Arcade AI's difficulty lever targets the wrong stat.
- **F4 — upgrade MAJOR → BLOCKING.** Three reviewers independently found that the VS/Arcade outcome depends on undefined values (wave delay, wave distribution, counter cap): me, Adversarial QA F3, and Feasibility #2. Combined with QA F1, the only loss condition in the game misfires in both directions. Uncapped releases kill instantly, and stranded columns create unrecoverable states that don't count as a loss. A game whose win/loss condition is undetermined can't be tuned or tested.
- **F2 — unchanged (BLOCKING).** Narrative #1, QA F5 and Feasibility #2 independently hit the same 5-vs-15 gap. QA adds that a Solo time-out is never declared a loss, which means the Solo difficulty curve doesn't even have a defined failure end.
- **F5 — unchanged (MAJOR).** Business #1 lists bonus/malus under committed scope. That sharpens the question of whether they ship, but it doesn't change the economic risk.
