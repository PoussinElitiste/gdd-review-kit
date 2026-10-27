# Narrative Critic — Round 1

Document reviewed: `gdd.txt` (GDD_V_0_2 "The Marble Effect" plus linked documents).

The game's narrative ambition is carried entirely by its Fantasy section: a contemplative audiovisual "voyage" whose payoff is a sense of escape and accomplishment. My review asks whether the rest of the document delivers that. Mostly it does not. The promise is written down, but nothing in the structure carries it out.

---

## Finding 1 — The central promise (the "voyage") has no content and no arc
**Severity: BLOCKING**

**Problem.** The document's only real narrative claim is that "Chaque niveau se parcourt comme un voyage à travers le temps, l'espace et la nature. une forma d'initiation à la persévérence et la comtemplation, un sentiment d'évasion et d'accomplissement aprés chaque épreuve, c'est leffet waw recherché" (Fantasy). It calls this *the* intended wow effect. Every piece needed to deliver it is missing:
- `Themes_niveaux.md` is marked "Document en attente de rédaction". All 5 rows of the theme table (Thème, Ambiance, Musique, Motifs, Difficulté) read "À définir".
- "Arc émotionnel du voyage, d'un thème à l'autre" is explicitly "Reporté à la version suivante du GDD".
- The document never says what "time, space and nature" means as a sequence, and never says why the order produces "initiation" or "persévérance".

**Why it matters.** "5 niveaux avec leur thème" is an MVP item, and the calendar is 7 weeks including GDD writing. The Agent Skin, Agent Visuel, Agent Ambiance and Agent Intro are all expected to produce per-theme content, but they have no themes to work from. Without a defined arc, the levels become five unrelated skins, and the "journey" exists only in the pitch.

**Ask.** Before production starts, name the 5 themes, put them in order, and write one sentence per theme describing its emotional beat, plus one sentence on how theme 5 resolves theme 1.

---

## Finding 2 — Two competing "wow moments" and a split tonal identity
**Severity: MAJOR**

**Problem.** The document names two different wow moments, and they point in opposite directions:
- Fantasy: the wow is contemplation, escape, accomplishment ("c'est leffet waw recherché"). It is backed by "ambiance lounge électro", an ASMR concern in the sound doc ("essentielle à préserver dans une ambiance ASMR"), and Zen mode.
- Agents section: "**Moment « wow » rendu possible par l'IA** : le challenge corsé que propose le NPC en mode arcade". That is a hard, aggressive AI ("l'IA devient plus agressive").

The Concept line adds a third register: "ensevelissez votre adversaire". The game is pitched as a meditative journey, a brutal versus game, and a party game all at once. Nothing reconciles these. It is never explained how burying an opponent fits an "initiation à la contemplation".

**Why it matters.** Tone decides what the music, VFX and intros are *for*. If the team doesn't know whether the core feeling is calm mastery or adversarial pressure, every downstream agent will guess differently.

**Ask.** Pick one emotional thesis. Then explain how the other modes serve it, for example by framing versus play as "perseverance under pressure" inside the voyage.

---

## Finding 3 — Arcade and the "Agent Narratif" promise progression but no protagonist, opponents, or framing
**Severity: MAJOR**

**Problem.** Arcade pits the player against "une série de 5, 10 ou 15 adversaires contrôlés par une IA". An "Agent Narratif" is budgeted (10 sessions) to "génère[r] la progression du joueur en modes solo et arcade". An "Agent Intro" is to produce "une séquence d'introduction différente pour chaque niveau". Yet the document never says:
- who the player is, or who the opponents are (identities, personalities, or even whether they are characters at all);
- what an intro sequence shows or communicates;
- what "progression narrative" means beyond rising difficulty.

There is also a structural mismatch. The voyage is built from 5 levels, but runs are 5, 10 or 15 levels or opponents. The document does not say whether a 15-level run passes through the voyage three times or whether themes repeat. Either answer weakens the idea of a one-way journey.

**Why it matters.** A "narrative" agent with no narrative brief will produce generic filler. Fifteen faceless AI opponents add nothing to a journey fantasy. They are just difficulty steps wearing skins.

**Ask.** Either state plainly that the game has no characters or story beyond the environmental journey (and rename or rescope the Agent Narratif), or define an opponent and framing layer and map 5/10/15-length runs onto the 5-theme arc.

---

