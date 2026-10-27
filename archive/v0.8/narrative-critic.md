# Narrative Critic — Round 1

Document reviewed: `gdd.txt` (The Marble Effect, GDD v0.5 + linked documents).
Lens: story logic, thematic promise vs. structural payoff, tone, emotional arc.

---

## Finding 1 — The cut list can remove the journey's ending, and nothing replaces it

**Severity: BLOCKING**

**Problem.** The document sells the game as a journey with a destination. The production plan allows that destination to be cut, and it gives no fallback ending. The executive summary promises "un voyage audiovisuel ... de La Source à La Nébuleuse". The emotional arc (Thèmes des niveaux, "Arc émotionnel du voyage") builds to step 5, "La Nébuleuse, la contemplation et l'accomplissement ... la victoire clôt le voyage."

**Passages.**
- Technical Strategy > Scope: "Ce qui peut être coupé, dans cet ordre : ... les niveaux 4 et 5, le mode Zen, puis l'adversaire 4. Le Solo, le VS en local et l'adversaire 5 (le moment « wow ») ne sont jamais coupés."
- Production order: "(5) niveaux complets, 3 niveaux avant les niveaux 4 et 5."

**Why it matters.** Levels 4 and 5 are third on the cut list, so they go early. If they are cut, the Solo journey runs nature, then the past, then stops at Les Sables. The arc labels that level "l'épreuve" and "premier vrai mur", which is a midpoint beat, not an ending. Both "temps futur" (La Cité) and "l'espace" (La Nébuleuse) vanish, so two of the three promised journey axes (nature, temps, espace) go too. The plan protects the systems climax (opponent 5) but not the narrative climax. It never says what the 3-level version's ending would be. It also doesn't say whether Les Sables would be re-staged as a finale (music, arc, "final épique" treatment). A cut this likely needs a planned narrative fallback, or the arc needs a different structure.

---

## Finding 2 — The document has two different "wow" moments and protects only one

**Severity: MAJOR**

**Problem.** The document can't decide what its emotional peak is.

**Passages.**
- Concept > Fantasy: the journey through time, space and nature, "une forme d'initiation à la persévérance et à la contemplation, avec un sentiment d'évasion et d'accomplissement ... C'est l'effet « wow » recherché."
- AI Architecture: "Le dernier adversaire est le moment « wow »"; Agents Roles: "Moment « wow » rendu possible par l'IA : le challenge corsé que propose le NPC en mode arcade."
- Executive Summary frames the experience as "inspiré de *Tetris Effect*", which is a contemplative, synesthetic journey.

**Why it matters.** The Fantasy section defines the wow as escape and contemplation. The AI section defines it as a hard, mastery-gated fight. These are opposite emotional registers: release versus pressure. The second definition wins every structural decision (never cut, Opus-tier production, its own row in the cut rules), and the first is left exposed (see Finding 1). As written, the document's opening promise isn't what the structure delivers. The GDD needs one stated emotional peak, or an explicit statement that Solo's peak is contemplative and Arcade's is competitive, with each protected accordingly.

---

## Finding 3 — "Contemplation" is promised but the structure works against it

**Severity: MAJOR**

**Problem.** Contemplation is the stated theme, but every mode that carries the journey punishes stillness, and the level billed as the most contemplative is mechanically the most frantic.

**Passages.**
- Arc émotionnel: "5. La Nébuleuse, la contemplation et l'accomplissement."
- Lien entre thèmes et difficulté: La Nébuleuse = "30 jauges", "une bille toutes les 2 s": the largest goal and the fastest refill in the game. The difficulty column says "la plus difficile : le final".
- Fin de partie: "Jeu au hasard : enchaîner des coups sans réfléchir est pénalisé par le remplissage automatique" — pressure is constant even with no time limit.
- Onboarding: "si le joueur ne fait aucune action pendant 5 secondes, le jeu met en surbrillance une bille" — idling is treated as a problem to fix.
- The only low-pressure space, Zen ("l'intérêt est d'apprendre et de se détendre"), has no theme or journey assigned to it, and it is on the cut list.

**Why it matters.** The text says "calme immense au départ, puis la nébuleuse s'embrase". At a 2-second refill, though, the calm opening is the moment of greatest danger. The audiovisual script and the pressure curve are out of sync. The document should either redefine the final beat as climax/triumph rather than contemplation, or specify how the calm phase is protected (for example, a slower refill during the opening milestone). It should also say how Zen relates to the journey, if at all.

---

## Finding 4 — Arcade has opponents but no characters, framing, or reason to fight

**Severity: MAJOR**

**Problem.** Arcade is the stated home of the climax, yet it has no narrative content. Opponents are identified only by number and behaviour ("Adversaire 1 ... 5"). They have no names, no identity, and no link to the journey. The "Agent Narratif" turns out to be a progression sequencer, not a narrative author.

