# Meditation phrases (Muscle Trainer, Meditate mode)

Source of truth: `PHRASES` in tools/voice_data.py, built into index.html as `PHR` and into the Kokoro notebook. If you change a line here, the audio clip for it must be made again (the app falls back to the browser voice for any line that has no clip).

## How a session is put together
1. **Intro** (one, names the body part) → pause 1.5 s
2. **Settle** (one breathing cue) → pause 3.5 s
3. For each muscle, deepest layer last:
   - **First** (for muscle 1) or **Next** (for the rest), ending "...the..." → the **muscle name**
   - optional: "Some people call it the [nickname]." and what the muscle does
   - **Hold** (one line)
   - the pause you choose (3–30 s); if it is 10 s or more, every second muscle gets a **Breath** line in the middle of the pause
4. **Close** (names the body part) → 2.5 s
5. **Exit 1** → 4 s → **Exit 2** (one of two) → 1.5 s → **Exit 3**

Rules:
- Within each kind, lines are drawn from a shuffled bag, so none repeats until all of that kind have been used.
- Lines marked **(b)** give a breathing instruction. Two (b) lines are never placed back to back.
- Intro and Close are the only lines that contain the body part (`{g}`, e.g. "the foot", "the hips and glutes"). Each is recorded once per body part.
- First/Next lines end with "the..." so the separate name clip follows naturally ("Next, we come to the... soleus").

## Intro (opening only; contains {g})
- Find a comfortable position, and let your body be still. In this session, we'll move slowly through {g}.
- Welcome. Let your eyes close, or soften your gaze, and let's turn our attention to {g}.
- Settle in, and let the day fall away for a moment. Together, we'll explore {g}, one muscle at a time.
- Take a moment to arrive. Feel the weight of your body, fully supported. When you're ready, we'll begin with {g}.
- Let's begin. There's nothing to do except breathe and notice. Our focus today is {g}.

## Settle (right after the intro; all (b))
- Take a slow breath in through your nose... and let it go.
- Breathe in deeply... and as you breathe out, let your shoulders drop.
- Let your breathing find its own easy rhythm.
- Breathe in for a count of four... and out, slowly, for a count of six.

## First (leads into the first muscle only)
- We'll start with the...
- Let your attention come to rest on the...
- Begin by bringing your awareness to the...

## Next (leads into every later muscle)
- (b) Breathe in, and on the out breath, we will recenter our minds on the...
- Now, gently move your attention to the...
- (b) As you exhale, let your awareness travel to the...
- Next, we come to the...
- Let that area soften, and turn your attention to the...
- (b) Breathing in... and breathing out... we arrive at the...
- Without hurrying, shift your focus to the...
- Now let your awareness settle on the...
- (b) Take one easy breath, and move on to the...
- From here, we drift to the...

## Hold (after the muscle's name)
- Notice any tension here, and let it soften.
- Simply notice how it feels, without needing to change anything.
- Imagine it growing warm and heavy.
- Let it rest.
- Picture where it begins and where it ends, and let the space between relax. *(pairs well with the origin/insertion idea of the app)*
- (b) Breathe into this area, and let it release.
- Let it be loose and easy.
- (b) If you can, feel it gently lengthen as you breathe out.

## Breath (fills long pauses)
- (b) Breathe in... and breathe out.
- (b) Stay with the breath.
- If your mind wanders, that's okay. Gently come back.
- (b) Soft, slow breaths.

## Close (ending; contains {g})
- Now let your awareness widen, to hold all of {g} at once.
- Feel the whole of {g}, calm and at ease.
- That completes our journey through {g}.

## Exit (always in this order)
1. (b) Take one more slow, full breath... and let it go.
2. One of:
   - When you're ready, let your attention return to the room around you.
   - Gently wiggle your fingers and toes, and open your eyes when you're ready.
3. Carry this sense of calm with you.
