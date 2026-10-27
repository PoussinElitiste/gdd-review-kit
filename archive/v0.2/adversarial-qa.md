# Adversarial QA — Round 1

Reviewer lens: contradictions, edge cases, sequence breaks, exploits, unplanned fail states.

---

## F1 — Columns can leave the median line and become unselectable: a soft-lock — BLOCKING

**Problem.** Every action starts by selecting a marble *on the median line* ("Un curseur se déplace sur la ligne du milieu du tableau pour sélectionner une bille", GDD Règle de déplacement). Moves are limited only by the top and bottom of the board ("son mouvement est bloqué par les limites haute et basse du tableau"). A 5-marble column can therefore move until it sits entirely above or below the median line. The movement diagrams say this outright: column c3 "touche le haut (L1)", so its 5 marbles fill L1–L5 and the median line (L8) is empty in that column. From that point:
- the column has no marble on the median line, so it cannot be selected and **can never be moved back**;
- the median line has an empty cell, and nothing says what a Switch does with a gap (does the gap rotate? does the switch become illegal?);
- alignments on that row now need 3 *contiguous* marbles out of 4 cells, and if two columns drift away a 5-line becomes impossible, so the counter can never be released (see F2).

The game-over rule doesn't catch this. "Plateau bloqué" fires only when "toutes ses colonnes sont bloquées par les limites du tableau", and any column with fewer than 15 marbles can still move one way. A player can strand columns, lose all agency, and get no game over until the continuous fill finally packs every column to 15. In Zen mode, where spawn frequency is a player option, that may never happen.

**Source.** GDD "Règle de déplacement" (cursor on the median line, move limits); Schemas_deplacement §4 "Limites du tableau"; GDD "Cas limites / Plateau bloqué".

**Fix direction.** Clamp moves so that at least one marble of every column always stays on the median line, or let the cursor select any column (not only marbles on the median line). Define the Switch behaviour for empty cells.

---

## F2 — The counter release rule contradicts itself across documents, and the edge cases break it — MAJOR

**Problem.** The GDD says "une ligne complète de 5 billes le libère" (Boucle de jeu step 4; Alignement et élimination), with no colour condition. Combo_system says the opposite for one case: a 5-line releases the counter **only if its colour is different** from the counter's. A same-colour 5-line just adds +3. Whoever implements the Gameplay agent's spec gets two incompatible rules. Cases nobody defined:
- **Release at zero.** The counter starts at 0 in a random colour. A different-colour 5-line formed while the counter is 0 "releases" a ×0 counter. Does that send nothing, fill nothing, and just change the colour? This is a free way to reroll the counter colour.
- **The 5-line is nearly unreachable by direct play.** Any 3+ alignment on the median line is eliminated at once, and a Move changes only one cell of that line. So a 5-line can only come from an `X X _ X X` pattern, and only if wrap-around runs (c5→c1) don't count. The document never says whether the row is cyclic, even though the Switch wraps. The only release trigger may effectively depend on cascades. That matters most in Solo, where gauge progress depends entirely on releases.
- **Simultaneous alignments** (one move creating several, or a cascade that lands a 5-line and a 3-line together) are explicitly "Reporté". Yet the stack tactic is the designed core, and it produces exactly these cases.

**Source.** GDD "Boucle d'une partie" step 4 and "Alignement et élimination" vs. Combo_system "Libération du compteur"; Combo_system "Reporté : Alignements simultanés".

---

## F3 — The counter has no cap and wave timing is undefined: hoarding gives one-shot kills in VS/Arcade — MAJOR

**Problem.** The counter has no stated maximum. Each unit sends 3 marbles. The whole board holds 75 cells (5×15) and starts with 25 marbles, so only about 50 marbles fit before every column is full. A counter of ×17 (51 marbles) overfills the opponent's board from any state. The document itself treats ×10 as a normal example, and the design encourages hoarding: "le laisser grossir pour une contre-attaque plus forte". The only defence is cancelling, and it depends on two values that are explicitly deferred: the "Délai entre deux vagues" and how marbles are spread across columns. If the delay is short, a large release can't be answered. If it's long, turtling wins. The win condition is decided by numbers the document doesn't contain. Also undefined:
- whether incoming marbles can be placed into a column that is already at 15 (overflow is lost? goes to another column? instant loss?);
- what happens when both players hit "Plateau bloqué" on the same tick (a draw in a best-of-1/3/5);
- in Solo, whether one huge release fills the gauge in a single shot, which sidesteps the per-level time pressure.

**Source.** Combo_system "Attaque" (3 marbles per unit, ×7 example) and "Contre-attaque" (×10 example, the hoarding choice); the two "Reporté" items under Attaque; GDD "Plateau bloqué".

---

## F4 — Zen mode breaks the "finite game" guarantee, and its leaderboard can be exploited — MAJOR

