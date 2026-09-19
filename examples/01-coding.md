# Example 1 — Tests passed. The install test found two bugs.

**Work:** a small command-line tool.
**Why this one:** it is the cleanest illustration of `Tested ≠ Deployed`, and of why a check that cannot fail proves nothing.

---

## Seal criteria (written first)

1. The tool installs into a clean directory and runs there
2. Uninstalling restores the directory to its prior state
3. Automated tests pass

Criterion 2 exists because an earlier tool in the same family left files behind.

## Implemented

Eleven source files, no third-party dependencies. Paths and line ranges recorded; all committed.

At this point the honest statement is *"written, not verified"*. Nothing had been run.

## Tested

```
npm test → tests 45 / pass 45 / fail 0
```

Full output saved; anyone can re-run it.

Correct claim: **"45 automated tests pass."**
Incorrect claim, and the tempting one: *"the tool works."* The tests were written by the same process that wrote the code, and they exercised the code in-process only.

## Deployed → this is where it broke

"Deployed" for a tool with no server means: **installed into a clean directory, executed there, and uninstalled.** That definition was written down rather than assumed.

The install test found two defects the 45 tests could not have caught:

| Defect | Why the tests were blind to it |
|---|---|
| The generated launcher contained non-ASCII comments; under a different console code page the shell parsed them as commands and printed an error on every run | Tests invoked the code in-process; no test ever went through the system shell |
| Running "uninstall" via the launcher deleted the launcher mid-execution, so the shell then failed reading the rest of the file | Each test ran in isolation; none simulated a file deleting itself while executing |

Both are **falsifiability** failures: the test suite was incapable of failing on either bug, so its passing said nothing about them.

After fixing, the tests **and** the install test were both re-run. The earlier passing run was void the moment the code changed (**freshness**).

## Human gate: required

The tool would be used by someone else, so a person had to run it once.

Handed over with exact steps:

```
1. cd into an empty folder
2. run <install command>
3. you should see "installed", and the folder should contain <files>
4. if not, tell me and I'll reopen it
```

Seal state at handover: **OPEN — awaiting the human gate.** Not "done".

---

## Takeaways

1. **In-process tests and a real install test verify different things.** One checks logic; the other checks whether it survives contact with a real environment.
2. **Define "deployed" for the project, in writing, before using the word.** No server does not mean the stage is skipped.
3. **Fixing invalidates prior verification.** Re-run everything downstream of the change.
