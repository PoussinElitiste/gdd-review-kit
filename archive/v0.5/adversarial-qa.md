# Adversarial QA — Round 1

Reviewer lens: contradictions between sections, unhandled edge cases, exploits, unplanned fail states.
Document reviewed: `gdd.txt` (GDD_V_0_2.md + Combo_system.md, Schemas_deplacement.md, Evenements_visuels_et_sonores.md, Themes_niveaux.md, Budget_tokens.md).

---

## 1. The lose/win condition is defined three different ways, and they give different results — BLOCKING

**Problem.** Losing in Solo, and both winning and losing in Arcade and VS, depend on one board state. The document defines that state three times, and the definitions do not agree:

- Concept: the goal is to fill the opponent's board *"pour le remplir et l'empêcher de déplacer ses billes"* (fill it so they cannot move).
- GDD, Cas limites, *Plateau bloqué*: game over only if *"toutes ses colonnes sont bloquées dans les deux sens ... **et** qu'aucun switch ne peut plus former d'alignement sur la ligne médiane"*.
- Combo_system, *Colonne pleine*: *"Si toutes les colonnes de l'adversaire sont pleines, c'est le game over pour lui"*. This version has no switch clause.

**Why it breaks.** The move rules (a column must keep one marble on L8 and stay inside L1–L15) mean any column of 14 marbles or fewer still has at least one legal move. Only a 15-marble column is frozen. So "all moves blocked" means all 5 columns are full (75 marbles). At that point the two documents disagree. Take a full board whose median row is `R R B B R`. One switch gives `R R R B B`, which is a legal alignment.
- Under the GDD rule the game continues: the player switches, eliminates 3, and the columns drop to 14, so they can move again.
- Under the Combo_system rule the player has already lost.

The same state is a loss in one document and playable in the other. Two more effects follow:
- The draw rule (*"si les deux tableaux se bloquent au même moment"*) is undefined, because "bloqué" is undefined. It also never says what counts as "the same moment" (same frame, same wave tick?).
- Zen uses yet another wording, *"si le tableau se remplit"*. It is unclear whether that is the full-board rule or the blocked-board rule.

**Source.** GDD §Mode arcade, §Cas limites / Plateau bloqué, §Égalité; Combo_system §Attaque / Colonne pleine; Schemas_deplacement §4 ("une colonne de 15 billes ne bouge plus").

---

## 2. Marbles added "on top" of a column that already touches L1 have nowhere defined to go, and pushing the column could create alignments by accident — MAJOR

**Problem.** Two systems add marbles to the top of columns:
- Attack waves: *"Elles s'empilent en haut de ses colonnes"*.
- Continuous refill: *"de nouvelles billes apparaissent aléatoirement au fil de la partie"* ... *"les nouvelles billes apparaissent toujours par le haut de la colonne"*.

The schematics show a 10-marble column at L1–L10 (*"une colonne de 10 billes qui touche le haut (L1)"*). The only overflow rule covers a column of 15 marbles (*"une bille ne tombe jamais dans une colonne déjà pleine (15 billes)"*). Nothing covers a 10-marble column sitting at L1 that gets another marble. The column has to be pushed down, or the marble has to be redirected, but the document says neither.

**Exploit / contradiction.** If the column is pushed down by one row, a different marble lands on the median row L8 with no player action. That can complete a 3-line on the median, which then gets eliminated, scores a combo, or even releases the counter. In VS, an opponent's attack could hand the defender a free elimination or a free release. This breaks the stated guarantee *"Seules les actions du joueur créent des alignements, ce qui évite les points et les combos gratuits"*. It also adds a second resolution trigger that the fixed *Ordre de résolution* (chute → remontée → cascade → remplissage) never mentions, since that order only runs "après chaque élimination".

**Source.** Combo_system §Attaque, §Colonne pleine; GDD §Remplissage continu, §Pas d'alignement dû au hasard, §Ordre de résolution; Schemas_deplacement §4b.

---

## 3. Instant elimination contradicts the "any marble anywhere" claim, so the counter-release guarantee is unproven — MAJOR

**Problem.** The GDD says that by combining move and switch the player *"peut amener une bille à n'importe quelle place du tableau"*. It also says *"4 switchs vers la droite équivalent à 1 vers la gauche"*. But each switch is a separate action, the board resolves after every action, and switches cannot be undone (*"il n'est pas possible d'annuler un switch"*). A "left switch" made of 4 right switches therefore passes through 3 intermediate median rows. Any of them can contain 3 adjacent same-colour marbles, which are eliminated on the spot. That throws away the setup, can change the counter, or can trigger an unwanted release.

