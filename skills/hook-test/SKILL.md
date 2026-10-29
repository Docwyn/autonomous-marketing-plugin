---
name: hook-test
description: Run a creative test: write several hooks for one ad and render a variation grid (hook x actor x ratio x language). Use for A/B tests, creative testing, hook variations or more versions of a winning ad.
---

# Hook test

## Steps
1. **Source.** A finished talking-actor video (`list_assets`, `origin: "generated"`) or a script.
2. **Hooks.** `generate_hooks` (5-8) for the product and platform, or use the user's own;
   `plan_hooks` adds the visual opening for each when the hooks need more than a new line. Remove
   near-duplicates; keep different angles (problem, curiosity, social proof, offer, contrarian).
3. **Grid size.** Hooks x actors x ratios x languages multiplies fast. Propose a lean grid first
   (e.g. 4 hooks x 1 actor x 9:16) and quote it: `create_variations` with `estimate_only: true`.
4. **Render.** `create_variations` with `source_asset_id` (or `script`), `hooks`, optional
   `actor_ids`, `ratios`, `locales`. Then `get_variation_group` to follow the grid; `wait_for_job`
   on each job.
5. **Report.** A table of hook -> saved video. To test the presenter instead of the line,
   `replace_actor` on the winning video keeps the script and swaps the person. Suggest launching all as separate ads with the same
   budget and judging on hook rate (3-second views / impressions) and cost per result.

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
