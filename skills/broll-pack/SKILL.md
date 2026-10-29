---
name: broll-pack
description: Generate a pack of B-roll clips (product in use, lifestyle, textures, establishing shots) to cut into ads. Use for b-roll, cutaways, filler shots, product scenes or video backgrounds.
---

# B-roll pack

## Steps
1. **Shot list.** From `get_brand` and the script (if any), list 4-6 shots: what is on screen,
   camera move, mood, 4-8 s each. Keep them generic enough to reuse across ads.
2. **Prompts.** `write_video_prompt` turns each shot line into a full video prompt (camera, light,
   motion), tuned to the model picked in the next step.
3. **Model.** `list_models` with `capability: "video"`; prefer the default unless the user wants a
   specific look. Note each model's price per second.
4. **Frames (optional).** For product shots, start from a real product image: `generate_image` with
   the product as reference, then use it as `start_frame_asset_id` (keeps the product accurate).
5. **Quote.** `estimate_cost` with all `video` jobs, or `generate_video` with `estimate_only`.
6. **Render** and `wait_for_job` each; deliver as a list with the prompt used. `extend_video`
   lengthens a clip that is too short, `upscale_media` sharpens one for 16:9, and `split_scenes`
   cuts a long clip into separate shots.
7. **Use them.** Show how to cut them in with `edit_video` (`insert_broll`, `concat`).

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
  handing it over: `analyze_ad` on a finished video (include its small cost in the quote) or look at
  the image yourself. Re-render a weak take instead of shipping it.
- **Save to a run folder.** Once per run, `mkdir -p ~/Downloads/autonomous/<skill>-<YYYYMMDD-HHMM>/`
  and reuse it for follow-ups. Download every deliverable into it with a descriptive file name and
  give the user a link to the local file.
- **Brand first.** Read `get_brand` (voice, audience, products, claims) before writing copy, and never
  make claims the Brain does not support.
- **Real people need consent.** Never clone, imitate or name a real person without their consent;
  never reuse a competitor's footage, voice, actors, logos or exact words.
