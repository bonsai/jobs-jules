---
name: ext-to-crx
description: Use this skill when migrating Chrome Extension repositories, tools, CLIs, scripts, manifests, events, and documentation from internal ext naming to crx naming across the bonsai organization. This is a heavy multi-repository migration: inspect first, preserve behavior and compatibility, process repositories sequentially, test each change, and create one migration PR per repository.
---

# EXT → CRX Migration Skill

## Mission

Take ownership of the full 'ext' → 'crx' migration across the bonsai organization.

This is a **heavy instruction set**. Do not reduce it to a mechanical search-and-replace. For each repository, independently investigate the architecture, decide which 'ext' references are internal Chrome Extension naming, make the smallest safe migration, test it, and produce a migration PR.

The goal is:

> **Change naming without changing behavior.**

Do not add unrelated features or refactor unrelated code.

## Target repositories

Process these repositories in order:

1. 'bonsai/ext-cli' → 'bonsai/crx-cli'
2. 'bonsai/ext-install-ext' → 'bonsai/crx-install-crx'
3. 'bonsai/ext-install-skill' → 'bonsai/crx-install-skill'
4. 'bonsai/cli-ext-man' → 'bonsai/cli-crx-man'
5. 'bonsai/chrome-ext-dummy' → 'bonsai/chrome-crx-dummy'
6. 'bonsai/hw-msedge-ext' → 'bonsai/hw-msedge-crx'
7. 'bonsai/gh-chatgpt-ext' → 'bonsai/gh-chatgpt-crx'
8. 'bonsai/sakura-usage-ext' → 'bonsai/sakura-usage-crx'
9. 'bonsai/gh-new-ext' → 'bonsai/gh-new-crx'
10. 'bonsai/repo-create-ext' → 'bonsai/repo-create-crx'
11. 'bonsai/soubi-ext' → 'bonsai/soubi-crx'
12. 'bonsai/chrome-synced-tabs-ext' → 'bonsai/chrome-synced-tabs-crx'

### Explicit exclusion

Do **not** migrate:

- 'bonsai/vonsai-vsx-extension'

It is a VSX project, not a CRX migration target.

## Existing work

Do not recreate migration PRs that already exist:

- 'bonsai/ext-cli#1'
- 'bonsai/ext-install-ext#24'
- 'bonsai/ext-install-skill#2'
- 'bonsai/cli-ext-man#1'
- 'bonsai/chrome-ext-dummy#1'

For these, inspect the existing PR state only if needed. Continue with the remaining repositories.

## Operating mode

### 1. Inspect before editing

For each repository, inspect:

- README / documentation
- package manifests
- Chrome manifest
- CLI entrypoints
- shell scripts
- PowerShell scripts
- batch files
- GitHub Actions
- npm/pnpm/bun scripts
- source filenames
- imports/requires
- command names
- environment variables
- configuration keys
- log/history filenames
- event names
- API command names
- cross-repository references

Search broadly first, then edit only confirmed internal naming.

### 2. Classify every 'ext' occurrence

Treat an occurrence as a migration candidate when it is an internal shorthand/name for the Chrome extension implementation.

Typical candidates:

- 'ext.ts' → 'crx.ts'
- 'ext.ps1' → 'crx.ps1'
- 'deploy-ext.cmd' → 'deploy-crx.cmd'
- 'ext-cli' → 'crx-cli'
- 'ext-install' → 'crx-install'
- 'ext.enabled' → 'crx.enabled'
- 'ext-history.jsonl' → 'crx-history.jsonl'

Do **not** mechanically change every occurrence.

Keep established formal terminology such as:

- Chrome Extension API
- browser extension
- WebExtension

unless the occurrence is clearly an internal project name or command.

### 3. Preserve compatibility

Before changing an API, event, command, file path, environment variable, or persisted filename, determine whether it is consumed externally.

If changing it would break existing consumers:

1. prefer compatibility;
2. add a compatibility alias only when it is small and clearly safe;
3. document the compatibility decision in the PR.

Do not silently break existing users.

### 4. Rename files safely

When an internal filename is part of the migration:

- create the new CRX-named file;
- update imports/references;
- verify the new path;
- remove the obsolete file only after references are migrated.

Do not delete files merely because their names contain 'ext'.

### 5. Tests and validation

Use the repository's own tooling.

Look for and run relevant commands such as:

- 'npm test'
- 'pnpm test'
- 'npm run lint'
- 'npm run build'
- 'pnpm build'
- project-specific CI checks

If tests cannot run, record exactly why.

Do not invent successful test results.

### 6. One repository at a time

Never perform the whole migration as one blind batch.

For each repository:

1. inspect;
2. plan;
3. edit;
4. verify references;
5. test;
6. create/update the migration PR;
7. record status;
8. move to the next repository.

Use a migration branch such as:

    rename/ext-to-crx

Avoid duplicate branches/PRs when existing work is already present.

### 7. Repository rename

If the execution environment provides a safe repository-rename capability, rename the GitHub repository to the target name.

If repository rename is unavailable:

- still complete the internal migration;
- create the PR;
- explicitly state that the repository-level rename remains pending.

Never claim a repository was renamed when only its contents were migrated.

## Hard constraints

- No unrelated refactoring.
- No new product features.
- No speculative API redesign.
- No destructive compatibility breaks.
- Do not overwrite existing work.
- Do not duplicate existing PRs.
- One repository = one migration PR.
- Do not use 'rm -rf'.
- Do not use 'git add .'.
- Do not use 'git add -A'.
- Prefer explicit file operations.
- Stop and report when a destructive change is genuinely unavoidable.

## Completion checklist

For every target repository:

- [ ] repository inspected
- [ ] all relevant 'ext' usages classified
- [ ] internal naming migrated to 'crx'
- [ ] filenames/references updated
- [ ] README/docs updated where appropriate
- [ ] package/scripts updated where appropriate
- [ ] manifest updated where appropriate
- [ ] events/API names reviewed
- [ ] cross-repository references reviewed
- [ ] tests/lint/build/CI checked
- [ ] migration PR created or existing PR verified
- [ ] repository rename status recorded

## Final report

After processing all repositories, provide a compact table:

| Repository | Internal migration | Tests | PR | Repository rename |
|---|---|---|---|---|

Include blockers explicitly.

Do not stop merely because one repository is unusual. Isolate the blocker, record it, and continue with the next repository unless continuing would risk data loss or a destructive compatibility break.

**Default behavior: investigate → implement → verify → PR → continue.**
