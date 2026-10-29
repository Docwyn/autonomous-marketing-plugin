---
name: ad-from-url
description: Go from a product page URL to a finished ad in one pass: pull the facts, write the script, pick the format, render. Use when someone pastes a product link and asks for an ad.
---

# Ad from a URL

## Steps
1. **Facts.** Check `get_brand` for this product first. If it is not there, `analyze_ad` with the
   `url` reads the page text (quote first; it is a light job) - pull name, price, top 3 benefits,
   audience, offer. Confirm them with the user; never guess prices or claims.
2. **Images.** `import_asset_from_url` for the main product image(s) on the page if the user owns
   or may use them.
3. **Format.** Physical product -> `ugc-ad` or `static-ad`; app/SaaS -> `product-demo`.
4. **Produce** with that skill's steps (quote, render, wait).
5. **Deliver** the ad plus 3 alternative hooks for a quick test.

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
