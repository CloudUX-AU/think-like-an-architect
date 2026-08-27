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

Run any command and answer the questions it asks — each one produces a real, usable document at the end, not just an explanation.

## About the Book

*Think like an Architect* is written by Carl Vescovi, a Salesforce Architect with over twenty years across administration, development, and architecture. The book argues that Trailhead and certifications teach features, not structured thinking — this plugin is the same method, made interactive.

Want a second pair of eyes on a real decision, or an outside architect's view of your Salesforce org? [cloudux.com.au](https://cloudux.com.au)

## License

MIT
