---
description: One-time (or re-run anytime) setup for where each Think like an Architect artifact gets saved — local file, Confluence, Jira (Decision Log only), or Google Doc. Run this before the other skills if you want their output saved somewhere durable instead of just printed to chat.
disable-model-invocation: true
---

# Setup

You are configuring where this plugin's five artifact types get saved: **Decision** (from `/think-like-an-architect:decision`), **Decision Log** entries, **Pros and Cons Grids**, **Impact vs Effort Matrices**, and **RACI Matrices**. This is optional — every skill still works and prints its output to chat with no setup at all. This skill exists for people who want the output to land somewhere durable and shared automatically instead of being copy-pasted by hand every time.

## Config File

All configuration lives in one JSON file. Resolve its path with this exact shell logic (don't hardcode one or the other — the plugin data directory env var may or may not be set depending on the runtime):

```bash
CONFIG_DIR="${CLAUDE_PLUGIN_DATA:-$HOME/.claude/plugins/data/think-like-an-architect}"
mkdir -p "$CONFIG_DIR"
CONFIG_FILE="$CONFIG_DIR/config.json"
```

If `$CONFIG_FILE` already exists, read it first and show the user their current setup before asking anything — let them skip artifact types they don't want to change rather than re-answering everything every time.

## How to Run This

Go through the five artifact types **one at a time, as a conversation**. For each one, ask: "Where should this be saved: a local file, a Confluence page, a Google Doc, or (Decision Log only) Jira? Or leave it as chat-only." Then branch:

**Local file**
Ask for a path. If it doesn't exist, offer to create it (and its parent directory) now — don't just assume it's fine, actually check with a Bash `test -f` and create it if the user confirms.

**Confluence** (uses the plugin's bundled Atlassian connector — see `.mcp.json`)
Tell the user: the first time a skill actually writes to Confluence, they'll be prompted to authorize via OAuth against their own Atlassian site — that's expected, not an error. Ask for the Confluence **space key** and, optionally, a parent page to nest new entries under (if they don't have one, each entry becomes a new top-level page in that space).

**Jira** (Decision Log only, same Atlassian connector)
Ask for the Jira **project key**. Each decision log entry becomes a new Jira issue in that project, with the decision summary as the title and the full entry as the description. Mention that this works alongside a local-file or Confluence choice — Jira here is "also raise a tracked issue for this decision," not a replacement destination for the log itself, so ask whether they want Jira *in addition to* or *instead of* another destination.

**Google Doc**
This one needs more upfront setup than the others, and it's honest to say so: Google Docs isn't available through a single built-in connector the way Confluence and Jira are — the user needs their own Google Workspace MCP connector already configured (a custom connector with their own OAuth client, per Google's official setup guide) before this will work. Ask if they've already got that configured. If not, tell them plainly this destination won't work yet and suggest local file or Confluence instead for now. If yes, ask for the target Google Doc's URL or ID.

**Chat-only**
If the user doesn't want a durable destination for a given artifact type, that's a valid choice — record it as `"destination": "chat"` and move on without pushing further.

## Output

Write the full configuration to `$CONFIG_FILE` in this shape:

```json
{
  "decision": { "destination": "local-file", "path": "/path/to/file.md" },
  "decision_log": { "destination": "jira", "project_key": "OPS", "also": {"destination": "confluence", "space_key": "ARCH", "parent_page": "Decision Log"} },
  "pros_cons": { "destination": "chat" },
  "impact_effort": { "destination": "confluence", "space_key": "ARCH", "parent_page": null },
  "raci": { "destination": "chat" }
}
```

Confirm back to the user in plain language what's configured for each of the five, and mention they can run `/think-like-an-architect:setup` again anytime to change it — this isn't a one-way decision.
