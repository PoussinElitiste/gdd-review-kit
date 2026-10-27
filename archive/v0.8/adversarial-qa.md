# Adversarial QA — Round 1

Reviewer lens: contradictions, edge cases, exploits, unplanned fail states.
Source: `gdd.txt` (GDD v0.5 + linked documents).

---

## F1 — Arcade/VS rounds have no forcing function and may never end — BLOCKING

**Problem.** The only loss condition in Arcade and VS is "tableau plein": a received marble must enter while *all five* columns hold 15 marbles (75 marbles total). Boards start at 25. There is no auto-fill in these modes, so the only way to add marbles is enemy attacks. The attack economy does not favor the attacker:

- 100 points = 3 marbles sent. A red 3-line (the heaviest) is worth 50 points and removes 3 of the sender's own marbles, so sending 3 marbles costs at least 6 of your own marbles without a combo. With a blue 3-line (10 points), it takes 10 lines (30 marbles removed) to send 3.
- Even the doc's own showcase combo (Combo_system, "Exemple pas à pas") removes 10 of the sender's marbles to send 6.
- The defender removes at least 3 marbles every time they clear a line, and refill only tops columns back up to 5 ("Les colonnes de moins de 5 billes reçoivent ensuite de nouvelles billes"). Marbles cleared from columns taller than 5 are never replaced.
- Received marbles go into the *shortest* columns, so the defender's board fills evenly. To lose, the defender has to fall behind by about 50 net marbles.

The result is that two competent players (or a player against an AI held to "un nombre maximal d'actions par seconde") both drift toward the 25-marble floor. Nothing forces the round to end. A purely defensive player who clears blue lines and never attacks is very hard to kill. The doc admits the gap: "La durée des manches et les cas d'égalité restent à définir" (Fin de partie). Ties are also undefined when both players' end-of-sequence attacks land at the same time and both boards overflow.

**Source.** GDD "Fin de partie" (Arcade et VS); Combo_system "Arcade et VS : billes envoyées" and "Exemple pas à pas"; Alignement_et_elimination "Chute et remplissage"; AI_Architecture "Son rythme est limité".

**Why blocking.** These are the win/loss rules for 2 of the 4 MVP modes. They also underpin Opponent 5, the "wow" moment that is "jamais coupé". The Agent Test requirement that each opponent "peut gagner et perdre" cannot be checked while rounds have no termination guarantee.

---

## F2 — The cylinder wrap (v0.5) breaks "up", "down", gravity and "top of column" — MAJOR

**Problem.** v0.5 makes each column a 15-cell cylinder ("la case L15 est voisine de la case L1, et une colonne reste continue en passant d'un bord à l'autre"). Several other rules still assume a board with a real top and bottom:

- **Gravity.** "les billes du dessus tombent". Take a 10-marble column whose last marble sits on the median at L8. It legally spans L15/L14 → L1…L8 across the seam. When its median marble is eliminated, the doc does not say whether L14/L15 count as "above" (wrapping down through L1) or "below".
- **Refill.** "les nouvelles billes apparaissent toujours par le haut de la colonne". On a wrapped column, the top end is L14 at the bottom of the screen.
- **Received marbles.** These enter "à une extrémité libre de la colonne : en haut s'il y a de la place, sinon en bas". On a cylinder, a contiguous column's two free ends are the same gap, so "sinon en bas" can never apply differently, or the rule means something unstated.
- **Determinism.** The doc claims "même situation, même résultat", but that only holds if the points above are specified.

The same area has related invariant contradictions:

- The GDD says "La ligne médiane ne doit jamais avoir de case vide" and the board "contient toujours au moins 5x5 billes". Alignement_et_elimination ("Colonne vidée par une cascade") explicitly allows a column with zero marbles and an empty median cell for the rest of a cascade.
- The GDD says "le joueur peut donc toujours faire un move". The Events doc ships a MVP "Move bloqué" event, and Schemas 4a says a 5-marble column at its limit "ne peut pas monter".

**Source.** GDD "Règle de déplacement" and "Plus de plateau bloqué"; Schemas_deplacement "Règle détaillée" and 4a/4b; Alignement_et_elimination "Chute et remplissage", "Colonne vidée par une cascade", "Ordre de résolution"; Combo_system "Elles tombent dans les colonnes les plus courtes"; Événements "Move bloqué".

---

## F3 — The Solo leaderboard is degenerate and rewards stalling — MAJOR

**Problem.**

