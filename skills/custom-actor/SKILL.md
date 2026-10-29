---
name: custom-actor
description: Design a new fictional presenter for the brand (look, setting, voice) and add them to the actor library. Use for custom actors, a brand ambassador, a recurring character or a mascot-like presenter.
---

# Custom actor

Creates a fictional person. For a real person, use `clone-yourself` (consent required). Using a
real photo as the face (`portrait_asset_id` on `create_custom_actor` / `update_actor`) also
needs `consent: true`, and only when the user confirms they are that person or have their
permission - never set it on their behalf.

## Steps
1. **Brief.** From `get_brand` (audience) agree: age range, look, clothing, setting, energy, and
   the voice (gender, accent, tone).
2. **Quote.** `create_custom_actor` with `name`, `description`, `setting`, `style` (`ugc_selfie` for
   phone-selfie realism, `studio`, `lifestyle`), `count: 4`, `estimate_only: true`.
3. **Generate.** Same call without `estimate_only`; `wait_for_job` on the returned job and show the
   4 portrait options.
4. **Pick + voice.** `update_actor` with `option_index` (0-3) and a `voice_id` from `list_voices`.
   The actor now appears in `list_actors` with `custom: true`.
5. **First take.** Offer a 10-second intro video with `generate_talking_actor` to check the voice and
   delivery before using the actor in ads.

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
