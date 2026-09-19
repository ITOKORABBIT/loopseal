# Example 5 — Handing off a loop that is still open

**Situation:** work is delivered and waiting on a person, and the session is ending. Another agent may pick it up.
**Why this one:** the classic handoff failure is not writing too little — it is filing "I did it" under "verified", so the next person skips a check that never happened.

The example below is this repository's own handoff, in the format from `references/handoff.md`.

---

# Handoff: publish the skill and verify it loads

- When / who: 2026-01-XX 20:10, Agent A
- Seal state: **OPEN**
- **Why still open:** one of the two target tools could not be launched from this environment, so "the skill loads" is verified for one tool and unverified for the other. The human gate is also uncleared.

## Objective

Publish the skill as a public repository and confirm a fresh install loads in both target tools.

Seal criteria:
1. A clean clone from the published URL contains only intended content
2. Each tool loads the skill from its own install location
3. A person confirms it works on their side

## Verified

- **Public clone is clean** — cloned fresh from the published URL; scanned working tree and full git history for internal terms → 0 matches
- **Install instructions work** — followed the README verbatim; both installs completed; 17 files each; checksums match the source
- **Tool A loads it** — appeared in the running session's skill list; invoked it; content loaded and base directory pointed at the install

## Unverified

- **UNVERIFIED**: tool B loads the skill — *why not:* could not be launched here, then it hit its own usage limit — *checkable by:* opening a tool B session and asking whether the skill is available
- **UNVERIFIED**: whether an agent applies it *unprompted* — only "it can be invoked" has been shown — *checkable by:* fresh session, ordinary task, no mention of the skill; see whether it separates verified from unverified on its own

## Unknown

- **UNKNOWN**: compatibility with tools beyond the two tested — *who would know:* their respective documentation

## What was changed

- Public repository created; content published
- Two skill directories on this machine now contain the skill (deleting the folders reverts it)
- Nothing else: no system settings, no PATH, no global config files

## Where the evidence is

- Clone check and scan output: in this session's transcript
- Load confirmation: the skill invocation showing its base directory
- Published commit: `<sha>`

## Known issues

- The link checker produces false positives on filenames used illustratively in prose
- A local archive directory adds files to a clone that only the skill's readers do not need

## Restrictions

- Do not push the local development history to the public repository — it contains internal notes; publish from the clean copy only
- Do not mark the human gate cleared; the person has not confirmed

## Next step

1. Ask the person to open a fresh session in tool B and confirm the skill is listed
2. If yes, ask them to run one ordinary task and check whether the agent separates verified from unverified without being told

---

## What makes this handoff usable

- **"Why still open" is the first thing**, in one sentence.
- **Verified and Unverified are separate sections.** The receiver may build on the first and must re-check the second. Merged into "done", both would be trusted equally.
- **Every unverified item carries "checkable by".** Otherwise the next person inherits a question with no route to an answer.
- **Restrictions are explicit**, including the one that would cause real damage if someone guessed.

## Receiver's duties

1. Read it before touching anything.
2. Where the handoff and the real system disagree, the system wins — and say so.
3. Do not forward "the previous agent said it works" as your own verification. Check it, or label it as reported.
