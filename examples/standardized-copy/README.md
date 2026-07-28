# Standardized Copy Example

> Before any Agent starts work, read `AGENTS.md` in the target project.

This directory is reserved for a Mode 2 delivery example. The source fixture remains in `evals/fixtures/legacy-project/`; this target is an independent location and must never be treated as the source.

## Expected target contents

- A target-specific `AGENTS.md`.
- A target-specific `CLAUDE.md` pointer.
- A factual `README.md` describing the copied project.
- A `DEVLOG.md` recording copy work and validation.
- A `CHANGELOG.md` for implemented user-visible target changes.
- The source's safe, non-sensitive project content in its native relative layout.
- No real `.env` values, private keys, caches, or source-machine absolute paths.

This placeholder is intentionally not a claim that Mode 2 has already been executed. A real evaluation must create the target beside a temporary source, record pre/post source inventories and hashes, and validate the target independently.
