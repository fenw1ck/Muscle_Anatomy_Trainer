# Plan: Expanding Muscle Trainer from 60 to every named muscle

Whiteboard version: https://claude.ai/artifact/B181KndVJN3PkTFhnEznJq
Status (22 Nov 2026): Phase 2 built and tested (106 muscles); sent to the user to upload. Peel-away layers are live on GitHub. Next: Phase 3 (limbs to Advanced).

Baseline (GitHub, 22 Nov 2026): one index.html, 60 muscles in 14 groups, front/back SVG, bone layer, origin/insertion landmarks (table L), Learn / Practice / Speed run, Beginner / Expert.

## Decisions
- DECIDED: ~320 distinct named muscles, each asked once. No left/right distinction.
- DECIDED (default, user can change): keep everything in one index.html so the upload stays a single file.
- DECIDED in Phase 1: tricky wrong answers in Practice (same-group / similar-name distractors) switch on at Advanced and Expert; Core and Standard get easier ones. Default level for a new visitor is Expert (all muscles); the chosen level is remembered in the browser.
- DECIDED (user, 22 Nov): no out-of-proportion "deep muscles drawn on top". Bones are the innermost layer; muscles sit in depth layers the user peels away. Draw every muscle at true size on its own layer.
- Intercostals moved from Expert to Standard (approved with the Phase 2 list).
- Phase 2: layers below the current one are dimmed to 50% so the current layer stands out (deeper muscles still show through gaps and can be clicked).

## Peel-away layers (live since 22 Nov)
- Each muscle has depth: 1 Surface, 2 Middle, 3 Deep, 4 Deepest. Bones lie under layer 4 ("Bones" = depth 5).
- Mouse: wheel down or right-click peels one layer; wheel up restores; right-click at Bones wraps back to Surface. Left-click selects. Touch: Layer buttons (Surface / Middle / Deep / Deepest / Bones).
- A peeled layer fades to 8%, gets dashed outlines, cannot be clicked, and slides outward (right half up-right, mirrored left half up-left). Layers below the current one are at 50%.
- Selecting a muscle, or a Practice / Speed-run question, jumps to that muscle's layer.
- Code: muscles render in <g class="dl" data-d="N"> per view, deepest first, each with a right-half <g class="side"> and a mirrored copy. setDepth(n), DN = layer names. Within a layer, later entries in M draw on top.
- Known limitation: bones always draw under all muscles, so inside-the-ribcage muscles (diaphragm) draw over the ribs. "Muscles + bones" makes muscles 60% opaque so ribs show through.

## Difficulty tiers (cumulative) — actual counts after Phase 2
| Tier | Now | Target | What's added |
|---|---|---|---|
| 1 Core | 40 | ~40 | Gym / first-aid big muscles |
| 2 Standard | 98 | ~100 | Intro A&P list: rotator cuff, hip flexors/adductors, main forearm, main face and neck, intercostals |
| 3 Advanced | 106 | ~200 | Every limb muscle by compartment, hyoid muscles, all facial expression, deep hip rotators, back layers |
| 4 Expert | 106 | ~320 | Eye, ear, tongue, palate, pharynx, larynx, transversospinalis, suboccipitals, pelvic floor/perineum, every lumbrical and interosseus |

### Muscle ids by tier
- Core (40): trap lat rhomb erector delt pec serr rect obl glutmax glutmed bic tric brach bra ili add rf vl vm sart bf st sm gas sol ta scm mass temp front orbi oris zyg | Phase 2: vint tabd iobl diaph ql admag
- Standard (+58 = 98): supra infra tmin tmaj scal plat spl lev tfl grac pect pirif fib edl pron fcr extd ecrl | Phase 2: occip corrug proc nasal lls zygmin dli dao ment buc ris ssc subsc pecmin subcl eic iic rhmin corbr pl fcu fds fdp fpl pq anc ecrb ecu sup apl epl glutmin addbrev plant popl tibpost fdl fhl fibbr ehl
- Advanced (+8 = 106): apb adm adp dio edb ahal adm5 fdb
- Expert: none yet