- Solo score is "le total des points de la partie" (Combo_system).
- A level ends as soon as the progression gauge fills, at a fixed count (10/15/20/25/30 attack gauges of 100 points), and there is "sans limite de temps".
- So every winning score on a level is roughly the threshold × 100, plus whatever the *final* sequence overshoots.
- Progression only advances "à la fin de la séquence", so the optimal leaderboard play is:
  1. Reach N−1 gauges.
  2. Stop forming median lines.
  3. Build the tallest possible stack.
  4. Fire a single huge chain. The rank multiplier is unbounded (×4, ×5…).
- Auto-fill is the only check on this. It costs nothing until the board is near full, and losing just allows a replay ("le niveau est perdu et peut être rejoué").

The leaderboard therefore measures one combo, not a run. Time is not a factor at all. The doc never says whether the leaderboard is per level or per run, or whether a failed-then-replayed level keeps its points. "Leaderboard (à rédiger)" is still unwritten even though Solo "classement" is MVP.

**Source.** Combo_system "Solo : jauge de progression" (score = total points) and "Multiplicateur" table ("× 4 … …"); Themes_niveaux progression table; GDD "Mode de jeu" (Solo); "Documents liés" (Leaderboard à rédiger).

---

## F4 — The cut list and onboarding can cut the player off from what Opponent 5 tests — MAJOR

**Problem.** The never-cut items depend on items the doc allows to be cut.

1. **Opponent 4.** It is cuttable ("puis l'adversaire 4"). It is the *only* opponent that teaches stacking ("la tactique du stack, en la voyant chez l'adversaire"). Opponent 5 is never cut and "demande de maîtriser toutes les tactiques que les précédents ont montrées". If Opponent 4 is cut, the finale tests a tactic nobody showed the player.
2. **Levels 4 and 5.** They are cuttable. Arcade opponents map to levels in the Themes table ("5 La Nébuleuse … n°5"). If level 5 is cut, the never-cut Opponent 5 has no theme, environment or music, and loses its "wow" staging.
3. **Zen.** It is cuttable, but it is the designated "entraînement et tutoriel".
4. **Onboarding.** The only other teaching (passive hint + alignable highlight) runs "uniquement lors de la toute première partie du joueur, quel que soit le mode":
   - A player who quits or loses within seconds never sees it again.
   - If the first game is local VS, the doc does not say which player gets the hint or whether the highlight is shown only to one side, which would be an unfair assist.
   - If the first game is Arcade, the hint competes with incoming attacks.

**Source.** Technical Strategy "Ce qui peut être coupé, dans cet ordre"; AI_Architecture opponent table (row 4, row 5); Themes_niveaux difficulty table; GDD "Mode de jeu" (Zen = tutoriel); "Contrôles et onboarding".

---

## F5 — The budget contradicts its own Haiku pricing, and the cost guardrail does nothing — MINOR

**Problem.**

1. **Haiku cost is wrong by its own rules.** Budget_tokens says Haiku's 0.10 $/0.50 $ rate applies only to requests under 100,000 tokens, and "0,50 \$ / 2,50 \$ au-delà". Every session is assumed to read about 1.5 M input tokens, uncached for Haiku. A Haiku session should therefore cost about 1.5 × 0.50 + 0.06 × 2.50 ≈ 0.90 $, not 0.18 $. Agent Test (40 sessions) comes to about 36 $, not 7 $. That is roughly 29 $ the MVP total leaves out. Separately, the doc does not say whether a 1.5 M-token context even fits a Haiku session.
2. **The overspend rule does nothing.** The rule is that if half the cap is spent before mid-project, "les tâches courantes passent d'Opus à Sonnet". But the budget table already puts routine tasks on Sonnet, and Opus only covers Gameplay, NPC and Multi. As written, the trigger only cuts online multiplayer, which is already outside the MVP. There is no defined action if the 450 $ cap itself is reached.

**Source.** Budget_tokens "Tarifs utilisés" (Haiku footnote), "Coût moyen d'une session" (Haiku line), "Estimation par agent" (Agent Test row); Technical Strategy "Contraintes techniques et API" item 4.

---

## Round 2 — Cross-examination

### 1. CONFLICTS

**C1 — Player Psychologist #5 ("VS ... a quick, one-sided loss") vs. my F1 (VS rounds may never end).**
Psych assumes that a stack-literate player sends "multiplied garbage that the other player cannot answer". The attack economy in the document does not support "quick". The doc's best showcase combo (260 points) sends **6 marbles**, and the defender's board needs about **50 net marbles** to overflow. Received marbles go into the *shortest* columns, which spreads them out and stops any one column from spiking. Every clear the defender makes, however weak, removes 3. A newcomer who keeps making blue 3-lines removes marbles faster than a skilled opponent can add them. The stomp scenario only happens if the newcomer *stops clearing* entirely.
I accept part of Psych's point. The "no counter-attack, no cancel" line ("Il n'y a plus de contre-attaque ni d'annulation des attaques") does remove the usual comeback tool. My position is that local VS has **both** failure modes and that they pull in opposite directions (see N1 below). Psych's MINOR rating understates the problem. The document cannot ship a party mode whose round may not terminate.
Correction to Psych's last sentence: the document does say something about what happens after an Arcade loss. GDD "Boucle de jeu" says the end screen offers to "recommencer en cas de défaite". What is undefined is the *scope* of the retry: does it restart the current round, the best-of-1/3/5 set ("en 1, 3 ou 5 manches gagnantes"), or the whole 5-opponent series?

