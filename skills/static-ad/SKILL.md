---
name: static-ad
description: Make a set of static image ads (product hero, benefit, social proof, offer) in the right sizes, on brand. Use for image ads, statics, banners, Meta/Instagram feed creatives or carousel cards.
---

# Static ads

## Steps
1. **Inputs.** `get_brand` (colors, voice, offer). Product images: `list_assets` with
   `tag: "product"`, or `import_asset_from_url` for public product photos the user owns.
2. **Concepts.** Plan 4 statics: product hero, benefit callout, social proof (only real reviews from
   the Brain), offer/urgency. Write each as one image prompt plus the headline text to overlay.
3. **Quote.** `generate_image` per concept with `reference_asset_ids` (the product), `brand_style:
   true`, `ratio` (`1:1` feed, `4:5` feed portrait, `9:16` stories) and `estimate_only: true`; or
   one `estimate_cost` with all of them.
4. **Render.** Start them (they run in parallel), then `wait_for_job` each.
5. **Finish.** `upscale_media` on the picks the user keeps (quote first) so they stay sharp in
   large placements, and `add_text_overlay` when the headline should be set as real text rather
   than drawn by the image model.
6. **Deliver.** Each image with its headline and primary text. Offer `resize_media` for other
   placements and a short `generate_video` animation of the best one.

## Quality bar
- The product must look like the real product (always pass a reference image).
- One message per image; text short enough to read in a second.

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
