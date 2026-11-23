# Plan: Expanding Muscle Trainer from 60 to every named muscle

Whiteboard version: https://claude.ai/artifact/B181KndVJN3PkTFhnEznJq
Status (22 Nov 2026, 22:45 AKST): Phases 6–8 and the Kokoro audio library (`audio/`, 22 files, 685 clips, voice af_heart) are LIVE on GitHub. Built and sent, awaiting upload: index.html with the "Sounds like" box, Phase 9 (ambient sounds + slideshow) and Phase 10 (eye socket redrawn to scale). Full Phase 6 audit: claude/phase6-audit.md. Meditation script: claude/meditation-phrases.md. Open items: user uploads index.html; user listens and reports mispronounced words; pronunciation suggestions arrive as GitHub Issues; optional real nature recordings in audio/ambient/; user's decision on splitting muscle families toward the "~320" figure.

## The "~320" figure (important)
Counting each distinct name once (left/right once, and families such as the lumbricals or the dorsal interossei as one entry each), the full list of named skeletal muscles in this game comes to about 227. The ~320 figure only appears if families are split into their members (lumbricals 1–4, dorsal interossei 1–4, palmar interossei 1–3, foot families, erector spinae and transversospinalis regional parts, etc.). Explained to the user; their choice whether to add such splits (the "split" field already supports it).

## Decisions
- ~320/each muscle once, no left/right (user). Keep one index.html. Tricky distractors at Advanced/Expert. Default level Expert; level remembered.
- Bones innermost; peel-away depth layers; every muscle at true size on its own layer (user).
- Every muscle physically reaches each origin and insertion; attachments accurate and testable (user).
- Hand/foot in enlarged insets; iliopsoas split; ovals redrawn; behind-bone painting (user, Phase 3).
- Group names taught at lower levels split into their parts from Advanced up ("split" field): erector spinae, scalenes.
- Muscles hidden behind a solid bone are not painted under it; "under" only where they show between bones or behind see-through structures.
- Phases 3–10 were implemented directly at the user's request.
- UI (user): zoom in/out without the wheel (the wheel is for layers); practice question and picture visible together; stronger flashing.
- Phase 6: official TA names accepted as typed answers; coccygeus shown as "Ischiococcygeus (coccygeus)"; sternocleidomastoid moved to the Middle layer (lies under the platysma).
- Phase 7 (user): nickname shortenings shown in bold inside the full name (e.g. **Lat**issimus dorsi); a Colloquial practice test; a phonetic guide for every name; a Meditation mode.
- Phase 8 (user): browser voice sounded robotic and mispronounced (user was on DuckDuckGo, which uses system voices such as Microsoft David/Zira/Mark). Browser voice now reads names spelled as everyday English words; guided meditation script with transition phrases; a Kokoro-recorded audio library (made by the user in Google Colab).
- "Sounds like" box (user): visitors type how a name sounds in everyday words, hear it, keep it for their browser voice, or send it as a GitHub Issue. Replaced the per-word ✎ tuner.
- Phase 9 (user): optional nature background sound (creek, breeze, birdsong) and a full-screen slideshow per muscle with a calm scene, the muscle on its bones with O/I, the rest of its group translucent, and the name large at the bottom. No photos or sound files could be downloaded, so scenes are painted SVG illustrations and sounds are synthesized in the page; real loops can be added later.
- Phase 10 (user asked whether the socket should stop at the eye): kept the deep socket (orbit ~45 mm from medial rim to apex, ~1 cm shorter laterally; eyeball ~23 mm) because the recti start at the apex; fixed the front instead (no bone bar across the front, eye moved forward so the cornea is ~14 mm in front of the lateral rim, normal exophthalmometry 12–21 mm), rectus insertions moved in front of the equator, obliques insert behind it.

## Phase 10 details (tools/eyefix.py, applied after slides.py)
Eye inset (from above, front = down, ~1 unit/mm): apex bar M31 9 L37 9; medial wall M24 47 L31 9; lateral wall M58 39 L37 9; lower rim/floor edge drawn faint and dashed M24 47 Q41 47 58 39; globe c(40,42.6) r10; cornea dot (40,51.2) r2; trochlea (25.6,43) r1.4; upper eyelid M31 53.4 Q40 56 49 53.4…; title y 59. Landmarks: eyelidup (40,55.3), globsup (40,44.6), globmed (30.6,46), globlat (49.6,44.8), globinf (40,45.3), globsupost (45,40), globinfpost (47.5,41.8), orbfloor (27.4,46.9); annulus (34,14), lesswing (34.5,9.5), sphbody (31.6,9.4) unchanged. Muscle shapes regenerated as capsules from origin to insertion (osup ends at the trochlea; its tendon to globsupost is drawn by the link code).

