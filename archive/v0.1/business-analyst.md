# Production/Business Analyst — Round 1

Document reviewed: `gdd.txt` ("The marbles effect"), about 40 lines. The brief at the top is the document's own standard. It asks for a name, a one-sentence player action, the AI-enabled "Un agent, un effet wow" moment, and a Technical Strategy built around that moment, all within 5 pages.

## Finding 1: The load-bearing feature the brief requires is missing. There is no AI moment, no scope and no agent plan.

- **Problem:** The brief says the AI-enabled moment is why anyone would play, and that everything else should be designed around it ("Concevez toute la section « Stratégie technique » autour de la réussite de ce moment, avant de développer quoi que ce soit d'autre"). The document never names an AI moment. The sections that would hold it are empty headers: `## Scope`, `# Agents Roles`, `## Token Cost budget`. As written, the product is a match-3 puzzle with ambient polish. The feature that is supposed to set it apart is absent, so none of the other features can be judged as load-bearing or decorative.
- **Source:** Lines 3-6 (brief) against lines 36-40 (empty Scope, Agents Roles and Token Cost budget sections).
- **Severity:** BLOCKING

## Finding 2: No audience, platform or business model, in a very crowded genre

- **Problem:** The document never says who the game is for, what it runs on, or how it makes money (premium, F2P or ads). Those choices decide nearly everything else. A match-3 with ASMR and lounge-electro presentation reads as a casual mobile product, and that market is dominated by long-running live-ops titles with large acquisition budgets. The only differentiator offered is sensory polish, and a buyer cannot see or judge polish from a store listing. With no audience and no platform, nobody can decide session length, monetization hooks, level count or input scheme. The cursor-driven row/column slide, for example, plays very differently on touch than on a gamepad.
- **Source:** Omission across the whole One Page Concept. There is no audience, platform or monetization line anywhere in lines 10-35.
- **Severity:** MAJOR

## Finding 3: The two inspirations point to two different games

- **Problem:** The inspirations are SegaSoft's *Lose Your Marbles*, a competitive puzzle game built around head-to-head versus play, and *Tetris Effect*, a single-player synesthetic experience where music and visuals respond to player input. Each implies a different product, audience and production profile. One is a versus or party puzzler with competitive balance, garbage/attack systems and possibly netcode. The other is a premium audiovisual journey that needs a deep music and VFX pipeline. The document never says single-player or versus, campaign or endless, so it may be trying to be both. The title, "The marbles effect", strengthens the *Tetris Effect* reading and invites a direct comparison to a polished, critically acclaimed product. That comparison also raises a brand-confusion question.
- **Source:** Lines 31-34 (Inspirations) and line 14 (title). There is no stated game mode anywhere.
- **Severity:** MAJOR

## Finding 4: The feature list is mostly decorative presentation, with a heavy and uncosted content commitment

- **Problem:** Of the six "Fantasy" bullets, five are audiovisual polish: light reflections in glass marbles, marble-clink SFX, glass-breaking sound, environmental VFX and a lounge-electro soundtrack. Only one has scope consequences: "Une thématique très prononcée entre niveau (niveau, tableau, billes)". That bullet commits the team to unique environment, board and marble art per level theme, the most expensive kind of content, and gives no level or theme count. Meanwhile the things that actually keep players coming back are absent: progression, level structure, fail states, goals per level and meta loop. The document puts polish ahead of the game structure that polish is meant to serve. One bullet is also left unfinished ("Bruit de verre cassé lors"), so it is unclear when the glass breaks or whether breakage is a mechanic at all.
- **Source:** Lines 20-29 (Fantasy). There is no progression or level-structure content anywhere.
- **Severity:** MAJOR

## Finding 5: The core pitch sentence underdefines the product's one verb

- **Problem:** The brief asks for one sentence describing what the player does. The sentence given describes a cursor that slides "une colonne de billes verticalement et horizontalement" while matches count only "horizontalement". From a business view, this is the hook that would go on a store page and in a trailer. It is ambiguous about whether rows or columns move, and it says nothing about what makes this verb fresher than existing slide-to-match puzzlers. Without a crisp, distinct verb, marketing has nothing to sell except ambience. (The mechanical consistency question belongs to the systems reviewer. My point is pitch clarity.)
- **Source:** Line 18 (Concept), checked against line 3's requirement.
- **Severity:** MINOR

## Round 2 — Cross-examination

### Conflicts

