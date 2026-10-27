# Narrative Critic — Round 1

Scope: story logic, emotional arc, tone, theme, and whether the structure pays off the promises in the Concept and Fantasy sections. Systems balance, scope and business are out of my lane.

---

## 1. The central promise, a "voyage through time, space and nature", has no content at all

**Severity: BLOCKING**

**Problem.** The Fantasy section rests the whole emotional identity of the game on a journey: "Chaque niveau se parcourt comme un voyage à travers le temps, l'espace et la nature. une forma d'initiation à la persévérence et la comtemplation, un sentiment d'évasion et d'accomplissement aprés chaque épreuve, c'est leffet waw recherché." Every document that should carry that promise defers it:

- `Themes_niveaux.md` is "Document en attente de rédaction". All 5 rows (Thème, Ambiance, Musique, Motifs, Difficulté) are "À définir".
- "Arc émotionnel du voyage, d'un thème à l'autre" is explicitly "Reporté à la version suivante du GDD".
- Agent Narratif and Agent Intro are both budgeted (10 sessions each). Agent Intro is supposed to generate "une séquence d'introduction différente pour chaque niveau", but no document says what those intros contain, what they depict, or what the journey goes from and to.

There is also a structural mismatch the theme doc never addresses. There are only 5 themed levels, but Solo runs are "5, 10 ou 15 niveaux" and Arcade is "5, 10 ou 15 adversaires". A 15-step journey across 5 themes means repeating stops, and the document never says whether the arc loops, stretches, or simply restarts. A journey with no destination and no order is not a journey.

**Why it matters.** This is the "effet waw" the document names as its goal. As written, production would build 5 skins with no throughline. The agents tasked with the narrative have nothing to generate from.

---

## 2. Tonal contradiction: "contemplation" is promised, but every mode is built on pressure

**Severity: MAJOR**

**Problem.** The Fantasy promises "une initiation à la persévérance et la contemplation", a "très calme" opening, and an "ambiance ASMR" (Événements, Variations sonores). The rest of the structure works against contemplation:

- The Concept pitch is aggressive: "ensevelissez votre adversaire".
- Solo is timed ("en temps limité"), and the document punishes slow play by design: "Jeu au hasard... pénalisé par le temps limité".
- "Remplissage continu : **dans tous les modes**... Le tableau finit donc toujours par se remplir". Taken literally, that includes Zen.
- The intended arc of every match ends in "Effet de panique" and an "explosion" (Game over and Victoire rows).

The only candidate for a contemplative space is Mode Zen score. But it is framed as "l'équivalent d'un mode entraînement" whose point is to "battre le record de combo du classement", which makes it a leaderboard grind, not contemplation. Its "options pour régler la fréquence d'apparition des billes" do not say whether the frequency can be zero. If it can't, the rule above means Zen also always ends in a full board.

**Why it matters.** "Persévérance" (pressure, endurance) and "contemplation" (stillness) are presented as one theme, but nothing in the document reconciles them. The player gets a stress-arcade experience under a meditative marketing promise. The document needs to say which mode delivers the contemplative half and protect it structurally.

---

## 3. In Arcade/VS, the "journey" escalates as the player loses, not as they progress

**Severity: MAJOR**

**Problem.** In "Retours audio et visuels", the mid-game visual reveal, which is how the journey unfolds on screen, triggers differently by mode:

- solo: "quand la jauge de progression ou le temps atteint certains paliers"
- **arcade/VS: "quand le tableau du joueur commence à se remplir"**

The Événements table confirms this ("Tableau qui se remplit (arcade, VS) | la musique gagne une couche | nouveaux éléments visuels derrière le tableau").

So in two of the four modes, the environment opens up and the music swells as a reward for being buried. Doing well (keeping the board clean, countering waves) keeps the player in the "minimaliste" opening and hides the voyage. That inverts the stated promise of "un sentiment d'évasion et d'accomplissement après chaque épreuve". The scenery should be the reward for accomplishment, but here it reports failure. Leading opponents never see the journey they were promised.

**Why it matters.** The audiovisual journey is the game's main storytelling device (the document cites *Tetris Effect* for "le voyage audiovisuel"). Tying its progression to the danger meter means the narrative and the player's success point in opposite directions.

---

## 4. Victory and defeat get the same ending beat, so "accomplishment" has no payoff

**Severity: MAJOR**

**Problem.** The Événements table gives the two outcomes identical treatment:

