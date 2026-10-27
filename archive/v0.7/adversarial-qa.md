# Adversarial QA — Round 1

Reviewed: `gdd.txt` (GDD v0.2, *The Marble Effect*, with linked documents Combo_system, Schemas_deplacement, Alignement_et_elimination, Controles, AI_Architecture, Evenements_visuels_et_sonores, Themes_niveaux, Budget_tokens).

---

## 1. Players can make themselves immune to attacks, and the "a round always ends" guarantee is false — BLOCKING

**Problem.** Attack balls are barred from any column "qui touche déjà le haut du tableau (L1)", and when no column has room "la bille est perdue". The move rules allow a column of 8 or more balls to be slid up until it touches L1, because it still covers the median line. So once all five columns hold 8+ balls (which attacks and continuous fill will produce, since attacks target the shortest columns), the defender can push every column to L1 and every incoming attack ball is lost. Attacks stop working, and attacks are the only way to win in VS and Arcade.

It gets worse. The game-over rule needs every column to be unable to move in both directions. Under the column-length rule, a column of n < 15 balls always has at least one legal move (a 14-ball column has two legal positions). So game over requires all five columns at exactly 15 balls (75 balls) **and** no switch that forms an alignment. A column touching L1 can only grow if something adds balls to it, and the document never says where continuous-fill balls spawn when a column is already at L1. If they follow the attack rule, they get lost too, the board never fills, and a defensive player (or two of them in VS) can stall forever. That breaks the sudden-death claim that "une manche se termine donc toujours". If they push the column down instead, that contradicts the stated principle that nothing but the player pushes a column down.

**Source.**
- Combo_system.md, "Colonne sans place": "une bille d'attaque ne tombe jamais dans une colonne pleine (15 billes) ni dans une colonne qui touche déjà le haut du tableau (L1) … Si aucune colonne n'a de place, la bille est perdue."
- Schemas_deplacement.md 4b: "une colonne de 10 billes qui touche le haut (L1) ne peut pas monter" (so reaching L1 is a normal, legal state).
- GDD "Fin de partie": "Une manche se termine donc toujours."
- Omission: no spawn rule for continuous fill into columns at L1 or at 15.

**Fix needed.** Define where fill and attack balls go when a column is at L1 (for example, push the column down, or make "no room for an incoming ball" count as a block or a loss). Then re-prove that sudden death always ends.

---

## 2. The release guarantee has no fallback once every column has more than 7 balls, so a Solo level can become unwinnable — MAJOR

**Problem.** The guarantee that "the counter stays releasable" is restored by spawning the useful colour "en haut des colonnes les plus courtes (7 billes au plus)". In late Solo levels (one ball every 2 s on La Nébuleuse) and under Arcade attacks, it is normal for all five columns to pass 7 balls. At that point the restore rule has no valid column, and the document's own simulation says a release becomes "souvent impossible à partir de 11 billes par colonne, et toujours à partir de 13". In Solo, the gauge only fills through releases, so the player can run out the clock with no possible way to win. This contradicts "pour que le compteur reste libérable" and the Agent Test promise that "chaque niveau peut être terminé". Attack balls are also said to be "soumises aux garanties de génération", but they can only land in non-L1 columns, so they cannot serve as the fallback either.

**Source.** Alignement_et_elimination.md, "Rétablir la garantie"; GDD "Alignement et élimination" ("La génération garantit aussi qu'au moins 5 billes d'une couleur utile peuvent toujours atteindre la ligne médiane"); Themes_niveaux.md spawn table; AI_Architecture.md, Agent Test row.

**Fix needed.** Specify what happens when no column has 7 balls or fewer, or reduce the claim to "usually releasable" and accept the Solo soft-lock as a designed loss state.

---

## 3. The cut list contradicts the 1:1 level-to-opponent mapping — MAJOR

