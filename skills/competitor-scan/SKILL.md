---
name: competitor-scan
description: Research competitors' running ads: what they run, for how long, which hooks and formats repeat, and what to test next. Use for competitor research, ad spying, market scans or 'what are my competitors running'.
---

# Competitor scan

Uses the ads the workspace tracks from the public Ad Library. Public information only.

## Steps
1. **Scope.** `get_brand` for the competitor list (Market section). If it is empty, ask the user
   for 3-5 competitors and suggest adding them in the app (Brain, Market) so the library fills.
2. **Pull.** `list_competitor_ads` with `brand_id`, `status: "running"`, `sort: "longest"`. Note:
   unknown run lengths are null - never treat them as zero.
3. **Pattern-find.** Group by advertiser and format; list the hooks/angles that repeat across ads
   that have run longest. Pick the 3 most informative ads.
4. **Deep-dive (optional, paid).** `analyze_ad` on those 3 (quote first) for hook, structure and
   why it works.
5. **Report.** Per competitor: what they push, formats, longest runners. Then "3 things to test"
   for our brand, each tied to a mechanism (never a copy). Offer `clone-competitor-hook` for one.

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