## Finding 4 — The payoff for "accomplishment" is identical to the payoff for failure, and the environment's reward reaction is cuttable
**Severity: MAJOR**

**Problem.** The Fantasy promises "un sentiment d'évasion et d'accomplissement aprés chaque épreuve". In the events table, though:
- "Game over (plateau bloqué)": "explosion / explosion visuelle, puis dissipation de la scène".
- "Victoire (fin de jauge, adversaire bloqué)": "explosion / explosion visuelle, puis dissipation de la scène".

These are word-for-word the same, so winning and losing end on the same emotional note. The GDD's "Fin de partie" audio phase also describes only one ending. In addition, "Élimination ou attaque réussie → effet gratifiant dans l'environnement" is marked **Optionnel**, even though the Tetris Effect inspiration is specifically "la réaction du niveau aux actions du joueur" and the Fantasy lists "Effet visuel gratifiant dans l'environnement" as a core feature.

**Why it matters.** The thematic payoff is supposed to be accomplishment after each trial. If victory doesn't feel different from defeat, and the world's response to skill can be cut first, then the player is never rewarded in the way the Fantasy says they will be.

**Ask.** Write distinct victory and defeat endings: for example, the voyage moves forward on a win, and the scene dissolves or rewinds on a loss. Raise the environmental reaction to skill to at least Important.

---

## Finding 5 — In VS and Arcade, the audiovisual crescendo is triggered by losing, not by progress
**Severity: MINOR**

**Problem.** The audio phases say the mid-game blossoming happens "solo : quand la jauge de progression ou le temps atteint certains paliers" but "arcade/VS : quand le tableau du joueur commence à se remplir". The events table repeats this: "Tableau qui se remplit (arcade, VS) → la musique gagne une couche / nouveaux éléments visuels". So in competitive modes, the "voyage" opens up as the player is being buried. In solo, it opens up as they succeed. The same musical layer means "you're winning" in one mode and "you're losing" in another.

**Why it matters.** This blurs what the escalation means emotionally. It can also reward the player who is falling behind with the richest visuals, which works against the "accomplishment" theme. It could be a deliberate "panic" design, but the document doesn't say so. The separate "Effet de panique" row suggests the team meant panic to be its own signal.

**Ask.** Decide whether escalation in VS and Arcade tracks danger or momentum, and state that choice explicitly. If it tracks danger, make sure the layer that unlocks reads as tension and not as reward.

---

## Round 2 — Cross-examination

### Conflicts

**C1. Business Analyst F3 and Feasibility Lead F1 recommend cutting the per-level intros and Agent Narratif. I partly disagree.**
Business Analyst F3 says to "Demote or cut Agent Intro and Agent Narratif" and calls them decorative. Feasibility F1 also lists intros among things to cut. I agree on the budget logic, and my own F3 already said the Agent Narratif has no brief. But I disagree that the intros are pure decoration. The document gives no other mechanism that moves the player *from* one theme *to* the next. A level ends with the same "explosion... puis dissipation de la scène" whether you win or lose (my F4), and then the next theme loads. If the intros are cut and nothing replaces them, the game has no authored transition at all, and the "voyage" becomes a level-select menu. My position: cut the Agent Narratif, but keep one short transition beat per *theme* (5, not 15; see Adversarial QA F3 on "5 intros or 15?"). Better still, fold it into the victory ending, which my F4 already asks to differentiate. That keeps the cost close to what my colleagues want while keeping the only structural piece the journey has.

**C2. Business Analyst F1 and Feasibility Lead F1 propose shipping 1-2 themes. That dissolves the Fantasy unless the Fantasy is rewritten.**
Both scope reviewers recommend an MVP with "1-2 finished themes". On production grounds that is probably right. But neither of them states the narrative consequence: a one- or two-stop "voyage à travers le temps, l'espace et la nature" is not a voyage. If this cut is made while the Fantasy text stays as it is, the shipped product breaks its central promise on the store page. I would not argue against the cut. I would argue it is only acceptable if (a) the full 5-theme arc is still *written* now, since my F1 ask is a page of text and not assets, and (b) the Early Access framing says explicitly that it ships the first leg of the journey. Otherwise Business Analyst F2's concern (one clear hook) gets worse rather than better.

