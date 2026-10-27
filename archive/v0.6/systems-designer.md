# Systems Designer — Round 1

Document reviewed: `gdd.txt` (GDD_V_0_2 + linked docs: Combo_system, Schemas_deplacement, Evenements_visuels_et_sonores, Themes_niveaux, Budget_tokens).

Overall read: the micro-rules (move, switch, resolution order, counter release, wave cancellation) are written with unusual precision. The macro-systems that sit on top of them (difficulty curve, pressure economy, what the player is actually optimizing in each mode) are either undefined or, where defined, don't produce the outcome the document says they do.

---

## 1. The entire difficulty curve and the solo win condition are unparameterized — BLOCKING

**Problem.** Every number that drives progression or pacing across a playthrough is missing. There is no solo time limit, no gauge size per level, no passive spawn rate per level, no definition of "IA plus agressive," and no solo score formula. The bonus/malus balls appear in the progression of all four modes and have no specification anywhere. The 5-level structure has rules for scaling difficulty but nothing to scale. Nobody can build, tune, or test a difficulty curve from this.

**Source.**
- Themes_niveaux.md: every cell of the theme/difficulty table reads "À définir", and "Comment l'ordre des thèmes suit la montée de la difficulté (taille de jauge, fréquence d'apparition des billes, agressivité de l'IA)" is "Reporté à la version suivante du GDD."
- GDD "Progression et thèmes": "la quantité de jauge à remplir … augmente à chaque niveau", "les billes apparaissent plus fréquemment". These give a direction but no values or ratios.
- GDD "Mode solo": "remplir une jauge de progression en temps limité" (no time given) and "Un système de score, basé sur le compteur de combo et l'utilisation du compteur de billes". No formula is given, but the score feeds a leaderboard.
- "Billes bonus/malus" is listed under "Documents liés" with no link and no content. It is still referenced in Arcade, VS, Solo progression and in Zen ("options pour choisir les bonus/malus actifs").

**Why it matters for systems.** The solo gauge is filled by counter value (1 to 10 per release), so gauge size divided by the expected release value sets the number of 5-lines needed per level. Together with the timer, that ratio is the whole solo difficulty curve. A 7-week schedule has no slack for discovering these values through playtesting from zero.

---

## 2. Attack pressure is inverted at low column heights: early and mid-size attacks make the defender's board *more* mobile — MAJOR

**Problem.** The document says that received balls make columns "bougent de moins en moins." The move rules it defines produce something else. A column of length *n* can occupy every vertical position where it still covers the median line (L8) and stays inside L1 to L15. The number of legal positions is `min(8, 16-n) - max(1, 9-n) + 1`:

| Column length | Legal positions |
|---|---|
| 5 (starting) | 5 |
| 6 | 6 |
| 7 | 7 |
| 8 | 8 (peak) |
| 10 | 6 |
| 11 | 5 (same as start) |
| 13 | 3 |
| 15 | 1 (frozen) |

So mobility *rises* from length 5 to 8. It only falls back to the starting value at length 11. The document's showcase number confirms the problem: a maxed counter (×10) sends 30 balls, which is 6 per column on a fresh 5x5 board. That gives columns of 11, so the opponent ends with exactly the mobility they started with. They also have 30 more balls of raw material to build stacks from. Losing requires every column at 15, about 50 net balls of inflow.

**Source.**
- Combo_system "Attaque": "Elles s'empilent en haut de ses colonnes, qui s'allongent et bougent de moins en moins".
- Combo_system "Compteur de billes": "Une libération envoie donc au plus 30 billes, ce qui empêche de remplir le tableau adverse d'un seul coup". This implies the designer thinks 30 is close to lethal. It isn't close.
- Schemas_deplacement 4: "Plus une colonne contient de billes, moins elle peut bouger". This is only true above length 8.
- GDD "Plateau bloqué": loss requires all columns blocked in both directions, which in practice means all at 15.

