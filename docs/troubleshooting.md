# Troubleshooting

This guide covers installation and portability failures. It explains symptoms and recovery steps; it does not replace the formal behavior in `SKILL.md` and `references/*.md`.

## Skill is not found

Check in this order:

1. Confirm the intended installation level: personal or project-level.
2. Confirm the directory name is `project-management`.
3. Confirm `SKILL.md` is directly inside that directory.
4. Open `SKILL.md` and confirm its YAML frontmatter contains `name: project-management`.
5. Confirm the file is readable and was not saved as `SKILL.md.txt`.
6. Start a new Claude Code session if the installed version does not refresh discovery dynamically.
7. Check current Claude Code documentation or built-in help for version-specific discovery behavior.

Do not solve discovery failures by guessing another product's directory or by creating unverified symbolic links.

## Extra repository directory after clone or ZIP extraction

### Symptom

The final structure resembles:

```text
<skill-directory>/project-management-skill/SKILL.md
```

### Resolution

Move or copy the **contents** of the inner repository directory into `<skill-directory>` so that the result is:

```text
<skill-directory>/SKILL.md
```

Inspect both directories before deleting any duplicate. Removal of an old copy requires confirmation of the exact path and any local customizations.

## Incorrect key-file casing

Case-insensitive filesystems may hide errors that fail on a case-sensitive filesystem. Preserve exact casing for:

```text
SKILL.md
AGENTS.md
CLAUDE.md
README.md
DEVLOG.md
CHANGELOG.md
AUDIT.md
```

Do not use variants such as `skill.md`, `Agents.md`, or `readme.md` for files whose names are part of discovery or the project protocol. When Git does not detect a case-only rename on Windows, use an explicit intermediate name, inspect the result, and avoid changing unrelated files.

## Encoding or line-ending problems

### Symptoms

- Chinese text appears garbled.
- YAML frontmatter is not parsed.
- A diff reports every line changed after editing on another platform.
- A file unexpectedly contains a UTF-16 or local-code-page representation.

### Resolution

1. Save Markdown and text files as UTF-8.
2. Use LF for repository text by default.
3. Check editor encoding and end-of-line settings before rewriting files.
4. If `.gitattributes` exists, follow its declared text rules.
5. Convert only the affected files and inspect the diff; do not normalize the entire project without authorization.

A readable display in one editor does not prove portable encoding. Record conversion verification separately.

## ZIP extraction depth is wrong

Some archive tools create an outer archive directory and preserve the repository root beneath it. Navigate until the directory directly containing `SKILL.md` is visible, then copy that directory's contents into the install target. Do not rename random parent folders until the actual root is identified.

## Permission denied or read-only files

- Confirm the user account can read the downloaded files and write to the chosen personal or project-level skill directory.
- On macOS, check filesystem permissions and security prompts for the specific directory rather than granting broad access by default.
- On Windows, check whether files inherited a read-only attribute or whether controlled-folder access blocked the operation.
- Corporate policy or managed-device restrictions are an environment limitation; do not claim installation passed.
- Do not elevate privileges unless the user understands and authorizes the exact operation.

If a project review or Mode 2 source is read-only, that is intentional. Do not change source permissions merely to run a command that may write there.

## Symbolic-link, junction, or reparse-point risk

The skill does not require links. A link can:

- make the apparent install directory point elsewhere;
- create a hidden extra nesting level;
- make a Mode 2 target resolve into the source;
- cause copy tools to include content outside the intended project;
- redirect writes back into a read-only source.

Inspect and resolve links before comparing source and target paths. On uncertainty, use a real directory copy and stop the operation rather than following the link. Never treat a string comparison as proof that two resolved paths are isolated.

## Optional runtime or command is missing

Python, Node.js, Bash, PowerShell, Git, GitHub CLI, `rsync`, and `robocopy` are not core dependencies. If a documented convenience command is unavailable:

1. use the operating system's file manager for copy and inspection;
2. use another available hashing or file-listing mechanism when evidence is required;
3. record the exact check as not run or environment-limited if no equivalent is available;
4. do not install dependencies or access the network without appropriate authorization.

Missing Git does not block the local fact layer and does not authorize `git init`.

## Codex installation path is uncertain

Do not reuse Claude Code paths or infer a Codex path from third-party examples. Open the current official [OpenAI Codex Skills documentation](https://developers.openai.com/codex/skills), note the product/version being used, and follow its documented discovery rules.

`AGENTS.md` remains the tool-neutral project handoff entry even when automatic skill discovery is unavailable. Unless real Codex installation and representative scenarios were executed and recorded, report Codex status as designed for compatibility or pending validation—not fully tested.

## Validation status was overstated

Use only these states:

- passed;
- failed;
- not run;
- environment-limited;
- uncertain.

Correct examples:

- “Static path and casing review passed; macOS real-device installation was not run.”
- “The directory structure is correct, but skill discovery is uncertain because the current session was not restarted.”
- “Designed for Codex compatibility through `AGENTS.md`; automatic discovery has not been tested.”

Incorrect examples include treating documentation review, command availability, simulated POSIX paths, or a successful file copy as proof of complete platform compatibility.

## Update leaves old files or duplicate versions

1. Identify every personal and project-level installation deliberately; do not run a broad deletion.
2. Compare paths and modification sources.
3. Confirm which installation is active.
4. Inspect old directories for local changes.
5. Remove only the exact obsolete directory after authorization.
6. Verify there is one intended `SKILL.md` at the direct installation level.

Do not delete managed-project fact files while cleaning skill installations.

## Uninstall leaves files behind

Check for:

- an extra nested repository directory;
- both a personal and project-level installation;
- ZIP files or extraction directories outside the installation target;
- editor backups, temporary files, or caches;
- links pointing to a directory that still exists elsewhere.

Report leftovers by exact path. Deleting downloads, caches, project files, or linked targets is a separate action and requires confirmation when its scope is not already authorized.

## Platform claim checklist

Before publishing a compatibility statement, record:

| Claim | Required evidence |
|---|---|
| Static cross-platform compatibility review | Encoding, LF policy, path placeholders, casing, dependency, and link checks |
| Simulated convention check | The host platform, simulation method, and limitations |
| Windows representative validation | Actual Windows version, filesystem/path scenarios, encoding checks, installation structure, and stated limitations |
| macOS representative validation | Actual macOS device/version, filesystem/path scenarios, encoding checks, installation structure, and stated limitations |
| Windows fully tested | Representative checks plus dynamic discovery and complete target workflows in the named environment |
| macOS fully tested | Representative checks plus dynamic discovery and complete target workflows in the named environment |
| Codex tested | Actual Codex version, documented install path, discovery result, and representative scenarios |

If the required evidence is absent, narrow the claim rather than upgrading the status.

## GitHub publication fails or targets the wrong repository

Before retrying any outward-facing operation:

1. Stop and inspect `git status`, the current branch, and `git remote -v`.
2. Confirm the intended owner, repository name, visibility, and exact remote URL.
3. Confirm the files and commits to be uploaded contain no secrets or private history.
4. Do not overwrite an existing `origin`, force-push, bypass hooks, or delete a remote repository as a troubleshooting shortcut.
5. Obtain fresh authorization if the proposed recovery changes the previously approved remote, branch, visibility, or publication scope.

A local commit succeeding does not prove a push occurred. A push succeeding does not prove README links, Release assets, visibility, or platform claims are correct. Validate each outcome separately. See the [GitHub publishing guide](github-publishing.zh-CN.md) for the complete publication sequence.
