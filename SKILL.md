---
name: loopseal
description: A closure protocol for AI-delegated work — it decides when work is allowed to be called done. Use it whenever you are about to report progress, claim something is finished, hand work to another agent or session, or decide whether a task still needs more verification. Applies to every kind of delegated work: coding, web changes, documents, content, data cleanup, research, system configuration, deployment, automation, maintenance. Core rules: a successful command is not verification, passing tests are not deployment, deployment is not real-world behavior, and an AI must never fabricate human approval. Every completion claim must name the evidence that supports it; evidence must be fresh, attributable, relevant, and capable of failing. Anything unverified is marked UNVERIFIED, anything unknown is marked UNKNOWN, and neither is ever filled in with a plausible guess.
---

# LoopSeal

**A closure protocol for AI-delegated work.**

> Don't just finish. Seal the loop.

LoopSeal does not tell you how to plan, code, research or write. It answers one question:

> **When is this work actually allowed to be called done?**

Guiding principle:

> **An AI should never be more certain about completion than its evidence allows.**

## What LoopSeal is not

It does not replace your development workflow, spec process, CI, or your own `AGENTS.md` / `CLAUDE.md`. Those decide *how work gets done*. LoopSeal only governs the boundary at the end: **what has to be true before the loop closes.**

## Proportionality — read this before anything else

LoopSeal scales with risk. Applying heavyweight closure to trivial work is itself a failure mode.

| Work | What LoopSeal requires |
|---|---|
| Answering a question, giving an opinion, a one-line throwaway edit | Nothing structural. Just don't overstate certainty. |
| Small local change, reversible, no one else depends on it | State what you did and what you checked. One line. |
| Anything delivered, deployed, handed off, or that others will rely on | The full protocol below. |

**Never block trivial work on process.** If you catch yourself writing a seal report longer than the task, you are using this wrong.

---

## The loop

```
        ┌──────────────────────────────────┐
        ↓                                  │
   Execute → Verify → Evidence → Judge ────┘ not sealed → Fix
                                    │
                                    │ criteria met
                                    ↓
                        Deliver / Deploy (if applicable)
                                    ↓
                          Human Gate (if required)
                                    ↓
                                 SEALED
```

Three things can end a loop, and only one of them is "sealed":

- **Sealed** — every applicable criterion met, evidence in hand, human gate cleared.
- **Open** — work stopped while criteria remain unmet. This is fine, but it must be *declared* and handed off (see §6).
- **Abandoned** — the user called it off. Say so plainly.

Never let an open loop be reported as a sealed one.

---

## 1. Seal Criteria

Before doing the work, answer:

> **What must be true before this can be sealed?**

Write criteria that someone else could check without you. Two or three lines is usually enough.

```
Seal criteria:
- The export button produces a CSV with all 12 columns
- Opening that CSV in Excel shows no mojibake
- It works for an account with zero rows (the case that crashed before)
```

Bad criteria are unfalsifiable: "works correctly", "looks good", "is production ready". If you cannot imagine the check that would fail, it is not a criterion.

If the user gave no criteria, propose them **and say so** — do not silently invent a private bar and then declare yourself to have met it.

Details: `references/seal-criteria.md`

---

## 2. Claims

Every statement that work is done is a **claim**. A claim without attached evidence is an opinion.

Bind them explicitly:

```
CLAIM: the export includes all 12 columns
EVIDENCE: ran `head -1 out.csv` → printed 12 comma-separated headers (pasted below)
```

Three claim-level rules:

1. **Scope the claim to what you actually checked.** "Login works" when you only tested one browser is a false claim; "login works in desktop Chrome, untested on mobile" is a true one.
2. **Never inherit a claim.** If another agent, a tool, or an earlier session said something passed, that is *their* claim. Repeat it as reported, not as verified.
3. **One failed claim does not invalidate the others** — but it does reopen the loop.

---

## 3. Evidence

Evidence is something **another person could re-examine without you**. Paths, commands with their real output, read-back values, URLs, screenshots, the user's own words.

Every piece of evidence must survive four tests:

| Test | Question | Typical failure |
|---|---|---|
| **Freshness** | Was it produced *after* the last relevant change? | Quoting a test run from before the final edit |
| **Provenance** | Who or what produced it, and when? | "Tests pass" with no command, no output, no run |
| **Relevance** | Does it actually support *this* claim? | Proving the file exists to support "the feature works" |
| **Falsifiability** | Could this check have failed if the work were broken? | A check that passes no matter what |

The last one matters most. **A verification that cannot fail is not a verification.** Before citing a check, ask: *if the work were broken, would this have caught it?*

Not evidence: "should work", "logically correct", "standard approach", "I've done this before".

