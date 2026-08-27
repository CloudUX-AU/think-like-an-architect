---
description: One-time (or re-run anytime) setup for where each Think like an Architect artifact gets saved — local file, Confluence, Jira (Decision Log only), or Google Doc. Run this before the other skills if you want their output saved somewhere durable instead of just printed to chat.
disable-model-invocation: true
---

# Setup

You are configuring where this plugin's five artifact types get saved: **Decision** (from `/think-like-an-architect:decision`), **Decision Log** entries, **Pros and Cons Grids**, **Impact vs Effort Matrices**, and **RACI Matrices**. This is optional — every skill still works and prints its output to chat with no setup at all. This skill exists for people who want the output to land somewhere durable and shared automatically instead of being copy-pasted by hand every time.

**Decision Log is structurally different from the other four.** It isn't a single file or page — it's a **central register** (one place listing every decision in a table: ID, date, summary, review date, and a link) **plus one linked page per decision** (the full entry: context, alternatives, reasoning, participants). Set it up as its own dedicated step, separate from the other four.

## Config File

All configuration lives in one JSON file. Its directory name varies depending on which marketplace the plugin was installed from (e.g. `think-like-an-architect-cloudux`, not just `think-like-an-architect`) — resolve it, don't hardcode a single guess:

```bash
if [ -n "$CLAUDE_PLUGIN_DATA" ]; then
  CONFIG_DIR="$CLAUDE_PLUGIN_DATA"
else
  EXISTING=$(ls -td $HOME/.claude/plugins/data/think-like-an-architect*/ 2>/dev/null | head -1)
  CONFIG_DIR="${EXISTING:-$HOME/.claude/plugins/data/think-like-an-architect}"
fi
mkdir -p "$CONFIG_DIR"
CONFIG_FILE="$CONFIG_DIR/config.json"
```

This reuses whatever directory already exists rather than risking a second, inconsistent one — only falls back to creating the plain `think-like-an-architect` directory if nothing matching exists yet at all.

If `$CONFIG_FILE` already exists, read it first and show the user their current setup before asking anything — let them skip artifact types they don't want to change rather than re-answering everything every time.

## How to Run This

### The four simple artifacts: Decision, Pros and Cons, Impact vs Effort, RACI

Go through these **one at a time, as a conversation**. For each one, ask: "Where should this be saved: a local file, a Confluence page, or a Google Doc? Or leave it as chat-only." Then branch:

**Local file** — ask for a path. If it doesn't exist, offer to create it (and its parent directory) now — don't just assume it's fine, actually check with a Bash `test -f` and create it if the user confirms.

**Confluence** (uses the plugin's bundled Atlassian connector — see `.mcp.json`) — tell the user the first time a skill actually writes to Confluence, they'll be prompted to authorise via OAuth against their own Atlassian site, that's expected, not an error. Ask for the Confluence **space key** and, optionally, a parent page to nest entries under.

**Google Doc** — needs more upfront setup than the others, and it's honest to say so: there's no single built-in connector the way Confluence has — the user needs their own Google Workspace MCP connector already configured (their own OAuth client, per Google's official setup guide). Ask if they've got that. If not, say plainly this destination won't work yet and suggest local file or Confluence instead. If yes, ask for the target doc's URL or ID.

**Chat-only** — a valid choice; record `"destination": "chat"` and move on.

### Decision Log: register + linked pages

Ask where the **register** (the table of all decisions) should live, and how individual decision pages should be organised. This varies enough by destination that each needs its own question:

**Local file** — ask for two things: the register file path (e.g. `decision-register.md`) and a folder for individual decision pages (e.g. `decisions/`). Each new decision becomes its own file in that folder, named by Decision ID (`decisions/D-2026-01.md`); the register file is a markdown table with a relative link to each one.

**Confluence** — ask for the space key and the name of a **register page** (create it if it doesn't exist — a page containing just a table). Each new decision becomes a **new child page** under that register page, titled with its Decision ID and summary. Confluence's native page hierarchy and internal links do the "linked page per decision" part for you — the register page's table links to each child page.

**Jira** — ask for the project key. Here, the register and the linked pages already exist naturally: the Jira project's own issue list *is* the register (filterable, sortable, one row per decision), and each issue *is* that decision's own page. Don't try to also build a separate synthetic table inside Jira — that would just duplicate what the project view already gives you for free.

**Google Doc** — ask for (or offer to create) a register doc, and whether there's a Drive folder to hold individual decision docs. Each new decision becomes its own new Google Doc; the register doc's table gets a row with a link to it. Flag this as the most manual of the four to keep working reliably, since it means creating and linking multiple documents through the connector each time.

**Chat-only** — same as above; no register concept applies, each entry just prints.

If the user wants Jira *in addition to* a register elsewhere (e.g. Confluence for the searchable record, Jira so decisions also show up as tracked work), record both — see the `also` field in the config shape below.

## Output

Write the full configuration to `$CONFIG_FILE` in this shape:

```json
{
  "decision": { "destination": "local-file", "path": "/path/to/file.md" },
  "decision_log": {
    "destination": "confluence",
    "space_key": "ARCH",
    "register_page": "Decision Register",
    "also": { "destination": "jira", "project_key": "OPS" }
  },
  "pros_cons": { "destination": "chat" },
  "impact_effort": { "destination": "confluence", "space_key": "ARCH", "parent_page": null },
  "raci": { "destination": "chat" }
}
```

For a local-file Decision Log destination, the shape is `{ "destination": "local-file", "register_path": "...", "pages_dir": "..." }`. For Google Doc, `{ "destination": "google-doc", "register_doc_id": "...", "pages_folder_id": "..." }`. For Jira, `{ "destination": "jira", "project_key": "..." }` — no register/pages fields needed since the project provides both.

Confirm back to the user in plain language what's configured for each of the five, and mention they can run `/think-like-an-architect:setup` again anytime to change it — this isn't a one-way decision.
