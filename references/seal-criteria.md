# Seal Criteria

> What must be true before this work can be sealed?

Criteria are written **before** the work, not reconstructed afterwards to match whatever happened. Criteria written after the fact always pass.

## What makes a criterion usable

A usable criterion is **checkable by someone who is not you** and **capable of failing**.

| Unusable | Usable |
|---|---|
| "The importer works correctly" | "A 5,000-row file imports with 5,000 rows in the table and no rows in the error log" |
| "The page looks good on mobile" | "At 375px width, no horizontal scrollbar and the CTA is visible without scrolling" |
| "The report is accurate" | "Every figure in the summary matches the source export, checked cell by cell" |
| "Config is production ready" | "`GET /health` returns 200 after a restart, and the new timeout value is visible in the running config" |

If you cannot describe the observation that would make the criterion fail, it is not a criterion — it is a hope.

## Scale with risk

| Risk | Criteria effort |
|---|---|
| Throwaway, reversible, nobody else sees it | None written down; keep your certainty honest |
| Normal task with a deliverable | 2–4 lines, in the reply |
| Deployment, external systems, money, data loss potential, irreversible | Written down, including the failure cases you specifically intend to rule out |

Do not turn a two-minute task into a checklist exercise. Over-applying this protocol is a failure of the protocol.

## Include the cases that already broke

The highest-value criteria come from prior failures. If the last attempt died on empty input, "works with empty input" is a criterion, not a nice-to-have.

## When the user gives no criteria

Propose them and say you are proposing them:

> "I'll treat 'done' as: the CSV opens in Excel without mojibake and contains all 12 columns. Tell me if you want a different bar."

The silent alternative — inventing a private bar and later declaring it met — is how "done" becomes meaningless.

## When criteria cannot all be met

Say which ones, why, and what it would take. Do **not** quietly lower the bar to reach a seal. Only the person who set the criteria can relax them.

```
Sealed: 2 of 3 criteria.
Not met: "works with 50k rows" — the largest file available here is 4k.
         Needs a real export from production to test.
```

That is an honest partial seal. Silently dropping the third criterion is not.

## Criteria and the human gate

Criteria decide whether the *work* is finished. A human gate decides whether the *loop* closes. They are different questions — meeting every criterion still leaves a `required` gate open. See `human-gates.md`.
