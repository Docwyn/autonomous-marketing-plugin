---
name: library-report
description: Summarise what the workspace produced: assets by type and status, review state, credits spent, and what is waiting for approval. Use for reports, recaps, 'what did we make', or credit usage questions.
---

# Library report

Read-only.

## Steps
1. `get_credits` and `list_credit_ledger` (recent settles and refunds) for spend.
2. `list_assets` (`origin: "generated"`) page by page for the period; count by `kind`, `ratio` and
   `review_status`.
3. `list_jobs` with `status: "failed"` to surface failures (they were refunded).
4. Report: produced this period, approved vs waiting, top spend by kind, failures and why, and
   links to the newest approved videos. Suggest next steps (approve pending, re-run failures).

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