**Passages.**
- AI Architecture table: opponents defined only by "Comportement visible" and "Ce que le joueur apprend".
- Thèmes des niveaux: opponents 1-5 sit in the same rows as levels 1-5, but the document never says whether Arcade matches happen in those themed environments or what the opponent *is* within that world.
- Agent Narratif: "l'enchaînement des 5 niveaux du solo et des 5 adversaires de l'arcade, et ce qui se débloque à chaque étape". What gets unlocked is never defined, and "Menu et options", "Mode Arcade" are "à rédiger".
- Agent Intro: "une courte séquence présente le thème de l'étape du voyage" — scoped to levels, not opponents.

**Why it matters.** A "wow" final opponent only lands emotionally if the player knows who or what they are facing, and why beating it means something. Right now opponent 5 is "tous ces comportements, au rythme le plus rapide": a difficulty spike with no dramatic setup. Even a light framing would let the Arcade climax pay off as a narrative moment and not just a stat check. Options: opponents as guardians of each realm, as echoes of the player, or something else.

---

## Finding 5 — Victory and defeat look and sound identical, and the journey has no ending sequence

**Severity: MINOR** (cheap to fix, but it affects the "accomplissement" promise)

**Problem.** The Fantasy promises "un sentiment d'évasion et d'accomplissement après chaque épreuve". The event table gives triumph and failure the same feedback, and the document specifies intros only, no outro.

**Passages.**
- Événements visuels et sonores: "Game over (tableau plein) | explosion | explosion visuelle, puis dissipation de la scène" and "Victoire ... | explosion | explosion visuelle, puis dissipation de la scène". Same sound, same visual.
- Retours audio et visuels: "Fin de partie : explosion visuelle, puis dissipation des éléments de la scène", with no distinction by outcome.
- Agent Intro covers "une séquence d'introduction différente pour chaque niveau". Nothing covers finishing the journey, even though the arc claims "la victoire clôt le voyage".

**Also noted (same finding, coherence):** the journey's axes are stated inconsistently. Fantasy says "à travers le temps, l'espace et la nature", while Progression and Thèmes say "de la nature au temps, puis à l'espace". "Temps" jumps from the past (Les Sables) to the future (La Cité) with no present, and the thematic logic of that jump is never explained. The final level's music (~100 BPM) is also slower than La Cité (~115 BPM), even though it is billed as the "final épique". That may be intentional, but the document should say so.

**Why it matters.** If winning and losing feel the same, the player gets no felt payoff for persevering, and perseverance is the document's own stated theme. Victory needs a distinct emotional signature, and the journey's completion needs at least a short closing beat.

---

## Round 2 — Cross-examination

### 1. CONFLICTS

**vs. Business Analyst F1/F2 (pick one identity; possibly reposition toward "mechanics-first competitive puzzle").** I agree with the diagnosis. My Finding 2 (two "wow" moments) is a narrative symptom of the identity split BA F2 describes, and I accept BA's framing as the root cause. I disagree with the implied remedy, though. Dropping the journey would not make the theme layer go away. The MVP is defined as "5 niveaux avec leur thème", there is a 5-step emotional arc, and Agent Intro builds one sequence per level. All of that would become dead weight with nothing to anchor it. The cheaper fix is narrative, not market-level. The Fantasy already names two themes, "persévérance" and "contemplation". If the journey is rebuilt around perseverance (trial, then wall, then triumph), it fits the pressure the Solo systems actually create (my F3, Psychologist F2) and the competitive Arcade climax (my F2). Contemplation then moves to Zen, which is the one mode built for it. That merges two of BA's three identities without cutting a mode. The board should not read BA F2 as "the journey has to go". It should read it as "the journey's emotional register is mis-specified."

**vs. Business Analyst F3 (per-level intro sequences are "decorative" and should be on the cut list).** I partly disagree. Agent Intro is currently the *only* narrative channel in the whole document. There is no text, no characters, and the Agent Narratif is a sequencer (my F4). Putting intros on the cut list is reasonable. Putting them there with no replacement leaves the journey with zero framing outside placeholder art (see Connection A). My position: intros can go on the cut list only if a near-zero-cost fallback is named, for example a title card with the level name and its arc line ("La Source — découvrir dans le calme"). Also, the missing *outro* (my F5) carries more load than any single intro, because it is where "la victoire clôt le voyage" is supposed to happen. If cut-list priority is being reordered, the closing beat should rank above intros 2-4.

**vs. Feasibility Lead F2 ("redefine opponent 5 so it doesn't need deliberate stacking").** From my lens this is acceptable, but it creates a problem the Feasibility Lead doesn't mention. If opponent 5 is no longer a mechanical spectacle, its "wow" has to come from staging: setting, music, framing, identity. Feasibility F4 and BA F1 both show that this presentation layer is unfunded and unscheduled. So the cheap AI fix moves the climax onto the channel least likely to exist. Whichever option is chosen, the board should require that the opponent 5 "wow" be defined in terms the plan can actually deliver. Today neither the AI route nor the presentation route is secured.

