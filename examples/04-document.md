# Example 4 — It read beautifully. Five of its links were dead.

**Work:** write the README and skill documentation for this repository.
**Why this one:** document work is where "I reread it and it's good" masquerades as verification. The prose was fine. The file was broken.

---

## Seal criteria

1. Every item in the brief appears in the document
2. Every factual claim traceable to something observed
3. Every internal link resolves to a file that exists

## Pass 1 — Content

The brief listed four required topics. Mapped each to a section rather than trusting a reread:

| Required | Where it landed |
|---|---|
| Closure principles | `SKILL.md` §1–2 |
| How verification differs by work type | `SKILL.md` §4 + `references/verification.md` |
| How to record evidence | `SKILL.md` §3 + `references/evidence.md` |
| How handoff works | `SKILL.md` §8 + `references/handoff.md` |

This pass caught a **design** error, not a typo: the first draft gave every work type its own named state chain. That contradicted the brief, which asked for engineering vocabulary to stay confined to coding. Rewritten as "what counts as verified per work type"; the draft moved to `archive/`.

## Pass 2 — Facts

Every path, directory layout and header field in the document came from something actually inspected on the machine or from official documentation — not from how such tools usually work. One claim that could not be sourced was cut rather than softened.

## Pass 3 — Format, done by machine

Checked every internal link against the filesystem:

```
README's 5 example links   → ✘ all five missing (not yet written)
SKILL.md's reference links → ✔ all present
```

**This is the whole point of a format pass.** The README was complete, well-organised, and pleasant to read — with five dead links. No amount of rereading finds that; only a mechanical check does.

After writing the missing files, the same check was re-run and passed. (Re-running matters: the first result was about a previous state of the repository.)

## The check's own limits, stated

The link checker also flags filenames that appear as illustrations inside prose — e.g. a sample path in an example block — as missing files. Those are false positives and need a human glance.

Saying so is part of the evidence. **A verification whose blind spots are undocumented invites over-trust in it.**

## Human gate: required

Someone else's brief, so someone else decides whether it is met. Delivered as open, awaiting their read.

---

## Takeaways

1. **Document verification is three different passes** — content, facts, format. They catch different things and cannot substitute for each other.
2. **Format is checked mechanically.** Prose quality and structural integrity are unrelated properties.
3. **Document what your check cannot catch.** A check presented as complete when it is partial is worse than no check.
