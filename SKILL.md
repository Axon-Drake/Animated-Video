---
name: cartoon-3d-short
description: Make a ~5-7 minute horizontal (16:9, 1920x1080) cartoon story video in the "2D characters in a colourful 3D world" style (flat bean-head cartoon cards with ink outlines, painted 3D town/world with black ink lines, saturated sky, depth-of-field backgrounds, gliding cameras, voice narration, synthesized SFX + march-style music), rendered with three.js inside Remotion. Proven on "The Bridge That Fell for a Song" (2026-10-31), which was the vertical short version of this style. Use when the user asks for another video "like the bridge video", "like the soldiers/bridge short", "in that cartoon 3D style", a long-form / horizontal / YouTube-style history or science story video with cartoon characters in 3D, or says "use the cartoon short skill".
---

# Cartoon 3D story video (three.js + Remotion)

**Format: horizontal 16:9, 1920x1080 at 30 fps, 5-7 minutes (9,000-12,600 frames).**
This is a long-form horizontal video, not a vertical short. The look, the pipeline and the user rules below are unchanged from the bridge short; only the frame shape, the length and the things that follow from them (script size, shot count, pacing, render time) are different.

Proven example: **The Bridge That Fell for a Song**. It is an original 67 s vertical short (1080x1920) about the 1831 Broughton bridge and 74 soldiers marching in step. It is the source of the look, the code and the template.
- Code: `src/videos/bridge/` of your Remotion project (composition `BridgeShort`, built as 1080x1920, 30 fps; it must be converted to 1920x1080, see "Format: 16:9 horizontal" below).
- Script, voice and sound: `~/bridge-short/`.
- Style research: `references/`.