**Problem.** The cut order removes "les niveaux 4 et 5" before "l'adversaire 4", and says "l'adversaire 5 (le moment « wow ») ne sont jamais coupés". But Themes_niveaux.md ties each Arcade opponent to a level: opponent n°5 is La Nébuleuse, with its music, skin and the "final épique" arc. If levels 4 and 5 are cut, opponent 5 (which can never be cut) has no level, theme, music or intro, and the "wow" finale loses the audiovisual climax the Fantasy promises. If opponent 4 is cut too, the Arcade run becomes 1, 2, 3, 5 across 3 levels, and the progression table ("chaque adversaire ajoute un comportement") skips the stack lesson that opponent 5 then requires ("demande de maîtriser toutes les tactiques que les précédents ont montrées"). The production order also builds opponent 5 before opponent 4, so opponent 5 is tuned without the behaviour it is supposed to combine.

**Source.** GDD Technical Strategy, "Ce qui peut être coupé" and "Ordre de production"; Themes_niveaux.md difficulty table (Niveau ↔ Adversaire arcade); AI_Architecture.md progression table.

**Fix needed.** Decide which level hosts opponent 5 in the cut scenario, and whether opponent 5's behaviour set drops stacking when opponent 4 is cut.

---

## 4. Bonus/malus balls are in scope for every mode but completely unspecified, and they collide with the rules most likely to break — MAJOR

**Problem.** Bonus/malus balls "peuvent apparaître dans tous les modes", "à chaque élimination de billes", and have their own event row (priority Important). Their design document is "(à rédiger)", and they are not in the cut list, so by the document's own logic they ship. Any bonus/malus effect touches rules that are currently stated as absolute: deterministic resolution ("même situation, même résultat"), "la génération ne crée jamais d'alignement tout fait", the release guarantee (finding 2), "Plateau bloqué" as "la seule condition de game over", and the AI that "ne triche pas" and evaluates only standard moves and switches. Nobody can test "aucune règle ne bloque la partie par erreur" (Agent Test) against a system with no rules. Zen also lets the player toggle them, which means two rule sets to verify.

**Source.** GDD Concept ("Des billes bonus/malus peuvent apparaître dans tous les modes"); Themes_niveaux.md "Progression par mode"; Evenements_visuels_et_sonores.md ("Bille bonus ou malus qui apparaît … Important"); Documents liés ("Billes bonus/malus (à rédiger)"); cut list in Technical Strategy (bonus/malus absent).

**Fix needed.** Either specify them before production, or explicitly move them out of the MVP and add them to the cut list.

---

## 5. The AI's 1-2 action lookahead cannot perform the behaviours the opponent table promises — MAJOR

**Problem.** The arcade AI "évalue les coups possibles à une ou deux actions (move ou switch)". Opponent 4 "stacke des alignements et enchaîne des combos", and opponent 5 "combine tous ces comportements". Stacking means building a 3-alignment above the median line, which needs several moves and switches with no immediate payoff. Releasing the counter needs a full 5-ball line of one colour, which with right-only switches (4 switches = 1 left) usually takes far more than two actions. An agent that only looks two actions ahead and picks "celui qui forme un alignement" will greedily clear triples and almost never stack or release on purpose. That makes "elle attaque en libérant son compteur" and the opponent 4/5 identities impossible without cheating, which the document rules out. The heuristic "prépare une ligne complète" is the only escape hatch and is not defined. Agent Test is said to check only that each opponent "peut gagner et perdre", not that it actually shows its advertised behaviour.

**Source.** AI_Architecture.md, "Ce que fait l'IA" and the 5-opponent table; GDD AI Architecture "Progression"; Schemas_deplacement.md (right-only switch, accessibility limits).

**Fix needed.** Define the AI as goal/plan-driven (target pattern plus planner) instead of 1-2 ply greedy search, or scale the opponent behaviours back to what that lookahead can do. Add "shows its signature behaviour" to Agent Test's acceptance criteria.

---