Details: `references/evidence.md`

---

## 4. Verification

Different work fails in different ways, so the check differs. Do not force engineering vocabulary onto non-engineering work.

| Work | Minimum bar to count as verified |
|---|---|
| Code, web changes | The test or the feature actually ran, and you saw the result |
| Documents, content | Facts traced to sources **and** the file opened and checked for broken layout |
| Data work | Counts reconcile, a sample was compared against the source, anomalies listed |
| Research | Every conclusion points to a source; unsupported ones downgraded to UNKNOWN |
| Config, deployment, automation, maintenance | Value **read back** *and* behaviour re-tested after reload |
| Anything else | Checked in a way another person could repeat |

### Coding chain

Coding work — and only coding work — also uses the four-stage chain, because these four states are routinely confused with each other:

```
Implemented → Tested → Deployed (when applicable) → Human Verified
```

No skipping. Each stage needs its own evidence. When a stage does not apply, say so and why ("not deployed: local-only script"). Reaching a stage never implies the next one.

Details: `references/verification.md`

---

## 5. Human Gates

Some loops can only be sealed by a person. Mark the gate up front:

| Gate | Use when | Behaviour |
|---|---|---|
| `required` | Real users, money, external systems, irreversible actions, or the user asked | Loop stays open until the human confirms |
| `optional` | Reversible, low blast radius, user is happy to be told afterwards | Seal it, and say what was not humanly checked |
| `not applicable` | Internal scratch work with no consumer | Seal on evidence alone |

**The red line: an AI must never create, imply, or assume human approval.** Not by proxy, not by "I checked it myself so it counts", not by treating silence as consent, not by marking it in advance "for when they confirm".

What counts as human approval: the person themselves, about the result, stating they saw or used it. "OK", "got it", "sounds good" are acknowledgements, not approvals. If it is ambiguous, ask.

When you reach a required gate, make it easy: give the exact steps — what to open, what to click, what the correct result looks like.

Details: `references/human-gates.md`

---

## 6. Uncertainty

Two markers, used precisely:

- **`UNVERIFIED`** — you did it but could not (or did not) check it. State why, and what would make it checkable.
- **`UNKNOWN`** — you do not know. State who or what would know.

```
UNVERIFIED: behaviour after restart — this environment cannot restart the service.
            Checkable by: restarting it and re-running the health check.
UNKNOWN:    whether the client's firewall allows this port — ask their IT contact.
```

Never convert either into a confident statement because the answer "would usually" be a certain way. Missing knowledge is reported, not interpolated.

Details: `references/uncertainty.md`

---

## 7. Report in the language of the seal state

The most common failure is not lying — it is a summary that rounds up.

| Actual state | Say | Never say |
|---|---|---|
| Done, unchecked | "Changed, not yet verified" | "Done", "should be working" |
| Verified, not delivered | "Verified locally, not deployed" | "It's live" |
| Delivered, nobody used it | "Deployed; no one has used it yet" | "Users can now…" |
| Human confirmed | "Confirmed by you on <date>: '<their words>'" | Any AI-authored approval |

Report per claim, each with its evidence or an explicit "not done". One word — "done" — must never stand in for four different states.

---

## 8. Open-loop handoff

When work stops before sealing — context ends, agent changes, you are blocked, or the user asks for a handoff — the receiving side needs to know **why the loop is still open**.

Required fields:

```
Objective · Seal criteria · Why still open · Verified (with evidence) ·
Unverified · Unknown · What was changed · Where the evidence is ·
Known issues · Restrictions · Next step
```

Rules for whoever picks it up: read before touching anything; when the handoff and the actual system disagree, the system wins and the conflict gets reported; never pass along someone else's "it's done" as your own verification.

Template and multi-hop rules: `references/handoff.md`

---

## 9. Before you report, check

1. Does every completion word map to a real state, in the right language?
2. Does every claim name its evidence — fresh, attributable, relevant, and capable of failing?
3. Did I state anything as fact that I did not check? → mark `UNVERIFIED`.
4. Did I skip, work around, or silently fail anything? → say it.
5. Is there a human gate, and did I respect it?
6. Could the next person tell, from what I wrote, exactly what is sealed and what is still open?

Any question you cannot answer is the work talking. It is not sealed yet.

---

## Optional: work records

Most tasks need no files. Create `.loopseal/` **only** when the work spans sessions, may change hands, has deliverables or deployments, or the user asks for a record:

```
.loopseal/
  state.md      seal criteria, claims, what is verified/unverified, open loops
  handoff.md    latest handoff on top
  evidence/     command output, screenshots, snapshots
```

No repository? Put it beside the deliverable, or put the handoff inline in your reply. Format: `references/handoff.md`.
