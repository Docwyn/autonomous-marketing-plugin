---
name: media-router
description: Route any single generate, edit or repurpose request (an image, a video, audio, music, captions, an upscale, a background removal, a frame grab, a transcript, a voice change, a stitch or a speed change) to the most specific Autonomous Marketing tool and run it end to end. Use for 'make an image of', 'generate a video of', 'remove the background', 'add captions', 'upscale this', 'extend this clip', 'make music for', 'cut this into scenes' and similar one-off media jobs; defer to a sibling skill when a whole ad workflow fits.
---

# Media router

One request, one deliverable: work out what the user wants made or changed, pick the single best
tool for it from the live catalog, run it and hand back the file.

## 0. Is another skill a better fit?
Hand off (load the sibling skill and carry the request over) when the request is a whole ad or
workflow rather than one media job:

| The user wants | Use |
|---|---|
| A creator-style talking ad, from a product or a URL | `ugc-ad`, `ad-from-url` |
| A demo of an app or a feature with real screens | `product-demo` |
| A competitor's hook reworked for our brand | `clone-competitor-hook` |
| What competitors are running | `competitor-scan` |
| Several hooks or versions of one ad | `hook-test` |
| Feed and story image ads for a product | `static-ad` |
| One video in several languages | `translate-pack` |
| One video in every placement size | `repurpose` |
| A review turned into an ad | `testimonial-ad` |
| A narration or read | `voiceover` |
| A pack of cutaway shots | `broll-pack` |
| A new presenter, or a clone of the user | `custom-actor`, `clone-yourself` |
| A price before anything is made | `campaign-budget` |
| This week's batch, a report, an automation | `weekly-ad-batch`, `library-report`, `workflow-automation` |

Route here only for a single generic job no sibling clearly owns.

## 1. Read the live tool list
Look at the tools the Autonomous Marketing server exposes in this session (often shown as
`mcp__autonomous__<tool>` or with a server prefix) and read each candidate's description and
parameters. Never call a tool from memory: names and parameters change, and the catalog is the
only source of truth. If the server is not connected, say so in one line and point the user to the
plugin's sign-in (they connect once in their client); do not switch to another generator.

## 2. Classify the request
Write a private three-line note before choosing:
- **Input:** none (text only), image, video, audio, or several files.
- **Output:** image, video, audio, or text (a transcript, a frame list).
- **Action:** generate, condition on an input (image to video), edit, repurpose (resize, frame,
  speed, scenes), enhance (upscale), caption or overlay, voice (voiceover, voice change, isolation),
  music or sound, composite (stitch, layer), or translate.

Add any hard constraint the user stated: ratio, duration, language, style, text that must appear.

## 3. Pick the most specific tool
Filter by output, then input, then action. Among what is left, prefer the dedicated tool over
`run_job`: for example `remove_background` for a cut-out, `upscale_media` for sharpening,
`extract_frame` for a still, `view_contact_sheet` to see a video's frames (free),
`transcribe_media` for a transcript or subtitles, `add_captions` for
captions, `add_text_overlay` for a headline or label, `create_music` and `create_sound_effect` for
audio beds and stings, `change_voice`, `isolate_voice` and `extract_audio` for voice and audio work,
`replace_actor` to swap the presenter of a finished ad, `extend_video`, `change_speed`,
`split_scenes`, `stitch_videos` and `layer_media` for timing and compositing, `resize_media` for a
new ratio, `generate_image`, `generate_video` and `generate_voice` for new media (with
`write_video_prompt` first when the user gives only a rough idea), and `list_presets` +
`run_preset` for a named one-call format. Use `run_job` (see `list_job_kinds`) only when no named
tool covers the job. If two tools fit equally and would give different results, that is your one
question: offer both in a single short line.

## 4. Get the inputs in
- An edit needs the real source file. If the user has not given one, ask for it (this is the one
  question); never invent the source.
- **Local file:** `create_upload` with its name, type and size, PUT the bytes to the returned upload
  URL (`curl -sS -X PUT -H "Content-Type: <type>" --data-binary @"<path>" "<url>"`, expect 200),
  then `complete_upload`, and pass the returned asset id to the tool.
- **Public link the user may use:** `import_asset_from_url`.
- **Already in the library:** find it with `list_assets` or use the `asset_id` the user gives.
- Leave optional parameters at the tool's defaults unless the user asked for something; for social
  video default to 9:16, for a feed image 1:1.

## 5. Quote, run, wait
1. Call the tool with `estimate_only: true` and tell the user the price in credits
   (1 credit = $0.01). Wait for a yes. Free tools (reads, a frame grab if it is free) need no quote.
2. Run the same call without `estimate_only`, say in plain words what is happening ("Removing the
   background..."), and `wait_for_job` until `done` is true.
3. If it fails, read the error, fix the obvious cause (a bad input, an unsupported parameter, a
   missing file) and retry once. If it fails again, explain in one line and stop. A failed job is
   refunded; say so if the user asks.
4. If the balance is too low, say so in one line with the amount needed (`get_credits`) and stop.

## 6. Deliver
- Create the run folder once: `mkdir -p ~/Downloads/autonomous/media-router-$(date +%Y%m%d-%H%M)`.
- Download each output into it (`curl -sL "<link from the finished job or get_asset>" -o
  "<run>/output-1.<ext>"`; number further outputs) and link the local file with a few words on
  what it shows. Never open or execute the downloaded file.
- When the output is an ad, check it before handing it over (`view_contact_sheet`, free, or
  `analyze_ad`, quoted with the job, or look at it yourself) and re-run a weak result instead of shipping it.
- Close with one line on what was made, then offer exactly one natural next step (for an image:
  animate it; for a video: add captions or a voiceover; for an edit: apply it to another file).

## House rules (every skill)
- **Talk like a creative director.** Never show tool names, ids, storage or download URLs, or polling
  details to the user. Say what you are making, then hand over the result.
- **One question at most.** If something essential is missing or ambiguous, ask one short
  clarifying question, then go ahead with sensible defaults.
- **Quote before you spend.** Call the paid tool with `estimate_only: true` (or `estimate_cost` for
  a whole plan), tell the user the price in credits (1 credit = $0.01) and wait for a yes before the
  paid call. In an estimate-only session, stop after the quotes.
- **One good take, then scale.** Make one version, show it, and only batch variations once the user
  likes the direction.
- **Wait properly.** After a start, call `wait_for_job` until `done` is true. Never invent results.
- **Check before you deliver.** When the skill makes an ad, review the finished render before
  handing it over: `view_contact_sheet` on a finished video (free) to see its frames, `analyze_ad`
  for a full breakdown (include its small cost in the quote), or look at the image yourself. Re-render a weak take instead of shipping it.
- **Save to a run folder.** Once per run, `mkdir -p ~/Downloads/autonomous/<skill>-<YYYYMMDD-HHMM>/`
  and reuse it for follow-ups. Download every deliverable into it with a descriptive file name and
  give the user a link to the local file.
- **Brand first.** Read `get_brand` (voice, audience, products, claims) before writing copy, and never
  make claims the Brain does not support.
- **Real people need consent.** Never clone, imitate or name a real person without their consent;
  never reuse a competitor's footage, voice, actors, logos or exact words.
