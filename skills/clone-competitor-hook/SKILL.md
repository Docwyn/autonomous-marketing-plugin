---
name: clone-competitor-hook
description: Study a competitor's public ad and write ORIGINAL hooks for your brand that use the same psychological mechanism, then render one. Use when someone says clone this hook, steal this angle, do what this ad does, or shares a competitor ad link.
---

# Clone a competitor hook (ethically)

We clone the *mechanism*, never the asset: no reused footage, voices, actors, logos or exact lines.

## Steps
1. **Find the ad.** If the user gave a public URL, use it. Otherwise `list_competitor_ads`
   (`status: "running"`, `sort: "longest"`) - long-running ads are the proven ones - and let the
   user pick.
2. **Break it down.** `analyze_ad` with `url` (or `asset_id` for an uploaded copy the user owns), quote
   it first. Wait for the job and summarise: the hook, the beat structure, the angle, why it works.
3. **Name the mechanism.** In one line: e.g. "pattern interrupt + specific number", "us-vs-them",
   "confession", "before/after in 3 s". This is the only thing we carry over.
4. **Write originals.** `get_brand` for our product and proof, then `plan_hooks` with the
   mechanism to plan the opening beats (what is seen and said in the first 3 s), and
   `generate_hooks` with the angle set to the mechanism for the lines. Rewrite any hook that is within a few words of the original ad: it must be
   clearly ours.
5. **Render one.** Pick an actor (`list_actors`), quote and render with `generate_talking_actor`
   (hook + a short body via `write_script`). Wait for it.
6. **Test.** Offer `create_variations` with the 3 best hooks to find the winner.

## Never
- Copy the competitor's script, claims, brand names, music, actors or visuals.
- Imply a partnership with or endorsement by the competitor.

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