**With Systems Designer F5 and Player Psychologist F4 (the AI moment rated MAJOR, not BLOCKING).** Four of us (me, Narrative F1, Feasibility F1, QA F1) rated the missing AI moment BLOCKING. Systems and Psychologist rated it MAJOR. I hold BLOCKING. The brief (lines 4-6) does not present the AI moment as one feature among several. It is the document's acceptance criterion and the thing every other section must be designed around "avant de développer quoi que ce soit d'autre". Systems writes that without it "I cannot judge whether an agent would change the loop ... or just decorate it." That is the argument for BLOCKING: until the agent is defined, nobody can decide whether the loop, the scope or the cost model is correct. Psychologist's framing ("the player has no reason to choose this over other match-3 games") is my positioning point from the player's side. A product with no differentiator in the most crowded casual genre is not a MAJOR gap. It means there is no product case.

**With Feasibility Lead F4 (the audiovisual layer is "the appeal", so cutting it "would take the pitch with it").** This conflicts with my F4, which called those bullets "decorative". Feasibility is right that the document *treats* the ASMR feel as the pitch. I still disagree that this makes it load-bearing in a healthy way. It is load-bearing only by default, because the real differentiator (the AI moment) is missing. That is the worst position for a business: the most expensive feature to build is the hardest to show on a store page or in a 30-second capture, and it is the feature most likely to be cut late (Feasibility's own words). Feasibility's reading makes my concern stronger, but the label "decorative" was wrong. See Revisions.

**With Narrative Critic F5 (adopt the *Lose Your Marbles* "losing composure" idiom as the unifying theme).** It is a good tonal idea, but it does not fix the product-shape problem in my F3, and it could hide it. A theme of "calm vs. unravelling" can sit on top of a solo zen journey or a versus pressure game equally well. The board still has to decide single-player or versus, campaign or endless, before it chooses a theme. Taking more from *Lose Your Marbles* also moves the design toward its competitive versus structure, which brings balance work and possibly netcode the document has not scoped. The theme should follow the mode decision.

### Connections

**Player Psychologist F3 plus my F3: there are three games, not two.** I argued that the two inspirations diverge from each other. Psychologist shows that both of them (versus pressure, speed-driven tension) also diverge from the ASMR/lounge promise. Together that makes three products with three audiences: a competitive versus puzzler, a synesthetic premium journey, and a relaxing ASMR casual game. Narrative F4 (calm vs. "surprendre" and breaking glass) is the same split at the level of tone. The document is trying to be several games at once, and this is the clearest evidence in the reviews.

**Adversarial QA F3 and Systems F1 plus my F2: the missing fail state is also a missing business model.** QA shows that with no move cost and no fail state the game can be brute-forced. In F2P match-3, move limits and fail states are the main monetization surface (extra moves, lives, boosters). In premium, the replacement is content volume and a mastery curve. So the fail-state design cannot be chosen until the business model is chosen, and the business model affects which fail state is acceptable to players. This is one decision, and it currently sits across two empty areas of the document.

**Systems F3 plus my F4 and Feasibility F2/F4: the most expensive content buys no gameplay.** Systems notes that the only thing that changes between levels is cosmetic theming. Feasibility notes that themed content cost grows with level count, which is not given. Combined, the document commits to the most expensive kind of per-level content while promising no mechanical variation per level. The budget goes to presentation while progression, the part that retains players, has no budget at all.

**Psychologist F5 and QA F5 plus my F2: the accessibility gap is a platform-dependent cost.** Colour-only matching under heavy glass reflections needs a second cue. How expensive that is, and how strictly reviewers and platform holders will expect it, depends on the platform and audience the document never names. That is another cost that cannot be estimated until F2 is answered.

### Revisions

- **F2 (audience, platform, business model): upgrade MAJOR to BLOCKING.** Feasibility F2 rated the empty Scope BLOCKING because estimation needs a platform. Psychologist F1 cannot design onboarding without knowing the input device. My QA connection above shows the fail state depends on the business model. Three colleagues' findings are blocked by this one omission, so it is upstream of a large part of the review.
- **F4: keep MAJOR, change the framing.** I withdraw "decorative". The more accurate statement is that the audiovisual layer is load-bearing by default because the real differentiator is missing. That makes it the costliest and least marketable part of the pitch, with no priority or cut list (as Feasibility F4 also says).
- **F3: keep MAJOR, broaden it.** Following Psychologist F3, the finding becomes "three implied products" (versus, synesthetic journey, ASMR relaxer), not two.
- **F5: keep MINOR, narrow it.** Systems F2, Psychologist F1 and QA F2 cover the mechanical contradiction properly and at higher severity, so I withdraw that part. What is left is in my lane: Feasibility notes the sliding verb has direct precedent in *Lose Your Marbles* (1997). The verb is therefore not a differentiator, and the pitch cannot rely on it any more than on polish.
