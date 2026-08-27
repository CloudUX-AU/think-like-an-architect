# Think like an Architect

Five guided decision-making tools for Claude Code, built from the book *Think like an Architect — A Compass for Salesforce Admins* by Carl Vescovi. Written for Salesforce Admins who are routinely asked to make architect-level decisions without architect training — this plugin puts the book's core toolkit directly into your terminal, working on your own real decisions instead of a hypothetical example.

## Install

```
/plugin marketplace add CloudUX-AU/think-like-an-architect
/plugin install think-like-an-architect
```

## Commands

- **`/think-like-an-architect:decision`** — The 5-Step Decision Method. Context, Constraints, Options, Decision, Implications. Use it for anything that affects multiple people, is hard to reverse, or will be in place six months or longer.
- **`/think-like-an-architect:decision-log`** — Turns a decision into a permanent record: what was decided, why, and what alternatives were considered, so nobody has to reverse-engineer the reasoning later.
- **`/think-like-an-architect:pros-cons`** — A weighted Pros and Cons Grid for comparing multiple real options, where some factors matter more than others.
- **`/think-like-an-architect:impact-effort`** — Sorts a backlog into Quick Wins, Big Projects, Low Priority, and Time Wasters, so you know what to actually work on next.
- **`/think-like-an-architect:raci`** — Builds a RACI matrix for a multi-person project and enforces the one-rule that keeps decisions from stalling: exactly one Accountable person per decision.
- **`/think-like-an-architect:setup`** — Optional. Configure where each of the five artifacts above gets saved: a local file, Confluence, Jira (Decision Log only), or Google Docs. Run it once, or skip it entirely and everything just prints to chat.

Run any command and answer the questions it asks — each one produces a real, usable document at the end, not just an explanation.

## Cross-Checking Against Your Decision Log

If you've got a Decision Log register configured (see below), the other five tools check it before proceeding — not just a keyword match, an actual read for anything related by topic. If you're building a RACI matrix and someone was Accountable on a closely related decision but isn't in this one's people list, it says so. If you're comparing options in a Pros and Cons Grid and one of them was already ruled out in a logged decision, it tells you before you re-litigate it. If a new Decision Log entry looks like it overlaps with one already on record, it asks whether this is genuinely new, an update, or a deliberate reversal — rather than quietly creating a second, possibly contradictory entry. No register configured, or nothing relevant found — it stays silent about the check; you won't get told "nothing to report" every time.

## Saving Output Automatically (Optional)

By default, every command just prints its output to chat — copy-paste it wherever you like. Run `/think-like-an-architect:setup` if you'd rather it landed somewhere durable automatically:

- **Local file** — always available, no setup beyond a path.
- **Confluence** — built in. The plugin bundles Atlassian's official remote MCP server; the first time a skill writes to Confluence, you'll be prompted to authorise via OAuth against your own Atlassian site.
- **Jira** — same built-in Atlassian connector, available as a Decision Log destination (each decision becomes a tracked issue).

**Decision Log specifically** maintains two things, not one: a central **register** (one table listing every decision — ID, date, summary, review date, and a link) plus a **separate linked page per decision** with the full entry. On local file, that's a register file plus one file per decision in a folder; on Confluence, a register page plus one child page per decision; on Jira, the project's issue list already is the register and each issue already is the page, so no separate table gets built there.
- **Google Docs** — needs more setup on your end than the others: there's no single hosted connector the way Confluence/Jira has, so you'll need your own [Google Workspace MCP connector](https://developers.google.com/workspace/guides/configure-mcp-servers) configured first (requires a Claude Enterprise/Pro/Max/Team plan and your own OAuth client). `/setup` will tell you plainly if this isn't ready yet rather than pretending it works.

## About the Book

*Think like an Architect* is written by Carl Vescovi, a Salesforce Architect with over twenty years across administration, development, and architecture. The book argues that Trailhead and certifications teach features, not structured thinking — this plugin is the same method, made interactive.

Want a second pair of eyes on a real decision, or an outside architect's view of your Salesforce org? [cloudux.com.au](https://cloudux.com.au)

## License

MIT
