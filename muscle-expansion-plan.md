# Plan: Expanding Muscle Trainer from 60 to every named muscle

Whiteboard version: https://claude.ai/artifact/B181KndVJN3PkTFhnEznJq
Status (22 Nov 2026): plan agreed, Phase 1 not started.

Baseline (GitHub, 22 Nov 2026): one index.html, 60 muscles in 14 groups, front/back SVG, bone layer, origin/insertion landmarks (table L), Learn / Practice / Speed run, Beginner / Expert.

## Decisions
- DECIDED: ~320 distinct named muscles, each asked once. No left/right distinction.
- DECIDED (default, user can change): keep everything in one index.html so the upload stays a single file.

## Difficulty tiers (cumulative)
| Tier | Total | What's added |
|---|---|---|
| 1 Core | ~40 | Gym / first-aid big muscles (delts, pecs, lats, quads, hamstrings, glutes, calves, abs, biceps, triceps, traps) |
| 2 Standard | ~100 | Intro A&P list: rotator cuff, hip flexors/adductors, main forearm, main face and neck |
| 3 Advanced | ~200 | Every limb muscle by compartment, hyoid muscles, all facial expression, deep hip rotators, back layers |
| 4 Expert | ~320 | Eye, ear, tongue, palate, pharynx, larynx, intercostals, transversospinalis, suboccipitals, pelvic floor/perineum, every lumbrical and interosseus |

Replaces the Beginner/Expert selector with Core / Standard / Advanced / Expert.

## Groups (14 now -> ~20)
Keep the existing 14. Add: Eye (orbit), Ear, Mouth/Tongue/Palate, Throat (pharynx/larynx), Breathing (diaphragm, intercostals), Pelvic floor & perineum. Split Back into superficial / intermediate / deep; split limbs into compartments (e.g. Forearm: anterior superficial / anterior deep / posterior).

## Data shape change
From arrays to objects: id, name, alt names, group, sub-group, tier, view (f/b), layer (superficial/deep), shape, action, origin landmarks, insertion landmarks, innervation (optional, Expert).

## Drawing: three display types
1. Full shape on the body (big surface muscles).
2. Deep layer: shown when the muscles above are hidden (new Superficial/Deep toggle).
3. Zoom insets for small or internal regions: eye, ear, larynx/pharynx, hand, foot, pelvic floor. Practice zooms into the inset.

## Phases (each phase ends with a working upload)
1. Restructure data, add tier selector, keep the 60 working.
2. Fill to Tier 2 (~100).
3. Limbs to Tier 3 (arm, forearm, hand, thigh, leg, foot).
4. Trunk, neck and face to Tier 3 (back layers, hyoid, face).
5. Tier 4 with insets (eye, ear, throat, tongue, pelvic floor, intercostals).
6. Check: names against Terminologia Anatomica 2, origins/insertions against a standard text, visual check of every view.

## Workflow per phase
Claude drafts the region's muscle list (name, tier, group, origin, insertion, action) for review -> user approves -> Claude draws and codes -> user uploads index.html to github.com/fenw1ck/Muscle_Anatomy_Trainer.
At the start of each new chat: clone the repo to get the live index.html, and update the Status line above when a phase is done.
