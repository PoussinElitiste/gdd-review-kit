# Adversarial QA — Round 1

## Finding 1 — The game-over rule says two different things, and nothing says what happens when marbles arrive with no room for them
**Severity: BLOCKING**

**Problem.** The two documents give two different lose conditions.
- GDD, *Cas limites / Plateau bloqué*: game over only if every column is blocked in both directions **and** "aucun switch ne peut plus former d'alignement sur la ligne médiane".
- Combo_system, *Colonne pleine*: "Si toutes les colonnes de l'adversaire sont pleines, c'est le game over pour lui". There is no exception for switches.

Under the movement rules, only a 15-marble column can't move. A column of 14 always has a legal position that still covers L8. So "every column blocked" really means "the board is full". On a full board where one switch rotation would line up 3 marbles, the GDD says play continues and Combo_system says the player has lost. Two programmers would build two different games.

Related gaps:
- *Remplissage continu* keeps adding marbles "par le haut" in every mode, and sudden death doubles the rate every 30 s. Nothing says where a marble goes when its column is full, or when the top marble already sits at L1. The "next shortest column" fallback is only defined for attack waves.
- The engine has to check every switch rotation every frame to decide the game is over. That cost isn't mentioned, and the rule isn't explained to the player anywhere.
- In Zen, "le niveau recommence" depends on the same unclear rule.

**Source:** GDD, Cas limites, "Plateau bloqué" and "Remplissage continu". Combo_system, Attaque, "Colonne pleine". Schemas_deplacement, 4b.

## Finding 2 — The board is called "déterministe", but key parts are random, which breaks leaderboard fairness and online sync
**Severity: MAJOR**

**Problem.** *Ordre de résolution* says the fixed order exists "pour que le plateau soit déterministe (même situation, même résultat)". But the same resolution draws random values:
- refill colours (step 4);
- the counter's starting colour ("tirée au hasard");
- continuous spawns;
- the tie-break when an attack wave picks between columns of equal length.

No seed is mentioned. As a result:
- **Solo leaderboard.** Two players on the same level get different openings, counter colours and spawns, so ranked scores can't be compared. A player can also restart until they get a good roll. Restarting is a one-click option on the end screen (*Boucle de jeu*).
- **VS / arcade fairness.** Nothing says whether both boards share one random stream.
- **Online (Agent Multi, "lobby + game state").** A lockstep or rollback model needs shared seeded randomness. Without it, each client's board drifts from the other's.

**Source:** GDD, Alignement et élimination, "Ordre de résolution" ("déterministe"). Combo_system, "couleur, tirée au hasard" and "le choix entre elles se fait au hasard". GDD, Mode solo ("classement").

## Finding 3 — Bonus/malus marbles appear in every mode and in MVP effects, but the rules are never written, and the docs disagree on which modes have them
**Severity: MAJOR**

**Problem.** Bonus/malus marbles show up in:
- Arcade and VS (Concept);
- Solo, Arcade and VS, "à chaque élimination" (Progression);
- Zen, where the player can toggle which ones are active;
- the event table ("Important").

But "Billes bonus/malus" in *Documents liés* has no link and no content. Specific breaks:
- **Mode list contradiction.** Concept lists bonus/malus only for Arcade and VS. Progression adds Solo. In Solo there is no opponent, so it's unclear what a malus does or who triggers it.
- **Possible conflict with generation guarantees.** If a bonus/malus marble spawns "à chaque élimination", it may break "pas d'alignement dû au hasard" or the guarantee of "au moins 5 billes de même couleur … différente du compteur". Neither section mentions special marbles. It's also unclear whether a bonus marble counts toward a 3-marble alignment or a 5-marble line, or whether it has one of the 5 colour+pattern pairs at all (Accessibilité).
- **Zen options with nothing behind them.** Zen promises toggles for bonus/malus types that have never been listed.

Since this is in MVP scope ("Modes Solo, Arcade, VS en local et Zen"), the Gameplay and Menu agents would have to make up a whole system on their own.

**Source:** GDD, Mode arcade, Mode VS, Progression et thèmes (all 4 modes), Documents liés (unlinked "Billes bonus/malus"). Evenements_visuels_et_sonores, "Bille bonus ou malus qui apparaît".

## Finding 4 — Local 2-player VS on PC has no input plan, and first-game help can unbalance a match
**Severity: MAJOR**

**Problem.** Local VS is a headline mode ("mis en avant") and part of the MVP. The PC controls, though, are written for one player:
- Arrows **or** A/D, and W/S **or** arrows, are given as alternatives for the same actions.
- There is one switch key ("espace ou Entrée droite").
- Mouse selection and the mouse wheel are not assigned to either player.

The document never says who gets which keys, whether the mouse is disabled in VS, or how controllers are detected. Shared keyboards also drop simultaneous key presses in a fast puzzle game. "Le jeu peut se jouer en local à deux sur toutes les plateformes" also covers mobile, where two 5×15 boards on one touchscreen are never described.

Onboarding edge cases:
- The idle hint and the alignment highlight are active during "la toute première partie du joueur, quel que soit le mode". If that first game is a VS match, it's unclear whose "first game" counts (the save profile? each player?).
- If only one board gets the alignment highlights, that player has an advantage in a competitive round.
- A player who starts in Zen, which is explicitly the tutorial, uses up their only hints there. Arcade's AI then gets no help on its side either.
- Nothing says whether quitting the first game early counts as having played it.

**Source:** GDD, Contrôles et onboarding (PC controls, "Note : … local à deux sur toutes les plateformes", onboarding passif, "Règle d'alignement horizontal"). GDD, Public visé.

## Finding 5 — Attack waves and round endings have timing gaps: no first-wave delay, unclear end-of-round behaviour, and arcade draws can repeat
**Severity: MAJOR**

**Problem.**
- **No first-wave delay.** The only timing given is "1,5 seconde" between waves. Nothing says how long before the *first* wave lands, so a ×1 release (one wave) may hit with almost no chance to counter. That undercuts the "vrai choix" the doc advertises.
- **The worked example skips time.** In the counter-attack example, B releases while all 7 of A's waves are still pending. At 1.5 s per wave, B would need to release within the first ~1.5 s. Otherwise some waves have already landed and "reste en place".
- **Stacked releases bypass the cap.** Back-to-back releases can queue 20+ waves (60+ marbles) against a 75-cell board. The ×10 cap was meant to stop a single release from filling the board, and stacking defeats that.
- **Pending waves when a round ends are undefined.** Unclear cases: whether pending waves still land when a board blocks, when sudden death starts, or after the opponent is already blocked. Also unclear: if a landing wave blocks the target at the same moment the attacker blocks from spawns, does that count as a draw?
- **"Au même moment" is undefined.** Same frame? Same resolution step? Since blocking is checked against switch rotations (Finding 1), a simultaneous block can happen more often than expected.
- **Arcade draws can repeat.** In arcade, "un match nul se rejoue contre le même adversaire". It's unclear whether the whole best-of-N restarts at 0-0. Nothing stops endless replays against a mirrored or high-skill AI, and a draw doesn't count toward Solo/Arcade progression or score.

**Source:** Combo_system, Attaque ("Délai entre deux vagues", "au plus 30 billes") and Contre-attaque (example, "Seules les vagues encore en attente…"). GDD, Cas limites, "Durée d'une manche" and "Égalité".
