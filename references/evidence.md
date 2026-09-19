# Evidence

> Evidence is something another person could re-examine without you.

Every claim of completion carries its evidence. A claim with no evidence is an opinion wearing a uniform.

## Recording format

```
CLAIM:     what you are asserting, scoped to what you actually checked
EVIDENCE:  the command, action, or source
RESULT:    what you actually saw — the real lines, not "success"
LOCATION:  file path, URL, or where the output is stored
WHEN:      timestamp (matters for freshness)
```

Long output goes in a file (`.loopseal/evidence/YYYY-MM-DD_topic.txt`) with the path cited. Screenshots likewise.

## The four tests

Every piece of evidence must pass all four.

### 1. Freshness

**Was this produced after the last relevant change?**

The classic failure: run the tests, find a bug, fix the bug, then report the earlier passing run. Fixing invalidates everything verified before the fix.

- Re-verify after every change, including "trivial" ones.
- When citing older evidence, say when it was produced and why it is still valid.
- If the system changed underneath you (a dependency updated, someone else deployed, the directory was replaced), previous evidence is stale even if you changed nothing.

### 2. Provenance

**Who or what produced this, and how?**

- Name the command, tool, or source. "Tests pass" with no command is not evidence.
- Evidence from another agent, another session, or the user is *reported*, not verified by you. Label it: `reported by <source>, not independently checked`.
- Never present a tool's summary of its own success as an independent check.

### 3. Relevance

**Does this actually support this claim?**

The most common substitution is proving the easy thing instead of the real thing:

| Claim | Irrelevant evidence | Relevant evidence |
|---|---|---|
| The feature works | The file was written | The feature was executed and produced the expected result |
| The config took effect | The file contains the new value | The running process reports the new value |
| The deployment is live | The deploy command exited 0 | The live URL returns the new content |
| The document is correct | Word count and section list | Facts traced to sources; the file opened and inspected |

### 4. Falsifiability

**Could this check have failed if the work were broken?**

A check that passes regardless of correctness is decoration. Before citing it, ask: *if this were broken, would this check have told me?*

- A test that never touches the changed code path proves nothing about it.
- `grep`-ing for a string you just wrote proves the string is there, not that it works.
- A smoke test that only checks HTTP 200 will not catch a page rendering an error message with status 200.

When the only available check is weak, say so: `verified only at the smoke-test level; a broken render inside the page would not have been caught`.

## Not evidence

- "Should work" / "logically correct" / "standard approach"
- "I followed best practice"
- A plan describing what you would do
- A summary of a command you did not run
- Another agent's claim, repeated as your own

## Evidence for things you did not do

Say it plainly. "Not tested" is a complete, acceptable answer. An invented or borrowed result is not.
