# Example 3 — The value read back. The behaviour only half-verified.

**Work:** install a skill into two AI tools' skills directories.
**Why this one:** configuration work has two layers, and the second one is where it usually fails. Here layer one passed for both tools and layer two passed for only one — reported as exactly that, rather than as "installed".

---

## Seal criteria

1. The files exist in both locations and match the source
2. Each tool actually loads the skill (not merely: the files are present)

Criterion 2 is deliberately separate. Files being in the right place is not the tool using them.

## Changed

Cloned the repository into each tool's skills directory. No system settings, no PATH, no services. Removal is deleting a folder.

## Config layer — verified

Not "the clone command exited 0", but read back:

```
both directories present, 17 files each
first two lines of the skill file read back and correct
sha256 of the skill file: identical across source and both installs
```

✔ Layer one holds: the right bytes are in the right places.

Note the **relevance** trap avoided here: this evidence supports "the files are installed correctly". It does not support "the tool uses them". Those are different claims and need different evidence.

## Runtime layer — one pass, one unverifiable

| Tool | Result | Evidence |
|---|---|---|
| A | ✔ verified | The skill appeared in the running session's available list; invoking it loaded the content and reported its base directory as the newly installed folder |
| B | ⚠ UNVERIFIED | Could not be launched from this environment, then later hit its own usage limit |

For B, the marker carries its escape route:

```
UNVERIFIED: whether tool B loads the skill
  Why not: cannot launch tool B from this environment
  Checkable by: opening a tool B session and asking whether the skill is available
```

## A later re-verification that mattered

The installed folders were subsequently replaced (reinstalled from a different source). The earlier "tool A loads it" evidence was then **stale** — it described a directory that no longer existed. It was re-verified against the new install rather than carried forward.

## Human gate: optional

Reversible, no external system, no users affected. Sealed on evidence, with the unverified item stated. Had this been a production service, the gate would have been `required`.

---

## Takeaways

1. **Config work is two layers.** Value read back, then behaviour re-triggered after reload. Stopping at layer one is the most common false completion in this category.
2. **"The command didn't error" is not evidence.** The read-back value is.
3. **When one environment verifies and another cannot, say so per environment.** "Installed" would have concealed that half of it was unchecked.
4. **Replacing the thing you verified voids the verification**, even when the contents look the same.
