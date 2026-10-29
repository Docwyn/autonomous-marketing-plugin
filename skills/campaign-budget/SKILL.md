---
name: campaign-budget
description: Plan a campaign's creative production and its exact credit cost before anything is made - a priced shot list the user can approve. Use for budgeting, estimates, 'how much would it cost', planning, or an estimate-only session.
---

# Campaign budget (estimate-only)

Spends nothing. Works in an estimate-only MCP session and with a read-only key.

## Steps
1. **Brief.** Goal, markets, placements, number of concepts and versions, deadline.
2. **Price list.** `get_price_list` - per-second, per-image and per-call prices - and
   `list_models` if a specific model is wanted.
3. **Draft the plan** as concrete jobs: talking actors (script length -> seconds), videos (seconds),
   images (count), translations (languages), resizes, captions.
4. **Quote.** `estimate_cost` with every job (up to 100). Group the result by deliverable.
5. **Present** a table: deliverable, jobs, credits, dollars (credits x $0.01), the total, and the
   current balance (`get_credits`). Offer cheaper options (fewer languages, shorter takes, the
   `lipsync` strategy for long scripts).

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