### 2. CONNECTIONS

**A. The journey's narrative depends entirely on the one unfunded channel (my F1/F4 + BA F1 + Feasibility F4).** The document has no dialogue, no named characters and no narrative text. Its whole story lives in environment art, per-level music and intro sequences. BA F1 and Feasibility F4 establish that this layer is placeholder, has an optional unnamed artist, and has no budget line. Combined, a placeholder build doesn't just look worse. It has *no story at all*: "La Source" and "La Nébuleuse" are indistinguishable if both are grey boxes with library music. This makes cutting levels 4-5 (my F1) more likely, and it means even the surviving levels may not convey their arc beat. The document needs at least one narrative carrier that costs nothing in art, such as level names, an arc line, or a closing text.

**B. The protected climax can lose its setting (my F4 + Adversarial QA F4 point 2).** QA notes that opponents map to levels in the Thèmes table, so cutting level 5 leaves the never-cut opponent 5 "with no theme, environment or music". Combined with my F4 (opponents have no name or identity), the realistic worst case is that the protected "wow" ships as an unnamed difficulty spike in an unthemed arena. The structure protects the label "moment wow" and nothing that would make it feel like one.

**C. Neither "wow" has a dramatic shape (my F2 + Systems F1 / QA F1).** Systems and QA both show that Arcade/VS rounds have no forcing function, and boards drift toward the floor. In narrative terms, the final fight has no rising action and no third act: nothing escalates, so nothing builds toward a climax. Their proposed fixes (escalating attack ratio, timer-triggered autofill, sudden death) are also the narrative fix, because escalation is what gives a climactic match its shape. So I'd revise my F2. It isn't that one wow is protected and the other exposed. *Neither* is structurally delivered: the contemplative one can be cut, and the competitive one has no ending.

**D. The scoring contradicts the theme of perseverance (my F5 + Systems F2 / QA F3).** The Fantasy's core promise is "initiation à la persévérance". Systems F2 shows the Solo score is capped at the win threshold. QA F3 shows the best leaderboard play is to stall, then fire one huge chain. So the only public measure of achievement rewards neither sustained effort nor endurance. It rewards a single burst. Combined with victory and defeat sharing the same feedback (my F5), the game currently has no signal anywhere, felt or numeric, that rewards persevering. That is a theme-versus-structure contradiction, not just a balance issue.

**E. Opponent 4 is a missing mentor beat (my F4 + Systems F4 / Psychologist F1 / BA F3).** Three colleagues flag opponent 4 as the sole teacher of the stack. Narratively it is the only opponent with a defined *role* ("ce que le joueur apprend"), which is a mentor/rival beat that sets up the final test. Cutting it breaks the one-to-one level/opponent ladder and removes the setup for the payoff. Feasibility F2 adds that cutting it saves almost no work. Four lenses therefore converge on the same recommendation: remove opponent 4 from the cut list. Light framing (my F4) would make its lesson legible as a lesson, not just a harder AI.

### 3. REVISIONS

- **Finding 5: upgrade MINOR to MAJOR.** Psychologist F3 points out something I missed: the Victory, Game over, "Combo × 2, × 3" and "Jalon atteint" events are all priority *Important*, not MVP. "Jalon atteint" (music layers added per milestone) is the mechanism behind the in-level arc ("calme immense au départ, puis la nébuleuse s'embrase"). So the in-level dramatic structure and the payoff beat are both outside MVP. That is a structural gap, not a cheap polish item.
- **Finding 5 sub-note: withdraw the BPM point.** A slower final track is a common, legitimate choice for a contemplative finale. Without evidence it is an error, it is not a finding. The axis-ordering inconsistency stays as a minor coherence note.
- **Finding 3: keep MAJOR, broaden scope.** Psychologist F2 shows the same contradiction at the *start* of the arc ("sans pression" against 4-second autofill from the first second). The stated emotional beats are contradicted at both ends of the journey, not just the finale, which strengthens the case for re-registering the arc around perseverance (see Conflicts above).
- **Finding 2: keep MAJOR, reframe.** Per Connection C, the finding becomes "neither emotional peak is structurally delivered," and BA F2 is acknowledged as the root cause.
- **Finding 4: keep MAJOR, narrow the ask.** Given Feasibility's schedule constraints, I'm not asking for characters or cutscenes. The minimum is a name plus a realm assignment for each opponent, shown on the match screen. Arcade matches should be set in the matching level environment, with a stated fallback if that level is cut.
- **Finding 1: keep BLOCKING, higher confidence.** BA F1 and Feasibility F4 make it more likely that levels 4-5 get cut, because the art they need is unfunded. The lack of a narrative fallback for a 3-level journey is now the likely outcome, not an edge case.
