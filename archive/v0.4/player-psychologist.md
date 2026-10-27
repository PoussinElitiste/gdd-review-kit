# Player Psychologist — Round 1

Lens: what a real player experiences in the first session and the first hour. Where they get confused, where they get frustrated and quit, and where the rewards don't motivate them.

---

## Finding 1 — The onboarding only teaches "match 3". The real objective is never taught.

**Severity: BLOCKING**

**Problem.** The core loop's actual goal is counterintuitive, and nothing in the onboarding covers it. The goal is to fill the counter with matches of *its* color, then release it with a full 5-line of a *different* color. The document itself says a 5-line of the counter's own color does **not** release it. A new player who builds a perfect 5-of-a-kind in the counter color will expect the big payoff and get none, and nothing will tell them why. Stacking, combos and counter-attack cancelling are also never taught, and those are the systems that make the game deep. The board will keep filling while the player still doesn't understand how to win. That is the classic "I don't get it" quit point in the first 10 minutes.

**Source.**
- "Un onboarding passif présente les mécaniques de base, uniquement lors de la toute première partie du joueur ... si le joueur ne fait aucune action pendant 5 secondes, le jeu met en surbrillance une bille et l'action à effectuer (switch ou move) pour aligner 3 billes." Only the 3-match is taught, and only to idle players.
- "Une ligne complète de la couleur du compteur ne le libère pas : elle le remplit avec un bonus." (GDD, Alignement et élimination; Combo_system, Libération du compteur.)
- Zen "sert aussi de mode d'entraînement (training mode) et de tutoriel", but nothing describes any tutorial content, steps or prompts for it. It is just Zen without a timer.
- The alignment-highlight aid is "active que pendant la première partie du joueur". After one game, every aid disappears.
- Omission: nothing teaches the counter, release, stack, combo or wave-cancel.

---

## Finding 2 — The control model is harder to learn than the "broad audience" pitch assumes

**Severity: MAJOR**

**Problem.** The document's audience reasoning rests on "le jeu ne demande pas une grande maîtrise des commandes". The movement model asks for real spatial reasoning:
- A switch rotates the *entire* median row, and only to the right. Going left one cell takes 4 switches, and each one also moves every other marble on the row, which can break alignments the player was building.
- Switches can't be undone.
- Moving a marble to a target spot takes a planned move, switch(es), then move sequence.
- Columns can snap back onto the median line by themselves after an elimination.

All of this happens under a solo time limit or continuous board fill. A casual player will experience the right-only rotation as the game "fighting" them, not as anticipation. The document frames no-undo as a feature ("pousser le joueur à anticiper"), but for a newcomer it reads as punishment for exploring. Mouse control is also split: the wheel moves and the space bar switches, so mouse players need two hands for a game pitched as low-dexterity.

**Source.**
- "Public visé : très large, car le jeu ne demande pas une grande maîtrise des commandes."
- "Switch : faire glisser la ligne de la bille vers la droite"; "4 switchs vers la droite équivalent à 1 vers la gauche"; "il n'est pas possible d'annuler un switch".
- "Colonne toujours sur la ligne médiane ... elle remonte jusqu'à ce que sa première bille revienne sur la ligne médiane."
- PC controls: "Move (haut/bas) : molette de la souris", "Switch : une touche du clavier".
- Omission: no first-hour difficulty ramp (for example a slower fill or a longer timer on the first level of a run) to make room for learning the controls.

---

## Finding 3 — The loss condition is opaque and can feel unfair

**Severity: MAJOR**

**Problem.** Game over is triggered when no *move* is possible, "même si un switch reste possible". From the player's seat they can still act, so the game ends while they feel they had moves left. That is the textbook recipe for "that's not fair, I wasn't dead". The danger state itself, columns getting longer and losing range of motion, is gradual and hard to read. The only signals that warn of an approaching loss are graded below MVP:
- the panic effect ("Approche de la défaite" = Important)
- the board-filling layers (Important)
- the game-over explosion itself (Important)

