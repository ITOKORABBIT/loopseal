# LoopSeal evaluation

This directory exists to answer one question honestly:

> **Does LoopSeal improve AI-delegated work enough to justify installing it?**

It is not a demo. It is built so that LoopSeal can lose, and the result is reported either way.

## Arms

| Arm | Setup |
|---|---|
| **A — Base** | The agent as shipped. No workflow skill, no extra instructions. |
| **B — Existing workflow skill** | An established workflow/verification skill of your choice, installed per its own instructions. |
| **C — LoopSeal** | LoopSeal installed, no other workflow skill. |
| **D — Both** | Arm B's skill plus LoopSeal. |

Arm D is the one that matters commercially. If **B ≈ D**, then for users of that skill LoopSeal adds little, and the README should say so.

## Method

1. Fresh session per run. No carry-over context.
2. Same model, same settings across arms.
3. **The prompt never mentions LoopSeal, sealing, evidence, or verification.** Scenarios are ordinary requests. Prompting the discipline you are measuring invalidates the measurement.
4. Run the setup script exactly; fixtures matter — most traps live in the fixture, not the prompt.
5. Record the full transcript and the resulting files.
6. Score with `rubric.md` against the transcript, not from memory.
7. Two arms minimum per scenario or the number means nothing.

## Fairness rules

- **No scenario may be written so that only LoopSeal's vocabulary can pass it.** The pass criteria are about behaviour — did the agent check, did it distinguish states, did it avoid fabricating — never about using particular words.
- **Scenario 12 is the counterweight.** It tests whether the protocol makes trivial work bureaucratic. LoopSeal can lose points there, and if it does, that is a finding.
- **Overhead is scored in every scenario**, not just 12.
- A tie is a result. "No measurable difference" goes in the README.

## What would falsify LoopSeal's value

State these before running, so the result cannot be reinterpreted afterwards:

- If **A ≈ C** across scenarios 1–11, the protocol is not changing behaviour and should not be recommended.
- If **B ≈ D**, it adds nothing for users of that skill; scope the recommendation to people not using one.
- If **C** costs substantially more tokens or turns with no reduction in false completion, the trade is bad.
- If **C** fails scenario 12 (ceremony on trivial work), the proportionality rule is not working and needs fixing before anyone is told to install it.

## Scenarios

| # | Failure mode probed |
|---|---|
| 01 | Tests pass, nothing deployed |
| 02 | Deployed, but the real thing is broken |
| 03 | Config written, never reloaded |
| 04 | Document produced, never opened |
| 05 | Research conclusion with no source |
| 06 | Evidence gone stale after a later change |
| 07 | Human approval fabricated or assumed |
| 08 | Handoff contradicts the actual repository |
| 09 | UNKNOWN turned into a confident fact |
| 10 | Declaring done too early |
| 11 | Cross-agent handoff loses critical context |
| 12 | Trivial task — overhead must stay near zero |
| 13 | Tested/deployed asserted by a file, unverifiable from here |

Each file contains: the failure mode, a reproducible fixture, the verbatim prompt, what a good response does, and what counts as a fail.

## Running one

```bash
cd evals/scenarios
# read the file, run its Setup block, start a fresh agent session in the fixture directory,
# paste the Prompt verbatim, save the transcript to ../results/
```

## Results

`results/` holds every run, including the ones that do not flatter LoopSeal. An empty or partial results directory means the benchmark has not been run, and the README must say "unproven" until it has.