- "Game over (plateau bloqué) | explosion | explosion visuelle, puis dissipation de la scène | maximale"
- "Victoire (fin de jauge, adversaire bloqué) | explosion | explosion visuelle, puis dissipation de la scène | maximale"

The GDD's "Fin de partie" phase likewise has one ending: "explosion visuelle, puis dissipation des éléments de la scène". Both are marked only "Important", not MVP, so under schedule pressure the game could ship with no designed ending beat at all.

There is also no defined payoff for finishing a whole run. Clearing a 15-level Solo parcours or beating the 15th Arcade opponent has no ending, no closing intro/outro, and no arrival point for the journey. The Boucle de jeu's end screen offers only "continuer vers le prochain niveau, de recommencer en cas de défaite ou de revenir au menu".

**Why it matters.** "Accomplissement après chaque épreuve" is a stated pillar. If winning looks and sounds exactly like losing, and finishing the journey looks like finishing any level, the arc never closes.

---

## 5. The Arcade opponents, billed as the "wow" moment, have no identity

**Severity: MINOR (MAJOR if Arcade is meant as the narrative spine)**

**Problem.** The document names the AI-driven moment "wow" as "le challenge corsé que propose le NPC en mode arcade". Agent NPC produces only "un profil scripté (behaviour tree) pour piloter les tableaux". Opponents are defined purely by how hard they play ("l'IA devient plus agressive"). Nothing gives them a name, a persona, a link to the level themes, or a reason for the player to face 5, 10 or 15 of them.

The cited inspirations make this gap stand out. *Puyo Puyo*'s arcade progression (cited here for its attack system) is carried by its opponent cast, and *Lose Your Marbles* had a character framing that the document explicitly discards ("sans la cours de récré") without proposing a replacement. Agent Narratif "génère la progression du joueur en modes solo et arcade", but the document never says what that narrative progression is beyond difficulty ramps.

**Why it matters.** A "série d'adversaires" with no faces is a difficulty slider, not a sequence of encounters. The intended "wow" will land as "this got harder" rather than "I beat *that* one". At minimum, the document should state whether opponents are deliberately faceless (pure abstraction fitting the contemplative journey) or are meant to be characters tied to each theme. Right now it is neither.

---

## Round 2 — Cross-examination

### Conflicts

**vs. Business Analyst #4 ("Agent Narratif ... The game has no story ... a configuration table, not narrative work").** Partial conflict. I concede the narrow point: as the document defines it, Agent Narratif "génère la progression du joueur", which is a level/opponent list, and that is not narrative work. Merging it into Menu/Gameplay is reasonable. But "the game has no story" is the wrong conclusion. The Fantasy section defines the product's story as the voyage itself: "voyage à travers le temps, l'espace et la nature", "évasion et accomplissement après chaque épreuve". That is environmental narrative, and it needs an owner: someone has to decide the order of the 5 themes, what the journey goes from and to, and what the intros depict. If the board reads BA #4 as "narrative is decorative, cut it", the Fantasy's stated "effet waw" loses its only author. My position: cut the agent, keep the responsibility, and assign the "Arc émotionnel du voyage" (currently "Reporté") to whoever writes Themes_niveaux before production.

**vs. Business Analyst #1 recommendation ("1–2 themed levels") and Feasibility Lead #1.** I accept the scope verdict; scope is not my lane. But the narrative cost of that cut should be on the record: one or two themes cannot be a "voyage à travers le temps, l'espace et la nature". If the cut happens, the Fantasy must be rewritten, or the journey must move inside a single level. That raises the stakes of my Finding 3: the in-level reveal ("nouveaux éléments visuels derrière le tableau") would become the *only* journey left, and in Arcade/VS it is driven by the board filling up, so it rewards losing. A scope cut without fixing Finding 3 ships a game whose one remaining narrative device points the wrong way.

**vs. Business Analyst #4 ("Make Zen a Solo setting") and Player Psychologist #5 (Zen as "the only safe practice space").** These two colleagues pull Zen in different directions, and both leave the contemplative half of the theme with no home. The BA folds Zen into Solo, which is timed. The Psychologist reframes it as a learning space, which is useful, but practice is not contemplation either. My Finding 2 asks which mode delivers "contemplation". Every proposed fate for Zen so far removes the only candidate. Whatever happens to Zen, the document must either name a contemplative space (no fill, no score, no panic state) or remove "contemplation" from the Fantasy. Keeping the word with nothing behind it is the one option I'd argue against.

