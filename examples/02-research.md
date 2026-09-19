# Example 2 — Two of three assumptions were wrong

**Work:** determine where two different AI tools load user-level skills from on this machine, and whether one skill folder can serve both.
**Why this one:** research fails quietly. Nothing crashes. If the assumptions had been written up as findings, the resulting work would have been installed into a directory nothing reads.

---

## Seal criteria

1. Every path claim traced to something observed on the machine or stated in official documentation
2. The compatibility conclusion tested against actual files, not inferred from tool similarity

## Gathered — with provenance

Nothing from memory. Each fact tied to how it was obtained:

| Question | How it was answered |
|---|---|
| Where are the config directories? | Listed the candidate directories; read the environment variables |
| Which one is actually in use? | Compared modification times of the config files |
| Are the skill formats compatible? | Opened an existing skill in each tool and compared the headers |

## Sources checked — assumptions falling over

| Assumed | Observed | How it surfaced |
|---|---|---|
| Two separate config trees | One is a symlink to the other — the same directory | `ls -la` showed the `->` target |
| One config directory | Two, one of them stale (config months old, main instructions file empty) | Compared file sizes and modification times |
| Formats probably differ, so maintain two copies | Identical format, same header fields | Opened one existing skill from each tool |

Two of three assumptions were wrong, and both wrong ones were comfortable: they sounded like how such tools usually work.

## The one that only documentation could settle

The install location was later challenged in review. The machine showed a plausible-looking skills directory under the tool's own config folder, and that had been treated as the answer.

Checking the official documentation showed the user-level path is a *different* directory, and the one that had been assumed was not listed at all. **The directory existed; that was never evidence it was the supported location.** Existence and support are different claims, and only documentation could settle the second.

## Conclusions, each pointing at a source

| Conclusion | Support |
|---|---|
| Install into directory A, not B | B's config unchanged for months; its main instructions file is zero bytes |
| One skill folder serves both tools | The header fields in both tools' existing skills are identical |
| The user-level path is X | Official documentation lists X; it does not list the directory that was assumed |

Left open rather than filled in:

- **UNKNOWN**: whether one of the tools rescans its skills directory automatically or only at startup — not stated in the documentation available, and untestable from this environment.

---

## Takeaways

1. **Sources first, conclusions second**, then re-read every conclusion asking *which source says this?*
2. **The dangerous state is not ignorance, it is a comfortable assumption.** Both wrong assumptions sounded like standard behaviour.
3. **"It exists on my machine" is not "it is supported."** Local observation and documentation answer different questions.
