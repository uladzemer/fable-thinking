# Final Self-Review of the Diff — Three Lenses

Read this file after completing code edits and BEFORE the "done" report.
The point: the author of a diff is blind to their own mistakes because they read "what
they meant to write", not "what is written". The three lenses below are three different
ways to break that blindness. Go through them in order; this takes 5–10 minutes.

Before starting, get the REAL picture of changes, not from memory: `git status`
(it also shows untracked files — new modules are not visible to `git diff`!) plus
`git diff HEAD` (shows staged and unstaged together); if there were intermediate commits
during the task, diff against the task's starting commit. Read new untracked files in
full as part of the "diff".

## Lens 1. Line by Line, With Surroundings

For each hunk:

1. Read the **entire function/block** containing the edit — not only the changed
   lines. Bugs live at boundaries: the edit is correct by itself, but conflicts with
   a line ten lines above.
2. For every line that handles input, a condition, or a boundary, ask: what happens
   with **empty / null / zero / negative / very large** input? Zero and empty string are
   falsy: `if (x)` on a number is almost always a bug. (Do not run this check on lines
   without branches and inputs — assignments, logging.)
3. Check boundaries: `<` vs `<=`, off-by-one in indexes and slices, first/last loop
   iteration.
4. Check error paths: what happens when await fails, when the file does not exist, when
   the response is not 200? Did the edit add a new error path — who handles it?

## Lens 2. Removed and Changed Invariants

Go through the diff again, looking ONLY at removed and replaced lines (minuses in the
diff):

1. Every removed line **guaranteed** something: a check, call order, initialization,
   resource release, data format.
2. For each one, ask: **who relied on this guarantee?** Find consumers (grep by name,
   by field, by event), and make sure they survive its disappearance.
3. Changed conditions (`if` rewritten) — check both directions: cases that used to pass
   and now do not, and the reverse. The second direction is always forgotten.

## Lens 3. Trace Through Call Sites

For every changed public contract (function signature, return value format, event name,
data schema, config key):

1. Find **all** callers/consumers: grep by name across the whole project, including
   tests, other layers (client/server, main/renderer/preload), and configs.
2. Do not forget **code duplicates**: the same module may exist as a second copy
   (forked directory, vendored file, second bundle). Fixing one copy out of two means
   "fixed, but it does not work for the user".
3. Grep does not find dynamic calls: string keys, dependency injection, dispatch tables,
   reflection. If the project uses them, check manually.

## Verdict for Each Finding

Not every finding requires an edit. Classify:

- **Fix now** — a real defect in my diff.
- **Report, do not fix** — a defect outside my scope (existed before me / neighboring
  system). Put it in a separate list in the report.
- **False alarm** — reread, the code is correct. Move on; do not "improve" it.

After fixes from findings, run verification (tests/run) again: an edit made because of
review is still an edit, and it can also break things.

## Sign of a Quality Pass

If the three lenses produced NOT A SINGLE finding and the diff is larger than 30 lines,
check yourself: did you skim the diff, confirming yourself? Go through Lens 2 one more
time (removed invariants are the richest angle) — and if it is empty again, **that is a
valid result**: clean diffs exist. Do not invent a finding so the "review looks done" —
a false finding and the edit made for it are more harmful than an empty review.
