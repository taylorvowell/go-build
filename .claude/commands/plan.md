Plan a new feature as a build track (or add a step to an existing one).

Invoke the `plan` skill immediately and follow it end-to-end. Everything after `/plan` is the feature to plan:

- `/plan <feature description>` — plan a brand-new track: clarify scope → decompose into numbered, verifiable steps → scaffold `.claude/feature-tracks/<id>/` → register the track in `.claude/ROADMAP.json`.
- `/plan add-step <track-id> <what>` — insert a new step into an existing track.

This CREATES the plan; it does not execute it. After planning, run `/build` (if it's the spine) or `/feature <id>` to advance it. Do NOT commit.

If the argument is empty, ask the user what they want to plan (one line).