So in a cut-down build, a player could lose with no warning and no clear moment of defeat. The document also never specifies a post-loss explanation, such as showing *why* the board was blocked.

**Source.**
- "Plateau bloqué : si le joueur ne peut plus faire aucun move ... c'est le game over, même si un switch reste possible."
- Événements table: "Approche de la défaite ou de la fin du temps ... Important"; "Game over (plateau bloqué) ... Important"; "Tableau qui se remplit (arcade, VS) ... Important".
- Omission: no MVP-level "danger" indicator (per-column remaining range, board-fill meter) and no end-screen explanation of the loss.

---

## Finding 4 — Feedback for skilled play is the most cuttable layer. In VS/arcade, audio intensity tracks losing.

**Severity: MAJOR**

**Problem.** The fantasy promises an "effet waw" that rewards the player. But the effects that reward *skill* are the ones marked as cuttable:
- environmental reward for an elimination or successful attack: **Optional**
- combo escalation: Important
- 4- and 5-matches: Important
- excess wave sent back: Important

Meanwhile the effect for a mere 3-match is MVP. So the MVP feels the same whether you set up a 4-chain stack combo or stumble into a 3-match. That removes the main motivation for learning the deep systems.

On top of that, in arcade/VS the mid-game visual and musical build-up is triggered "quand le tableau du joueur commence à se remplir". The audiovisual crescendo, the game's signature reward, is tied to the player *doing badly*. A player who plays well keeps a calm, sparse scene and never sees the "voyage". That inverts the Tetris Effect model the document cites, where the journey responds to the player's actions.

**Source.**
- Fantasy: "Effet visuel gratifiant dans l'environnement quand le joueur élimine des billes ou lance une attaque"; "c'est leffet waw recherché".
- Événements table: "Élimination ou attaque réussie ... effet gratifiant dans l'environnement ... Optionnel"; "Combo × 2, × 3 ... Important"; "Alignement de 4 / 5 ... Important".
- Retours audio et visuels: "arcade/VS : quand le tableau du joueur commence à se remplir".
- "Ce qui peut être coupé : certains événements du tableau d'événements (ceux marqués Optionnel)".

---

## Finding 5 — There is nothing to pull a player back after the first hour, and the "wow" is a difficulty spike

**Severity: MAJOR**

**Problem.**
- **The journey has no content yet.** All 5 themes are "À définir", including how their order maps to difficulty and the emotional arc. Yet the "voyage à travers le temps, l'espace et la nature" is the stated reason to keep playing.
- **The runs are padded.** Solo and arcade offer runs of 10 or 15 levels, but only 5 levels exist, so long runs must repeat themes. That cuts against the "sentiment d'évasion" of moving forward.
- **No long-term hooks.** No unlocks, cosmetic rewards or meta-progression are described. The Leaderboard document is listed but doesn't exist, so the only long-term goal can't be judged.
- **The "wow" works against the core audience.** The AI-driven moment is "le challenge corsé que propose le NPC en mode arcade". For a "très large" audience pitched on calm and contemplation, a punishing AI is more likely to cause churn than delight. No arcade difficulty selection or adaptive tuning is described.
- **The fantasy and the default modes disagree.** The contemplative fantasy ("persévérance et contemplation") is only really delivered by Zen. Every other mode is timed or under constant fill pressure.

**Source.**
- Themes_niveaux: every row "À définir"; "Reporté à la version suivante du GDD : Arc émotionnel du voyage".
- "parcours de 5, 10 ou 15 niveaux" / "série de 5, 10 ou 15 adversaires" vs. "5 niveaux distincts".
- Documents liés: "Leaderboard", "Mode Solo", "Mode Arcade" with no content.
- "Moment « wow » rendu possible par l'IA : le challenge corsé que propose le NPC en mode arcade".
- Arcade progression: "l'IA devient plus agressive", with no player-facing difficulty option mentioned.

