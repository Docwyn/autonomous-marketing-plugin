---
name: workflow-automation
description: Automate ad production: build a workflow from a description, give it an inbound trigger URL (for Zapier, Make, n8n, Shopify) and a completion webhook. Use for automation, pipelines, 'every time X happens make Y', or connecting another tool.
---

# Workflow automation

Needs an **admin** key or an admin approval for triggers and webhooks.

## Steps
1. **Describe.** `plan_workflow` with what should happen (e.g. "new product URL -> UGC script ->
   talking actor in 9:16 -> captions -> 3 languages"). Show the planned nodes.
2. **Template?** `list_workflow_templates` may already have it; `create_workflow` with
   `from_template_id`, or with the planned `graph`.
3. **Price one run.** `estimate_workflow` - tell the user what each trigger will cost.
4. **Test run.** `run_workflow` with sample `inputs`; `get_workflow_run` until it ends.
5. **Trigger.** `create_workflow_trigger` -> a secret URL (shown once). Anything that POSTs JSON
   `{"inputs": {...}}` to it runs the workflow. Tell the user to keep it secret.
6. **Notify.** `create_webhook` for `workflow_run.succeeded` / `failed`, or pass `webhook_url` per
   run. Explain the `X-Autonomous-Signature` check (docs: Webhooks).

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