**Problem.** The GDD promises "une partie ne peut pas durer indéfiniment" because the continuous fill applies "dans tous les modes". Zen mode then exposes "options pour régler la fréquence d'apparition des billes" and "options pour choisir les bonus/malus actifs", with no time limit and no opponent. Unless there is a minimum frequency, the fill can be set to zero or very low and the game never ends. The F1 soft-lock then has no exit at all. Zen also feeds a **combo-record leaderboard** ("battre le record de combo du classement"). Players can set the lowest spawn rate and only beneficial bonuses, so scores from different settings are not comparable and the board ends up full of whatever the easiest legal configuration allows. The bonus/malus document that would bound this ("Billes bonus/malus") is listed under Documents liés but doesn't exist. Zen also has no defined end condition, and no rule for what a counter release does when there is neither a gauge nor an opponent.

**Source.** GDD "Remplissage continu" vs. "Progression et thèmes / Zen score"; "Mode Zen score"; "Documents liés" (Billes bonus/malus, Leaderboard: no link, no content).

---

## F5 — 15-level and 15-opponent runs are promised, but only 5 levels exist; difficulty and fail states past level 5 are undefined — MAJOR

**Problem.** Solo offers runs of "5, 10 ou 15 niveaux" and Arcade offers "5, 10 ou 15 adversaires … à travers différents niveaux". The MVP and Progression sections commit to exactly "5 niveaux distincts". The themes document links difficulty to theme order, and all 5 rows are "À définir". So nothing says what levels 6–15 are (repeated themes? reused intros, even though the Intro agent makes "une séquence différente pour chaque niveau"?) or how gauge size, spawn rate and AI aggression keep rising past level 5. Related fail states are also missing:
- **Solo time-out is never stated to be a loss.** The only defined game over is "Plateau bloqué". The end screen offers "recommencer en cas de défaite", but nothing says whether that restarts the level or the whole 15-level run, or how a mid-run restart affects the run's leaderboard score. That is a score-farming path.
- **Onboarding fires in the "toute première partie … quel que soit le mode"**, including VS and Arcade. Highlighting alignable marbles for one player in a competitive match is an asymmetric aid. In local VS, which player is "first-time" is undefined. If the first game is quit after a few seconds, the onboarding is consumed and never shown again.

**Source.** GDD "Mode de jeu" (Solo/Arcade run lengths) vs. "Progression et thèmes" and MVP (5 levels); Themes_niveaux (all "À définir"); GDD "Boucle de jeu" end screen; "Contrôles et onboarding".

---

### Minor notes (not counted above)
- Two incompatible generation rules: "au moins 5 billes par colonne" (Alignement et élimination) vs. "garantir au minimum 5 billes de même couleur" (Cas limites). The number of marble colours is never stated (the diagrams suggest 4 patterns).
- "Plateformes : PC, console, mobile et VR". VR has no control scheme. Local two-player "sur toutes les plateformes" has no input scheme on mobile.
- MVP lists "VS en local" while the cut list says "Mode multijoueur". The VS mode text includes online play. Whether online VS is in or out is ambiguous.

---

## Round 2 — Cross-examination

### Conflicts

**vs. Systems Designer F3 ("Combos are cut off from the attack economy").** I disagree with the conclusion that the stack "has no versus payoff". Systems treats the release as one more action that a combo neither helps nor hurts. But my F2 shows that a direct 5-line is almost impossible to build by hand. Any 3-run on the median is eliminated the moment it forms, and a Move changes only one median cell. So the only manual route is `X X _ X X` plus one Move (or a Switch that happens to close the gap). That leaves cascades, where a stored row falls onto the median, as the realistic way to *produce* a 5-line. So the stack is coupled to attack, through the release trigger rather than a multiplier. The real defect is that this coupling runs through the one case the document explicitly defers: "Alignements simultanés" (a cascade that lands a 5-line together with other matches). Systems is right that combos don't scale attack size. But "nothing rewards it beyond tempo" understates the problem. The release trigger depends on an undefined rule, which is worse than a weak reward.

**vs. Player Psychologist F3 (no undo, right-only switch as a frustration loop).** The friction diagnosis is fair. But the obvious fix, undo, is an exploit vector unless the document restricts it carefully. Eliminations spawn "une bille de couleur aléatoire" from the top. An undo that reverts a move after its elimination resolved would let a player reroll the RNG until a good spawn appears. In VS, an undo during a pending-wave countdown would have to define whether a release that cancelled waves can be taken back. A left Switch is harmless from my lens and is the cheaper fix. If undo is added at all, it must be Solo/Zen only, limited to a Switch that triggered no elimination, and it must not touch the RNG state.

