---
name: voiceover
description: Write and record a voiceover for an ad or explainer in any voice and language. Use for voiceovers, narration, text to speech or radio-style reads.
---

# Voiceover

## Steps
1. **Script.** Use the user's text or `write_script` (`style` to taste). Read it aloud in your head:
   ~2.4 words per second, so 30 s is ~70 words.
2. **Voice.** `list_voices` filtered by gender and language; offer 3 with their style. A workspace
   actor's cloned voice is also a voice id (from `list_actors`).
3. **Quote, then record.** `generate_voice` with `text`, `voice_id`, `locale`; quote with
   `estimate_only` first (priced per 1k characters).
4. **Deliver** the saved audio. Offer a bed of music with `create_music` (or a sting with
   `create_sound_effect`) and `layer_media` to mix it under the voice or lay both over B-roll.
   To re-voice an existing recording, `change_voice`; to clean a noisy one, `isolate_voice`.

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
