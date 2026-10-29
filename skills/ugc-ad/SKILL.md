---
name: ugc-ad
description: Make a UGC-style talking-actor video ad for a product or URL: brief, hooks, actor, quote, render, captions. Use when someone asks for a UGC ad, a creator-style video, a talking head ad or a TikTok/Reels ad.
---

# UGC ad

Turn a product (or a product URL) into a 15-40 second creator-style ad: a real-looking person talking
to camera, captions on, 9:16.

## Steps
1. **Context.** `get_workspace` (credits), `list_brands` then `get_brand` for the product, audience,
   voice and proof points. If the user gave a URL that is not in the Brain, ask for the 2-3 facts that
   matter (what it is, who it is for, the offer).
2. **Angle + hooks.** `generate_hooks` with the topic and the platform (`tiktok` or `meta`). Pick the
   3 strongest; show them with the angle each one uses. Let the user choose (default: the first).
3. **Script.** `write_script` with `style: "ugc"`, the chosen hook, `duration_s` 20-30 and the
   platform. Keep it spoken: short sentences, one idea per line, a clear CTA. Read it back to the
   user and tighten it (under ~75 words for 30 s).
4. **Actor.** `list_actors` filtered to the audience (gender, age, setting such as `home`,
   `kitchen`, `car`). Offer 3 options with their names and settings. Custom or cloned actors of the
   workspace (`custom: 1`) come first if the brand has them.
5. **Quote.** `generate_talking_actor` with `actor_id`, `script`, `ratio: "9:16"`, `captions: true`,
   `caption_style: "bold"` and `estimate_only: true`. Say the price.
6. **Render.** Same call without `estimate_only` (add `brand_id`, and a `project_id` if the user keeps
   projects). Then `wait_for_job`.
7. **Deliver.** Save the video to the run folder and link it with the script, then offer two next
   steps: `create_variations` for a hook test, `translate_video` for other markets. For a new
   version of the same ad, `replace_actor` swaps the presenter and `change_voice` the voice
   without rewriting anything.

## Quality bar
- The first 2 seconds must earn attention: a question, a surprising claim the Brain supports, or a
  visible problem.
- Product name said clearly once, CTA in the last 3 seconds.
- If the take looks off (lip sync warning on the job's `sync`), offer a re-render with the
  `lipsync` strategy rather than shipping it.

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