The same rule limits how a 5-line (the only way to release the counter) can be built:
- It cannot be built one marble at a time on the median, because the third matching marble triggers an elimination of 3 straight away.
- It can only form in a single action from a `X X _ X X` row closed by a move, or from a stored 5-line dropping in during a cascade. A switch cannot do it, because rotating `X X _ X X` gives 3 adjacent before 5.

**Consequence.** The generation guarantee *"au moins 5 billes de même couleur sur le tableau, dans une couleur différente de celle du compteur, pour qu'une libération du compteur reste toujours possible"* treats "5 marbles exist" as if it meant "a 5-line can be built". It does not. Whether a release is reachable depends on where those marbles are and on the instant elimination of any 3. If no release is reachable, Solo progress stops: the gauge cannot fill, so the player can only time out. The document also never says whether the player can act during a cascade or fall animation, so the input-buffering exploit is unhandled.

**Source.** GDD §Règle de déplacement (switch paragraph), §Alignement et élimination, §Garanties de génération; Combo_system §Libération du compteur, §Un alignement à la fois.

---

## 4. The counter-attack window is never defined, so VS's main decision cannot be tuned — MAJOR

**Problem.** VS and Arcade are built on the Puyo-style choice: *"libérer son compteur tout de suite pour se défendre, ou le laisser grossir"*. Cancelling only works while waves are *"encore en attente"* during the *"décompte"*. The only timing given is the gap between waves (*"1,5 seconde"*). The length of the countdown before the **first** wave lands is never given. If it is 0, a ×1 attack (one wave) can never be countered, and the "vrai choix" disappears for small attacks.

Related gaps:
- **Ping-pong:** can the attacker cancel the excess that gets sent back (*"3 vagues repartiraient vers A"*) with their own release? Does that excess get a new countdown?
- **Timing collision:** if B releases at the moment a wave lands, does that wave count as pending or as fallen?
- **Wave placement:** waves go to the shortest columns with random tie-breaks (*"le choix entre elles se fait au hasard"*). Together with random refill, this means VS outcomes are not reproducible. That clashes with the stated aim *"pour que le plateau soit déterministe (même situation, même résultat)"* and makes desync bugs likely if online play is added.

**Source.** Combo_system §Attaque (Délai entre deux vagues, Répartition d'une vague), §Contre-attaque; GDD §Ordre de résolution; Evenements "Vagues en attente".

---

## 5. Bonus/malus marbles are live in every MVP mode but have no spec, and they collide with the board guarantees — MAJOR

**Problem.** Bonus/malus marbles *"peuvent apparaître à chaque élimination de billes"* in Solo, Arcade and VS. Zen offers *"options pour choisir les bonus/malus actifs"*. All four modes are in the MVP. The only spec is a related document, "Billes bonus/malus", which is listed with no link and does not appear in the package. The events table still gives their appearance an "Important" priority.

Unanswered questions that break other rules:
- Does a bonus/malus marble have one of the 5 colours/patterns? Is it a wildcard? Can it be part of an alignment? If it is a wildcard, the *"Pas d'alignement dû au hasard"* guarantee and the 5-colour pattern accessibility scheme both need revising.
- Does it count toward the *"au moins 5 billes de même couleur"* release guarantee or the 5-marbles-per-column minimum?
- Can it be sent in attack waves?
- Does it spawn on the median? Refill is forbidden there, but this marble spawns "at elimination", which is a different trigger.

**Hoarding exploit risk.** Because bonuses appear on every elimination, a Solo player chasing the leaderboard is pushed toward spamming cheap 3-eliminations to farm bonuses. The design intends the opposite (combos and the stack). This cannot be assessed until the effects exist.

**Smaller related edge (MINOR, noted here):** the alignment-highlight aid and the passive onboarding are active in the player's *"toute première partie, quel que soit le mode"*. If that first game is a Solo run, a score earned with alignment hints goes onto the leaderboard. In a local VS first game, it is unclear which player gets the hints, or whether both do.

**Source.** GDD §Mode arcade, §Mode VS, §Progression et thèmes, §Contrôles et onboarding, §Documents liés; Evenements "Bille bonus ou malus qui apparaît".