**C2 — Systems Designer #2 fix direction vs. my F3 (stalling exploit).**
I agree with Systems on the diagnosis, but we read the symptom differently. Systems says the score "barely differentiates players" (overshoot noise). I say overshoot is *unbounded*, because the rank multiplier has no ceiling ("× 4 … …"). Under Systems' reading the leaderboard is flat. Under mine it measures a single final chain. Both readings agree it doesn't measure the run. Two of Systems' proposed fixes make the exploit worse:
- **"Board height at completion"** directly rewards what my F3 exploits: hoarding marbles until autofill nearly kills you. It turns the leaderboard into a game of chicken with the game-over condition, and failure is free ("peut être rejoué").
- **"Points per marble placed"** has no clear referent in Solo. The player never places marbles; autofill and refill do. If it means "per action", it rewards long chains even more, which again means one final mega-stack.
- **"Time-to-clear"** is the only fix that resists stalling. But it contradicts "sans limite de temps" and the contemplative framing in Narrative #3, so it is a design-identity decision, not a tuning tweak.
- **"Keep the level running past the gauge"** is safe only if autofill keeps accelerating. Otherwise it just moves the stall to a later point.

**C3 — Narrative Critic #1 (BLOCKING) vs. my reading of the same cut.**
Narrative rates "levels 4 and 5 cuttable" as BLOCKING because the journey loses its ending. From a QA lens, the narrative loss is real but sits downstream of a structural defect Narrative did not mention. The Themes table binds *opponent n* to *level n* ("5 La Nébuleuse … n°5"). Cutting levels 4-5 therefore strips the stage from the never-cut opponent 5 as well as from the Solo ending. That makes this a cross-mode dependency failure, not only a story gap. I support BLOCKING, but on the grounds that the cut list is internally invalid (see R1), not on narrative-payoff grounds alone.

**C4 — Player Psychologist #1 ("the stack is the whole game", BLOCKING) vs. my F4 (MAJOR).**
Taken literally, this overstates things for Solo. In Solo, stacking only makes you finish *faster*. Time isn't scored (Systems #2, my F3), and holding stored lines off the median increases autofill risk. A player who never stacks can still clear every Solo level by grinding rank-1 lines; it is just slower. So Solo is not blocked by missing stack teaching. The stack is load-bearing in **Arcade**, where opponent 5 "demande de maîtriser toutes les tactiques". Teaching is a symptom there. The root defect is the dependency graph of the cut list, which is why I am upgrading F4 on that basis (R1) and not on Psych's.

**C5 — Systems Designer #3, last bullet ("can a nearly lost player keep rotating full columns forever?").**
This is not a real fail state. The loss trigger is event-based: "quand une bille doit entrer et que toutes les colonnes sont pleines". It is not state-based, and rotating changes neither the marble count nor the trigger. In Solo the next autofill tick ends the game anyway. In VS a player at 75 marbles is not lost and can still clear: every full column has a marble on the median, so lines remain formable. The real edge case is the reverse one. In VS, a player sitting at exactly 75 marbles is safe until the opponent *finishes a sequence*. So the opponent's last move has to be a chain that ends, and per my F1 the defender can clear first. This strengthens F1 rather than adding a new bug.

### 2. CONNECTIONS

