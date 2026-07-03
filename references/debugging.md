# Debugging Protocol — Full Rebuild After "Two Strikes"

Read this file when the "two strikes — stop" rule has triggered (2 failed attempts on
one problem) or when you take on a bug whose origin is "unclear".

The point of the protocol is to replace "I will try this other thing" with controlled
narrowing of the hypothesis space. Every step either confirms or kills a hypothesis —
movement is always forward, even when the fix has not been found yet.

## Step 0. Freeze Your Hands

No **fixes** until steps 1–4 have been completed. A patch written before the cause is
understood is a bet, not engineering. If you are here after "two strikes", you have
already lost two such bets; if you are taking on an unfamiliar bug from scratch, do not
make even the first one.

Diagnostic edits (temporary logs, prints, debugging asserts) are allowed and encouraged:
they gather data, not change behavior. Mark them and remove them before the final diff.

## Step 1. Reproduction

- Find the **minimal** way to trigger the symptom. One command is better than ten clicks.
- If the bug does not reproduce, that is your task #1, not "I will try a blind fix".
  A non-reproducible bug cannot be fixed: there is nothing to verify the fix with.
- Record the exact error text / incorrect value / screenshot. "Something is wrong" is
  not a symptom.

## Step 2. The "Works / Does Not Work" Boundary

Formulate pairs of observations:

- Works in X, does not work in Y — how does X differ from Y?
- Worked in commit A, broken by commit B — `git log A..B` over affected paths.
- Works with input P, fails with input Q — what is special about Q?

The boundary is the most informative fact about the bug. Often one boundary immediately
points to the layer.

## Step 3. Hypothesis Table

Write 3–5 hypotheses, and for each one specify **how to check it cheaply** and **what
would disprove it**:

```
H1: stale cache on the client       | check: hard reload / incognito       | disproved by: same bug in a clean profile
H2: init-order race                 | check: log event order               | disproved by: order is stable and correct
H3: my edit was not delivered       | check: grep deployed code            | disproved by: edit is present
```

A mandatory item in the table is the humiliation hypothesis: "my edit does not execute at
all" (wrong file, wrong process, cache, not rebuilt, second copy of the code). In
experience, this is the cause of every third "unclear" bug.

## Step 4. One-Variable Experiments

Check hypotheses from cheap to expensive, **one at a time**. If you changed two factors
and it got better, you do not know which one worked, and the hypothesis table is spoiled.

Narrowing tools:

- **Print reality**: log the actual value/structure at the suspicious point. Arguing
  with reality always loses — look at what is really there.
- **Binary search by layer**: does it work one layer lower? (API responds correctly →
  bug is higher; data in DB is correct → bug is in reading/rendering.)
- **Binary search by time**: `git bisect` or a temporary checkout of the last known
  working commit. Careful: before any command that touches the working tree (`checkout`,
  `stash`, especially `reset --hard`), check `git status` — unsaved changes from others
  must not be lost; destructive variants only with explicit user permission.

## Step 5. Cause Named — Check the Depth

Before writing the fix, answer: "is this the cause or another symptom?"
Test: "if I fix it here, could the same class of error arise in a neighboring place?"
If yes, the cause is deeper (validation at input, not a check at every use; module
contract, not each call).

## Step 6. Fix + Regression

- The fix is written only now — and it is usually small because it hits the cause.
- First show that the symptom reproduces; apply the fix; show that it disappears.
  If tests exist, add a test that would fail without the fix.
- Run neighboring tests: a cause-level fix sometimes changes behavior that someone
  relied on.

## Anti-Patterns (if you are doing this — return to Step 0)

- A fix with the words "just in case", "while at it", "should help"
- Two changes in one experiment
- "Fixed" without reproducing the bug even once
- Suppressing the symptom (try/catch around the crash, `|| default` on undefined)
  without answering why the value is wrong in the first place
- Debugging from memory of the code instead of reading the current code
