# Jules Agent Instructions

## Heavy reusable skill

This repository contains a reusable agent skill for the bonsai EXT → CRX migration:

.agents/skills/ext-to-crx/SKILL.md

When a task concerns migrating Chrome Extension repositories, internal 'ext' naming, or the bonsai CRX migration, load and follow that skill.

The skill is intentionally heavyweight: do not collapse its repository-by-repository workflow into a blind global replacement.

For the current migration plan and repository list, also consult EXT_TO_CRX.md.

## Working principle

Investigate first. Preserve behavior. Make narrow changes. Test. Create one PR per repository. Continue sequentially and report blockers without abandoning unrelated repositories.