### Also noted (not scored)
- "Si les deux tableaux se bloquent au même moment" has no tolerance window (same frame? same resolution tick?), so a draw is effectively impossible or depends on the frame.
- First-game onboarding and alignment highlighting fire "quel que soit le mode". In local VS, it is not defined whose first game counts, and the highlight is an asymmetric competitive aid. It is also unclear whether an abandoned first game uses up the tutorial for good.
- Counter colour changes mid-cascade (a release during a combo): it is unspecified whether later cascade alignments count against the old or the new colour, and whether several releases in one cascade are separate attacks.

---

## Round 2 — Cross-examination

### CONFLICTS

**vs. Player Psychologist #4 ("Game over is invisible until it happens") — I dispute the diagnosis.**
PP argues the blocked-board loss is a hidden computed state that one mis-pressed switch can trigger without warning. Under the rules as written, that cannot happen. Systems Designer #1 and my #1 derived independently that "blocked in both directions" requires every column at exactly 15 balls, i.e. 75/75 cells filled. A completely full board is the most visible state the game has. One irreversible switch cannot produce it, because switches don't add balls. The real player-facing failure is the reverse: in VS and Arcade the loss condition almost never triggers (defenders can push to L1 and void attacks), and in Solo the actual loss is the timer, not the board. PP's "panic cue is noise" point (#3) is correct, but #4 should be re-aimed. The invisible failure is the Solo soft-lock in my #2: release becomes impossible with time still on the clock, and nothing tells the player. I'd ask PP to downgrade #4 to MINOR, or rewrite it around the release soft-lock.

**vs. Systems Designer #1 — same finding, but I object to one of the proposed fixes.**
SD suggests changing the loss condition to "any column reaching L1 with no move" or a height threshold. Watch the exploit surface. Pushing a column up to L1 is a *legal player move* (Schemas_deplacement 4b). If touching L1 becomes a loss trigger, the player can lose by choice, and the AI's 1-2 ply search will happily make that move unless it is explicitly penalised. A height threshold also makes attack balls landing in the shortest columns irrelevant to the loss, because the threshold is reached by the tallest column. A safer fix is to make "no legal placement for an incoming ball" the loss event. That is an event the opponent causes, not one the defender walks into, and it closes the stall loop directly.

