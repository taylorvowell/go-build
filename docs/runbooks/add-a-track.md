# Runbook — add a build track

- **Last updated:** 2025-01-01
- **Applies to:** the track-based build system (`.claude/ROADMAP.json` + `.claude/feature-tracks/`)

## When to use this

You want to start a new, multi-step piece of work and have the build system track it. Usually you don't do this by hand — run **`/plan <feature description>`** (the `plan` skill does every step below for you). This runbook is the underlying procedure, for reference or manual use.

## Prerequisites

- Read the step-file template and `_STATUS.json` shape in `.claude/ai-instructions/00 - README.md`.
- Know which `phase` (from `ROADMAP.json`'s `phases[]`) the work belongs to.

## Steps

1. **Pick a kebab-case track id** — short and descriptive (e.g. `user-accounts`, `search-v2`).
2. **Decompose the feature into steps** — the fewest steps that each leave the repo in a verifiable, coherent state. Each step needs an objective Verification oracle (typecheck + lint + scoped tests).
3. **Scaffold `.claude/feature-tracks/<id>/`:**
   - `_STATUS.json` — `schemaVersion: 1`, `currentStep: "01"`, the `phase` label, every step listed as `not-started`, empty `blockers`/`skipped`.
   - `_PROGRESS.md` — header + a one-paragraph scope summary + the `<!-- entries below this line -->` marker.
   - `NN - Title.md` — one numbered step file per step, using the template (Overview, Dependencies, Files & Areas Touched, Steps, Quality Standards, Verification, Definition of Done, Notes).
4. **Register the track in `.claude/ROADMAP.json`** — append a track object with `id`, `goal`, `spine`, `phase`, `statusFile`, `dir`, `lifecycle: "active"`, `dependsOn`, `owns`, `unblockTrigger`, `notes`. Declarations only — never write step status into `ROADMAP.json`.
5. **Set the spine if needed** — if this is the new top priority, set `spine: true` on it and `spine: false` on the track that previously had it (only one spine at a time).

## Verification

Run **`/roadmap`** — the new track appears in the macro rollup with its step count and `0%` progress, and the consistency checks pass (no missing `statusFile`, no two spines, no dependency cycle).

## Rollback / if it goes wrong

Delete the `.claude/feature-tracks/<id>/` folder and remove the track object from `ROADMAP.json`. Nothing else references it until you run `/build` or `/feature <id>`.

## Notes

- To **add a step** to an existing track later, use `/plan add-step <track-id> <what>` — don't renumber existing steps (it breaks references); append or use a sub-step like `02a - Title.md`.
- Graduating a parked backlog idea (`/future develop <id>`) routes here: the idea becomes a planned track via this same procedure.
