# Plan: Expanding Muscle Trainer from 60 to every named muscle

Whiteboard version: https://claude.ai/artifact/B181KndVJN3PkTFhnEznJq
Status (22 Nov 2026, 20:50 AKST): GitHub still has the UI-update build (Phase 6 never uploaded). Phases 6, 7 and 8 (voice) are built and tested in one index.html, sent to the user to upload, plus MuscleTrainer_voice.ipynb (Kokoro notebook for Google Colab) that makes the `audio/` folder. Full Phase 6 audit: claude/phase6-audit.md. Meditation script: claude/meditation-phrases.md. Open items: user runs the notebook and uploads `audio/`; user's decision on splitting muscle families toward the "~320" figure.

## The "~320" figure (important)
Counting each distinct name once (left/right once, and families such as the lumbricals or the dorsal interossei as one entry each), the full list of named skeletal muscles in this game comes to about 227. The ~320 figure only appears if families are split into their members (lumbricals 1–4, dorsal interossei 1–4, palmar interossei 1–3, foot families, erector spinae and transversospinalis regional parts, etc.). Explained to the user; their choice whether to add such splits (the "split" field already supports it).

## Decisions
- ~320/each muscle once, no left/right (user). Keep one index.html. Tricky distractors at Advanced/Expert. Default level Expert; level remembered.
- Bones innermost; peel-away depth layers; every muscle at true size on its own layer (user).
- Every muscle physically reaches each origin and insertion; attachments accurate and testable (user).
- Hand/foot in enlarged insets; iliopsoas split; ovals redrawn; behind-bone painting (user, Phase 3).
- Group names taught at lower levels split into their parts from Advanced up ("split" field): erector spinae, scalenes.
- Muscles hidden behind a solid bone are not painted under it; "under" only where they show between bones or behind see-through structures.
- Phases 3–8 were implemented directly at the user's request.
- UI (user): zoom in/out without the wheel (the wheel is for layers); practice question and picture visible together; stronger flashing.
- Phase 6: official TA names accepted as typed answers; coccygeus shown as "Ischiococcygeus (coccygeus)"; sternocleidomastoid moved to the Middle layer (lies under the platysma).
- Phase 7 (user): nickname shortenings shown in bold inside the full name (e.g. **Lat**issimus dorsi); a Colloquial practice test; a phonetic guide for every name; a Meditation mode.
- Phase 8 (user): browser voice sounded robotic and mispronounced (user was on DuckDuckGo, which uses system voices). Browser voice now reads names spelled as everyday English words; guided meditation script with transition phrases; a Kokoro-recorded audio library (user approved).

## Phase 7 details
- Data (in the page, after the quiz-helper header): NK {id: {n: nickname, h: name HTML with bold root}} for 34 muscles; GN [everyday group name, description, member ids] for 20 groups (Quads, Hamstrings, Calves, Rotator cuff, Glutes, Hip flexors, Adductors, Abs, Obliques, Erectors, Peroneals, Deep six, Pecs, Rhomboids, Suboccipitals, Strap muscles, Scalenes, Intercostals, Kegel muscles, Chewing muscles); PH {latin word: respelling} for 194 words (CAPS = stressed syllable; shown on screen).
- NMH(m) renders the name with the bold root (Learn header, chips, multiple-choice buttons). In the Colloquial test the choices are plain names so the bold doesn't give the answer away.
- Learn card: nickname and group nickname line, "Say it:" respelling, 🔊 hear, ✎ pronunciation tuner.
- Colloquial practice (qt = "nick"): nickname → muscle; muscle (flashing) → nickname; "Which of these is one of the Quads?". Only groups/members present at the current level are used. Typed answers also accept a muscle's own nickname.

