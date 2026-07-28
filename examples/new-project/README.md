# Offline Budget Tracker Example

> Before any Agent starts work, read `AGENTS.md`.

This is a small native code-project example for demonstrating Mode 1. It keeps source files in `src/` and does not introduce a forced `技术/` directory.

## Purpose

Provide a minimal offline budgeting project whose durable project facts can be handed between Agents through local files.

## Current capability

- The project scope and non-goals are recorded here.
- The source entry point is `src/main.txt`.
- The current work state and next step are recorded in `DEVLOG.md`.
- No external service, credential, or runtime dependency is required for this fixture.

## Native layout

| Path | Purpose |
|---|---|
| `src/main.txt` | Source entry description |
| `AGENTS.md` | Agent handoff protocol |
| `DEVLOG.md` | Current state and work-unit history |
| `requirements/INDEX.md` | Input-material assessment |

## Verification

Read `src/main.txt` and confirm that it describes the expected offline budgeting behavior. A production implementation would add its native build or test command here; this fixture intentionally has no external toolchain.

## Known limits

This is a documentation-and-fixture project, not a complete budgeting application. It exists to test the local fact layer and Agent handoff behavior.

## Next step

Add a small local input/output implementation while preserving the native `src/` layout and recording the work in `DEVLOG.md`.
