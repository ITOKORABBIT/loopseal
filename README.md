# LoopSeal

**A closure protocol for AI-delegated work.**

> Don't just finish. Seal the loop.

LoopSeal is a skill that decides **when work is allowed to be called done**. It does not tell an agent how to plan, code, research or write — it governs the boundary at the end of the work, where "finished" is claimed.

Guiding principle:

> **An AI should never be more certain about completion than its evidence allows.**

---

## Why the name

**Loop** — delegated work is rarely a straight line. It runs `Execute → Verify → Fix → Verify again → Deliver`, sometimes several times.

**Seal** — a loop may only be closed when every applicable completion criterion is backed by evidence and any required human confirmation has actually happened.

Which means:

```
a successful command   ≠ sealed
passing tests          ≠ sealed
deployed               ≠ sealed
"looks right to me"    ≠ sealed
```

The loop closes when the work is genuinely ready to close — not when the agent runs out of steps.

---

## The problem it addresses

Six recurring failures when real work is delegated to an AI:

| Failure | What LoopSeal requires instead |
|---|---|
| Declaring completion mid-way | Report in the language of the actual state; "changed" is not "verified" |
| Treating tests as deployment, deployment as working software | Each stage needs its own evidence; reaching one never implies the next |
| Producing a file nobody opened | Document work is verified by opening the artefact, not by writing it |
| Changing config without reading it back | Config work has two layers: value read back, behaviour re-tested after reload |
| Filling gaps with plausible guesses | `UNKNOWN` and `UNVERIFIED` are first-class outputs |
| Losing the thread across agents and sessions | An open loop hands off with the reason it is still open |

And one boundary: **an AI must never create, imply or assume human approval.**

---

## What it is not

LoopSeal does not replace Superpowers, Spec Kit, BMAD, CI pipelines, or your own `AGENTS.md` / `CLAUDE.md`. Those govern **how work gets done**. LoopSeal governs **when it may be called done**. They compose; it is not a competitor to any of them.

