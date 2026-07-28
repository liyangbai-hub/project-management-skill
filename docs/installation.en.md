# Installation

This guide explains how to install the repository for Claude Code and how to approach other Agents without turning this document into a second behavioral specification. The formal instructions are the Simplified Chinese `SKILL.md` and `references/*.md` files.

## Requirements

The skill itself requires only a filesystem that can preserve UTF-8 text files and the documented directory structure. Git, GitHub CLI, Python, Node.js, Bash, PowerShell, `rsync`, and `robocopy` are optional conveniences, not runtime requirements.

Before installing, confirm that the downloaded repository root directly contains:

```text
SKILL.md
references/
```

## Install for Claude Code

### Personal installation

Use a personal installation when the skill should be available across your Claude Code projects.

Target directory:

```text
~/.claude/skills/project-management/
```

Windows equivalent:

```text
$HOME\.claude\skills\project-management\
```

### Project-level installation

Use a project-level installation when the skill should travel with or apply only to one project.

Target directory:

```text
<project-root>/.claude/skills/project-management/
```

Do not confuse the skill installation with the managed project's fact-layer files. A project may contain `AGENTS.md`, `CLAUDE.md`, `README.md`, and `DEVLOG.md` without embedding a personal skill installation.

## Installation methods

### Git clone

Git is optional. If it is available, clone the repository into the target skill directory or clone elsewhere and copy the repository contents into that directory.

macOS and Linux example:

```bash
git clone https://github.com/liyangbai-hub/project-management-skill.git \
  "$HOME/.claude/skills/project-management"
```

Windows PowerShell example:

```powershell
git clone https://github.com/liyangbai-hub/project-management-skill.git `
  "$HOME\.claude\skills\project-management"
```

If the target directory already exists, inspect it for local changes and choose an explicit backup, comparison, or update strategy instead of cloning over it. Creating a local clone does not authorize creating a remote repository, pushing, opening a pull request, or publishing a release.

### ZIP download

1. Download the repository ZIP from its release or repository page.
2. Extract it to a temporary location.
3. Open the extracted directory and locate the level that directly contains `SKILL.md`.
4. Copy the **contents of that directory** into the target skill directory.
5. Confirm the final path is `<skill-directory>/SKILL.md`.

A common incorrect result is:

```text
~/.claude/skills/project-management/project-management-skill/SKILL.md
```

Move the inner repository contents up one level instead of relying on that extra nesting.

### Manual copy

Without Git or shell commands, create the target directory in your file manager and copy all repository files and directories into it. Preserve exact filename casing and do not replace files with shortcuts, aliases, symbolic links, or junctions.

## Verify the installation

Perform all applicable checks:

1. Confirm `SKILL.md` is directly inside the installed `project-management` directory.
2. Confirm `references/project-templates.md`, `references/migration-copy-mode.md`, `references/audit-checklist.md`, and `references/cross-platform.md` are present.
3. Confirm the name in the YAML frontmatter is `project-management`.
4. Changes inside an already discovered skill directory normally take effect in the current Claude Code session. If the top-level `~/.claude/skills/` or project `.claude/skills/` directory did not exist when the session started and was just created, open a new session if the skill is not discovered.
5. Ask for a non-destructive explanation of the skill's three modes, or use Claude Code's current skill-listing mechanism if available in your installed version.
6. Record the result accurately as passed, failed, not run, environment-limited, or uncertain.

A file-presence check proves only installation structure. It does not prove that every workflow, operating system, or Agent integration was tested.

## Update

1. Identify whether the active installation is personal or project-level.
2. Back up local customizations, if any.
3. Obtain the new release by Git or ZIP.
4. Compare the existing directory with the new release before replacing files.
5. If unrecognized user files or conflicts are present, stop and resolve them explicitly; do not silently overwrite them.
6. Copy the new repository contents so `SKILL.md` remains at the same direct level.
7. Repeat the verification checks.

If the installation is a Git clone, `git pull` is an optional update method only when the working tree and remote are understood. It is not a core requirement and must not be used to discard local changes.

## Uninstall

Delete only the specific installation directory that you intend to remove:

```text
~/.claude/skills/project-management/
```

or:

```text
<project-root>/.claude/skills/project-management/
```

Before deletion, inspect the directory for local customizations. Removing the skill does **not** authorize deleting managed projects or their `AGENTS.md`, `CLAUDE.md`, `README.md`, `DEVLOG.md`, `CHANGELOG.md`, requirements, or audit history.

After uninstalling, check for a second personal or project-level installation and for an old extra-nested copy.

## Codex and other Agents

`AGENTS.md` remains the tool-neutral handoff entry for managed projects. Current Codex documentation lists these local skill locations:

| Scope | Target directory |
|---|---|
| Personal | `$HOME/.agents/skills/project-management/` |
| Repository | `<project-root>/.agents/skills/project-management/` |
| Administrator | `/etc/codex/skills/project-management/` |

Personal Git clone example:

```bash
git clone https://github.com/liyangbai-hub/project-management-skill.git \
  "$HOME/.agents/skills/project-management"
```

Some existing Codex versions or older local setups may still load skills from `~/.codex/skills/project-management/`. Treat that as compatibility guidance, not the preferred current location. Check the official [OpenAI Codex Skills documentation](https://developers.openai.com/codex/skills) for your installed version and verify discovery with `/skills` or explicit `$project-management` invocation. Codex detects local skill changes automatically; if an update is not visible, restart Codex.

If an adapter is needed, prefer a thin entry layer that points to this repository rather than maintaining a duplicate behavioral specification.

Codex compatibility has not been fully tested unless a release record explicitly names the tested Codex version, environment, discovery method, and scenarios.

## Platform status

- **Windows:** representative filesystem, path-safety, encoding, and installation-structure checks passed on Windows 11 with PowerShell 5.1. Dynamic skill discovery and full three-mode execution were not run. See [Windows validation](validation-windows.md).
- **macOS:** representative filesystem, symlink path-safety, encoding, and installation-structure checks passed on macOS 26.5.2 (arm64) with Claude Code 2.1.220. Dynamic skill discovery and full three-mode execution were not run. See [macOS validation](validation-macos.md).
- **Codex:** designed around `AGENTS.md` and conservative skill integration; full real-environment validation remains pending.

See [Troubleshooting](troubleshooting.md) for discovery, nesting, casing, encoding, permission, and cleanup problems.
