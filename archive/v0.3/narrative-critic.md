# Narrative Critic — Round 1

Lens: story logic, motivation, tone, theme, and whether the document pays off the promises it makes. The Marble Effect has no plot, so "narrative" here means the experiential arc the GDD itself promises: a contemplative journey through time, space and nature that teaches perseverance and ends in a feeling of accomplishment.

---

## Finding 1 — The core fantasy, "the journey", has no content, and the agents meant to build it have no brief

**Severity: BLOCKING**

**Problem.** The Fantasy section rests the game's identity on a journey: "Chaque niveau se parcourt comme un voyage à travers le temps, l'espace et la nature. une forma d'initiation à la persévérence et la comtemplation, un sentiment d'évasion et d'accomplissement aprés chaque épreuve, c'est leffet waw recherché." The Positioning section repeats that the differentiator is a modernised production "avec un environnement « wow »". Nothing in the package describes what that journey is.

**Evidence.**
- `Themes_niveaux.md` calls itself "Document en attente de rédaction". All 30 cells of the theme table (Thème, Ambiance, Musique, Motifs, Difficulté for levels 1-5) read "À définir".
- The two items that would make it a journey are explicitly deferred: "Comment l'ordre des thèmes suit la montée de la difficulté" and "Arc émotionnel du voyage, d'un thème à l'autre".
- The MVP still requires "5 niveaux avec leur thème", and production hands this work to agents: the Agent Intro "génère une séquence d'introduction différente pour chaque niveau", the Agent Skin generates "les skins de chaque thème", and the Agent Narratif "génère la progression du joueur en modes solo et arcade". None of them has anything to depict. The Agent Narratif also has no definition beyond that one line. The doc never says whether "progression" means unlocks, text, framing or nothing at all.
- The 7-week calendar includes writing the GDD ("rédaction du GDD comprise"). The narrative design is therefore unwritten and has no time set aside for it.

**Why it blocks.** The mechanics are borrowed from *Lose Your Marbles* and *Puyo Puyo*. The only thing the document claims as its own is the audiovisual journey. Without a theme list, an order and an emotional arc, the Intro, Skin, Ambiance and Narratif agents will each invent their own answer. "Time, space and nature" will turn into five unrelated backdrops, not a voyage.

---

## Finding 2 — The tone is split between "contemplation" and "bury your opponent / panic", and no mode holds both

**Severity: MAJOR**

**Problem.** The document promises two emotional registers that work against each other and never says how they fit together.

**Evidence.**
- Contemplative register: "initiation à la persévérance et la contemplation", "ambiance très calme", "ambiance lounge électro", and in the audio doc "une ambiance ASMR".
- Aggressive register: the opening pitch says "ensevelissez votre adversaire". The audio arc has an "Effet de panique à l'approche de la défaite ou de la fin du temps" and "musique qui s'emballe". Solo is timed. The "wow" moment named in Agents Roles is "le challenge corsé que propose le NPC en mode arcade".
- The only mode that matches the contemplative promise is Zen. But Zen is framed as a training and tutorial mode ("sert aussi de mode d'entraînement ... et de tutoriel"), not the destination. The modes the document puts forward ("Le mode solo et le VS en local (party game) sont mis en avant") are the timed and adversarial ones.
- Every match runs the same three-phase arc: calm, then rising tension, then panic, then explosion. The journey framing is per level ("Chaque niveau se parcourt comme un voyage"), yet each level's emotional shape is the same pressure curve. The document never says how a "nature" level should *feel* different from a "space" level beyond its skin and music.

**Why it matters.** *Tetris Effect* is cited as the model, and it resolves this exact tension by making the music and visuals turn pressure into flow. This document lists the panic effect as a feature but never states what player emotion the end of a level is meant to produce: serenity, triumph or relief. The "perseverance and contemplation" theme is therefore a statement of intent with nothing in the structure to deliver it.

---

## Finding 3 — Victory and defeat use identical feedback, so the promised "accomplishment" never lands