**C3. Player Psychologist F2 treats the no-undo switch as punishment. The document's theme supports it, but only if it is framed.**
Player Psychologist F2 reads "il n'est pas possible d'annuler un switch" as "punishment for exploring". From a thematic angle, irreversibility is the one mechanic in the document that actually expresses its stated theme, "initiation à la persévérance". The problem is not the rule. The problem is that nothing in the onboarding or the tone frames the rule as part of that discipline (Player Psychologist F1 shows the onboarding teaches only match-3). I would keep no-undo and fix the framing, rather than adding undo. Player Psychologist F5 also suggests the contemplative fantasy is "only really delivered by Zen". I disagree that this is the fix. Shifting the fantasy into Zen alone abandons the claim that "chaque niveau" is a journey. The right fix is my F2 ask: one thesis that the pressure modes also serve.

### Connections

**X1. Systems Designer F2 + my F5: the music signals danger exactly when the board is getting healthier.**
Systems Designer F2 shows that early attacks of 3-12 marbles push the victim's columns toward length 7-8, which *increases* mobility. My F5 notes that in VS and Arcade the music "gagne une couche" when "le tableau du joueur commence à se remplir". Taken together, the first attacks a player receives make their board easier to play *and* trigger the audiovisual escalation. The game's feedback language says "you are in trouble / here is your reward" (ambiguous already, per my F5) at a moment when, mechanically, neither is true. The emotional meaning of escalation is not just unclear; in early VS it points the wrong way. This takes F5 from a tonal ambiguity to a feedback contradiction.

**X2. Systems Designer F3 + Adversarial QA F2 + my F4: some matches have no ending beat at all.**
My F4 says victory and defeat share one ending. Systems Designer F3 shows that VS matches can stall indefinitely, and Adversarial QA F2 shows that a player can be left unable to release but not yet lost. Adversarial QA F5 adds draws, which have no audiovisual event in the events table. So there are four end states (win, loss, draw, unwinnable stall) and none of them has a distinct authored payoff. The stall case is the worst one for the theme: a game about "accomplissement après chaque épreuve" can produce an épreuve that never ends.

**X3. Player Psychologist F3 + my F4: an ending that feels unfair cannot also feel like a dignified dissolution.**
Player Psychologist F3 notes that game over fires "même si un switch reste possible". The authored defeat is "dissipation de la scène", a soft, contemplative exit. Put together, the player gets a gentle dissolve at the exact moment they feel the game cheated them. That clash of tone is worse than a plain, clear loss screen would be. A distinct defeat ending (my F4 ask) must also *explain* the loss, as Player Psychologist F3 asks. These are the same fix.

**X4. Feasibility Lead F2 + my F1: the arc must be written for what can actually be produced.**
Feasibility F2 shows that the agents cannot generate textures, models or music, and that no artist or composer is confirmed. My F1 asks for an emotional arc across 5 themes. If the arc assumes bespoke art per theme, it will not be delivered. The arc should be carried by the things the pipeline can produce reliably: music layering, color, pacing, and the order of difficulty. That gives the narrative brief a practical constraint it currently lacks.

### Revisions

- **F5: upgrade MINOR → MAJOR.** Player Psychologist F4 reached the same finding on its own (the crescendo "is tied to the player *doing badly*"), and Systems Designer F2 (connection X1 above) shows the signal is also mechanically wrong in early VS. Two independent lenses plus a mechanical confirmation is enough to raise it.
- **F4: keep MAJOR, but broaden it.** It now covers all end states (X2), not just win vs. loss. Business Analyst F3, which independently flags the Optionnel tag on "effet gratifiant dans l'environnement" as cutting the product's identity, strengthens the second half of this finding.
- **F3: keep MAJOR, but narrow the ask.** Given the scope findings from Feasibility F1 and Business Analyst F1, I withdraw the "define an opponent and framing layer" option. The recommended path is the first one: state plainly that the game has no characters, cut or rescope the Agent Narratif, and map runs onto themes (see C1).
- **F1: keep BLOCKING, and clarify the ask.** It is a writing task, not an asset task, so it is compatible with the scope cuts my colleagues propose (C2). It should be done *before* those cuts are decided, because the arc determines which 1-2 themes are worth shipping first.
- **F2: keep MAJOR.** Business Analyst F2 independently reached the same "two contradictory wow moments" diagnosis from a market angle, and Player Psychologist F5 from a churn angle. Three lenses agree, so this is high-confidence.