**vs. Systems Designer #5 (bonus/malus rated MINOR) — I hold MAJOR.**
SD treats bonus/malus as an undefined progression lever. The risk is not missing content. It is a rule system with no rules sitting on top of the three guarantees already shown to break (my #1 and #2, SD #1). Any "malus" that adds balls or locks a column interacts directly with the L1 stall and the release soft-lock. Feasibility F2 and Business #2 also note that the cut list doesn't cover it, so by default it ships. An unspecified rule-modifier that ships by default, in a game whose termination proof is already broken, is not MINOR.

**vs. Narrative Critic #1 (cut list = BLOCKING) — I keep the cut-list conflict at MAJOR.**
Five of six reviewers flagged the cut list (my #3, SD #3, Feasibility F2, Business #4, Narrative #1). That strengthens confidence, but it doesn't raise severity. The contradiction only bites if the cuts are actually executed, and it can be fixed with one paragraph naming opponent 5's host level. BLOCKING should be kept for defects that break the build as specified even when nothing is cut, like #1 here.

**vs. Business Analyst #4 and Player Psychologist #5 ("protect adversary 4 to keep the stack lesson") — protecting it fixes nothing on its own.**
Both argue that opponent 4 should be protected because it is the only teacher of the stack. My #5 says the specified AI (1-2 action lookahead, picks "celui qui forme un alignement") cannot stack at all. A protected opponent 4 built to the current spec will greedily clear triples, and the "lesson" will never appear on screen. Agent Test only checks that it "peut gagner et perdre", so nobody will notice. Moving opponent 4 up the cut list is necessary but not sufficient. The AI spec has to change, or the stack lesson has to move to Solo/Zen, as BA's fallback suggests.

### CONNECTIONS

**Feasibility F4 (no test harness) + my #1, #2, #5: none of the BLOCKING/MAJOR rule defects can be caught by the QA plan as written.**
Agent Test's acceptance criteria are "chaque niveau peut être terminé", "aucune règle ne bloque la partie par erreur" and "peut gagner et perdre". Each one is an existence check that passes despite my findings. A level is "completable" on a lucky seed even if it soft-locks on others (#2). An AI "can lose" even when the defender is immune to attacks (#1), because it loses on the timer or the board fills in Solo. An opponent "can win" without ever stacking (#5). Combined with F4's point that no headless deterministic simulator is scoped, and that the guarantee *already* failed in simulation once, the testing plan structurally cannot detect the class of bug this document is most prone to. The criteria need to change from "can" to "always / with probability ≥ X over N seeds", and the simulator must share code with the game.

**Systems Designer #1 + my #2: the generation guarantee is applied where it hurts and missing where it's needed.**
SD notes that attack balls are "soumises aux garanties de génération" and may therefore hand the victim the useful colour, which makes attacks a gift. My #2 notes that the guarantee-restore rule has no valid column once all columns exceed 7 balls. Put together, the guarantee fires on early, small attacks (when columns are short, and it helps the defender) and goes silent on late, heavy pressure (when columns are tall, exactly when the guarantee was meant to prevent soft-lock). The rule is inverted relative to its purpose.

**Player Psychologist #2 + Systems Designer #2 + my #2: Solo level 1 is plausibly unwinnable for a novice, with no signal why.**
PP computes that level 1 needs about five full 5-lines in 3 minutes at ×2. SD shows that release rows and counter growth compete for the same 5 median cells. My #2 shows that release can become impossible outright. PP also notes there is no "close to release" feedback event. There is also no "release impossible" state or feedback. The first-hour experience can be a player making legal moves on a board where winning is mathematically closed, until a panic cue and a timer loss. That is the worst version of Narrative #2's "sans pression" contradiction.

**Business Analyst #3 (protect online over local VS) + my #1 and "also noted" draw timing.**
If online VS gets promoted, as BA suggests, my #1 stall exploit becomes a griefing vector: two defenders, or one, can hold a match open forever with nothing to end it. The frame-dependent simultaneous-block draw also becomes a netcode desync question. Any move toward online multiplayer has a hard dependency on fixing the termination proof first.

**Narrative Critic #3 (win and loss events are identical) + my "also noted" draw rule.**
In local VS, a simultaneous block (draw) and a single block (one win, one loss) both fire the same "explosion, puis dissipation" event on both boards. Players sharing one screen get no audiovisual signal of who won, or whether anyone did. Narrative raises this as a thematic problem. On a split-screen it is also a functional ambiguity.

### REVISIONS

- **#1 (attack immunity / round never ends): stays BLOCKING, strengthened.** Systems Designer derived the same 75/75 result and the same L1-immunity exploit independently.
- **#2 (release soft-lock): upgraded MAJOR → BLOCKING.** Solo is "never cut" and has one win path: the gauge filled by releases. Combined with PP #2 and SD #2 (throughput math), the soft-lock is not a late-level edge case. It can plausibly trigger in the mode the project guarantees to ship, and Agent Test as written (F4) won't catch it.
- **#3 (cut list vs. level mapping): stays MAJOR.** Confidence went up because four colleagues flagged it, but severity did not (see the conflict with Narrative #1 above).
- **#4 (bonus/malus): stays MAJOR.** I disagree with SD's MINOR, for the reasons above.
- **#5 (AI lookahead): stays MAJOR, strengthened.** It now explains why BA's and PP's "protect opponent 4" fix won't deliver the stack lesson on its own.
- **"Also noted": VS first-game onboarding promoted to a scored MINOR.** Player Psychologist #1 independently raised the same ambiguity about whose "first game" counts in VS. With two reviewers on it, it deserves a line in the defect list.
