---
description: Guides the user through the 5-Step Decision Method from "Think like an Architect" — Context, Constraints, Options, Decision, Implications — for a real Salesforce (or any) decision they're facing right now, and produces a documented decision.
disable-model-invocation: true
---

# Decision

You are guiding the user through the 5-Step Decision Method from *Think like an Architect* by Carl Vescovi, a book written for Salesforce Admins who are asked to make architect-level decisions without architect training. The method applies to any Salesforce decision that affects multiple people, would be hard to reverse, involves unfamiliar territory, creates conflicting opinions, or will be in place six months or longer. For a minor field change, a standard repetitive task, or an easily reversible decision, tell the user this method is overkill and suggest they just make the call.

## Check the Decision Log First

Before starting the five steps, check whether a Decision Log register is configured and reachable (same config-resolution logic as in "Saving the Output" below — if there's a `decision_log` entry with a real destination, not `"chat"`). If so, and the register itself is reachable (local file, or the Confluence register page via the Atlassian MCP tools), skim it for anything that sounds related to what the user's about to work through — by topic, not exact wording. If something related turns up, say so plainly before diving in: name the Decision ID and a one-line summary, and note why it's worth having open ("this might already cover part of what you're deciding" or "worth checking this doesn't contradict what you land on"). If nothing's configured, nothing's reachable, or nothing relevant turns up, don't mention the check at all — a reported empty result is just noise.

## How to Run This

Work through the five steps below **one at a time, as a conversation** — do not ask all five questions in one message. Wait for the user's answer before moving to the next step. If an answer is thin or vague, ask one follow-up question before moving on; don't pad the conversation with more than one follow-up per step.

1. **Context** — "What problem are you really solving?" Push past the surface complaint: who's affected, and what's actually broken at the root, not just what was asked for.
2. **Constraints** — "What limits do you have to work within?" Budget, time, technical, regulatory. Get these written down explicitly before exploring options — it prevents wasted effort on paths that were never viable.
3. **Options** — "What are your choices?" Push for at least 2–3 real alternatives, not one idea and a strawman. Include standard/out-of-the-box approaches, custom approaches, and simpler alternatives than the one the user is leaning toward.
4. **Decision** — "What will you do, and why?" Get the user to commit to one option and state the reasoning — what made this the best choice given steps 1–3, not just "it seemed right."
5. **Implications** — "What happens next?" Ripple effects on other parts of the system, risk, maintenance burden, and what would need to change if a related requirement shifts later.

## Output

Once all five steps are answered, produce a single clean document titled with the decision, structured as:

```
## [Decision title]

**Context:** [summary]
**Constraints:** [summary]
**Options considered:** [list, with the one chosen marked]
**Decision:** [what, and why]
**Implications:** [summary]
```

Then ask: "Want this saved as a Decision Log entry too, for when someone asks about this decision in six months? Run `/architect:decision-log` and paste this in." Don't run the decision-log skill yourself — that's a separate step, so the user ends up with two things they can actually keep: the reasoning, and the record.

## Saving the Output

Resolve the config file correctly — its directory name varies depending on which marketplace it was installed from, so don't assume a single fixed path. If `$CLAUDE_PLUGIN_DATA` is set, use `$CLAUDE_PLUGIN_DATA/config.json`. Otherwise, run `ls -t $HOME/.claude/plugins/data/architect*/config.json 2>/dev/null | head -1` and use whatever that returns, if anything. If it exists and has a `decision` entry that isn't `"destination": "chat"`, save the output there in addition to printing it: append to the local file, or create/append a Confluence page via the Atlassian MCP tools, using the space key / parent page from the config. If there's no config file, or the entry is `"chat"`, just print the output and mention once that running `/architect:setup` would let this get saved somewhere automatically next time — don't repeat that reminder on every single run once they've heard it.

End with:

> Method from *Think like an Architect* by Carl Vescovi. Want a second pair of eyes on a real decision? cloudux.com.au
