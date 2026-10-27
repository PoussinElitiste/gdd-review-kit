# Narrative Critic — Round 1

Lens: story logic, motivation, tone, theme, and whether the structure pays off what the document promises. The GDD makes one central experiential promise, the "voyage" from La Source to La Nébuleuse as "une forme d'initiation à la persévérance et à la contemplation, avec un sentiment d'évasion et d'accomplissement après chaque épreuve. C'est l'effet « wow » recherché" (Fantasy). The findings below show where the production plan, the mode structure and the feedback spec work against that promise.

---

## Finding 1: The cut order removes the journey's payoff and keeps a different "wow" in its place

**Severity: BLOCKING**

**Problem.** The document gives two incompatible answers to "what is the wow moment?", and its cut list protects the wrong one. If the cuts happen, the narrative arc ends at its lowest point.

**Evidence.**
- Fantasy: the journey through the levels is "l'effet « wow » recherché."
- Technical Strategy / Agents Roles: "Moment « wow » rendu possible par l'IA : le challenge corsé que propose le NPC en mode arcade." AI Architecture repeats this: adversary 5 is "le moment « wow »."
- Cut order: "les niveaux 4 et 5, le mode Zen, puis l'adversaire 4. Le Solo, le VS en local et l'adversaire 5 (le moment « wow ») ne sont jamais coupés."
- Themes_niveaux, emotional arc: 3 = "Les Sables, l'épreuve : la difficulté monte nettement", described in the difficulty table as "premier vrai mur"; 5 = "La Nébuleuse, la contemplation et l'accomplissement : … la victoire clôt le voyage."

**Why it matters.** If levels 4 and 5 are cut, the Solo journey (which is "never cut") ends on Les Sables, the beat the document itself calls the trial and the wall. The player gets the ordeal with no catharsis, and the "accomplissement" and "contemplation" beats disappear. Adversary 5 survives the cut, but the difficulty table pairs it with "5 La Nébuleuse", a theme that no longer exists, so the protected "wow" no longer has a setting. The team needs to decide which wow is the game's identity, and the cut list has to protect the arc's ending (even as one compressed finale) rather than only a mechanical boss.

---

## Finding 2: "Contemplation" is promised but the structure leaves no room for it

**Severity: MAJOR**

**Problem.** The theme is persévérance *and* contemplation. The design delivers perseverance through timers, continuous fill and panic effects. It never builds a space where contemplation can happen, and the one contemplative mode is on the cut list.

**Evidence.**
- Arc beat 1: "La Source, l'éveil : le joueur découvre le jeu dans le calme, sans pression." But level 1 still has a 3-minute timer ("Temps solo 3 min"), a bille every 4 s, the rule that "le tableau finit toujours par se remplir", and game over on a blocked board. "Sans pression" is not true under these rules.
- Arc beat 5 calls La Nébuleuse "la contemplation". It is also the most punishing level in the game (30-unit gauge, a bille every 2 s, adversary 5 "au rythme le plus rapide"). The theme description says "calme immense au départ", yet nothing in the rules creates calm at the start of the hardest level. The board fills at its fastest rate from second one.
- Zen is the only mode without defeat or ranking ("l'intérêt est d'apprendre et de se détendre"), but it sits in the cut list ahead of adversary 4.
- Accessibilité and the audio spec add "Effet de panique à l'approche de la défaite" as a core beat of every session.

**Why it matters.** The thematic word that sets this game apart from Puyo Puyo is "contemplation". Right now that word appears only in the arc labels and nothing in the systems backs it. The document should name the moments where contemplation happens: between levels, in the intro sequences, in a post-victory dissolve, or in a genuinely pressure-free first level. Otherwise it should drop the claim.

---

## Finding 3: Victory and defeat are the same beat, and the voyage has no ending

**Severity: MAJOR**

