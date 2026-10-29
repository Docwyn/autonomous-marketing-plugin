---
name: testimonial-ad
description: Make a testimonial-style ad from REAL customer reviews: pick a genuine review, script it faithfully, cast an actor and render with a clear disclosure. Use for testimonial ads, review ads or social proof videos.
---

# Testimonial ad (honest)

## Rules first
- Use only real reviews from the brand's Brain or ones the user provides, with permission to use
  them. Never invent a customer, a quote or a result.
- An actor reading a customer's words is a dramatization: add an on-screen note such as
  "Dramatization of a real customer review" (an `edit_video` `overlay_text`), unless the user is
  using the real customer's own clone with consent.

## Steps
1. `get_brand` -> reviews / proof. Pick one specific, concrete review with the user.
2. `write_script` with `style: "testimonial"`, keeping the reviewer's meaning and key phrases.
3. Cast a matching actor (`list_actors`), quote and render `generate_talking_actor`.
4. Add the disclosure with `add_text_overlay` for the first 2 s (or `edit_video` with
   `overlay_text`), then `wait_for_job`.
5. Deliver; offer `hook-test` on the opening line.

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