The style reference is a channel studied privately (`references/`: STYLE_BIBLE.md, reports/*.md, frames/).
- Those videos are private study material. Never upload, publish or send them.
- Never copy their scripts, characters or shots. Story, characters and voice are always OUR OWN.
- Never say the channel's name in a voice-over or description.

User rules that apply:
- No captions or labels in the video.
- English only.
- Voice: one steady tone (for the whole 5-7 minutes, every act, same voice and same settings).
- Jokes are researched real facts.
- Standard mix: music +3 dB, SFX -6 dB, voice unchanged.
- Final export 2K horizontal: 2560x1440, `--scale 1.3333`.
- Opus max verifies every check.
- Save after every step.
- "Just do it" once the script is approved.

## Format: 16:9 horizontal (what changes from the vertical bridge template)
`templates/bridge/` is still the vertical 1080x1920 source. When you copy it to `src/videos/<name>/`, convert it to horizontal once, and if you like save the converted copy as `templates/bridge-wide/` so later videos start from it.
- **Composition:** register it in `src/Root.tsx` with `width={1920} height={1080} fps={30}` and `durationInFrames` = last word of `words.json` + tail (9,000-12,600). Name it for the new video (e.g. `<Name>Wide`), not `BridgeShort`.
- **Pixel scale:** the bridge look values are defined at a frame whose long side is 1920 px (1080x1920 vertical). In 16:9 the long side is the width, so every pixel parameter (ink widths, lens cInf, bloom radius, grain, shadow blur) scales with **output width / 1920** instead of height / 1920. At 1920x1080 the scale is 1.0, so every "px" number in this file stays numerically the same. At 2K (2560x1440) the scale is 1.3333. Search `ink.ts`, `lensblur.ts`, `grade.ts`, `Bridge.tsx` and `world.ts` for `1920`, `1080`, `height` and `aspect` and fix any 9:16 assumption.
- **Camera:** keep the same vertical fov values; three.js `fov` is vertical, so the horizontal view automatically becomes wider (16:9 instead of 9:16). Re-check every `CAMS` key, `FOREGROUND` prop and crowd placement after the switch: a framing that was tall and narrow now shows far more to the left and right.
- **Composition:** use the width. Put the subject on the left or right third with the other side showing the setting; two- and three-shots of characters fit side by side now; crowds spread in a line or an arc instead of stacking in depth. Avoid a dead-centre subject in every shot.
- **Cards:** the 600x1000 character design canvas and the head-height rules do not change. Characters look smaller in the frame, so the CU/MS shots do the emotional work and the wides show the world.
- **Sky, fog and sun:** the sky shader, fog distances and camera-relative sun are unchanged; just verify nothing clips at the wider horizontal fov (sun disc, horizon seam, river edges, culled houses at the sides).
- **Stills and the final:** `bridge-stills.mjs` and the render command read the composition size, so nothing else is needed.

## Pipeline
1. **Topic and facts.** Pick one surprising true story with a physical cause and effect (it should be visual), plus a simple everyday analogy that explains it.
   - A 5-7 minute video needs more material than a short: choose a story that has at least 3-5 distinct chapters (a setup, several escalating events or locations, a failure/climax, an investigation or explanation, a consequence). Add 1-2 real side facts or related cases for the middle, so the story never repeats itself.
   - Write the facts with their source at the top of `SCRIPT.md`.
2. **Script** (~3.7 words/s, 300-420 s, so roughly 1,100-1,550 words; aim for about 6 minutes, ~1,300 words). The short's beat formula becomes the shape of the opening and the ending, with the middle split into acts:
   - **Hook:** the absurd fact in one line ("Seventy-four soldiers knocked down an iron bridge just by walking across it.").
   - **Setting:** year, place, pride. Take 30-45 s: who the people are, what the place looks like, why it matters.
   - **Acts (3-5, each ~45-80 s):** each act has its own mini-escalation over three or four beats, each a little bigger than the last, and ends on a small hook or reversal that pulls into the next act. Give every act a new place, time of day, light (day / golden) or character, so the picture keeps changing.
   - **Snap:** the moment it breaks (the climax, around 60-70 % through).
   - **Deadpan double line:** "Luckily ... / Unluckily ...". Repeat the device once or twice in the video at most, with fresh content each time.
   - **Explanation:** one physics idea through an everyday analogy (the kid on a swing). In long form this gets a full chapter: set up the analogy, show it, then map it back onto the real event.
   - **Investigation / aftermath:** what people did next, who was blamed, what was learned. Real facts only.
   - **Consequence:** the rule it created.
   - **Callback twist:** the ending, calling back to the hook.
   - Never describe on screen what the viewer already sees.
   - Pacing for retention: something new (a new location, character, prop or reveal) at least every ~20-30 s, and a punchline or surprise every ~45 s.
3. **Voice.** Record or generate `voice.wav`, then make its word timings `words.json` (any speech-to-text with word times).
   - For 1,100+ words record or generate in act-sized chunks (same voice, same settings), then join them with consistent gaps. The tone must stay steady from the first word to the last.
   - Put them in the project folder, e.g. `~/bridge-short/voice/`.
   - Hand-fix the word timings. Everything is timed to words.json.
4. **Shot plan** in `story.ts`: copy `templates/bridge/` (the final bridge source, same as `src/videos/bridge/` of your Remotion project) to `src/videos/<name>/`, convert it to 16:9 (see above), register a new composition next to `BridgeShort` in `src/Root.tsx`, and rename it.
   - `SHOTS`: id, a/b frames, light day/golden, crowd mode, `tau` story time. A 6-minute video is roughly 80-140 shots; group them by act (e.g. one array or section per act, spread together at the end) so the file stays readable.
   - `XFADE`: crossfades at place/time changes (act boundaries are the natural place).
   - `FLASH_AT`: a white flash after the hook.
   - `CAMS`: per-shot keys `{f, dur?, cut?, s: f => {pos, look, fov, roll}}`.
   - `KICKS`: impact flip-shakes.
   - `DOF`/`dofAt`: lens blur per beat.
   - `FOREGROUND`: near framing props.
   - Every visual reaction lands 0-3 frames before its word.
   - Vary the shot size and the camera move from act to act so a long video does not feel like one repeated shot.
5. **World.**
   - `lights.ts`: LIGHTS plus the LOOK sky/river/cloud palette per light.
   - `shade.ts`: two-tone shading and `untone()`.
   - `build.ts`: town, houses and Kenney CC0 kit props, recoloured, with sight-line culling of anything blocking a camera. A 5-7 minute story usually needs more than one location: build each as its own set and cull per shot.
   - `paint.ts`: HD Canvas2D painted textures with seams painted in as dark tints.
   - `world.ts`: sky shader, fog = horizon colour, river, per-frame posing, sun frozen per shot and camera-relative.
6. **Characters.**
   - `toon.ts`: the Canvas2D `Pen` drawing API, 600x1000 design canvas, soles at y = 975, bean heads, stick limbs, plum variable-width outline, face library.
   - `cast.ts`: camera-facing billboards, crowds, effect cards, blinks, walk cycles. All billboards go on the `NOINK` layer.
   - `cardshade.ts`: cards lit by the scene light, no emissive.
   - A longer story has a bigger cast: give each recurring character one clear silhouette (hat, colour, height) so viewers can tell them apart in a wide shot.
7. **Post** (`Bridge.tsx` + `ink.ts` + `lensblur.ts` + `grade.ts`).
   - Composer order: ink → lens pre → lens main → bloom → NEUTRAL tone map → grade (MSAA 4).
   - Every pixel parameter scales with output width / 1920 (long side; see "Format: 16:9 horizontal").
8. **Sound** (`~/bridge-short/audio/`: timeline.py, sfx_gen.py, sfx.py, music.py, instruments.py, dsp.py, mix.py).
   - Event table → synthesized SFX plus an original score ducked under the voice.
   - The score must last 5-7 minutes without sounding like a loop: write it in sections that follow the acts (quiet setting, building march, tense snap, calm explanation, resolution), with variations in instrumentation and key, and let it drop out under dialogue-heavy beats.
   - Output: mix.wav (48 kHz, exact length), stems and cues.md.
   - Lock the picture's time constants before the sound is made. Any later timing change means re-running the sound.
9. **Look checks.** Run `node scripts/bridge-stills.mjs out/<name>/x_ 100,400 0.5`.
   - Pick stills from every act and both lights, not only the opening: at least 2-3 frames per act.
   - Set `BRIDGE_ENTRY=src/sandboxes/<n>/index.tsx` to render a sandbox copy.
   - Make side-by-sides against reference frames with `python scripts/sbs_sheet.py out.jpg --ours ... --ref ...`.
   - Send them to an Opus max verifier: 1-3 images, a ≤200-word verdict.
10. **Final render.**
    - `npx remotion render src/index.ts <Comp> out/<name>/video.mp4 --codec h264 --crf 16 --scale 1.3333` (output 2560x1440).
    - 2K renders run ~4 fps, so a 6-minute video (~10,800 frames) takes about 45-60 minutes: render with the TEMP folder set on the big drive (see Gotchas), expect it to run unattended, and do not start another heavy render at the same time.
    - Mux with `-map 0:v:0 -map 1:a:0` (Remotion writes a silent audio track).
    - QA: `python scripts/short_qa.py final.mp4 out/<name>/qa shots.json` checks every frame for black, white, frozen and pop frames, plus loudness and sheets. It is slower on a 6-minute file; give it the full shot list and review the contact sheets act by act.
    - Fix anything it finds, then run a deep Opus audit.
11. **Deliver.** SendUserFile the final mp4, update memory, write notes. Check the file size first: 2K at crf 16 was ~250 MB for 67 s, so a 6-minute video will be roughly 1-1.5 GB. If it is too large to send, tell the user and offer a crf 18-20 copy, but keep crf 16 as the master.

## Final look values (bridge after 4 verifier rounds; all in `templates/bridge/`)
Start every new video from `templates/bridge/` (copy to `src/videos/<name>/`, then convert to 16:9): the look lives in these values, the
story lives in story.ts / build.ts layout / cast.ts roles. Opus max scores at the end: colour day 7, golden 5-6,
lighting 5-6, ink 6, textures 5-6, characters 7, depth 6-7, composition 6-7 (from 5/3/4/5/4/7/4/6 at the start).
All "px" values below are at the reference scale (long side 1920 px, which is 1920x1080 in 16:9; scale 1.0).
- **Light presets** (`lights.ts`, sun camera-relative and frozen per shot):
  - day: az 75 / el 48, lit [1.58,1.5,1.33], shade [.30,.29,.36], lift [.05,.06,.11] (cool tint ONLY in shade), sky
    top #3e7fd2 → horizon #b8d0ec, fog = horizon, 25→300 m, uSat 1.08, ~15 % blue haze past 60 m.
  - golden: az 6 / el 7 (backlit low sun), key toward #FFB36B ×1.5, violet-blue shade (facade ≈ #444380), sky
    #ffc47c → #8a6f8a, peach fog only past 50 m, sun disc #ffcf9e + bloom, contact blobs + real card shadows.
- **Albedos** (`paint.ts`/`build.ts`): muted brick / cream / ochre / terracotta houses with plinth, cornice, sills,
  hoods, door step, chimney + pot, heights 7 / 8.4 / 9.8 m; warm grey/beige stone (saturation −35 % from "storybook");
  ochre wood deck C* ~51-58 with near-black seams; iron dark and desaturated. Never lemon, never lavender lit faces.
- **Ink** (`ink.ts`): #030303, 5.1 px near (6 m) → 3.3 px far (90 m) at the reference scale, creases ×0.8 from 35°, depth
  jumps from 1 %, fog tint 0.3 only far; character cards on layer `NOINK` (they carry their own plum ink).
- **Lens** (`lensblur.ts` + `dofAt` in story.ts): cInf px at the reference scale: hook/river CU 15-16, close-ups 12-14, MS 10-12,
  park 7, golden sway 5, wides/aerials 0. Focus on the eyes of the front row.
- **Post** (`Bridge.tsx`): ink → lens pre → lens main → bloom (thr 1.0, int .5, 6 levels, radius .6) → NEUTRAL tone
  map → grade; MSAA 4. Film grain exists but is OFF (reference measures no grain; it quadruples the file size).
- **Characters** (`toon.ts`/`cast.ts`/`cardshade.ts`): bean heads h/w .94, plum ink #241726, eyes .26 W apart, no blush,
  14 expressions; adults 3.5 head-heights; walk bob 5 %, lean 1-7°, planted 0.8 m stride; blinks 0.5-2.3 s; tight
  contact shadow ~.5 under the feet + sun-direction tail by day; golden rim inner only (never an offset outer copy).
- **Camera** (`story.ts`): cuts on big jumps, smootherstep glides 18-40 f, every hold creeps (no static hold ≥4 f),
  flip kicks decaying over 7 f on impacts, 4-4.6° rolls, foreground props (`FOREGROUND`) in ~1/3 of shots.

## Look targets (measured from the reference; see references/reports/)
- **Ink:**
  - World lines: about 3 px at 1080 wide, near-black #0b0a0c, nearly constant with distance. Silhouettes, depth jumps and normal creases only. No line on wall/floor joins or the horizon. Character cards are never outlined by the world ink.
  - Character ink: dark plum #201628-#2f1d28, thicker on the bottom/shadow side.
- **Colour:**
  - Day median luma ~.57. Non-ink p5 ~.23 (no crushed blacks). Lit chroma C* p90 ~55. Day sky top ~#5e93d9, horizon 1.28x brighter.
  - Golden: peach horizon #ffd08a, violet-blue shadows (never brown), backlit sun.
- **Depth:**
  - DOF on about 1/3 of screen time (CU, MS, inserts), cInf 10-16 px at 1080, focus on the eyes. Establishing and wide shots stay sharp.
  - Fog = horizon colour, 25→420 m day, 20→360 m golden.
  - Bloom only on HDR emissives: threshold 1.0.
- **Camera:**
  - Hard cuts on big jumps. Everything else is a smootherstep glide of 18-40 f, peak ≤ 25 px/f at 720p.
  - Every hold pushes in 0.15-0.35 %/f, with 0.6 %/f on punch words.
  - Rolls of 4-5°.
  - No hand-held float. Impact shakes flip sign every frame and decay over 7 f.
- **Characters:**
  - About 3.5 head-heights tall, legs 1.4 HH.
  - Stick limbs about 0.055 of head width, ink-dot hands.
  - Bean head (h/w ~0.93, narrower chin, pointier crown).
  - No blush or eye highlights. Faces at brightness ~.93.
  - Walk bob 3-7 %, lean 1-7°, planted feet.
  - Blinks every 0.5-2.3 s, lasting 4 f.

## Look-match loop (how the bridge look was reached; reuse for any "make it look like X" pass)
1. **Measure the reference first.** One analysis agent per topic (motion, depth, textures, characters, lighting, outlines, grade), each writing numbers, not adjectives, to `references/reports/<topic>.md`. Shared brief: `compare/CONTEXT.md`. Reports are the source of truth for every agent after.
2. **Sandboxes, one owner per file.** Copy the project folder to `src/videos/<name>_{cam,world,mat,char,post}` with entries `src/sandboxes/<n>/index.tsx` (same `index.tsx` as main, only the import path changed). Ownership used for the bridge:
   - cam: story.ts (camera keys, cuts, kicks, DOF table, foreground props)
   - world: world.ts, lights.ts, shade.ts, grade.ts (sun, sky, fog, river, colour, grade)
   - mat: paint.ts, build.ts (painted textures, buildings, props)
   - char: toon.ts, cast.ts, cardshade.ts (character drawing, posing, card lighting, fx cards)
   - post: ink.ts, Bridge.tsx, lensblur.ts (outline pass, composer, lens blur, bloom)
   Agents only edit their own files in their own sandbox and render stills with `BRIDGE_ENTRY=src/sandboxes/<n>/index.tsx node scripts/bridge-stills.mjs out/<name>/<n>/x_ <frames> 0.5`.
3. **Merge** with `python scripts/bridge_merge.py [cam,world,mat,char,post]` (backs main up to `backup/bridge_<time>/`, copies owned files only). Then `npx tsc --noEmit` (filter to the project folder), then **re-sync every sandbox from main** (`cp -f src/videos/<name>/* src/videos/<name>_<n>/`) before the next round.
4. **Verify** with a fresh Opus max agent: side-by-side sheets (`python scripts/sbs_sheet.py out.jpg --ours a b --ref r1 r2`), score each area out of 10 (colour day / golden, lighting, ink, textures, characters, depth, composition) plus a numbered fix list with frame numbers. Map each fix to the file owner and resume that owner's agent with SendMessage (keeps its context). Bridge scores: round 1 5/3/4/5/4/7/4/6 → round 2 5/4/5/6/4/7/6/6 → round 3 see notes.
5. Contracts between owners live in exported constants (e.g. `NOINK` layer from ink.ts, `dofAt`/`FOREGROUND` from story.ts, `LIGHTS`/`LOOK` from lights.ts). Changing a contract means telling every owner that reads it.

## Gotchas
- Kenney nature-kit materials are teal: recolour them by material name in build.ts.
- Cameras end up inside houses: the sight-line culler in build.ts/world.ts removes anything blocking any camera ray.
- Ink in the same merged effect pass as DOF gets overwritten. Ink must be its own pass before the blur.
- Pixel-sized post parameters must scale with resolution: stills are at 0.5, the final at 1.333. R3F clamps dpr to ≥ 1, so `--scale 0.5` stills still render a 1920x1080 buffer.
- **Disk:** every Remotion bundle copies `public/` (~1.3 GB) into `the temp folder\remotion-webpack-bundle-*` and never deletes it. 60+ GB filled the system drive during the bridge passes and a render died with ENOSPC. Always render with `export TEMP='<big drive>/remotion-tmp' TMP='<big drive>/remotion-tmp'` (bridge-stills.mjs sets it itself) and ask the user before deleting old bundle folders in the shared the temp folder (auto mode blocks the sweep). <big drive>/remotion-tmp fills too (~500 folders / 110 GB after the 4 bridge rounds): when a project is finished and nothing is rendering, delete its `remotion-webpack-bundle-*` and `puppeteer_dev_chrome_profile-*` folders there. A 5-7 minute render also writes many more frames, so check free space on the big drive before starting.
- In JS strings write `"<big drive>/remotion-tmp"`, never `"<big drive>\remotion-tmp"` (`\r` becomes a carriage return).
- Remotion writes a silent audio track: mux with `-map 0:v:0 -map 1:a:0`.
- Sub-agents can't write report files in this harness: ask them for a ≤150-word final message and save it yourself to `out/<name>/<n>/REPORT.md`.
- Scratch `.ts` files inside `out/` break `tsc`: name them `.txt`.
- 2K renders run ~4 fps (≈10 min for 67 s, so ≈45-60 min for a 5-7 minute video). Stills at 0.5 take 3-8 s each after a ~40 s bundle.
- Film grain at 2.5 % made the 2K file 910 MB instead of ~250 MB (for the 67 s short) and will turn to mush on YouTube; the reference has none. Keep it off.
- A long video means a bigger scene: keep texture memory and the number of billboards in check (build each location once, cull the rest per shot) so the render does not run out of GPU/Chrome memory partway through.

## Readability traps the verifier caught (check every new prop/shot for these)
- **A swing drawn as one rope that disappears behind the kid reads as a HANGING.** Always two ropes to a wide plank
  with the kid between them (`swingKid` in toon.ts), and a frame whose legs clear the arc (A-frame at each end).
- Props or flotsam closer than ~2 m to a lens become huge cut-off blobs at the frame edge. When a camera moves, re-check
  every prop near its new position (the river cameras moved into the floating hats).
- A foreground bush cut by the frame edge reads as a "green ball"; a foreground prop must be recognisable or gone.
- Flags: red + yellow star/diamond reads as Vietnam's. Use a period-correct flag (St George's cross for England 1831).
- Painted signs lit by a side sun go grey-blue and unreadable: give the sign face an unlit material.
- Golden rim light done as an offset outer copy turns every hat into a picture frame: inner rim only, head/torso only.
- Balcony/railing rails at chest height through characters: put the lens ~2.5 m up so rails cross legs, not torsos.
- Flat holds read as freezes in QA (frozen span ≥12 f): every hold needs a creeping push-in.
- 16:9 only: the wide frame shows the sides of the set, so check the left and right edges of every shot for culled-house gaps, floating props, an empty void, and the end of the ground plane; and a centred subject in every shot looks static, so vary the placement.
- 16:9 only: a long video repeats itself fast. If two acts use the same location, light and camera moves, change at least one of them.