**Problem.** The emotional payoff the Fantasy promises ("un sentiment d'évasion et d'accomplissement après chaque épreuve") has no specified delivery. Winning and losing look and sound identical, and the journey has an opening sequence per level but no closure.

**Evidence.**
- Evenements_visuels_et_sonores: "Game over (plateau bloqué) | explosion | explosion visuelle, puis dissipation de la scène | maximale | Important" and "Victoire … | explosion | explosion visuelle, puis dissipation de la scène | maximale | Important". The rows are word-for-word the same.
- GDD audio phases: "Fin de partie : explosion visuelle, puis dissipation des éléments de la scène", with no distinction by outcome.
- Agent Intro produces "une séquence d'introduction différente pour chaque niveau". No agent, document or event row covers an outro, a between-level transition, or the end of the voyage, even though the arc says "la victoire clôt le voyage."
- Both end events are "Important", not "MVP", so under the stated priorities they may not ship with the game at all.

**Why it matters.** "Accomplishment after each trial" is the reward that justifies the perseverance theme. If failure produces the same climax as success, the player gets no emotional signal that the trial was overcome, and the last line of the arc has nothing to land on. Define distinct win and loss beats, and at least one closing moment for completing the voyage.

---

## Finding 4: The adversary has no fiction, and the combat framing clashes with the initiation theme

**Severity: MAJOR**

**Problem.** Arcade, which the document calls the game's wow moment, is the mode with the least narrative framing. The pitch tone ("ensevelissez votre adversaire") doesn't fit the journey tone, and the document never explains who the player is fighting on an initiatory voyage, or why.

**Evidence.**
- Concept tagline: "faites monter vos combos et ensevelissez votre adversaire". Compare the Fantasy: "initiation à la persévérance et à la contemplation."
- The adversaries are defined only as numbers and behaviours ("petites attaques fréquentes (1), grosses attaques (2) …"). They have no name, presence, theme link or reason to oppose the player.
- "Agent Narratif" is only responsible for "l'enchaînement des 5 niveaux du solo et des 5 adversaires de l'arcade, et ce qui se débloque à chaque étape". That is sequencing and unlocks, not narrative. No narrative content (characters, framing, text) is assigned to any agent.
- "Mode Solo (à rédiger)" and "Mode Arcade (à rédiger)" are both unwritten.
- It is never stated whether Arcade travels through the five themes. The difficulty table implies adversary *n* plays on level *n*, but AI Architecture never says so.

**Why it matters.** AI_Architecture argues that "chaque défaite montre une tactique que le joueur peut ensuite reproduire". That is a strong mentor/student premise that fits "initiation" well. But the document never presents the adversaries that way, and the "bury your opponent" tagline pushes in the opposite direction. Either give the five adversaries a framing that serves the initiation theme (guardians or teachers of each stage, for example), or accept that Arcade has no story and stop calling it the wow moment of a journey.

---

## Finding 5: The journey's axis is described inconsistently

**Severity: MINOR**

**Problem.** The document describes the voyage's structure three different ways, so the themes blur at the edges.

**Evidence.**
- Fantasy: "un voyage à travers le temps, l'espace et la nature" (time, space, nature).
- Progression et thèmes / Themes_niveaux: "de la nature au temps, puis à l'espace" (nature, then time, then space).
- But La Canopée, a nature level, is the one where "le temps qui passe se voit dans les saisons", so it already carries the time theme. Les Sables is labelled "le temps passé" while being a desert, which is itself a nature setting.
- The Executive Summary says "de La Source à La Nébuleuse" and gives no axis at all.

**Why it matters.** This is minor in isolation. But Agent Intro is to generate "une courte séquence [qui] présente chaque étape du voyage", and Agent Skin and Agent Ambiance work per theme. Agents fed contradictory descriptions of the arc will produce intros that don't add up to one journey. Pick one canonical axis and state it identically everywhere.

---

## Round 2 — Cross-examination

### CONFLICTS

