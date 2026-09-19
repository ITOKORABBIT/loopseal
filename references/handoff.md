# Open-loop handoff

A handoff exists because a loop is **still open**. The single most important thing the next person needs is not what you did — it is:

> **Why is this loop still open?**

## When to write one

Context is ending · agent or session changes · you are blocked · the user asks for a handoff or a record · work is going to someone for review.

## Format

```markdown
# Handoff: <task>

- When / who: 2026-01-15 14:30, <agent>
- Seal state: OPEN
- **Why still open:** <the actual blocker or missing criterion, in one sentence>

## Objective
<what it is for, and the seal criteria>

## Verified
- <claim> — evidence: <command/action → real result> — <where>

## Unverified
- UNVERIFIED: <item> — why not: <reason> — checkable by: <method>

## Unknown
- UNKNOWN: <item> — who/what would know: <source>

## What was changed
- <path or system>: <what changed>
- includes anything created but unused, and anything temporarily altered that must be restored

## Where the evidence is
- <paths, URLs, commits, output files>

## Known issues
- <bugs, workarounds taken, debt created>

## Restrictions
- <do not touch X · do not re-run Y · approaches the user already rejected>

## Next step
1. <the first concrete action, specific enough to execute>
```

`Verified` and `Unverified` are separate sections for a reason: the receiver may build on the first and must re-check the second. Never merge them into "done".

## Rules for the receiver

1. **Read it before touching anything.** Skipping this is the main source of duplicated work.
2. **Reconcile with reality.** Where the handoff and the actual system disagree, the system wins — and the disagreement gets reported, not silently absorbed.
3. **Do not re-verify what is listed as verified** (unless something changed since), and **do not trust what is listed as unverified**.
4. **State what you checked.** Your first report should say which parts of the handoff you confirmed and where it was wrong.

## Multi-hop handoffs

When work passes through several hands (agent → agent → reviewer):

- Keep the previous handoff; add yours on top.
- Never forward "the previous agent says it's done" as your own verification. Either check it, or label it: `reported upstream, not independently verified`.
- Each hop that loses the "why still open" line makes the next hop guess.

## Optional persistent record

For work spanning sessions, keep it beside the work:

```
.loopseal/
  state.md      seal criteria, verified / unverified, open loops
  handoff.md    latest handoff on top, older ones below
  evidence/     command output, screenshots, snapshots
```

Keep it human-readable Markdown. Update it when state changes, not at the end. Never delete history — a closed loop that reopens is exactly when the old record matters.

Do not create this for small single-session work, and do not put secrets, tokens or personal data in it — especially in a repository that might become public.
