# Human Gates

Some loops can only be closed by a person. LoopSeal makes that explicit instead of leaving it to be assumed.

## The red line

**An AI must never create, imply, or assume human approval.**

All of these are violations:

- Recording a human-verified state the person never granted
- "I checked it myself, so that counts"
- Running the user's acceptance steps and logging the result as their acceptance
- Treating silence, or an acknowledgement like "ok", as approval
- Marking the gate in advance "for when they confirm"
- Reporting another agent's claim that the user approved, as if you saw it

An AI's own checks are real verification — they belong to the stage before the gate. They are not the gate.

## Choosing the gate

Decide at the start, not when you want to finish.

| Gate | When | What it means |
|---|---|---|
| `required` | Real users or customers affected; money; external systems; irreversible or hard-to-reverse actions; anything the user asked to approve; anything where you cannot observe the real-world result | Loop stays **open** until the person confirms. No exceptions. |
| `optional` | Reversible, small blast radius, the user is content to be told afterwards | You may seal on evidence, but you must state what a human has not checked |
| `not applicable` | Internal scratch work with no consumer | Seal on evidence alone |

When unsure between `required` and `optional`, ask. It costs one sentence.

## What counts as approval

All three must hold:

1. **The person themselves** said it — not another agent, not a system, not an inference from their silence.
2. **About the result** — not about the plan, the process, or your explanation.
3. **Indicating they saw or used it** — not that they trust you.

| Counts | Does not count |
|---|---|
| "I opened it, the new version is there" | "ok" / "got it" / "thanks" |
| "Ran it, output looks right" | "sounds good" |
| "Customer confirmed they received the email" | "you said it works, so fine" |
| "Read the doc, approved" | (no reply at all) |

Ambiguous? Ask directly: *"Is that you confirming you've seen it, or would you like the steps to check?"*

## Recording it

```
HUMAN GATE: required — cleared 2026-01-15 14:30
Their words: "I opened admin, the new schedule shows up, looks right"
Covered: schedule page renders
Not covered: CSV export (they did not test it)
```

Quote them verbatim. Record what the approval covers — approving A is not approving B.

## Make the gate cheap

The AI's job is to make confirmation take thirty seconds:

```
To confirm:
1. Open <exact URL>
2. Look at the row for 22 Sept
3. It should say "Morning — A. Chen". If it doesn't, tell me and I'll reopen it.
```

Never "please confirm everything works". If the person is not technical, say exactly what to click and what correct looks like.

## When the gate is not cleared

- The loop stays open. Report it as open.
- Do not seal, do not close, do not soften the wording.
- You may ask whether they want to accept it without verification — that is their decision to make, and their decision must be recorded as such.

## Revocation

If the person later says it is wrong, the approval is void and the loop reopens at the failing stage. Evidence gathered before the fix is stale (see `evidence.md`).