**C1. Business Analyst, Finding 4 ("Put intro sequences high on the cut list"; Agent Narratif and per-level intros listed as "decorative items").** I disagree with half of this. I agree that Zen and adversary 4 are load-bearing and should not be cut first. That matches my Finding 1 and Finding 2. But treating the intros and the Agent Narratif layer as the obvious things to sacrifice removes the only narrative delivery the document actually specifies. As I noted in Finding 3, the per-level intro is the *only* moment that frames each level as a stage of a voyage. There are no outros, no between-level transitions, and no text. If the intros are also cut, the "voyage" becomes five skin swaps in a row, and the Fantasy that the Executive Summary sells has no delivery mechanism at all. The Agent Narratif layer is not decorative either. Its brief is "ce qui se débloque à chaque étape", which is exactly the missing retention layer that the Player Psychologist (Finding 5) and the Systems Designer (Finding 5) both flag as absent. My position: per-level *cinematic* intros can be cut, but they have to be replaced by a cheap, protected narrative spine, such as one text or title card per stage plus a distinct ending beat. Cutting the delivery entirely while keeping the "voyage" pitch is a promise the product can't keep.

**C2. Business Analyst, Finding 1, fix direction (b), "reposition early access as a mechanics-first build and drop 'wow' as the launch pitch."** This is cleaner commercially, but it doesn't finish the job. The document has two "wows" (my Finding 1): the audiovisual journey and adversary 5. Dropping the audiovisual wow from the pitch still leaves the Fantasy, the arc, the theme names and Agent Intro all built around a journey of initiation. If the team takes option (b), the narrative claims (Fantasy, Arc émotionnel, "contemplation") have to be rewritten at the same time. Otherwise the store page says "mechanics-first" while the game's structure still promises a voyage. I also reject the premise that narrative payoff depends on the missing art budget. Distinct win/loss beats, a closing moment and a framing for the adversaries (my Findings 3 and 4) are writing and sequencing decisions. They are cheap compared with final assets. Repositioning should not be read as permission to drop the arc.

**C3. Systems Designer, Finding 1, and Adversarial QA, Finding 1 (the fixes proposed).** Both propose making the loss condition harsher, for example a height threshold, or treating "no room for an incoming ball" as a block or loss. I accept the diagnosis. My concern is that a uniformly harsher loss condition pushes the whole game further from "calme" and "contemplation" (my Finding 2). Every mode, Solo level 1 included, shares the one game-over rule ("la seule condition de game over"). If the fix tightens it globally, La Source's "sans pression" gets even less true. My position: the fix should be scoped to the competitive modes (VS and Arcade), or Solo's early levels should get their own looser loss rule. The rules change and the arc have to be designed together, not sequentially.

### CONNECTIONS

