# Plan: Expanding Muscle Trainer from 60 to every named muscle

Whiteboard version: https://claude.ai/artifact/B181KndVJN3PkTFhnEznJq
Status (22 Nov 2026): Phase 1 live on GitHub. Peel-away layers built and sent to the user to try (not yet uploaded). Phase 2 muscle list (46 muscles) drafted, waiting for the user's approval. Next: draw Phase 2 at true proportion on the correct layers.

Baseline (GitHub, 22 Nov 2026): one index.html, 60 muscles in 14 groups, front/back SVG, bone layer, origin/insertion landmarks (table L), Learn / Practice / Speed run, Beginner / Expert.

## Decisions
- DECIDED: ~320 distinct named muscles, each asked once. No left/right distinction.
- DECIDED (default, user can change): keep everything in one index.html so the upload stays a single file.
- DECIDED in Phase 1: tricky wrong answers in Practice (same-group / similar-name distractors) switch on at Advanced and Expert; Core and Standard get easier ones. Default level for a new visitor is Expert (all muscles); the chosen level is remembered in the browser.
- DECIDED (user, 22 Nov): no out-of-proportion "deep muscles drawn on top". Bones are the innermost layer; muscles sit in depth layers the user peels away. Draw every muscle at true size on its own layer.
- Intercostals moved from Expert to Standard (proposed in the Phase 2 draft).

## Peel-away layers (built 22 Nov, replaces the old "Superficial/Deep toggle" idea)
- Each muscle has depth: 1 Surface, 2 Middle, 3 Deep, 4 Deepest. Bones lie under layer 4 ("Bones" = depth 5).
- Mouse: wheel down or right-click peels one layer; wheel up restores; right-click at Bones wraps back to Surface. Left-click selects. Touch: Layer buttons (Surface / Middle / Deep / Deepest / Bones).
- A peeled layer fades to 8%, gets dashed outlines, cannot be clicked, and slides outward (right half up-right, mirrored left half up-left).
- Selecting a muscle, or a Practice / Speed-run question, jumps to that muscle's layer.
- "Bones only" button removed (peeling to Bones replaces it). "Muscles" / "Muscles + bones" still toggles bone visibility.
- Code: muscles render in <g class="dl" data-d="N"> per view, deepest first, each with a right-half <g class="side"> and a mirrored copy. setDepth(n), DN = layer names.
- Depths given to the original 60: Middle (2) = spl lev scal supra infra tmin tmaj rhomb glutmed ili sm sol bra adp; Deep (3) = erector pirif; all others Surface (1). Existing shapes are still the old schematic sizes; redraw at true proportion as each region is worked on.

## Difficulty tiers (cumulative)
| Tier | Target total | What's added |
|---|---|---|
| 1 Core | ~40 | Gym / first-aid big muscles (delts, pecs, lats, quads, hamstrings, glutes, calves, abs, biceps, triceps, traps) |
| 2 Standard | ~100 | Intro A&P list: rotator cuff, hip flexors/adductors, main forearm, main face and neck, intercostals |
| 3 Advanced | ~200 | Every limb muscle by compartment, hyoid muscles, all facial expression, deep hip rotators, back layers |
| 4 Expert | ~320 | Eye, ear, tongue, palate, pharynx, larynx, transversospinalis, suboccipitals, pelvic floor/perineum, every lumbrical and interosseus |

### Tiers given to the original 60 (Phase 1)
- Core (34): trap lat rhomb erector delt pec serr rect obl glutmax glutmed bic tric brach bra ili add rf vl vm sart bf st sm gas sol ta scm mass temp front orbi oris zyg
- Standard (+18 = 52): supra infra tmin tmaj scal plat spl lev tfl grac pect pirif fib edl pron fcr extd ecrl
- Advanced (+8 = 60): apb adm adp dio edb ahal adm5 fdb
- Expert: none yet

### Phase 2 draft (46 muscles, awaiting approval) -> Core 40, Standard 106
- Core +6: vastus intermedius, transversus abdominis, internal oblique, diaphragm (new Breathing group), quadratus lumborum, adductor magnus
- Standard +40: occipitalis, buccinator, zygomaticus minor, levator labii superioris, depressor anguli oris, depressor labii inferioris, mentalis, nasalis, corrugator supercilii, procerus, risorius, semispinalis capitis, subscapularis, pectoralis minor, subclavius, external intercostals, internal intercostals, rhomboid minor, coracobrachialis, anconeus, palmaris longus, flexor carpi ulnaris, flexor digitorum superficialis, flexor digitorum profundus, flexor pollicis longus, pronator quadratus, supinator, extensor carpi radialis brevis, extensor carpi ulnaris, abductor pollicis longus, extensor pollicis longus, gluteus minimus, adductor brevis, tibialis posterior, fibularis brevis, flexor digitorum longus, flexor hallucis longus, extensor hallucis longus, popliteus, plantaris
(Full origin/insertion/action table was given in chat on 22 Nov.)

## Groups (14 now -> ~20)
Keep the existing 14. Add: Eye (orbit), Ear, Mouth/Tongue/Palate, Throat (pharynx/larynx), Breathing (diaphragm, intercostals; created in Phase 2), Pelvic floor & perineum. Split Back into superficial / intermediate / deep; split limbs into compartments (e.g. Forearm: anterior superficial / anterior deep / posterior).

## Data shape
Each muscle in M is one object: id, name, alt (optional other names accepted when typing), group, tier (1-4), view (f/b/fb), depth (1-4), action, origin and insert (landmark ids from L), shape. Still to add when needed: sub-group, innervation (Expert).
Code helpers: P = muscles at the chosen level, VG = groups that have any at that level, applyLevel(), setLevel(n). Muscles above the level get class "lvx" (hidden).

## Drawing
1. Every muscle at true proportion on its depth layer (peel-away, above).
2. Zoom insets for small or internal regions: eye, ear, larynx/pharynx, hand, foot, pelvic floor. Practice zooms into the inset.

## Phases (each phase ends with a working upload)
1. DONE (22 Nov 2026): restructure data, add level selector, keep the 60 working.
1b. BUILT (22 Nov 2026): peel-away depth layers.
2. Fill to Tier 2 (~106), drawn at true proportion on layers.
3. Limbs to Tier 3 (arm, forearm, hand, thigh, leg, foot).
4. Trunk, neck and face to Tier 3 (back layers, hyoid, face).
5. Tier 4 with insets (eye, ear, throat, tongue, pelvic floor).
6. Check: names against Terminologia Anatomica 2, origins/insertions against a standard text, visual check of every view.

## Workflow per phase
Claude drafts the region's muscle list (name, tier, group, depth, origin, insertion, action) for review -> user approves -> Claude draws and codes -> user uploads index.html to github.com/fenw1ck/Muscle_Anatomy_Trainer.
At the start of each new chat: clone the repo to get the live index.html, and update the Status line above when a phase is done.
Testing used each phase: headless Chrome (Playwright) — every muscle and bone info card compared against the previous live version, plus Practice (all 3 question types), Speed run, each level checked for muscles leaking in from higher levels, and each layer checked so a click hits the top unpeeled muscle.