**Severity: MAJOR**

**Problem.** The Fantasy promises "un sentiment d'évasion et d'accomplissement aprés chaque épreuve". At the moment that should pay this off, the document treats winning and losing as the same event.

**Evidence.** In `Evenements_visuels_et_sonores.md`:
- "Game over (plateau bloqué) | explosion | explosion visuelle, puis dissipation de la scène | maximale | Important"
- "Victoire (fin de jauge, adversaire bloqué) | explosion | explosion visuelle, puis dissipation de la scène | maximale | Important"

The GDD's audio section also describes only one ending: "Fin de partie : explosion visuelle, puis dissipation des éléments de la scène pour marquer la fin de la partie". Both rows are "Important" rather than MVP, so neither ending is even guaranteed to ship.

**Why it matters.** The end of a level is where the "voyage" should resolve. You arrive somewhere, the scene opens up, and you earn the next destination. As written, success and failure look and sound the same, and both end with the world dissolving. That fits defeat. For victory it reads as loss, which is the opposite of the promised "accomplishment". In VS, both players see the same explosion at the same moment, so the winner gets no distinct payoff at all.

---

## Finding 4 — The progression structure doesn't fit the journey: 15-stage runs over 5 themes, and arcade opponents with no identity

**Severity: MAJOR**

**Problem.** The game's structure works against the idea of a voyage that moves forward.

**Evidence.**
- Solo offers "parcours de 5, 10 ou 15 niveaux" and Arcade "une série de 5, 10 ou 15 adversaires ... à travers différents niveaux". But the game has exactly "5 niveaux distincts". A 10- or 15-stage run must revisit destinations. The document never says how (in order, randomly, with variations) or what that does to a "journey through time, space and nature". A journey that loops back to its first stop twice is not a journey.
- Arcade puts the player against 5-15 AI opponents. The document's only description of them is mechanical: "l'IA devient plus agressive", plus a behaviour-tree profile from the Agent NPC. They have no names, no faces, no reason to fight and no tie to the theme of the level they appear in. The "wow" moment is defined purely as difficulty ("le challenge corsé"). The opponent is the one element that could carry character and motivation, and it is reduced to a difficulty setting.
- It is also never explained why a contemplative traveller is "burying" anyone in the first place. Arcade and VS have no fictional framing that links the attack system to the voyage theme.

**Why it matters.** Even a narrative-light puzzle game needs one consistent answer to "where am I going and why". *Tetris Effect*'s Journey mode, the cited model, has a fixed authored sequence for exactly this reason. Here the parameterised run lengths and the faceless opponents mean the journey is never actually authored.

---

## Finding 5 — The "level reacts to the player" promise is the first thing allowed to be cut

**Severity: MINOR**

**Problem.** The Inspirations section takes from *Tetris Effect* "la réaction du niveau aux actions du joueur, le voyage audiovisuel", and the Fantasy lists "Effet visuel gratifiant dans l'environnement quand le joueur élimine des billes ou lance une attaque". The production priorities put this at the bottom.

**Evidence.** "Élimination ou attaque réussie | accent musical | effet gratifiant dans l'environnement | moyenne | Optionnel". The Scope section's "Ce qui peut être coupé" explicitly lists "certains événements du tableau d'événements (ceux marqués Optionnel)". The glass reflections ("Reflets lumineux dans les billes en verre"), which are part of the "glass marble" identity in the one-line concept, are also Optionnel. Accessibility adds that "Tous les éléments visuels du niveau se trouvent derrière le tableau", which further separates the environment from the play.

**Why it matters.** The link between the player's actions and the world is what turns a backdrop into a journey the player *makes*, not one they watch. If the Optionnel tier is cut, as the 7-week plan makes likely, the environment only reacts to timers and how full the board is, not to skill. The thematic claim borrowed from *Tetris Effect* is then gone. This is MINOR because the remaining tiers ("Palier atteint", "Tableau qui se remplit") still give some reactivity, but the document should either promote this event or stop claiming the inspiration.
