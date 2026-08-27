---
description: Creates a Decision Log entry from "Think like an Architect" — a structured record of what was decided, why, and what alternatives were considered, so the reasoning survives staff turnover and doesn't get re-litigated later.
disable-model-invocation: true
---

# Decision Log

You are helping the user create a Decision Log entry, from *Think like an Architect* by Carl Vescovi. A decision log is organisational memory: a record of an important design choice that captures not just what was decided, but why, and what alternatives were considered — so a future admin (or future you) never has to reverse-engineer the reasoning from the configuration alone, or accidentally re-litigate a decision that was already made carefully.

## When This Is Worth Doing

Decision logs are for choices that would be difficult to change later, decisions involving multiple people, or times the user did something different from the standard/obvious pattern. Not every minor adjustment needs one. If the user's decision doesn't sound like it clears that bar, say so plainly and ask if they still want an entry — don't silently create paperwork for a trivial choice.

## How to Run This

If the user has just come from `/think-like-an-architect:decision` and pastes in that output, use it directly — don't re-ask what's already there. Otherwise, ask for the decision conversationally, gathering:

- **Decision ID** — help them pick one if they don't have a convention yet (e.g. `D-YYYY-NN`)
- **Date**
- **Summary** — one line: what was decided
- **Context** — what led to this decision
- **Alternatives Considered** — even ones ruled out quickly; this is what stops someone re-exploring a dead end later
- **Decision Reasoning** — why this approach over the others, specifically
- **Participants** — who was involved
- **Review Date** — when to check whether this still holds

## Output

Produce the entry in this exact shape (this is the book's own format, chapter 3):

```
Decision ID: [id]
Date: [date]
Summary: [one line]
Context: [what led to this]
Alternatives Considered: [numbered list, including ones ruled out quickly]
Decision Reasoning: [why this approach won]
Participants: [names/roles]
Review Date: [date]
```

Then remind the user, briefly: put this somewhere the whole team can find it — a shared doc, a wiki, wherever they'll actually look later — a decision log nobody can find is just a file. Suggest starting with only their most significant decisions rather than trying to log everything at once; consistency on a few matters more than a burst of effort that gets abandoned.

## Saving the Output

Check for `${CLAUDE_PLUGIN_DATA:-$HOME/.claude/plugins/data/think-like-an-architect}/config.json`. If it exists and has a `decision_log` entry that isn't `"destination": "chat"`, save the entry there in addition to printing it:
- `local-file` — append to the configured path.
- `confluence` — create/append a Confluence page via the Atlassian MCP tools, using the configured space key and parent page.
- `jira` — create a new Jira issue via the Atlassian MCP tools in the configured project, with the Decision Log's Summary as the issue title and the full entry as the description. If the config also has an `also` destination (e.g. Confluence alongside Jira), save to both — Jira is "raise a tracked issue for this," not necessarily a replacement for the log itself.

If there's no config file, or the entry is `"chat"`, just print the output and mention once that running `/think-like-an-architect:setup` would let this get saved somewhere automatically next time — don't repeat that reminder on every single run once they've heard it.

End with:

> Method from *Think like an Architect* by Carl Vescovi. Want a second pair of eyes on a real decision? cloudux.com.au