## Phase 8 (voice) details
- Source of all voice data: tools/voice_data.py — WORDS {latin word: (everyday-English spelling for browser voices, Kokoro/misaki phonemes)}, NICKW (nickname words), GSAY (spoken group names, e.g. "the hips and glutes"), PHRASES (meditation script; see claude/meditation-phrases.md). Built into the page as SAY, PHR, GSAY.
- Browser voice text: spokenName() ("X (foot)" → "X of the foot."; "X (Y)" → "X, also called Y.") then each Latin word replaced by its SAY spelling (e.g. gastrocnemius → "gas trock knee me us"). With the built-in voice, lead phrase + name are one utterance ("Next, we come to the... soleus") for natural flow. Default speed 0.95.
- Pronunciation tuner (✎ on the Learn card): edit the spelling per word, ▶ to hear, Save (localStorage "mtsay", overrides SAY), "Copy all my fixes" (word = spelling lines) for the user to paste to Claude.
- Recorded library: index.html fetches audio/manifest.json (only over http/https; skipped on file://). If present, the voice menu gets "Recorded calm voice (Kokoro)" (default). Manifest: {clips: {key: [file, start s, duration s]}}. Keys: name:ID, nick:ID, act:ID, and "p:" + exact phrase text (intro/close with the group substituted). Files: g0.mp3 … g19.mp3 (that group's names, nicknames, actions, intros and closings) and phrases.mp3 (everything else). Played with Web Audio (decode whole file, play a slice); any missing clip falls back to the browser voice.
- Meditation engine: async script with pause (current line is said again on resume) and stop; phrases drawn from shuffled bags; no two breathing cues back to back; breath filler in long pauses (>= 10 s) every second muscle.
- Notebook: tools/make_notebook.py builds MuscleTrainer_voice.ipynb from the built index.html (muscles, actions, nicknames) + voice_data + tools/audio_build.py. Kokoro text uses [word](/phonemes/) overrides for Latin words. Default voice af_heart, speed 0.9; options af_bella, af_nicole, bf_emma, am_michael. Output: audio.zip (~7 MB, 685 clips in 22 files) for the user to upload as an `audio` folder next to index.html.
- Tested in the sandbox with tones in place of Kokoro (same builder code): every line played from the library, none fell back; browser-voice path also runs end to end.

## Controls
- Buttons under the picture: Front, Back, ＋ (zoom in), − (zoom out), Whole body (reset).
- Mouse: plain wheel = layers; Ctrl/Cmd + wheel (and trackpad pinch) = zoom at the pointer; left drag = move; quick left click = select / answer; hold left (380 ms) = zoom in at the pointer; hold right = zoom out; quick right click = peel one layer.
- Touch: two-finger pinch zooms; one finger drags/taps.
- Keys (ignored while typing): + / = zoom in, - zoom out, 0 whole body, arrows move, [ and ] change layer.
- Code: toSvg(), zoomAt(f,cx,cy), panBy(), holdZoom(); manual zoom/pan cancels a running tween (twk++).

## Layout
- #wrap > #stage (svg + view/zoom/layer/group rows) and #side (#pq, #spc, #med, #info, #bcard).
- Wide (>= 960 px): two columns, cards sticky on the right; svg height min(100vh - 24px, 920px).
- Narrow: question card directly under the picture; in Practice/Speed the svg is min(52vh, 480px).
- Switching to Practice, Speed run or Meditate scrolls the picture to the top of the window.

## Flashing
.mu.q: gold fill, dark outline, 0.75 s pulse. While a question flashes, svg has class "qz" and other muscles fade to 25%.

## Close-ups (insets)
INBOX (x, y, w, h): hand+foot [221,390,79,250] (hand frame 222,394,76,126; foot 222,524,76,114); eye [4,4,72,56]; ear [4,64,72,40]; tongue & palate [224,4,72,58]; throat [4,394,72,122]; pelvic floor [4,524,72,112]. Points inside a box are drawn once, never mirrored; taps inside are not mirrored; tap tolerance 4.5. Rendering order: skin -> frames -> under-muscles -> body bones -> hand/foot bones -> close-up decor and bones -> other muscles -> close-up titles.

## Muscle-to-bone links
Cream tendon or tapered muscle band from belly to each unreached point; INS_FLESHY / ORI_TENDON override the defaults. 752/752 attachment points touched; every bone point sits on its bone.

## Peel-away layers
1 Surface, 2 Middle, 3 Deep, 4 Deepest; Bones = 5.

## Counts (cumulative levels)
Core 41 | Standard 99 | Advanced 163 | Expert 227 (M has 229 entries; erector and scal hidden from Advanced up).

### Muscle ids by tier
- Core (41): trap lat rhomb erector delt pec serr rect obl glutmax glutmed bic tric brach bra psoas iliac add rf vl vm sart bf st sm gas sol ta scm mass temp front orbi oris zyg vint tabd iobl diaph ql admag
- Standard (+58): supra infra tmin tmaj scal plat spl lev tfl grac pect pirif fib edl pron fcr extd ecrl occip corrug proc nasal lls zygmin dli dao ment buc ris ssc subsc pecmin subcl eic iic rhmin corbr pl fcu fds fdp fpl pq anc ecrb ecu sup apl epl glutmin addbrev plant popl tibpost fdl fhl fibbr ehl
- Advanced (+64): apb adm adp dio edb ahal adm5 fdb epb ei edm fpb op fdmb odm pb lumb pio psmin gems obi gemi qf obe fibt ehb diof qp lumbf fhb addh fdmbf plio lcap lcol dig styh myh genh sth stt thh omoh scapost scamid scaant rcpma rcpmi ocs oci splc sps spi ilco long spinl lao llsan dsn aura aurs aurp ptm ptl pyr ttx
- Expert (+64): lps rsup rmed rlat osup rinf oinf | hmaj hmin trag atrag taur oaur tty stap | hg sg pg uvu gg tvp lvp slong ilong ttr tvt | ipc pca oblary mpc tranary spc styph cthy lca salph palph tharyt voc | eas bsp isc stp dtp eus pbr pcg icg ccg | tpar dsup rca rcl ssce sst rot mult isp itr lcos subco iicm crem artg

## Groups (20), menu order
Face, Eye, Ear, Mouth tongue and palate, Throat, Neck, Shoulder, Chest, Breathing, Back, Abdomen, Upper arm, Forearm, Hand, Pelvis, Pelvic floor, Hip & Glutes, Thigh, Lower leg, Foot.

## Data shape
M entries: id, name, alt, group, tier, view, depth, action, origin, insert, optional inset, under, split, shape. L: id: [name, bone ids, [[x,y],...]]. B: [id, name, views, svg, midline?, deep-view, description]; added bones: hyoid, ossic. Phase 7 adds NK, GN, PH; Phase 8 adds SAY, PHR, GSAY.

## Coordinate notes
Skull x124-176 y8-67; ears (±30.2,44); neck outline now M127.5 66 L172.5 66 ... 181 104 (widened in Phase 6); hyoid (150,85); cervical rects x144-156 (C1 y76.1 ... C7 y99.4-102.6); thoracic rects x144.3-155.7 from y104 (12, h ~6.5); lumbar x141.9-158.1 at y 201.7, 214.9, 228, 241.2, 254.3 (h 11.4); ribs as 10 bezier curves; scapula x171-222 y113-176.

## Phases
1, 1b, 2, 2b, 3, 4, 5, UI update DONE (live). 6, 7, 8 BUILT (one file, awaiting upload; audio library awaiting the user's Colab run). Optional next: family splits toward ~320 (user's call).

## Workflow and tests
Clone the repo at the start of a chat and compare with the newest delivered file. Build scripts live only in the chat sandbox; the delivered index.html is the source of truth. Build chain: p10 (Phase 6) → tools/nick.py (Phases 7–8, imports voice_data.py) → index.html; tools/make_notebook.py index.html notebook [dry-run-audio-dir].
Tests: old muscles' info unchanged unless intended; every muscle at each level (info, dots, layer jump, typed + alt names, 4 distinct origin/insertion choices, questions only from the level); level and layer controls; hit test (>= 35% clickable on its layer); reach test; landmark-on-bone; inside-body-outline test (<= 8% outside, Phase 6); tap quiz (confirm misses directly); group zoom (wait ~1.5 s); zoom controls; practice layout fits picture + choices on 1366x768 and 390x844. Phase 7: Colloquial test 60 questions at each level, all three question kinds, 4 distinct choices, wrong-then-right flow. Phase 8: medtest.py serves the page + dry-run audio over http and runs a full session with the recorded voice and with a stubbed browser voice (order, pause silence, no fallbacks); tuner save/reload.