**N1 — VS has two opposite failure modes, and no lever fixes both (my F1 + Psych #5 + Systems #1).**
Symmetric skill leads to a stalemate (F1). Asymmetric skill leads to a stomp, assuming the weaker player stops clearing (Psych #5). Systems #1 proposes escalation, such as a rising attack ratio or autofill on a timer. That fixes the stalemate but makes the stomp faster. A handicap fixes the stomp but extends the stalemate. The document has neither lever and has "Mode VS (à rédiger)". Both must be designed together, which is a stronger requirement than either finding alone.

**N2 — The Agent Test harness cannot be built on the current rules (Feasibility #3 + my F1 + my F2).**
Feasibility correctly says Agent Test needs a headless simulator plus a bot. From my side:
- A bot-vs-bot Arcade simulation under the current rules **may not terminate** (F1). "Vérifier qu'il peut gagner et perdre" therefore needs a timeout, and that timeout *is* the undefined "durée des manches". Agent Test would be inventing a design rule.
- A deterministic simulator needs gravity, refill and attack placement defined on a wrapped column (F2). Today Agent Gameplay (Opus, step 1) will silently choose a convention, and Agent Test (Haiku) will validate against whatever was chosen. The doc's "même situation, même résultat" claim will be true of the code but not traceable to the design.

**N3 — A greedy AI makes F1 worse, not better (Feasibility #2 / Systems #4 + my F1).**
Both colleagues show that a 1-2 ply search ("à une ou deux actions") can't reliably build stacks. Without stacks the AI's attacks are rank-1 lines: at most 50 points, so at least two red lines per 3 marbles sent. Against that output, a human defender at any skill level who just keeps clearing **cannot be overflowed**. Opponent 5, the never-cut "wow", is then not only hard to build. It may be *structurally unable to win*, which fails the Agent Test criterion "peut gagner et perdre" from the other side.

**N4 — Identical win/loss feedback becomes a correctness bug in VS and on ties (Narrative #5 / Psych #3 + my F1 tie case).**
Both colleagues treat the identical "explosion, puis dissipation de la scène" for Game over and Victoire as an emotional problem. In local VS it is a **readability bug**, because both boards are on screen and the doc defines no result screen beyond "score et détail des combos". In the tie case I raised in F1 (both boards overflow at the end of simultaneous sequences), both boards trigger the same maximal explosion. The game has no defined winner and no way to show a draw.

**N5 — Local VS on keyboard has no second-player mapping (Psych #4 + Business #4 + Controles.md).**
Controles.md lists "A/D … ou 4/6 du pavé numérique", "W/S, ou 8/5", "Q/E, ou 7/9" as *alternatives* for one player, while stating "le jeu peut se jouer en local à deux". The doc never assigns the WASD/QE cluster to P1 and the numpad to P2. Two consequences:
- Laptops without a numpad, which are common among itch.io PC buyers (Business #4 platform-fit concern), cannot run keyboard-only VS at all.
- Mixed keyboard + gamepad sessions are unspecified.

VS is never cut, so this is a gap in a protected mode.

**N6 — Making Level 1 gentler cheapens my F3 exploit (Psych #2 + my F3).**
Psych argues that level 1's autofill (1 marble / 4 s) is too punishing for novices. Autofill is the *only* cost on stalling for a final mega-chain. Any slow-down for La Source (or Narrative #3's proposed calm opening phase for La Nébuleuse) makes the exploit cheaper on exactly those leaderboards. These fixes are compatible only if the score formula is decoupled from completion first (C2).

### 3. REVISIONS

**R1 — F4 upgraded MAJOR → BLOCKING.**
Five of six reviewers independently hit the cut list: Systems #4, Narrative #1, Psych #1, Business #3 and Feasibility #2. Combined, they show there is **no valid cut sequence**. Cut item 3 (levels 4-5) removes the stage for never-cut opponent 5 (C3). Cut item 4 (Zen) removes the declared tutorial. Cut item 5 (opponent 4) removes the only stack lesson. Per Feasibility #2, it also saves almost no work, because opponent 5 still needs stacking. The cut list is the production plan's contingency mechanism, and as written every step of it damages something marked "jamais coupé". I upgrade on dependency grounds, not on Psych's "stack is the whole game" grounds (C4).

**R2 — F1 unchanged at BLOCKING, now with stronger evidence.**
Systems #1 reached the same conclusion with the same arithmetic. N3 adds that the specified AI likely can't escape the stalemate even as the attacker.

**R3 — F2 stays MAJOR, with a schedule note.**
The fix is cheap: a designer can define "chute" on a cylinder in one paragraph. But N2 shows the decision must be made before step 1, because Agent Gameplay and Agent Test both encode it at the first milestone. If it is not specified by then, it becomes an unreviewable implementation choice.

**R4 — F3 stays MAJOR, with an amendment.**
Systems #2 and I describe the same defect from two angles: flat score versus single-chain score. I add that two of Systems' four fix directions reintroduce or worsen the exploit (C2), so the fix must be chosen with stalling in mind.

**R5 — F5 stays MINOR, confirmed.**
Feasibility #5 reproduces the same Haiku arithmetic independently (~$0.90/session, ~$36 vs. $7). One clarification: Budget_tokens "Choix du modèle" assigns Opus to gameplay, NPC *and* network (Multi). Feasibility's "only Gameplay and NPC use Opus" is accurate for the MVP. With Multi already out of the MVP, the overspend lever still has no working target in either reading.
