---
name: weekly-ad-batch
description: Run the weekly production loop: review what worked, research, plan a fresh batch of ads, quote it as one budget, render, and report. Use for weekly creative batches, content calendars, always-on testing or 'make this week's ads'.
---

# Weekly ad batch

A repeatable loop. Run it every week (or set it up as a workflow + trigger with `workflow-automation`).

## Steps
1. **Last week.** `list_jobs` and `list_assets` (`origin: "generated"`, newest first) to see what was
   made; ask the user which ads won (or read the brand's winning ads in `get_brand`).
2. **Fresh signal.** `list_competitor_ads` (running, longest) for new angles; note anything new since
   last week.
3. **Plan.** 5-10 ads: double down on last week's winner (new hooks on the same body via
   `create_variations`), 2-3 new concepts (`generate_concepts`, with `plan_hooks` for the openings),
   1-2 statics (`generate_image`), a preset format where it fits (`list_presets`), and
   translations for markets that work.
4. **Budget.** One `estimate_cost` for the whole plan. Present it as a table with the total and the
   balance (`get_credits`). Get approval; trim if needed.
5. **Produce.** Start everything (`run_jobs_batch` for raw jobs, or the specific tools), then
   `wait_for_job` on each. Pass a `webhook_url` if the user has one for notifications.
6. **Report.** What was made (links), credits spent, and what to measure next week.

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
