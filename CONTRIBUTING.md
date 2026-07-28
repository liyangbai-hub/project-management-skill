# Contributing

Thank you for helping improve `project-management`. This repository is being prepared for a public release; contributions should preserve portability, evidence quality, and clear authorization boundaries.

## Before proposing a change

1. Read `AGENTS.md`.
2. Read the relevant formal files: `SKILL.md` and the applicable `references/*.md`.
3. Keep the change focused. Explain the user problem, affected mode, and expected behavior.

No external workflow, plugin, or extra skill is needed to contribute. Everything required is in this repository.

## Types of contribution

### Human documentation

Installation guides, examples, landing pages, and troubleshooting material should explain the formal behavior without duplicating it. Link to `SKILL.md` or the relevant reference instead of creating a parallel rule set.

### Formal behavior

Changes to `SKILL.md` or `references/*.md` require:

- a concrete scenario that motivates the change;
- an explanation of authorization and failure behavior;
- updates to affected evaluations and examples;
- checks for contradictions across all formal files;
- confirmation that Mode 2 never modifies its source and that Mode 3 never treats severity as modification authority.

Formal behavior remains in Simplified Chinese so the project has one maintained behavioral source.

### Evaluations

Add realistic prompts, fixtures, and objective assertions where possible. Cover both expected triggers and non-trigger cases. A successful response should be judged from produced files and observable behavior, not an Agent's self-report.

Fixtures must use synthetic data. Never include real credentials, private records, customer data, internal URLs, or author-machine paths.

### Compatibility claims

State exactly what was tested:

- static review;
- simulated conventions;
- or a real operating system and Agent environment.

Include relevant versions, installation level, discovery method, scenarios, and limitations. Do not upgrade Windows, macOS, Claude Code, or Codex claims beyond the available evidence.

## Text and file conventions

- Save text as UTF-8 with LF line endings.
- Preserve exact casing for key files.
- Prefer ASCII lowercase filenames with hyphens for new files.
- Use portable placeholders such as `<project-root>` and `<repository-url>`.
- Do not require symbolic links or a particular scripting runtime for core behavior.
- Keep relative links valid from both English and Simplified Chinese documentation.

## Security and privacy

Do not submit secrets or sensitive data. If a sample needs configuration, use explicit placeholders or an `.env.example`-style file with non-working values. Reports should identify a sensitive category and relative location without reproducing the value.

## Validation checklist

Run the applicable checks in [`docs/maintenance-checks.md`](docs/maintenance-checks.md), which covers formal-source consistency, required files, link validation, path/secret/placeholder scans, behavioral evaluations, cross-platform evidence, and fresh-computer usability.

Record failures, skipped checks, environment limitations, and uncertainty honestly, and review the diff for unrelated or generated files.

## GitHub and remote operations

A contribution guide does not authorize any remote action. Creating forks, branches, commits, pushes, pull requests, Releases, or other publication remains an explicit action by the contributor or repository owner. Automated Agents must obtain the user's authorization before performing outward-facing operations.

Repository owners can use the Chinese [GitHub publishing guide](docs/github-publishing.zh-CN.md) for web upload, Git CLI publication, repository settings, and optional Release gates. Contributors do not need GitHub CLI; ordinary Git or GitHub's web interface is sufficient when the corresponding external action is authorized.

## License

Contributions are accepted under the repository's MIT License. Copyright attribution is recorded in [`LICENSE`](LICENSE).