---

## Round 2 — Cross-examination

### CONFLICTS

**C1. Against Feasibility F1 and Business F1: "cut the MVP" must not cut learning. "Zen as the tutorial" doesn't fix onboarding.**
I agree the MVP is too big. But both scope cuts remove *modes and content* and add nothing to teach the core loop. Business F1 suggests "Zen as the tutorial", and the GDD already claims that ("sert aussi de mode d'entraînement ... et de tutoriel") without giving Zen a single tutorial step. Relabelling Zen doesn't teach the counter-colour / different-colour-release rule (my F1). The scope arguments treat onboarding as free. My position: a short scripted tutorial (counter, release, stack, wave-cancel) belongs *inside* any reduced MVP, ahead of a second theme or Arcade. It is mostly text and code, which is what the budget says the agents can actually produce (Feasibility F2). Without it, cutting the game to 1-2 themes just means fewer players see the themes before they quit.

**C2. Against Narrative F1 (voyage = BLOCKING): for the first hour, an untaught loop is a bigger risk than an unwritten arc.**
The narrative critic grades the empty voyage BLOCKING. From the player's chair, the order is reversed. A player who doesn't understand why their 5-line in the counter colour did nothing quits in theme 1 and never reaches the arc. The empty theme table is a serious retention problem (my F5, MAJOR), but it costs the players who *stayed*. My F1 costs everyone. I'd ask the moderator to rank "the player can learn the win condition" above "the voyage has an arc" for production order, even if both end up BLOCKING.

**C3. Against Narrative F5 (crescendo-tracks-losing = MINOR), with a partial concession.**
The narrative critic is right that escalation tied to danger can be a legitimate panic design. Versus puzzle games commonly speed up the music as the stack climbs. So my claim that this "inverts the Tetris Effect model" overstated it, and I'm withdrawing that framing (see Revisions). But I disagree with MINOR for a reason only my lens sees, and combining it with Systems F2 makes it sharper (see Connection X1). The issue isn't the *meaning* of the layer. It's that the layer fires on a board state that the game's own rules say may *not* be dangerous.

**C4. Against Adversarial QA F5.2: in-match hints are the wrong fix. My F1 should not be read as "more highlights".**
QA shows that the first-game highlight leaks into competitive play: one player in local VS gets live hints, and a ranked Solo score gets posted with assists on. That conflicts with any onboarding fix that adds more in-match aids. Resolution from my side: teaching should happen in a separate, unscored tutorial sequence (or a scripted Zen first run), and assists should be disabled in Solo-ranked and VS. That satisfies QA's fairness concern and my learnability concern. The current design, a 5-second idle hint during a real first game in whatever mode the player picks, fails both.

### CONNECTIONS

**X1. Systems F2 + my F3/F4 + Narrative F5: the game's feedback lies about danger.**
Systems F2 shows that a victim's columns gain mobility as they grow from 5 to about 8 marbles, so small attacks *help* the receiver. Meanwhile, in VS/arcade the music gains a layer and new visuals appear "quand le tableau du joueur commence à se remplir". Together, a player who receives a small attack hears and sees escalation (danger, or reward, depending on Narrative's reading) while their board has actually become easier to play. Players learn danger from feedback. Here the feedback and the mechanics disagree, so players will build a wrong mental model: they'll panic when they're safe and feel calm when long columns are actually locking up. Neither reviewer alone sees this. It's a mechanics bug *and* a feedback bug, and the player only experiences the combination.

**X2. Systems F3 + QA F2 + my F3: the real loss experience is a zombie state, not a sudden death.**
Systems F3 shows that "no move possible" effectively requires all 75 cells to be full. QA F2 shows that release can become impossible well before that, because the guaranteed colour sits out of reach in long columns. From the player's seat: they can't release, so they can't attack or counter-attack, but the game isn't over. They have to sit through a match they already know they've lost, with no concede option described and, in VS, no round timer. In a party mode that's the moment someone puts the controller down. It's a worse experience than the "unfair game over" I described in Round 1 (see Revisions).

