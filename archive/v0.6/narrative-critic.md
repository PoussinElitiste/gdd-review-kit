# Narrative Critic — Round 1

Scope: story logic, thematic promise vs. structural payoff, tone, emotional arc. Systems balance, scope and business are left to other reviewers.

## Finding 1 — The central fantasy (the "voyage") has no content, yet it is an MVP deliverable
**Severity: BLOCKING**

**Problem.** The document's only narrative and emotional promise is the journey: "Chaque niveau se parcourt comme un voyage à travers le temps, l'espace et la nature. une forma d'initiation à la persévérence et la comtemplation, un sentiment d'évasion et d'accomplissement aprés chaque épreuve, c'est leffet waw recherché" (Fantasy). The companion doc that should deliver it, `Themes_niveaux.md`, is explicitly "Document en attente de rédaction". Every cell of the theme table (Thème, Ambiance, Musique, Motifs, Difficulté) for all 5 levels reads "À définir", and both the theme-to-difficulty mapping and the "Arc émotionnel du voyage, d'un thème à l'autre" are "Reporté à la version suivante du GDD".

**Why it matters.** The MVP lists "5 niveaux avec leur thème", and the calendar is 7 weeks with the GDD writing included. The production plan also has agents (Skin, Visuel, Ambiance, Intro) that each work "pour chaque thème de niveau". Right now none of them has a brief. "Temps, espace, nature" is three axes spread over five levels with no stated order, so we cannot tell whether the journey runs chronologically, cosmically, or something else. We also cannot tell what "persévérance" means level to level, or where "contemplation" peaks. Without an arc, five unrelated skins will be built and labeled a voyage after the fact. This is the experience the game says it is selling ("c'est l'effet waw recherché"), so it cannot be deferred past the version that drives production.

## Finding 2 — Tonal split: contemplative initiation vs. "bury your opponent," and the document names the wrong "wow"
**Severity: MAJOR**

**Problem.** The document pulls toward two incompatible tones and never settles which one leads.
- Contemplative pole: "initiation à la persévérance et la contemplation", "ambiance lounge électro", "ambiance ASMR" (Variations sonores), Zen mode "pour apprendre et se détendre".
- Aggressive pole: the Concept's very first line is "ensevelissez votre adversaire". The game also has "Effet de panique", sudden death where fill speed "double toutes les 30 secondes", and an arcade AI that "devient plus agressive".

The contradiction is made explicit in the Agents section: "**Moment « wow » rendu possible par l'IA** : le challenge corsé que propose le NPC en mode arcade". The Fantasy says the wow is evasion and accomplishment across the journey. The production plan says the wow is a hard AI opponent. These are two different games.

**Why it matters.** Music, VFX intensity, panic effects and intro sequences all inherit their tone from whichever pole wins. The document needs one sentence saying which mode carries the thematic voyage and which is the competitive frame, and whether the voyage tone is supposed to survive inside VS and arcade. Otherwise "contemplation" is just a word in the pitch.

## Finding 3 — The audiovisual arc rewards failure in arcade/VS, and victory and defeat share the same ending
**Severity: MAJOR**

**Problem.** The Tetris Effect inspiration ("la réaction du niveau aux actions du joueur, le voyage audiovisuel") promises a scene that grows *with* the player's success. The structure breaks that promise in two places:
1. **Escalation is tied to losing in arcade/VS.** The middle phase adds visual elements "solo : quand la jauge de progression ou le temps atteint certains paliers" but "arcade/VS : quand le tableau du joueur commence à se remplir". The events table repeats this: "Tableau qui se remplit (arcade, VS) | la musique gagne une couche | nouveaux éléments visuels". In competitive modes, the journey unfolds as the player's board fills up, which means the "voyage" is a picture of approaching defeat. The "effet gratifiant dans l'environnement" for a successful elimination or attack is the only success-driven environmental feedback, and it is marked **Optionnel**, the first thing to be cut.
2. **No emotional difference at the end.** The events table gives "Game over (plateau bloqué)" and "Victoire" identical treatment: "explosion | explosion visuelle, puis dissipation de la scène | maximale". The Fantasy promises "un sentiment ... d'accomplissement après chaque épreuve", but the closing beat is the same whether the player was buried or triumphant. The "accomplissement" payoff has no design.

**Why it matters.** Climax and resolution are where a journey structure pays off. As specified, success-driven spectacle is optional, and the ending cannot tell the player whether they succeeded.

## Finding 4 — Narrative is delegated to agents with no narrative spec; arcade opponents have no identity
**Severity: MAJOR**

**Problem.** The roster includes an "**Agent Narratif** : génère la progression du joueur en modes solo et arcade" and an "**Agent Intro** : génère une séquence d'introduction différente pour chaque niveau". The game loop confirms "Une introduction se joue" before every match. Nowhere does the document say what is being narrated:
- Arcade is "une série de 5 adversaires contrôlés par une IA", yet the opponents get no names, personas, motives, or relation to the journey. They differ only by aggression ("elle enchaîne les combos et envoie ses billes plus vite"). Who is the player fighting through time, space and nature, and why?
- The intros have no content brief, no length, no tone. They depend on themes that are "À définir" (Finding 1).
- "Mode Solo" and "Mode Arcade" appear in Documents liés with no link and no content.

**Why it matters.** Story intent has been handed to a generative agent with no authorial direction. That agent will either invent the game's narrative identity unreviewed, or produce generic filler. Ten budgeted sessions of "Agent Narratif" have nothing to execute against. An opponent ladder with no faces is also a missed payoff for the "challenge corsé" that the document itself calls the wow moment.

## Finding 5 — The journey only exists in one mode, and Zen undercuts "persévérance → accomplissement"
**Severity: MINOR**

**Problem.** The voyage is framed as a sequence ("chaque niveau se parcourt comme un voyage", "après chaque épreuve"), but only Solo has a fixed "parcours de 5 niveaux". In Arcade, opponents are fought "à travers différents niveaux" with no stated order. In VS, players set "niveaux" as a parameter. So for two of the four modes, the arc is either shuffled or player-picked. Zen is the mode most aligned with "contemplation", and in it "le niveau recommence automatiquement" with no endpoint, so there is perseverance with no accomplishment beat. The planned "parcours de 10 et 15 niveaux" updates would also stretch an arc that has not yet been defined for 5.

**Why it matters.** The document should say which mode is the canonical journey and how the theme order holds (or deliberately doesn't) elsewhere. Otherwise the "initiation" reads as marketing copy and not as structure.