**vs. Narrative Critic F2 and Player Psychologist F5 (Zen should be a truly calm, pressure-free space).** Both reviewers want Zen to stop forcing a game over, which means a spawn frequency of zero. From my lens, that is exactly what produces F4: a game with no end condition, a combo leaderboard that can be gamed through settings, and, combined with F1, a stranded board with no exit. I don't oppose a calm Zen. I oppose a calm Zen that keeps a shared leaderboard. If the board takes the narrative/psychology position, the document must also add (a) a voluntary end-and-score action, (b) leaderboards segmented by spawn and bonus settings, or no leaderboard at minimum frequency, and (c) a fix for F1, because without fill pressure a soft-lock never ends. I'm escalating this as a design decision, not an oversight.

**vs. Business Analyst F4 ("Make Zen a Solo setting").** This is a cheap scope cut, but it moves F4's exploit into Solo. If "no timer" and "spawn frequency" become Solo options, the Solo leaderboard ("enregistrer ses records dans un classement") inherits the non-comparable-scores problem. The merge is only safe if scores from modified settings are excluded or tagged.

### Connections

- **Feasibility F5 (Agent Test can't really test) + my F1.** The soft-lock in F1 is the kind of state a headless, deterministic board simulation finds in minutes (any column of fewer than 8 marbles can reach a position with no median marble). A Haiku agent "testing the loop of each level" without a simulation will not find it, and a human playtester may not either, because skilled players avoid it. Without the simulation Feasibility asks for, F1 is likely to ship.
- **Feasibility F3 (behaviour-tree NPC) + my F1, F2, F3.** Arcade has no time limit. Only continuous fill ends a match. An NPC that strands its own columns (F1) makes an Arcade match that can only end through fill. An NPC that plays well will also find the ×0 release colour reroll (F2) and hoarding to about ×17 for a one-shot kill (F3, which Systems F4 derived independently). "Plus agressive" with no cap on these behaviours means the difficulty ramp can produce unwinnable matches. The NPC spec needs explicit rules on release thresholds.
- **Systems F1 + Psychologist F2 + Feasibility F2 + my F2.** Four lenses land on the same rule. Systems: no probability model for the release. Psychologist: players will think the rule is broken. Feasibility: an implementing agent will pick one of the two readings. Mine: the GDD and Combo_system state contradictory rules, and the ×0 case is undefined. Combined, the only progress action in every mode is contradictory, untaught, statistically unmodelled and exploitable at zero. I'm upgrading accordingly (see Revisions).
- **Narrative F4 (victory and defeat share an ending) + my F3 (simultaneous lockout).** The document has one ending beat for both outcomes and no outcome at all for a draw. When both boards lock on the same tick in a best-of-3, the match state, the scoring and the presentation are all undefined. Narrative's fix (distinct win/lose beats) has to include a draw case or a tie-break rule.
- **Business F3 (the competitive audience depends on cut online play) + my F3.** Even if online play were kept, an uncapped counter with an undefined wave delay makes high-level VS a hoarding race decided by an unknown constant. The competitive claim fails on rules as well as on scope.
- **Psychologist F1 (onboarding too thin) + my F5 (onboarding asymmetric in VS).** Any expansion of onboarding makes my asymmetry worse if it keeps firing "quel que soit le mode". Richer onboarding must be per-player, opt-in or replayable, and not consumed by a game quit in the first seconds.

### Revisions

- **F2: upgrade MAJOR to BLOCKING.** Three colleagues independently hit the release rule from different angles (see Connections). The contradiction is no longer an edge case. It is the spec of the only progress action in all four modes, and an implementing agent will encode one of two incompatible rules.
- **F1: keep BLOCKING, and strengthen it.** Re-reading the Switch rule makes this worse than I stated. A Switch rotates the median row ("la bille en bout de ligne passe au début de la ligne"). If one column has no median marble, the Switch moves a gap into its neighbour's column. That splits the neighbour's column, and gravity ("les billes situées au-dessus tombent") then collapses it, which changes column lengths and can trigger the under-5 spawn. Schematic 4 itself describes a column moving from L1 "jusqu'à ce que sa dernière bille touche L15", which passes through positions with no median marble, so the documents confirm the state is reachable. No other reviewer flagged this. I'd ask the moderator to weigh it as a single-source but document-verified finding.
- **F3: keep MAJOR, now with higher confidence.** Systems F4 derived the same ×17 one-shot independently.
- **F4: keep MAJOR,** reframed as the counter-position in the Zen disagreement above.
- **F5: narrow the scope and keep MAJOR.** The "levels 6–15 undefined" half duplicates Systems F2 and Narrative F1, which own it. My distinct contribution is the fail states: Solo time-out is never declared a loss, the scope of "recommencer" (level or whole run) is undefined, which enables score-farming, and onboarding is asymmetric in VS. Those keep it at MAJOR.
