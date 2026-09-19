# Verification by work type

Different work fails in different ways. The check has to match the failure mode, not the vocabulary of whatever field you happen to come from.

General test: **if the work were wrong, would what I just did have revealed it?**

---

## Code and web changes

**Verified means:** the test or the feature actually ran, and you saw the result.

Coding — and only coding — also carries the four-stage chain, because these four states get confused constantly:

### Implemented
Code changed; you can state the scope.
Evidence: paths with line ranges, diff, commit.
Common false completion: half-finished; edited the wrong file or branch; edits not saved.

### Tested
The test ran and passed. No test suite? Then the feature itself was executed at least once.
Evidence: command plus real output.
Common false completion: predicting the result instead of running it; running an unrelated subset; treating warnings or partial failures as a pass; testing a path the change does not touch.

### Deployed (when applicable)
The result exists where users reach it, and you confirmed that.
Evidence: deploy output plus version/deployment id, **plus** an independent check of the live target.
Common false completion: the deploy command exited 0 and you stopped there; a manual publish step was skipped; the wrong environment; cached old content still being served.
Not applicable? Say so and why.

### Human Verified
A person used it and accepted it. Only they can grant it. See `human-gates.md`.

---

## Documents, content, decks

**Verified means:** facts traced to sources **and** the file opened and visually checked.

Three separate passes — running them together is how "it reads fine" hides a broken file:

1. **Content** — every requested item is present.
2. **Facts** — figures, names, dates, quotes, links each traced to a source; anything inferred is marked as inference.
3. **Format** — the file actually opens; headings, tables, images, page breaks and export render correctly.

Common false completion: shipping a `.docx`/`.pptx`/PDF that was never opened; tables broken in the exported format; a confident number remembered rather than checked.

---

## Data work

**Verified means:** counts reconcile, a sample was compared against the source, anomalies are listed.

1. **Counts** — rows in, rows out, and the difference explained.
2. **Sample** — pick several records at random and compare field by field with the source.
3. **Anomalies** — duplicates, nulls, encoding damage, silently dropped rows, listed rather than discarded.

Common false completion: "the script finished without errors"; a dropped column nobody counted; defaults silently standing in for missing values.

---

## Research and analysis

**Verified means:** every conclusion points at a source, and unsupported ones were downgraded.

Order matters: sources first, conclusions second. Then re-read your own conclusions and ask of each sentence, *which source says this?* Anything with no answer becomes an inference (labelled) or `UNKNOWN`.

Also judge the sources themselves: who produced it, when, with what interest. Date anything time-sensitive.

Common false completion: training-data recall presented as research; a second-hand summary cited as primary; a link that was never opened; conclusions stated more strongly than the evidence.

---

## Configuration, deployment, automation, maintenance

**Verified means:** the value reads back correctly **and** behaviour is correct after reload.

Two layers, both required:

1. **Config layer** — read the setting back and show the value. "The write command did not error" is not evidence.
2. **Runtime layer** — restart or reload, then trigger the behaviour and observe it.

The runtime layer is where this work usually fails: many settings need a restart, a new session, or a cache flush before they take effect. If the environment cannot restart or trigger it, mark `UNVERIFIED` and state what would make it checkable — do not assume the value implies the behaviour.

---

## Anything else

**Verified means:** checked in a way another person could repeat.

Use this rather than forcing an ill-fitting chain onto the work.

---

## After a failed verification

1. Reopen the loop at the stage that failed.
2. All evidence produced after that stage is void.
3. Fix, then **re-verify from that stage forward** — fresh evidence, not the earlier run.
4. Record why it failed. That record is what stops the next person re-walking the same hole.
