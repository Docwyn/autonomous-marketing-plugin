---
name: clone-yourself
description: Turn a founder, employee or creator into a reusable talking actor from a short video of them, with their consent. Use for clone myself, founder-led ads, digital twin or 'make videos of me'.
---

# Clone yourself (consent required)

## Before anything
Ask the user to confirm **they are the person in the video, or have that person's explicit written
permission** to use their face and voice for ads. If not, stop and suggest `custom-actor` instead.
The confirmation is stored with the actor.

## Steps
1. **Video.** A 30-120 s clip, face to camera, good light, one speaker, no music. Get it into the
   library: `create_upload` + PUT + `complete_upload`, or `import_asset_from_url` for a link the
   user controls. If the clip has background noise or music, `isolate_voice` first and clone from
   the clean result.
2. **Quote.** `clone_actor_from_video` with `video_asset_id`, `name`, `consent: true` and
   `estimate_only: true`.
3. **Clone.** Same call without `estimate_only`. The voice is cloned and a portrait is generated
   from the footage; `wait_for_job` on the returned job. If `voice_error` is set, explain it and
   offer a library voice via `update_actor`.
4. **First take.** A short `generate_talking_actor` line so the user can check likeness and voice.

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
