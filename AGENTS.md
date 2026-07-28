# Repository Working Agreement

This repository develops the `project-management` skill. Before changing any file:

1. Read `SKILL.md` and the relevant file in `references/` for formal behavior changes. For documentation-only changes, read the affected documentation and verify it against the formal behavior source.
2. Read `README.md`, then inspect the actual files covered by the change. Documentation is navigation, not proof.
3. Confirm the requested scope and authorization. Do not broaden a documentation change into behavior changes, destructive cleanup, Git initialization, publication, or remote operations.

This repository is self-contained. Working on it requires no external workflow, plugin, or additional skill—only the files here.

## Behavioral source of truth

Only `SKILL.md` and `references/*.md` define formal skill behavior. They are written in Simplified Chinese. Human-facing files such as `README.md`, `README.zh-CN.md`, `CONTRIBUTING.md`, and `docs/` explain installation, use, and maintenance; they must not become a second behavioral specification.

When behavior changes:

- update the smallest appropriate formal file;
- check every reference from `SKILL.md`;
- update human documentation only where its explanation is affected;
- add or revise representative evaluations;
- preserve the three-mode boundaries, especially Mode 2 source read-only isolation and Mode 3 authorization separation.

## Editing discipline

- Preserve existing project history. Do not rewrite past changelog, evaluation, audit, or validation records to make current work look cleaner.
- Keep the skill portable: UTF-8 text, LF repository line endings, exact key-file casing, no symbolic-link dependency, and no author-machine absolute paths in public files.
- Use placeholders such as `<project-root>`, `<source-project>`, and `<target-project>` in examples.
- Never add real credentials, private data, internal endpoints, personal paths, or secrets to fixtures, examples, logs, screenshots, or documentation.
- Do not make Python, Node.js, Bash, PowerShell, Git, GitHub CLI, `rsync`, or `robocopy` a core dependency.
- Do not silently overwrite user files during examples, installation, update, migration, or troubleshooting guidance.

## Validation and claims

After changes, run the applicable checks in `docs/maintenance-checks.md`. Inspect actual outputs rather than accepting an Agent's completion report as evidence. Record each check as passed, failed, not run, environment-limited, or uncertain.

Review all user-visible text for:

- stale or broken relative links;
- placeholders that block release;
- unsupported compatibility claims;
- conflicting formal rules;
- author-machine paths or sensitive values.

Static analysis and simulation are not real-device testing. Do not describe an operating system or Agent as fully tested without recorded real-environment evidence. Check `docs/validation-windows.md` and `docs/validation-macos.md` before changing compatibility claims.

## Git and publication

Git is optional. Do not run `git init`, create commits, discard local changes, create a remote repository, push, change remote branches, open a pull request, create a Release, or publish externally unless the user explicitly authorizes that exact action and scope.
