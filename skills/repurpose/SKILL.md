---
name: repurpose
description: Repurpose one video for every placement: resize to 9:16, 1:1, 16:9 and 4:5, add captions in the right style, and deliver each. Use for repurposing, resizing, making versions for TikTok/Reels/YouTube/feed.
---

# Repurpose for every placement

## Steps
1. **Source.** `get_asset` for the video (ratio, duration, captions?).
2. **Plan.** Placements -> ratios: TikTok/Reels/Shorts 9:16, feed 4:5 or 1:1, YouTube/web 16:9.
   Skip the source's own ratio.
3. **Quote.** `estimate_cost` with one `resize` per ratio (`mode: "blur_pad"` keeps the whole frame,
   `crop` fills it), plus `captions` where the source has none.
4. **Run** the resizes, then `add_captions` on the results that need them (`style: "bold"` for
   social, `clean` for web). `wait_for_job` each.
5. **Extras when asked.** `stitch_videos` to join an intro, the ad and an end card;
   `add_text_overlay` for a placement-specific headline or offer; `change_speed` or
   `split_scenes` for a shorter cutdown; `extract_frame` for a thumbnail or cover image.
6. **Deliver** a table: placement, ratio, link to the saved file.

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
