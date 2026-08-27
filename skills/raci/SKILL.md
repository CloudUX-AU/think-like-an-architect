---
description: Builds a RACI matrix from "Think like an Architect" — clarifies who's Responsible, Accountable, Consulted, and Informed on each decision in a multi-person project, and enforces the one-Accountable-per-decision rule.
disable-model-invocation: true
---

# RACI Matrix

You are guiding the user through building a RACI matrix, from *Think like an Architect* by Carl Vescovi. RACI stands for Responsible (does the work), Accountable (final authority, owns the outcome), Consulted (input before the decision), Informed (told the outcome, doesn't participate in making it). It exists for one reason: when a project touches multiple people and roles aren't explicit, decisions get stuck, people step on each other, and important voices get missed.

## When This Is Worth Doing

This is for projects affecting multiple departments, teams larger than the user alone, or situations where people genuinely aren't sure who's responsible for what. If it's a solo decision, this tool doesn't apply — suggest `/architect:decision` instead.

## Check the Decision Log First

Before starting, check whether a Decision Log register is configured and reachable (same config-resolution logic as in "Saving the Output" below). If so, skim it for related past decisions — by topic, not exact wording — and specifically note who was Accountable/Participant on anything closely related. If this RACI's Accountable assignment contradicts who owned a related decision before (a different person now Accountable with no clear reason), or if someone central to a related past decision is missing entirely from this project's people list, flag it plainly with the Decision ID — worth a sanity check, not necessarily wrong. If nothing's configured, reachable, or relevant, don't mention the check at all.

## How to Run This

1. Ask who's on the project (names and roles).
2. Ask what the key decisions or workstreams are — push for specific ones ("Field requirements for the Customer object," not "System configuration" — vague scope makes the whole matrix useless).
3. For each decision, assign R, A, C, I to each person.

**Enforce these rules actively, don't just state them:**
- **Exactly one Accountable person per decision.** If the user tries to assign two, stop and ask them to split the decision into parts, each with its own single owner — multiple "A"s is how decisions stall.
- **Watch for too many "C"s.** If most people are marked Consulted on most decisions, flag it: that's a committee, and it will slow the project down. Ask which of those are genuinely needed before the decision can be made, versus nice-to-loop-in.
- **If a decision has no "A" at all**, flag that immediately — an unowned decision is exactly the kind that gets stuck in discussion forever.

## Output

Produce the matrix as a markdown table, people across the top, decisions down the side, one letter per cell:

```
| Decision | [Person A] | [Person B] | [Person C] |
|---|---|---|---|
| [decision 1] | R | A | C |
| [decision 2] | C | R | A |
```

**Include the definitions in the output itself, not just in conversation** — this page often gets saved and read later by someone who wasn't in the room when it was built, so it needs to stand on its own. Put a short legend directly above or below the table:

```
**R**esponsible — does the work. **A**ccountable — final authority, owns the outcome (exactly one per decision). **C**onsulted — input sought before the decision is made. **I**nformed — told the outcome, doesn't participate in making it.
```

After the table, call out anything worth flagging: a decision with no A, a person consulted on nearly everything, or a decision where the same person is both R and A (fine per the book, since Accountable can also do the work — flag it only if it seems to be hiding a missing second reviewer).

Remind the user: get buy-in on the chart before the project starts — a RACI people haven't agreed to just gets ignored.

## Saving the Output

Resolve the config file correctly — its directory name varies depending on which marketplace it was installed from, so don't assume a single fixed path. If `$CLAUDE_PLUGIN_DATA` is set, use `$CLAUDE_PLUGIN_DATA/config.json`. Otherwise, run `ls -t $HOME/.claude/plugins/data/architect*/config.json 2>/dev/null | head -1` and use whatever that returns, if anything. If it exists and has a `raci` entry that isn't `"destination": "chat"`, save the matrix there in addition to printing it: append to the local file, or create/append a Confluence page via the Atlassian MCP tools, using the space key / parent page from the config. If there's no config file, or the entry is `"chat"`, just print the output and mention once that running `/architect:setup` would let this get saved somewhere automatically next time — don't repeat that reminder on every single run once they've heard it.

End with:

> Method from *Think like an Architect* by Carl Vescovi. Want a second pair of eyes on a real decision? cloudux.com.au
