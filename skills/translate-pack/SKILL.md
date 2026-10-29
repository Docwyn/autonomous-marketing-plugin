---
name: translate-pack
description: Translate a finished video ad into several languages at once (dubbed in the same voice, or subtitled), then deliver every version. Use for localisation, new markets, translate this video, or a language pack.
---

# Translate pack

## Steps
1. **Source.** Find the video (`list_assets` with `kind: "video"`, or the id the user gives).
   `get_asset` to confirm the title, duration and spoken language; if the language is unclear,
   `transcribe_media` reads it (and gives the source script to check the translations against).
2. **Languages.** Ask which markets (up to 10 per call), suggest the brand's markets from
   `get_brand`. Ask: dubbed voice (default, keeps the actor's voice) or subtitles only.
3. **Quote.** `translate_video` with `asset_id`, `target_locales`, `mode` and `estimate_only: true`.
4. **Run.** Same call without `estimate_only`. It returns one job per language.
5. **Wait.** `wait_for_job` for each job (call again while `done` is false). If one fails, say which
   language and why; the others are still delivered and the failed one was refunded.
6. **Deliver.** A table: language, status, link to the saved file. On-screen text burned into the
   source is not translated by the dub: offer `add_text_overlay` with the translated line per
   language. Ad copy around the video can be translated with `translate_text` (keeps glossary
   terms such as the brand name).

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