**X1. Adversarial QA Finding 5 plus my Finding 4: the mentor premise is mechanically impossible as specified.** My Finding 4 suggested that the strongest available fiction for the adversaries is the one AI_Architecture already implies: "chaque défaite montre une tactique que le joueur peut ensuite reproduire", meaning the opponents are teachers. QA shows that the 1-2 action lookahead cannot perform stacking or deliberate releases. So the teacher can't demonstrate the lesson it exists to teach. This is worse than a missing framing. The document's only implicit narrative logic for Arcade is broken at the systems level. Any initiation fiction for the adversaries depends on QA's fix (a plan-driven AI, plus "shows its signature behaviour" in Agent Test). Add the Player Psychologist's Finding 5 (players under load don't watch the other board) and the "learn by watching defeat" premise fails on both the AI side and the player side.

**X2. Adversarial QA Finding 2 plus my Findings 1 and 2: the voyage's final beat is the one most likely to be unwinnable.** QA shows the release guarantee lapses once every column has more than 7 balls, and that this is normal on La Nébuleuse (one ball every 2 s). In Solo the gauge fills only through releases, so the player can be soft-locked with time still on the clock. Narratively, that means the "accomplissement" stage is the one where perseverance can become mathematically pointless. The theme is "initiation à la persévérance". A finale where persisting cannot succeed, and the player isn't told so, inverts the theme's moral. This raises the stakes of my Finding 2, which was about the absent "calme immense au départ". The final level isn't just missing calm. It can betray the core value the journey claims to teach.

**X3. Systems Designer Finding 4 plus my Findings 1 and 2: the numbers invert the emotional arc.** The Systems Designer shows that nothing makes level 3 a wall, and that the steepest spawn step is 4→5 (0.40 → 0.50 balls/s). Put that next to the arc text. Level 3 is labelled "l'épreuve" and "premier vrai mur", but the numbers place the real wall at level 5. Level 5 is labelled "contemplation", but it gets the biggest pressure jump. The labels and the levers point in opposite directions at both ends of the arc. This also undercuts one argument in my Finding 1. Even with every level shipped, the arc does not currently land where the document says it does.

**X4. Player Psychologist Finding 4 plus my Finding 3: in local VS, players may not know who won.** The Psychologist shows game over arrives without readable warning, and the warning cue is only "Important". My Finding 3 shows the victory and game-over events are word-for-word identical ("explosion visuelle, puis dissipation de la scène"). In local VS, with two boards on one screen, an unannounced blocked state followed by an identical explosion means the moment of victory can be indistinguishable from the moment of defeat. This is no longer only a matter of missing catharsis. It's a readability failure at the most important beat of the party mode the document says is "mis en avant".

**X5. Business Analyst Finding 3 plus my Finding 4: the tone split is an audience split.** My Finding 4 flagged the clash between "ensevelissez votre adversaire" and "initiation à la persévérance et à la contemplation". The Business Analyst shows "très large" covers competitive players, party players and ASMR/relaxation players. Those are the two tones in the document. The combat tagline speaks to the first two audiences and the Fantasy speaks to the last. The narrative can't be made coherent until the primary audience is chosen. I'd now frame Finding 4's fix as dependent on the Business Analyst's Finding 3 decision.

### REVISIONS

- **Finding 1 (BLOCKING): keep, strengthened.** The cut list versus level/opponent mapping is raised independently by the Systems Designer (F3), Feasibility Lead (F2), Adversarial QA (F3) and Business Analyst (F4). My contribution, which none of them make, is that the cut leaves Solo's arc ending on its "wall". After X3 I'm adding one caveat: the arc doesn't land correctly even without cuts, so the fix is both "protect a finale" and "make the numbers match the labels."
- **Finding 2 (MAJOR): keep, strengthened.** The Player Psychologist's Finding 3 confirms "sans pression" from the first-hour angle, and the Systems Designer's Finding 4 confirms there is no rest level. With X2 (the finale can soft-lock), I'd sit at the upper edge of MAJOR. I'm not upgrading to BLOCKING because a contemplative interstitial or a looser level 1 is a contained fix.
- **Finding 3 (MAJOR): keep, but I'm narrowing my ask.** Given the Feasibility Lead's Finding 1 (7 weeks, no milestones) and Finding 3 (no route to final assets), I withdraw the implied need for a produced outro sequence. The minimum is distinct win and loss audio/visual beats, promoted to MVP because of X4, plus one voyage-completion moment that can be a text card. That's cheap enough to be non-negotiable.
- **Finding 4 (MAJOR): keep, reframed.** After X1 and X5, the main issue is no longer "add flavour to the adversaries". It's that the implied teacher fiction depends on an AI the spec can't deliver, and on an audience decision that hasn't been made.
- **Finding 5 (MINOR): downgrade toward withdrawal if the intros are cut.** Its impact runs through Agent Intro, Agent Skin and Agent Ambiance producing inconsistent framing. If the team follows the Feasibility Lead (F3) and the Business Analyst (F4) on cinematic intros, the exposure shrinks to a wording fix in the Fantasy section. I keep it at MINOR only because the canonical axis is still needed for whatever replaces the intros (C1).