### Depths
- Middle (2): spl lev scal supra infra tmin tmaj rhomb glutmed ili sm sol bra adp corrug ment buc pecmin subcl rhmin iobl corbr fds sup apl epl vint addbrev plant fibbr ehl
- Deep (3): erector pirif ssc subsc eic iic tabd fdp fpl pq glutmin admag popl tibpost fdl fhl
- Deepest (4): diaph ql
- Everything else Surface (1).

### Phase 2 redraws of old shapes (closer to true proportion)
zyg (was pointing the wrong way), bra, pron, fcr, brach, extd, sol, pect (was fully hidden under add), lls (to uncover nasalis).

## Groups (15 now -> ~20)
Original 14 + Breathing (index 14: diaphragm, external & internal intercostals). Still to add: Eye (orbit), Ear, Mouth/Tongue/Palate, Throat (pharynx/larynx), Pelvic floor & perineum. Split Back into superficial / intermediate / deep; split limbs into compartments (e.g. Forearm: anterior superficial / anterior deep / posterior). Breathing currently lists last in the group menu.

## Data shape
Each muscle in M is one object: id, name, alt (optional other names accepted when typing), group, tier (1-4), view (f/b/fb), depth (1-4), action, origin and insert (landmark ids from L), shape ("p <path>" or "e cx cy rx ry rot", right half; x >= 150). Still to add when needed: sub-group, innervation (Expert).
Landmark L: id: [name, bone ids, [[x,y],...] on the right half]. 81 landmarks added in Phase 2.
Code helpers: P = muscles at the chosen level, VG = groups that have any at that level, applyLevel(), setLevel(n).

## Coordinate notes (300 x 640 viewBox, right half drawn, mirrored)
Skull x124-176 y8-67 (eye sockets 139/161,39; nose 145-155 y45-55; mandible y58-78). Clavicle y103-112. Ribs: 10 curves from (154.8, 109.8 + 8.3k) to ends around x188-210. Sternum x143-157 y107-172. Scapula (right) x171-222 y113-176. Humerus head (230,122); epicondyles medial (237,230) lateral (256,230). Ulna x239-249, radius x252-268, y231-321. Carpals y320-339, metacarpals 335-362, fingers to ~381, thumb tip ~(272,367). Iliac crest ~y268; ASIS (204,282); femoral head (196,307); femur to condyles y440-465; patella (189,455); tibia x180-200 y463-596; fibula x200-209; foot y593-621.

## Phases (each phase ends with a working upload)
1. DONE (22 Nov 2026): restructure data, add level selector, keep the 60 working.
1b. DONE (22 Nov 2026): peel-away depth layers.
2. BUILT (22 Nov 2026): +46 muscles (Core 40, Standard 98, total 106) on layers; Breathing group.
3. Limbs to Advanced: remaining arm, forearm (EPB, EDM, EI...), hand (thenar, hypothenar, lumbricals?), thigh, deep hip rotators, leg (fibularis tertius...), foot.
4. Trunk, neck and face to Advanced (back layers incl. erector parts, hyoid muscles, remaining face, pterygoids).
5. Expert with insets (eye, ear, throat, tongue, pelvic floor).
6. Check: names against Terminologia Anatomica 2, origins/insertions against a standard text, visual check of every view.

## Workflow per phase
Claude drafts the region's muscle list (name, tier, group, depth, origin, insertion, action) for review -> user approves -> Claude draws and codes -> user uploads index.html to github.com/fenw1ck/Muscle_Anatomy_Trainer.
At the start of each new chat: clone the repo to get the live index.html, and update the Status line above when a phase is done.
Testing each phase: headless Chrome (Playwright) — every old muscle's info card compared with the live version; every muscle checked for info, attachment dots, layer jump, typed name and alt names, 4 distinct origin/insertion choices; levels checked for leaks; layer controls; a hit test that each muscle has at least 35% of its area clickable on its own layer (catches muscles hidden under neighbours).
