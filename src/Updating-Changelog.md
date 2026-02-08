# Deterministic CHANGELOG Update Rules

This document defines a deterministic pattern for updating an existing `CHANGELOG.md` that already contains the `# Changelog` header and prior releases. It describes how to append the next release entry in the same pattern.

## What an “update” means

A changelog update means adding exactly one new release block for a new version, appended to the end of the file (unless you are correcting an older entry as a special case described below).

## Deterministic update procedure

### Step 1 — Choose the next version (SemVer)

Use SemVer: `MAJOR.MINOR.PATCH`.

Deterministic bump rules:

1) If any change is breaking (requires user code changes, changes meaning of existing API, removes/renames a public symbol without aliasing) → bump MAJOR.
2) Else if any change adds new capability (new module, endpoint, feature, model, new public function/class, new CLI/tooling behavior) → bump MINOR.
3) Else → bump PATCH.

If multiple apply, choose the highest bump.

### Step 2 — Set the release date (format rules)

Release heading date rules:

- Month is full English name: `January` … `December`
- Day is integer without leading zero: `1`, `2`, `31`
- Ordinal suffix must be correct using the deterministic rules below

Ordinal suffix rules:

- If day is 11, 12, or 13 → `th`
- Otherwise:
  - ends with 1 → `st`
  - ends with 2 → `nd`
  - ends with 3 → `rd`
  - else → `th`

Examples: `1st`, `2nd`, `3rd`, `4th`, `11th`, `12th`, `13th`, `21st`, `22nd`, `23rd`, `31st`.

### Step 3 — Append the new release heading (exact template)

Append a new heading at the end of the file using exactly:

## v{MAJOR}.{MINOR}.{PATCH} on {DAY}{ORDINAL} of {Month}, {YEAR}

No alternative phrasing, no extra punctuation, no extra tags.

### Step 4 — Spacing rule after heading

There must be exactly one blank line between the heading and the first bullet.

### Step 5 — Convert changes into bullets (one change per bullet)

Rules:

- Each bullet is a single line starting with `- `
- No nested bullets
- No numbered lists
- No trailing whitespace
- Prefer one atomic change per bullet:
  - If a bullet contains “and” joining separable actions, split it
  - If the combined action is truly inseparable (e.g., a rename requires coordinated updates), keep it as one bullet

### Step 6 — Verb style (deterministic)

Bullets must start with an imperative verb. Use this canonical verb set unless there’s a strong reason not to:

- Add
- Fix
- Refactor
- Move
- Rename
- Remove
- Update
- Improve
- Simplify
- Generalize
- Standardize
- Implement
- Configure
- Create
- Organize
- Disable
- Enable

Avoid past tense (“Added”, “Fixed”).

### Step 7 — Code-ish formatting rules

Wrap these in backticks:

- Function names: `get_klines_data`
- Class names: `UniversalExperimentLoop`
- Parameters and flags: `n_permutations`, `futures=False`
- Modules: `limen.metrics`
- File paths: `utils/get_klines_data.py`
- Literal config keys: `save_to_sqlite`

If referencing a specific file, prefer a Markdown link with backticked visible text:

- [`get_klines_data`](utils/get_klines_data.py)

Use exact spelling and casing as in the codebase.

### Step 8 — Notes and breaking changes

Notes must use exactly:

- `- **NOTE**: ...`

Breaking changes must use exactly:

- `- **BREAKING**: ...`

Placement rule:

- All `**BREAKING**` bullets must be first in the release.
- All `**NOTE**` bullets must come immediately after breaking bullets.

### Step 9 — Bullet ordering (deterministic)

Within a release, bullets must be ordered by category in this exact sequence:

1) `**BREAKING**` items
2) `**NOTE**` items
3) Add
4) Move / Rename
5) Refactor / Simplify / Generalize / Standardize
6) Fix
7) Update / Improve / Configure
8) Tests / CI / Tooling / Docs
9) Remove / Deprecate (unless breaking; then category 1)

Within each category:

- Sort lexicographically (A→Z) by the text after the initial verb/prefix.

Exception:

- If one bullet logically depends on another for readability, keep dependency order (e.g., “Rename X” before “Update docs for X”).

### Step 10 — Spacing rule after the release

After the last bullet, add exactly one blank line before anything else.

## Deterministic “done” checklist

The update is valid if:

- The new release is appended at the end of the file.
- The heading matches `## vX.Y.Z on D{suffix} of Month, YYYY`.
- Ordinal suffix is correct (11/12/13 always `th`).
- Exactly one blank line after the heading and one blank line after the bullet list.
- Bullets are flat and start with `- `.
- Bullets use imperative verbs (Add/Fix/Refactor/…).
- Code identifiers/params/paths are wrapped in backticks.
- File references use Markdown links where appropriate.
- Bullets are ordered by the category ordering rules.
- No trailing whitespace.

## Append-only template (fill in and remove unused lines)

Append this block at the very end of `CHANGELOG.md` when releasing:

## vX.Y.Z on D{suffix} of Month, YYYY

- **BREAKING**: ...
- **NOTE**: ...
- Add ...
- Move ...
- Rename ...
- Refactor ...
- Fix ...
- Update ...
- Configure ...
- Add test ...
- Update docs ...

Remove any unused lines; do not keep placeholders.
