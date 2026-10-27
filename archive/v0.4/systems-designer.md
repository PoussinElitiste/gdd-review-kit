# Systems Designer — Round 1

Scope: core loop, progression, pacing, difficulty curve, resource/economy math.
Document reviewed: `gdd.txt` (GDD v0.2 + Combo_system, Schemas_deplacement,
Evenements_visuels_et_sonores, Themes_niveaux, Budget_tokens).

---

## Finding 1 — The difficulty curve has no numbers, and the runs are longer than the content

**Severity: BLOCKING**

**Problem.** Every difficulty lever in the game is named but none is given a value
or a curve: refill spawn rate, solo gauge size per level, solo time limit, AI
"aggressiveness", and bonus/malus frequency and effects. The one document meant to
hold the curve is empty. On top of that, the run lengths offered (5, 10 or 15
levels/opponents) exceed the 5 levels that exist, and nothing says how difficulty
continues past level 5.

**Source.**
- "Progression par mode": Solo "la quantité de jauge à remplir ... augmente à chaque
  niveau", "les billes apparaissent plus fréquemment"; Arcade "l'IA devient plus
  agressive". No values, no rates, no time limit anywhere in the document.
- Themes_niveaux.md: the whole table, Difficulté column included, is "À définir". "Comment
  l'ordre des thèmes suit la montée de la difficulté (taille de jauge, fréquence
  d'apparition des billes, agressivité de l'IA)" is "Reporté à la version suivante".
- "Mode solo : ... parcours de 5, 10 ou 15 niveaux" / "Mode arcade : ... 5, 10 ou 15
  adversaires" vs. "L'idée est de proposer 5 niveaux distincts" and MVP "5 niveaux".
- Bonus/malus marbles appear in the progression of all four modes, and Zen lets the
  player choose which are active, but "Billes bonus/malus" is listed under
  "Documents liés" with no content. No bonus or malus is ever defined.

**Why it matters.** As Finding 3 shows, spawn rate decides whether a match ever ends.
A 15-level solo run has to either repeat the 5 themes with harder parameters, which
is not specified, or flatten. With a 7-week schedule, the game cannot be balanced or
tested ("Agent Test : teste la boucle de jeu de chaque niveau") without a curve to
test against.

---

## Finding 2 — Attacks make the victim's board more flexible at low volume (the punishment is inverted)

**Severity: MAJOR**

**Problem.** The design assumes that receiving marbles hurts: "colonnes ... s'allongent
et bougent de moins en moins". By the document's own movement rules, that is false
from 5 to 8 marbles per column. A column must keep one marble on the median row
(L8) and stay inside L1–L15, so the number of positions it can take (each one puts a
different marble on the median) is:

| Column length | Legal positions (marbles reachable on median) |
|---|---|
| 5 (start / minimum) | 5 (top at L4..L8, as Schemas §4a says) |
| 6 | 6 |
| 7 | 7 |
| 8 | **8** (peak) |
| 10 | 6 (top at L1..L6, as Schemas §4b says) |
| 12 | 4 |
| 14 | 2 |
| 15 | 0 |

Attack waves go to "les colonnes les plus courtes", so they spread evenly. A typical
early release (×1 to ×4 = 3 to 12 marbles) therefore raises the victim's columns
toward 7–8 and increases both mobility and the colours they can reach. Nothing
hurts until each column holds about 11 or more marbles, which is around 30 marbles
above the starting 25. That is exactly the ×10 cap ("au plus 30 billes"). In the
early and mid game, attacking helps the opponent.

The colour of attack marbles is also never specified. If they are normal random
colours, as nothing says otherwise, they are just extra material the receiver can
stack. Unlike Puyo Puyo garbage, which the document cites as its inspiration, they
are not inert.