**Consequence.** The attack/counter-attack loop, which the GDD presents as the Puyo Puyo heart of VS and Arcade, has weak or negative bite for most of a round. Outcomes are likely to drift toward the 3-minute sudden-death rule ("la vitesse de remplissage double toutes les 30 secondes"). Then passive spawn decides rounds, not player attacks. The "wow" Arcade AI challenge rests on a pressure system that doesn't pressure.

---

## 3. Inflow balls (passive spawn and received attacks) have no defined properties — MAJOR

**Problem.** The game has three sources of balls: refill to 5, passive spawn, and received attacks. Only refill is specified (from the top, never on the median line, never creating an alignment). For the other two, the document never says:
- what colors they get (random? weighted? a single "garbage" color?);
- whether the "no free alignment" and "≥5 of a non-counter color" generation guarantees apply to them;
- for passive spawn: which column, how many at a time, and the base rate;
- whether received balls can be matched like normal balls, which links to finding 2. If they can, attacks are free material for the victim's stacks.

**Source.**
- GDD "Remplissage continu": "de nouvelles billes apparaissent aléatoirement au fil de la partie". No placement or color rule.
- GDD "Garanties de génération" covers "la génération des billes, au départ comme pendant la partie". It is unclear whether received attack balls count as "génération."
- Combo_system "Répartition d'une vague": placement (shortest columns) is defined, color is not.

**Consequence.** In Puyo, garbage is inert until cleared nearby, and that is what makes attacks hurt. Here, attack balls may be ordinary colored marbles. If they are, attacking feeds the opponent's combo economy. This is the key variable in the VS economy and it is not written down.

---

## 4. Combos generate no attack value, so the "stack" tactic the document prescribes has a weak payoff — MAJOR

**Problem.** The document calls stacking "La tactique à adopter" and cites Puyo Puyo as the attack model. But a combo chain adds nothing to the attack unless every link in the cascade happens to be the counter's color. Only counter-color alignments fill the counter (+1/+2/+3). Off-color cascades do nothing ("un alignement d'une autre couleur n'a aucun effet sur le compteur"). The combo counter only feeds the solo score, and that formula is undefined (finding 1). The counter is also hard-capped at ×10 with overflow lost. So the best play is to grind counter-color 3-lines one at a time up to 10 and then release, not to build chains. Chains are flavor, not strategy.

There is also a resolution gap. If a cascade forms an off-color 5-line partway through, the counter is released and resets to ×1 in the new color. The document doesn't say whether later cascade links are scored against the old counter or the new one. That is a determinism hole in a section that claims "même situation, même résultat."

**Source.**
- Combo_system "Combos": "Le compteur de combo augmente alors, et le compteur de billes se remplit en conséquence". This implies combos feed the counter, but the scoring table only rewards counter-color alignments.
- Combo_system "Tactique : le stack" vs "Compteur de billes" (cap ×10, "les gains au-delà sont perdus").
- GDD "Ordre de résolution" defines Chute/Remontée/Cascade/Remplissage but not when a release fires within a cascade.

**Consequence.** The loop sold in "Boucle d'une partie" (stack, then cascade, then combos, then reward) doesn't connect to the reward that matters in 3 of 4 modes (attack size). Add a combo multiplier to counter gains or to waves sent, or drop the Puyo framing.

---

## 5. Draws count as a win for both players, which hands match points to the leader — MINOR

**Problem.** A drawn round "compte comme une manche gagnée pour chacun des deux joueurs." In a best-of-3 at 1-0, a draw makes it 2-1 and the leader wins the match. So the trailing player can never benefit from a draw, and the leader is rewarded for it. In sudden death the fill rate doubles every 30 seconds and both boards are flooded by the same passive mechanism, so simultaneous lockouts become more likely late in a round. The rule then decides matches in a way that feels arbitrary. In Arcade only a *match* draw is replayed, and the round-level asymmetry is left as it is.

**Source.** GDD "Égalité (VS et arcade)" and "Durée d'une manche".

**Fix direction.** A drawn round should award nothing and be replayed, or be broken by a tiebreak such as remaining free cells.