**X3. Systems F4 + my F1: the hardest event in the game is also the untaught one.**
Systems F4 shows that a 5-line can't be built up gradually on the median because any 3 adjacent marbles clear immediately. It has to be completed in one move into an `X X _ X X` gap or dropped from a stack. That's the single event that drives the gauge, attack and counter-attack, and it requires the most counter-intuitive technique, which the onboarding (3-match only, idle-triggered, first game only) never shows. A new player will be told by the HUD to "fill the gauge" and will have no idea that the method is to *avoid* the 3-matches the tutorial just taught. This strengthens my F1.

**X4. Systems F5 + my F4: in Solo, the stack/combo skill has neither a mechanical nor a sensory reward.**
Systems F5 shows that combos barely touch the gauge, which decides Solo wins, and that score has no formula. My F4 shows that combo and 4/5-match feedback is not MVP. So in the mode the GDD headlines, a player who learns "la tactique à adopter" gets no progress toward winning *and* no distinct audiovisual payoff. Mastery that is neither useful nor felt doesn't get learned. That's why I'm upgrading the skill-feedback half of F4 (see Revisions).

**X5. Feasibility F4 + QA F5.1 + my F2: the hardest control scheme meets the least practiced players.**
Local VS is the "party game" front door, and party guests are by definition first-time players. My F2 found the single-player PC scheme already needs two hands (wheel plus space) and spatial planning. Feasibility and QA show that no second-player mapping exists at all, so the second player will get whatever is left of the keyboard. That's very likely the more awkward half, handed to the less experienced player. A couch session where one player is fighting the controls is the fastest way to make the mode feel unfair.

**X6. Business F2 + my F5: the document never chooses a "front door".**
Business F2 shows that three audiences are being pitched. From the player side, the concrete consequence is that the GDD never says which mode a new player lands in first, or which one the menu recommends. The first mode played *is* the game for that player: Zen says "meditative", Arcade says "punishing", VS says "party". Since the onboarding only runs during "la toute première partie ... quel que soit le mode", the first-hour experience is decided by a menu choice the document doesn't design.

### REVISIONS

- **F1 (BLOCKING): keep, strengthened.** Systems F4 (X3) shows the untaught objective is also the hardest event to execute. I'm also narrowing my recommendation per QA F5.2 (C4): fix onboarding with an unscored tutorial, not with more in-match hints.
- **F2 (MAJOR): keep, strengthened for VS** by Feasibility F4 and QA F5.1 (X5).
- **F3 (MAJOR): reframe. I withdraw the "game ends while you still have moves" sub-point.** Systems F3 shows that a move-blocked board essentially means 75/75 cells full, which is visible, not surprising, so my "I wasn't dead" scenario was wrong. I'm replacing it with the zombie-state problem from X2: the player can't release, can't win and can't lose, and no concede or timeout exists. The point about a missing MVP danger indicator and post-loss explanation stays. It's reinforced by Narrative F4, which shows victory and defeat use word-for-word identical endings, so a loss isn't even clearly signalled as different from a win. Severity stays MAJOR.
- **F4 (MAJOR): split.** The *crescendo-tracks-losing* half is downgraded to MINOR in itself, conceding Narrative's point that danger-driven escalation can be deliberate (C3). It only regains weight through X1, where the trigger fires on a non-dangerous board. The *skill feedback is cuttable* half is upgraded to the core of the finding, given Systems F5 (X4) and Business F3's independent call to promote environmental reactivity to MVP. Overall: MAJOR, with the emphasis moved.
- **F5 (MAJOR): keep.** I agree with Feasibility F1 and Business F1 that Arcade should be cut or deferred. That also removes my "wow is a difficulty spike" concern from the first-hour path. If Arcade stays, it needs a difficulty selection.