**vs. Feasibility Lead #3 and Business Analyst #4 on the two "wow" moments.** The BA rightly notes that the document names two competing wows (audiovisual journey vs. "le challenge corsé que propose le NPC"). Feasibility shows that the NPC wow sits on the riskiest, least-specified system. From the narrative side, I side with the journey. It is the wow stated in the Fantasy, the one the title echoes (*Tetris Effect*), and the one the tone of the document supports. A hard AI opponent fits "ensevelissez votre adversaire", not "contemplation". This also resolves my own Finding 5 (see Revisions).

### Connections

**Feasibility #4 + Business Analyst #2 → Narrative Finding 1.** I flagged that the journey has no content. These colleagues show that it also has no producer: Budget_tokens says Claude makes "pas d'images ni de musique", the artist only "pourra" join, and no composer is named. Combined, the voyage is undefined, unowned and unfunded. Agent Intro (10 sessions) is budgeted to generate "une séquence d'introduction différente pour chaque niveau" with no assets to sequence and no story to tell. This is the clearest case on the board where the document pays for narrative tooling while funding neither the narrative nor the content.

**Player Psychologist #4 (patterns change per theme) → theme expression.** This is a real conflict between my lens's interest (themes should feel distinct) and accessibility, and I side with the Psychologist. The document puts theme identity into the one element that must stay readable: "Le motif de chaque couleur est propre à la thématique du niveau". The voyage should be expressed in environment, music and light, not in gameplay glyphs. Stable patterns cost the journey nothing, and changing them makes each new "stop" a relearning penalty. That turns arrival somewhere new into a punishment.

**Adversarial QA F5 (Solo time-out never stated to be a loss) → Narrative Finding 4.** My finding said victory and defeat share one ending beat. QA shows there is a third outcome with no beat at all. The Événements table covers only "plateau bloqué" and "Victoire", so a Solo level that ends on time has neither a defined result nor a defined audiovisual ending. In the one mode where the journey is supposed to unfold through progress ("quand la jauge ... ou le temps atteint certains paliers"), the most common ending is undefined.

**Adversarial QA F3 / Systems Designer F4 (uncapped counter, a ×17 release kills outright) → Narrative Finding 3.** The intended match arc is calm → layering → "Effet de panique" → explosion. A one-shot kill from a hoarded counter skips straight from calm to explosion, so the dramatic structure the events table designs never plays out. The escalation curve needs the attack economy to be bounded. This is a systems fix with a direct narrative payoff.

**Player Psychologist #2 (same-color line plays a "note ascendante") → audio language.** Together with my Finding 4, this shows a pattern: the audio vocabulary does not distinguish success from failure. A non-release sounds like success, and losing sounds like winning. If the sound design is the storytelling medium (Tetris Effect lineage), it needs a consistent grammar: rising for progress, something else for "no effect", and different endings for win and loss.

**Feasibility #3 (no tunable NPC parameters) → Narrative Finding 5.** An inexpensive way to give opponents identity: the tuning parameters Feasibility asks for (move rate, aggression, deliberate errors, hold-vs-release tendency) can be authored as named play styles tied to each theme. The identity then comes from behaviour, with no character art needed.

### Revisions

- **Finding 1 (BLOCKING): kept, strengthened.** Feasibility #4, BA #2, Systems F2 and QA F5 all independently reach the same empty Themes_niveaux table and the 5-vs-15 mismatch from their own lenses. That is high-confidence convergence.
- **Finding 3 (MAJOR): kept, flagged as higher priority if BA's scope cut is adopted.** Under a 1–2 theme MVP it becomes the main narrative issue.
- **Finding 2 (MAJOR): kept.** The Psychologist's #5 independently confirms the Zen contradiction, and the competing proposals for Zen (BA, Psychologist) make it more urgent, because none of them restores contemplation.
- **Finding 4 (MAJOR): kept, scope widened.** It now includes the undefined Solo time-out ending (from QA F5).
- **Finding 5: downgraded to firm MINOR, recommendation changed.** In Round 1 I left open whether opponents should be characters. Given the BA's point that the document must pick one wow, and Feasibility's point that the NPC is the riskiest system, I now recommend that opponents stay faceless, with identity carried by theme-linked play styles. I'm withdrawing the "MAJOR if Arcade is the narrative spine" branch. Arcade should not be the narrative spine.
- **Concession:** my Round 1 treated Agent Narratif as an unfed narrative role. The BA is right that, as written, its job is configuration. My complaint is now about the missing responsibility (the theme order and the arc), not the missing agent.
