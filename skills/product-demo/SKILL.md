---
name: product-demo
description: Make a product demo video: a presenter explains one feature while the video cuts to real screenshots and screen recordings of the product. Use for SaaS/app demos, feature launches and explainer ads.
---

# Product demo

A presenter explains a feature; the edit cuts to the real product while they talk.

## Steps
1. **Pick the feature.** `get_brand`, then `list_product_features` for the brand. Each feature has
   real shots (screenshots, screen recordings). If there are none, ask the user to upload a few
   (`create_upload` / `complete_upload` with `tags: ["product"]` and the `brand_id`) or import
   them from public URLs with `import_asset_from_url`.
2. **Script.** `write_script` with `style: "demo"`, `concept` = the feature, `product` = its summary
   and details, `duration_s` 30-45. One problem, the feature as the answer, one proof point, CTA.
3. **Presenter.** `list_actors` (setting `office` or `studio`) or a custom actor of the brand.
4. **Quote, then render.** `generate_talking_actor` with `product_shots: {feature_id, layout:
   "auto"}` (or `asset_ids` for specific shots, `layout: "pip"` for a picture-in-picture demo),
   `ratio` 9:16 for social or 16:9 for a website. Quote with `estimate_only` first.
   **Preset instead?** For a quick format, `list_presets` and `run_preset` with the show_app
   preset (a hand showing the app on a phone) or unboxing_pov (a first-person unboxing of a
   physical product); quote first like any paid call.
5. **Wait and deliver.** `wait_for_job`; the result lists which line cut to which shot.
6. **Repurpose.** Offer `resize_media` for other placements and `add_captions` if captions were off.

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