**Source.** Combo_system "Attaque" (marbles stack at the top, "bougent de moins en
moins"; distribution to the shortest columns); Schemas §4 "Plus une colonne contient
de billes, moins elle peut bouger"; GDD "Règle de déplacement" movement limits. No
colour or inertness rule for sent marbles.

**Why it matters.** This is the VS/Arcade win loop. If the penalty is
non-monotonic, the "release now vs. let it grow" decision the document is built on
gets distorted: small releases are actively bad.

---

## Finding 3 — "The board always ends up filling" is not true as written; matches can stall

**Severity: MAJOR**

**Problem.** The doc asserts that continuous refill guarantees termination "même en VS
si aucun joueur n'attaque". But:
- Spawn frequency increases "d'un niveau à l'autre". It is a per-level constant, and
  nothing ramps it within a match.
- Each elimination removes at least 3 marbles. Only columns that fall below 5 are
  refilled, so clearing from long columns is a net reduction.
- The game-over condition ("Plateau bloqué") is only reached when every column is
  full. Per the table in Finding 2, a column of fewer than 15 marbles can always move
  in at least one direction, so a block requires 75/75 marbles.

So a player whose clear rate matches the spawn rate can hold the board below 75
indefinitely. Termination depends on an unstated inequality between spawn rate and
achievable clear rate. Local VS (the featured "party game") has no round timer and no
sudden-death escalation as a backstop. Solo is bounded by its timer, but VS, Arcade
and the non-time side of Zen are not.

**Source.** GDD "Cas limites → Remplissage continu" and "Plateau bloqué"; Combo_system
"Colonne pleine ... Si toutes les colonnes de l'adversaire sont pleines, c'est le game
over"; "Progression par mode" (spawn rate tied to level, not elapsed time).

**Why it matters.** Pacing across a 3- or 5-round VS set has no ceiling. It needs
either an in-match spawn ramp or a round timer with a tiebreak.

---

## Finding 4 — The release condition, which drives the whole economy, is under-specified and its frequency is unmodelled

**Severity: MAJOR**

**Problem.** Every payoff (solo gauge, attack, counter-attack) flows through one event:
a full 5-of-a-kind line on the median in a colour different from the counter's.
Three things about this event are undefined or unexamined:

1. **Contiguity and wraparound.** "Au moins 3 billes de même couleur, à l'horizontale"
   never says the marbles must be adjacent. The median row is cyclic: Switch moves
   c5 to c1, and "4 switchs vers la droite équivalent à 1 vers la gauche". It is
   undefined whether c4-c5-c1 counts as an alignment, and whether `X X Y X X` is
   inert. "5 cases ne peuvent pas contenir deux alignements de 3" implies contiguity
   without wrap, but under wrap it is also false, for example c1-c3 and c4-c1.
2. **Construction difficulty.** Any 3 adjacent marbles on the median eliminate
   immediately, so a 5-line cannot be built up incrementally on the median. It has to
   be finished by a single move into a `X X _ X X`-type gap, or dropped in from a
   stack above. Switch, the only horizontal tool, rotates the row and tends to
   trigger an early 3-match. Nothing estimates how often a 5-line is achievable, but
   that rate sets the attack rate, the gauge fill rate and the viability of the
   counter-attack window (×10 = 10 waves × 1.5 s = 15 s to produce a 5-line).
3. **Release is automatic.** The stated "vrai choix : libérer son compteur tout de suite
   ... ou le laisser grossir" is really a choice to avoid completing 5-lines of other
   colours. Since 5-lines are the hard event, the player rarely has the option to
   wait. It is also unspecified whether the 5 marbles of the releasing line count
   toward the new counter, which "repart à × 1".

**Source.** GDD "Alignement et élimination", "Règle de déplacement" (switch wrap);
Combo_system "Libération du compteur", "Contre-attaque", "Un alignement à la fois".

**Why it matters.** Without a defined match rule and a target release frequency
(for example, releases per minute at each level), the counter cap (×10), the per-unit
payout (3 marbles), the wave delay (1.5 s) and the gauge sizes cannot be tuned against
each other. They currently coexist on the page without interlocking.

---

## Finding 5 — Solo scoring and combo payoff have no formula, and combos barely touch the counter

**Severity: MINOR**

**Problem.** The solo loop's long-term goal is a leaderboard score "basé sur le compteur
de combo et l'utilisation du compteur de billes", but no formula is given: no
per-alignment points, no combo multiplier, no time bonus. The "Leaderboard" document
does not exist. Combos only feed the marble counter when the cascaded lines happen to
be the counter colour ("le compteur de billes se remplit en conséquence" is
conditional on colour per the Barème). Most cascades therefore affect score only,
and score is undefined. Meanwhile the gauge, which actually decides solo win/loss, is
fed only by releases. In solo, the stack/combo tactic the document presents as "la
tactique à adopter" has no defined reward. It may be strategically irrelevant to
winning a level.

**Source.** GDD "Mode solo"; "Boucle d'une partie" step 3; Combo_system "Combos" and
"Barème"; "Leaderboard" listed under "Documents liés" with no content.

---

## Round 2 — Cross-examination

### CONFLICTS

**vs. Feasibility Lead F3 and Player Psychologist F5 (the "challenge corsé" AI).**
Both treat the arcade AI as too hard: hard to build (Feasibility) or so punishing that
it drives players away (Psychologist). My Finding 2 points the other way. The only
difficulty spec the document gives is "elle enchaîne les combos et envoie ses billes
plus vite". Sending faster means releasing smaller counters more often. By the
mobility table in my F2, small waves of 3 to 12 marbles push the player's columns
toward 7–8 marbles, which is their most mobile state. An AI built to the letter of
the spec gets *weaker* as it gets "more aggressive". The psychologist's churn concern
assumes the knob works as intended, and I don't think it does. The risk isn't too
much challenge. It's that nobody can predict the challenge at all until the attack
curve is monotonic. Feasibility asks for "difficulty knobs" in an AI spec. My
position is that those knobs can't be written until the attack payout is fixed:
inert garbage, a minimum wave size, or a payout that front-loads height. Otherwise
the AI spec would be tuning against an inverted response.

**vs. Business Analyst F1 and Feasibility Lead F1 (cutting the MVP down to Solo + VS + Zen, or one competitive mode).**
I agree with the scope verdict but not with the assumption behind it, which is that
cutting modes cuts risk. Every mode the document has (Solo gauge, VS/Arcade attack,
counter-attack) is paid for by one event: the 5-line release from my Finding 4. Solo
win/loss is fed *only* by releases (my F5). Cutting Arcade removes AI risk. It does
not remove the core economy risk, which every surviving mode shares. Any reduced
MVP needs a paper prototype or simulation of release frequency **before** it picks
modes. If 5-lines turn out to be rare, Solo fails as badly as VS. If both scope cuts
go ahead, keep VS over Arcade, because Arcade stacks an untuned AI on top of
unvalidated attack math.

**vs. Player Psychologist F1 (onboarding never teaches the counter/release rule).**
The finding is correct but treats this as a teaching problem, and I think it is
mainly a rate problem. Per my F4, a 5-line can't be built incrementally on the median
because any 3 adjacent marbles clear immediately. It has to be completed in one move
into an `X X _ X X` gap or dropped in from a stack. A tutorial can explain the
counter-colour rule in one sentence. It can't make the action reachable for a
first-hour player. Better onboarding won't fix this. The fix is to make the release
event more attainable, or to give the player an explicit release action so the
"vrai choix" really exists.

### CONNECTIONS

**Adversarial QA F2 + my F2 + my F3: the attack curve has a dead zone and then a cliff, and the cliff doesn't end the match.**
My F2 shows attacks do nothing, or even help, up to about 8 marbles per column. QA F2
shows that once columns are long, the "release always possible" guarantee fails:
the guaranteed marbles sit on rows that will never reach the median. Put together,
the VS loop goes harmless → harmless → sudden soft-lock, with no gradual pressure
band in between. My F3 shows that game over needs 75/75 cells, and the only
backstop is a fill rate that isn't specified. So the soft-locked player can't
release, can't counter-attack, and can't lose until the opponent finishes the job or
the undefined fill does it. QA F5 adds that a simultaneous block draws, and the
document doesn't say whether a draw counts toward the 1/3/5 rounds. A best-of-1 can
therefore repeat forever. VS termination is undefined at three levels: within a round,
in the soft-lock state, and across the set.

**Player Psychologist F3 + my F4: the "unfair" game over may really be unfair, not just feel unfair.**
The psychologist frames "game over même si un switch reste possible" as a perception
problem. Under the document's own match rule, it is also a rules problem. On a full
board, the median row still holds 5 marbles, and a switch rotates it. If alignments
are contiguous *without* wrap, which the "5 cases ne peuvent pas contenir deux
alignements de 3" line implies, a row like `A A B C A` has no match, but one switch
gives `A A A B C`, which clears and unblocks the board. The rule declares a player
dead while a legal input would save them. Whether this happens depends entirely on
the wrap ambiguity I flagged in F4.1. One rules decision fixes both: define wrap,
then either let a switch count as a valid escape or make the median row cyclic for
matching.

**Narrative Critic F5 + Player Psychologist F4 + my F2: "le tableau commence à se remplir" has no threshold, and filling isn't danger.**
Both colleagues object that the VS/Arcade audiovisual crescendo tracks losing. My
math adds a further problem: in this design, a filling board is not a losing board.
From 5 to 8 marbles per column the player gets *more* options. The layer would fire
when the player gains mobility, then stay unchanged through the 11+ zone where real
danger begins. Any audio, visual or "panic" trigger, and the psychologist's requested
danger indicator, has to key off reachable positions per column (the table in F2),
not marble count. Otherwise the signal is wrong in both directions.

**Adversarial QA F1 + my F4: the release frequency depends on the undefined resolution order.**
In QA's "refill first" order, refill marbles spawn directly on L8, the median. If that
applies across several columns after a full-line clear, the median gets refilled by
random marbles, which could make 5-lines much more or much less frequent. Then QA's
scope question decides whether "no random alignment" covers stored stacks falling
onto the median. If it does, it cancels the stack path, which is one of only two ways
my F4 found to build a 5-line. The economy's central rate can't be estimated until
QA's ordering is fixed.

**Adversarial QA F4 + my F5: solo leaderboard integrity.**
QA notes that bonus marbles roll "à chaque élimination", so players can farm them
with cheap 3-matches. My F5 notes that score has no formula. With no formula and a
per-elimination random roll, the ranked mode rewards spamming 3-matches over the
stack/combo play the document calls "la tactique à adopter". The leaderboard would
then push players away from the intended skill.

### REVISIONS

- **F3 upgraded MAJOR → BLOCKING.** On its own, a possible stall was a pacing risk.
  With QA F2 (soft-lock with no release) and QA F5 (draws with no tiebreak), the
  featured mode, local VS, has no guaranteed end at any level. A party game that
  can't guarantee a round ends isn't shippable. Minimum fix: a round timer with an
  in-round spawn ramp (sudden death), and draws that count as replayed rounds with a
  cap.
- **F2 strengthened, stays MAJOR.** It now also breaks the only AI difficulty spec
  (see the Feasibility/Psychologist conflict above). I'm not raising it to BLOCKING
  because one rules change fixes it, either inert/locked garbage marbles or
  height-weighted distribution, and doesn't cascade.
- **F1 stays BLOCKING but narrowed.** QA F3, Feasibility F1 and Psychologist F5 all
  raise the 5-vs-15 run mismatch, so that part is shared. The part only my lens
  covers is that no lever has a number. The Psychologist's request for a gentler
  first-hour ramp (F2) can't be specified until a baseline curve exists.
- **F4 unchanged, MAJOR**, but it is now the dependency for several colleagues'
  fixes: Psychologist F1/F3, QA F1, and the Feasibility/Business scope cuts. The
  board should treat "define the match rule and target release rate" as a
  precondition, not a parallel task.
- **F5 stays MINOR.** QA F4 adds farming and luck, but the leaderboard is the most
  likely casualty of the scope cuts the board recommends, so its urgency stays low.
- **Nothing withdrawn.**
