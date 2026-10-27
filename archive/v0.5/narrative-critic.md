# Narrative Critic — Round 1

Lens: story logic, tone, theme, emotional arc, and whether the structure pays off what the document promises. Systems balance, scope, and business questions are out of scope here.

---

## 1. The central fantasy is promised but entirely unwritten, and production is set up to generate content from an empty brief. BLOCKING

**Problem.** The only thematic promise the game makes is the "voyage" ("journey"). The GDD (Fantasy, l.26) says each level "se parcourt comme un voyage à travers le temps, l'espace et la nature", "une forme d'initiation à la persévérance et la contemplation, un sentiment d'évasion et d'accomplissement après chaque épreuve, c'est l'effet waw recherché." The positioning section (l.49) also rests on a "wow" environment. But `Themes_niveaux.md` is "Document en attente de rédaction": every cell of the theme table (theme, mood, music, motifs, difficulty) is "À définir", and both "Arc émotionnel du voyage, d'un thème à l'autre" and how theme order follows difficulty are "Reporté à la version suivante du GDD."

**Why it matters narratively.** The design depends on this journey, yet the document never says what it is a journey from or to, what time, space and nature mean in practice, or how five levels make up an initiation. Downstream roles assume the content already exists. The **Agent Intro** must make "une séquence d'introduction différente pour chaque niveau" (one different intro sequence per level), the **Agent Narratif** must make "la progression du joueur en modes solo et arcade", and the **Agent Ambiance/Skin** must theme five levels. None of them has anything to work from. On a 7-week calendar that already includes writing the GDD, the agents will invent the arc themselves, or there will be no arc. MVP lists "5 niveaux avec leur thème" as required, so this gap blocks production.

---

## 2. Victory and defeat end the same way, which undercuts "accomplissement après chaque épreuve". MAJOR

**Problem.** In `Evenements_visuels_et_sonores.md`, "Game over (plateau bloqué)" and "Victoire (fin de jauge, adversaire bloqué)" have identical entries: sound "explosion", visual "explosion visuelle, puis dissipation de la scène", intensity "maximale". The GDD's audio phases (l.187) also describe only one ending: "Fin de partie : explosion visuelle, puis dissipation des éléments de la scène."

**Why it matters.** The Fantasy promises a feeling of accomplishment after each trial and an initiation to perseverance. Perseverance only means something if the player can feel the difference between falling short and getting through. When winning and losing play out the same way, the emotional payoff of a level is the same no matter what happens. There is also no defined beat for losing in a way that says "try again" rather than "it's over", which is the beat an initiation to perseverance needs most. Even "Approche de la défaite" uses a "panic" effect, and nothing is said about how failure ties back into the journey.

---

## 3. The tone is split between "bury your opponent" and contemplative ASMR, and the document never reconciles them. MAJOR

**Problem.** The one-line concept (l.9) is aggressive: "faites monter vos combos et ensevelissez votre adversaire, au rythme d'une ambiance électro qui s'emballe". The Fantasy (l.23-27) and the sound document describe the opposite feel: "ambiance très calme", "lounge électro", "contemplation", "évasion", and an "ambiance ASMR" whose sound fatigue must be protected. Three of the four modes run on panic (time limits, a board that keeps filling, "effet de panique", sudden death that doubles the fill rate). The only mode that matches the contemplative promise is Zen, which is also described as the training mode and tutorial, and "pas de classement" (no leaderboard).

**Why it matters.** *Tetris Effect* (named as an inspiration) gets away with this because its journey frames the pressure as part of a flow state. This document never gives a framing. It doesn't say whether the journey is something you contemplate or a fight, or how "initiation à la contemplation" fits a game that mostly asks you to bury an opponent against a clock. Without that statement, every mode will feel different in tone, and the "voyage" will read as wallpaper behind a competitive game, not as the experience itself. The document needs one sentence of thematic thesis that ties pressure and calm together. A simple example: the calm returns *because* you persevered through the panic.

---

## 4. Arcade opponents have no identity, so the advertised "wow" moment has no dramatic content. MAJOR

**Problem.** Arcade pits the player against "une série de 5 adversaires contrôlés par une IA" (l.15). The only thing that changes between them is that "l'IA devient plus agressive" (l.165). The GDD names "le challenge corsé que propose le NPC en mode arcade" (l.234) as the AI-enabled "wow" moment. The document never says who these opponents are, whether they belong to the themed levels or the journey, how they are presented, or what beating the fifth one means. The **Agent NPC** produces a behaviour tree, and the **Agent Narratif** is given "progression" in arcade with no character brief.

**Why it matters.** A series of five opponents, presented as a ladder with a climax, sets up the expectation of rivals, escalation, and a final confrontation. As written, the opponents are only difficulty settings. The game's signature moment is then defined purely as being hard. That is a systems property, not an experience the player will remember or tell anyone about. Even a light characterization pass (a name, a play style that matches the theme, a short exchange in the intro) would turn the ladder into an arc. Without one, the arcade has no payoff at the end.

---

## 5. The journey has no designed endpoint, and its planned length changes after launch. MINOR

**Problem.** The game loop (l.53) ends with "continuer vers le prochain niveau, de recommencer en cas de défaite ou de revenir au menu". The document never describes what happens after the fifth level: no ending, no arrival, no closing beat for the journey or for the initiation it promises. On top of that, solo routes and arcade series of "10 et 15 niveaux viendront en mise à jour" (l.13, l.15).

**Why it matters.** An emotional arc across levels (already deferred, see Finding 1) can't be written if the number of steps isn't fixed. A 5-level arc stretched to 15 either repeats itself or has to be rewritten. The document should decide now whether the journey is a fixed authored arc with a destination, or an endless set of moods with no ending. That choice determines what the deferred "arc émotionnel" document can even contain, and what the level-5 ending must deliver for the Early Access audience who will only see the 5-level version.