## Phase 7 details
- Data (in the page, after the quiz-helper header): NK {id: {n: nickname, h: name HTML with bold root}} for 34 muscles; GN [everyday group name, description, member ids] for 20 groups (Quads, Hamstrings, Calves, Rotator cuff, Glutes, Hip flexors, Adductors, Abs, Obliques, Erectors, Peroneals, Deep six, Pecs, Rhomboids, Suboccipitals, Strap muscles, Scalenes, Intercostals, Kegel muscles, Chewing muscles); PH {latin word: respelling} for 194 words (CAPS = stressed syllable; shown on screen).
- NMH(m) renders the name with the bold root (Learn header, chips, multiple-choice buttons, slideshow). In the Colloquial test the choices are plain names so the bold doesn't give the answer away.
- Learn card: nickname and group nickname line, "Say it:" respelling, 🔊 hear, and at the bottom the "Sounds like" box.
- Colloquial practice (qt = "nick"): nickname → muscle; muscle (flashing) → nickname; "Which of these is one of the Quads?". Only groups/members present at the current level are used. Typed answers also accept a muscle's own nickname.

## Phase 8 (voice) details
- Source of all voice data: tools/voice_data.py — WORDS {latin word: (everyday-English spelling for browser voices, Kokoro/misaki phonemes)}, NICKW (nickname words), GSAY (spoken group names, e.g. "the hips and glutes"), PHRASES (meditation script; see claude/meditation-phrases.md). Built into the page as SAY, PHR, GSAY.
- Browser voice text: spokenName() ("X (foot)" → "X of the foot."; "X (Y)" → "X, also called Y.") then each Latin word replaced by its SAY spelling (e.g. gastrocnemius → "gas trock knee me us"). A visitor's own "Sounds like" spelling (localStorage "mtsayn", per muscle id) wins; older per-word fixes ("mtsay") still apply. With the built-in voice, lead phrase + name are one utterance. Default speed 0.95.
- "Sounds like" box: placeholder example = sayExample(m); ▶ speaks typed text (or the example) with the browser voice; "Use for my voice" saves; "Suggest it" opens github.com/fenw1ck/Muscle_Anatomy_Trainer/issues/new with title "Pronunciation: <name>" and the spelling in the body (needs a GitHub account; Issues must be enabled on the repo — default for public repos; not verified from the sandbox).
- Recorded library: index.html fetches audio/manifest.json (only over http/https; skipped on file://). If present, the voice menu gets "Recorded calm voice (Kokoro)" (default). Manifest: {clips: {key: [file, start s, duration s]}}. Keys: name:ID, nick:ID, act:ID, and "p:" + exact phrase text (intro/close with the group substituted). Files: g0.mp3 … g19.mp3 and phrases.mp3. Played with Web Audio; any missing clip falls back to the browser voice. Changing any phrase text or the muscle list means re-running the notebook.
- Meditation engine: async script with pause (current line is said again on resume) and stop; phrases from shuffled bags; no two breathing cues back to back; breath filler in long pauses (>= 10 s) every second muscle.
- Notebook: tools/make_notebook.py builds MuscleTrainer_voice.ipynb from the built index.html + voice_data + tools/audio_build.py. Colab's default Python is too new for Kokoro (kokoro 0.9.x requires Python < 3.13), so step 1 makes a Python 3.12 environment with uv (/content/kenv); steps 2 and 5 run /content/kgen.py in it. Default voice af_heart, speed 0.9.

## Phase 9 details (tools/slides.py, applied after nick.py)
- Ambient: Meditate panel "Background" (Silence / Creek / Breeze in the trees / Birdsong), level slider, "Try" (7 s preview); settings saved in mtmed (amb, av, sl). Starts with the session (1.5 s fade in), fades out at the end or on Stop; switching mid-session works. Synthesized with Web Audio (noise loops + moving band-pass filters for the creek; low-passed brown noise with random gusts + leaf rustle for breeze; breeze + random FM chirps/trills, randomly panned, for birdsong). If audio/ambient/<creek|breeze|birds>.mp3 exists on the site it is looped instead (the probe shows a harmless 404 in the console when absent).
- Slideshow: checkbox "Slideshow: full screen, a calm scene for each muscle". The main #s svg is moved into #slpanel (glass panel) for the session and put back on close; class "slv" hides other groups (.mu.dim), shows the rest of the group at 30% fill, hides labels, inset frames and pointer lines; drawAtt() shows O/I. Six painted SVG scenes (Misty mountains at dawn, Forest lake, Cairn by the sea, Lanterns at dusk, Zen garden, Northern lights), randomly drawn per muscle with a 2.5 s cross-fade; gentle drift/float/twinkle animations (off with prefers-reduced-motion). Name in large serif at the bottom with the bold nickname root, respelling, nickname and "i of n" beneath. Controls top right: full screen, Pause, Close; Space pauses, Esc closes. End slide "All of <group>" with a Close button.

## Controls
- Buttons under the picture: Front, Back, ＋ (zoom in), − (zoom out), Whole body (reset).
- Mouse: plain wheel = layers; Ctrl/Cmd + wheel (and trackpad pinch) = zoom at the pointer; left drag = move; quick left click = select / answer; hold left (380 ms) = zoom in at the pointer; hold right = zoom out; quick right click = peel one layer.
- Touch: two-finger pinch zooms; one finger drags/taps.
- Keys (ignored while typing): + / = zoom in, - zoom out, 0 whole body, arrows move, [ and ] change layer.
- Code: toSvg(), zoomAt(f,cx,cy), panBy(), holdZoom(); manual zoom/pan cancels a running tween (twk++).

## Layout
- #wrap > #stage (svg + view/zoom/layer/group rows) and #side (#pq, #spc, #med, #info, #bcard). #slide overlay is a body-level fixed element.
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
M entries: id, name, alt, group, tier, view, depth, action, origin, insert, optional inset, under, split, shape. L: id: [name, bone ids, [[x,y],...]]. B: [id, name, views, svg, midline?, deep-view, description]; added bones: hyoid, ossic. Phase 7 adds NK, GN, PH; Phase 8 adds SAY, PHR, GSAY; Phase 9 adds SCENES.

## Coordinate notes
Skull x124-176 y8-67; ears (±30.2,44); neck outline now M127.5 66 L172.5 66 ... 181 104 (widened in Phase 6); hyoid (150,85); cervical rects x144-156 (C1 y76.1 ... C7 y99.4-102.6); thoracic rects x144.3-155.7 from y104 (12, h ~6.5); lumbar x141.9-158.1 at y 201.7, 214.9, 228, 241.2, 254.3 (h 11.4); ribs as 10 bezier curves; scapula x171-222 y113-176. Eye inset: see Phase 10.

## Phases
1, 1b, 2, 2b, 3, 4, 5, UI update, 6, 7, 8 DONE (live, including the audio library). "Sounds like" box, 9 and 10 BUILT (awaiting upload). Optional next: pronunciation fixes from listening/suggestions; real ambient recordings; family splits toward ~320 (user's call).

## Workflow and tests
Clone the repo at the start of a chat and compare with the newest delivered file. Build scripts live only in the chat sandbox; the delivered index.html is the source of truth. Build chain: p10 (Phase 6) → tools/nick.py (Phases 7–8 + Sounds like box, imports voice_data.py) → p11 → tools/slides.py (Phase 9) → p12 → tools/eyefix.py (Phase 10) → p13 = index.html; tools/make_notebook.py index.html notebook [dry-run-audio-dir].
Tests: old muscles' info unchanged unless intended; every muscle at each level (info, dots, layer jump, typed + alt names, 4 distinct origin/insertion choices, questions only from the level); level and layer controls; hit test (>= 35% clickable on its layer); reach test; landmark-on-bone; inside-body-outline test (<= 8% outside, Phase 6); tap quiz (confirm misses directly); group zoom (wait ~1.5 s); zoom controls; practice layout fits picture + choices on 1366x768 and 390x844. Phase 7: Colloquial test 60 questions at each level, all three question kinds, 4 distinct choices, wrong-then-right flow. Phase 8: medtest.py serves the page + audio over http and runs a full session with the recorded voice and with a stubbed browser voice (order, pause silence, no fallbacks); audio library check (every manifest clip inside its file, RMS > 0.01); Sounds like box (example placeholder, save, issue URL). Phase 9: slidetest.py (slideshow opens with svg moved into the panel, steps per muscle, ambient kinds start and switch, Close restores svg and stops sound; screenshots of all six scenes). Phase 10: reach, onbone, hit and inside tests all pass after the eye redraw.