If your work is entirely coding, inside one session, with strong CI and you review every diff, most of this is already covered by your pipeline. See [Who this is for](#who-this-is-for).

---

## Core model

Six concepts, defined in `SKILL.md` and expanded in `references/`:

| Concept | Question it answers |
|---|---|
| **Seal Criteria** | What must be true before this can be sealed? |
| **Claims** | What exactly am I asserting, and how far does it extend? |
| **Evidence** | Is it fresh, attributable, relevant, and could it have failed? |
| **Verification** | What counts as checked for *this kind* of work? |
| **Human Gates** | Does a person have to confirm, and did they actually? |
| **Open-loop Handoff** | Why is this loop still open, and what does the next person need? |

Proportionality is built in: trivial work stays trivial. If the seal report is longer than the task, the protocol is being misused.

---

## Install

No runtime, no dependencies, no PATH changes, no admin rights. It is a folder of Markdown.

```bash
# Claude Code
git clone https://github.com/ITOKORABBIT/loopseal.git ~/.claude/skills/loopseal

# Codex (user-level skills live in ~/.agents/skills)
git clone https://github.com/ITOKORABBIT/loopseal.git ~/.agents/skills/loopseal
```

Windows (PowerShell):

```powershell
git clone https://github.com/ITOKORABBIT/loopseal.git "$env:USERPROFILE\.claude\skills\loopseal"
git clone https://github.com/ITOKORABBIT/loopseal.git "$env:USERPROFILE\.agents\skills\loopseal"
```

For a single project, Codex also reads `.agents/skills/` inside a repository.

Restart the tool. To confirm it is loaded, ask: *"Do you have the loopseal skill? What are the four evidence tests?"* (Answer: freshness, provenance, relevance, falsifiability.)

### Update

```bash
cd ~/.claude/skills/loopseal && git pull
```

### Make it the default (optional)

Skills are normally applied when the agent judges them relevant. To apply it always, add one line to your global instructions file (Claude Code's `~/.claude/CLAUDE.md`, Codex's global `AGENTS.md`):

```markdown
Apply the loopseal skill to all delegated work: claims need evidence, unverified is stated, human approval is never assumed.
```

LoopSeal does not edit your configuration files. That step is yours.

### Remove

```bash
rm -rf ~/.claude/skills/loopseal ~/.agents/skills/loopseal
```

Nothing else is left behind.

---

## Contents

| Path | What it is |
|---|---|
| `SKILL.md` | The protocol itself — this is what the agent reads |
| `references/seal-criteria.md` | Writing criteria that can actually fail |
| `references/evidence.md` | The four evidence tests, recording format |
| `references/verification.md` | What "verified" means per work type; the coding chain |
| `references/human-gates.md` | required / optional / not applicable, and the red line |
| `references/uncertainty.md` | `UNKNOWN` vs `UNVERIFIED`, and not interpolating |
| `references/handoff.md` | Open-loop handoff format and receiver duties |
| `examples/` | Real cases, including ones where verification failed |
| `evals/` | Benchmark scenarios and rubric for testing whether this actually helps |
| `templates/` | Blank state and handoff templates |

`references/` is loaded on demand, not all at once.

---

## Examples

- [Tests passed; the install test found two bugs](examples/01-coding.md)
- [Research where two of three assumptions were wrong](examples/02-research.md)
- [Config read back correctly; runtime could not be verified](examples/03-system-config.md)
- [A document that read well and had five dead links](examples/04-document.md)
- [Handing off a loop that is still open](examples/05-handoff.md)

---

## Does it actually help?

**Unproven after two rounds. Neither found a measurable advantage, and the second round could not run the comparison that matters.**

[Round 2](evals/results/2026-09-20-round2.md) attempted the four-arm design and hit three walls: the comparison skill had no content installed, the "base" arm turned out to carry an existing evidence-discipline instruction file, and the agents' working directory contained both this protocol and the benchmark's own pass/fail criteria — two base-arm agents confirmed reading them. Of 20 cells, 16 were scorable by a blinded grader: **6 pass / 1 partial / 1 fail without LoopSeal, 7 pass / 1 partial / 0 fail with it** — a difference smaller than the variation between two runs of the *same* condition. The single outright failure and the single best run came from the same arm, on the same scenario.

The round 1 pilot is below and reached the same place by a shorter route.

`evals/` contains 13 scenarios and a rubric comparing four arms: a base agent, an existing workflow skill, LoopSeal, and both together.

A partial pilot has been run — 4 of the scenarios, base agent versus LoopSeal, one run each. Result: [**both arms passed all four**](evals/results/2026-09-19-pilot.md). A current frontier model already refused to fabricate human approval, already declined to state an unsourced fact, already checked that a config change had actually loaded, and already kept a trivial task trivial. Token cost was a wash (+0.6%).

The only differences observed were in how completion was *described*, not in what was *done*:

- a tighter claim boundary, with the untested part named explicitly
- distinguishing "the file says this was tested" from "I verified it was tested"

That is a much smaller claim than "prevents false completion", and the larger claim is not supported.

What is still unknown: the eight scenarios the hypothesis says should differentiate (deployment versus reality, stale evidence, handoff conflicts, cross-agent context loss) were **not run**, nor were the two arms involving an existing workflow skill — so whether LoopSeal adds anything on top of one is entirely unanswered. Weaker models, long sessions and real multi-agent handoffs are untested.

The benchmark is designed so LoopSeal can lose, and this pilot is what that looks like. Full write-up, including the method's limitations: [`evals/results/`](evals/results/).

## Who this is for

The working hypothesis — **not yet proven, and not supported by the pilot above** — is that LoopSeal earns its place when you **delegate real work to an AI**, rather than using an AI to write code you then review line by line:

- multiple agents or sessions handing work to each other
- work that gets deployed or touches external systems
- a mix of coding, research, documents and operations
- you do not inspect every action the agent takes

**Probably not worth installing if:**

- your work is purely coding, single-session, covered by CI, and you read every diff — your pipeline already enforces most of this
- you are on a current frontier model doing short, self-contained tasks — the pilot suggests it already behaves this way unaided

Install it if you have personally been burned by a confident "done" that was not, and you want the boundary written down rather than left to the model's disposition on the day. That is an honest reason. "It measurably improves agent output" is not yet one.

---

## Licence

MIT
