# Autonomous Marketing - skills plugin for Claude, ChatGPT and Codex

Ad-production playbooks that drive the **Autonomous Marketing hosted MCP server**
(`https://app.mantinea.com/mcp`): every step uses the same tools, credits and quality checks as the
app, and every paid step is quoted before it runs.

The plugin connects with **OAuth**: the first time a tool is used, your client opens the
Autonomous Marketing consent page, you sign in and pick a workspace, and that is it. No key to copy.

## Install in Claude Code

```text
/plugin marketplace add Docwyn/autonomous-marketing-plugin
/plugin install autonomous-marketing@autonomous-marketing
```

Then run `/mcp`, pick `autonomous` and sign in.

## Upload the ZIP (Claude, ChatGPT, Codex)

Download the build for your client and upload it where the client takes plugins:

- Claude: https://app.mantinea.com/plugins/autonomous-marketing-claude.zip
- ChatGPT and Codex: https://app.mantinea.com/plugins/autonomous-marketing-codex.zip

Both contain the same skills and the same `.mcp.json`; only the manifest differs
(`.claude-plugin/` or `.codex-plugin/`). Sign in when the client asks.

## Connect without the plugin

Any MCP client can use the server directly. Add a remote (Streamable HTTP) MCP server with the URL
`https://app.mantinea.com/mcp` and sign in when asked. You get every tool but not the playbooks.

## Use an API key instead of signing in

For scripts, CI or clients without OAuth: create a key in the app (**Settings > API keys > Create
key**; pick *Generate*, or *Read only* for a setup that can never spend) and pass it as a bearer
header:

```bash
claude mcp add --transport http autonomous https://app.mantinea.com/mcp --header "Authorization: Bearer scs_live_..."
```

**Planning only:** use `https://app.mantinea.com/mcp?mode=estimate` and every paid tool returns a
price quote instead of starting work.

## Skills

| Skill | What it does |
|---|---|
| `media-router` | Any single generate / edit / repurpose request, routed to the most specific tool |
| `ugc-ad` | Creator-style talking-actor ad from a product or URL |
| `ad-from-url` | Product page link to a finished ad in one pass |
| `product-demo` | Presenter + real product screenshots/recordings cut in |
| `testimonial-ad` | Real-review testimonial with an honest dramatization label |
| `clone-competitor-hook` | Learn a competitor hook's mechanism, write original hooks, render |
| `competitor-scan` | What competitors run, for how long, and what to test next |
| `hook-test` | Hook x actor x ratio x language variation grid |
| `static-ad` | On-brand static image ads in every feed size |
| `broll-pack` | Reusable B-roll clips (optionally from real product frames) |
| `voiceover` | Script + voiceover in any library or cloned voice |
| `translate-pack` | Dub or subtitle a video into up to 10 languages at once |
| `repurpose` | Resize + caption one video for every placement |
| `custom-actor` | Design a fictional presenter and add it to the actor library |
| `clone-yourself` | Founder/creator clone from a video, consent required |
| `weekly-ad-batch` | The weekly production loop: review, research, plan, budget, render, report |
| `workflow-automation` | Workflow + inbound trigger URL + completion webhook |
| `campaign-budget` | A priced shot list before anything is made (estimate-only) |
| `library-report` | What was produced, approved, failed and spent |

Every skill ends with the same house rules: no tool names, ids or storage links in replies, at most
one clarifying question, a quote and a yes before any spend, one good take before scaling, a check of
every finished ad before delivery, deliverables saved to `~/Downloads/autonomous/<skill>-<date>/`,
copy grounded in the brand's Brain, and never cloning or imitating a real person (or reusing a
competitor's assets) without consent.
