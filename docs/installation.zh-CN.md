# 安装说明

本说明介绍如何为 Claude Code 安装本仓库，以及如何在不制造第二套行为规范的前提下适配其他 Agent。正式执行规则以简体中文 `SKILL.md` 和 `references/*.md` 为准。

## 环境要求

技能核心只要求文件系统能够保留 UTF-8 文本和规定的目录结构。Git、GitHub CLI、Python、Node.js、Bash、PowerShell、`rsync`、`robocopy` 都只是可选便利工具，不是运行前提。

安装前确认下载所得仓库根目录直接包含：

```text
SKILL.md
references/
```

## 为 Claude Code 安装

### 个人级安装

希望技能在多个 Claude Code 项目中可用时，安装到：

```text
~/.claude/skills/project-management/
```

Windows 等价路径：

```text
$HOME\.claude\skills\project-management\
```

### 项目级安装

只希望技能用于某个项目，或希望技能随项目目录共同交接时，安装到：

```text
<project-root>/.claude/skills/project-management/
```

不要把技能安装目录与受管项目事实层混为一谈。项目可以有 `AGENTS.md`、`CLAUDE.md`、`README.md` 和 `DEVLOG.md`，但不一定需要在项目内安装技能副本。

## 安装方式

### 使用 Git clone

Git 是可选工具。若当前环境有 Git，可直接克隆到目标技能目录，也可先克隆到其他位置，再复制仓库内容。

macOS 与 Linux 示例：

```bash
git clone https://github.com/liyangbai-hub/project-management-skill.git \
  "$HOME/.claude/skills/project-management"
```

Windows PowerShell 示例：

```powershell
git clone https://github.com/liyangbai-hub/project-management-skill.git `
  "$HOME\.claude\skills\project-management"
```

若目标目录已经存在，先检查本地改动，并明确选择备份、比较或更新策略，不要直接覆盖。克隆本地仓库不代表获准创建远程仓库、push、创建 PR 或发布 Release。

### 使用 ZIP

1. 从仓库页或 Release 页面下载 ZIP。
2. 解压到临时位置。
3. 打开解压目录，找到**直接包含 `SKILL.md`** 的那一层。
4. 将这一层的全部内容复制到目标技能目录。
5. 确认最终结构为 `<skill-directory>/SKILL.md`。

常见错误结构：

```text
~/.claude/skills/project-management/project-management-skill/SKILL.md
```

应把内层仓库内容上移一级，而不是依赖额外嵌套目录。

### 手工复制

没有 Git 或 shell 时，用文件管理器创建目标目录，并把仓库中的全部文件和目录复制进去。保持文件名的精确大小写，不用快捷方式、替身、符号链接或 Windows 联接点代替真实文件。

## 验证安装

执行所有适用检查：

1. 确认 `SKILL.md` 直接位于安装后的 `project-management` 目录中。
2. 确认以下文件存在：
   - `references/project-templates.md`
   - `references/migration-copy-mode.md`
   - `references/audit-checklist.md`
   - `references/cross-platform.md`
3. 确认 YAML frontmatter 中的名称为 `project-management`。
4. 已被发现的技能目录中发生增删改时，Claude Code 通常会在当前会话实时生效。如果会话启动时顶层 `~/.claude/skills/` 或项目 `.claude/skills/` 尚不存在，而本次才首次创建，技能没有出现时请重新打开会话。
5. 请求 Agent 只读说明技能的三个模式，或使用当前 Claude Code 版本提供的技能列表机制检查发现情况。
6. 将结果如实记录为通过、失败、未执行、环境受限或结果不确定。

仅检查文件存在只能证明安装结构，不能证明全部工作流、操作系统或 Agent 集成都经过实测。

## 更新

1. 确认当前使用的是个人级还是项目级安装。
2. 如有本地自定义内容，先备份并记录。
3. 通过 Git 或 ZIP 获取新版本。
4. 替换前比较现有目录与新版本。
5. 发现无法识别的用户文件或冲突时停止并逐项处理，不静默覆盖。
6. 复制新版本仓库内容，确保 `SKILL.md` 仍处于直接层级。
7. 重新执行安装验证。

若安装目录本身是 Git clone，可在理解工作树和远程状态后选择 `git pull`；它不是核心要求，也不能被用于丢弃本地修改。

## 卸载

只删除明确要移除的技能安装目录：

```text
~/.claude/skills/project-management/
```

或：

```text
<project-root>/.claude/skills/project-management/
```

删除前先查看目录中是否有本地自定义内容。卸载技能**不代表**获准删除受管项目，或删除项目中的 `AGENTS.md`、`CLAUDE.md`、`README.md`、`DEVLOG.md`、`CHANGELOG.md`、需求材料和审查历史。

卸载后检查是否仍有另一份个人级或项目级安装，以及是否遗留额外嵌套的旧版本。

## Codex 与其他 Agent

`AGENTS.md` 仍是受管项目的工具中立接力入口。当前 Codex 官方文档列出的本地技能位置如下：

| 范围 | 目标目录 |
|---|---|
| 个人级 | `$HOME/.agents/skills/project-management/` |
| 仓库级 | `<project-root>/.agents/skills/project-management/` |
| 管理员级 | `/etc/codex/skills/project-management/` |

个人级 Git clone 示例：

```bash
git clone https://github.com/liyangbai-hub/project-management-skill.git \
  "$HOME/.agents/skills/project-management"
```

部分现有 Codex 版本或较早的本地安装仍可能从 `~/.codex/skills/project-management/` 加载技能。请把它视为兼容位置，而不是当前首选路径。按已安装版本查看 OpenAI 官方 [Codex Skills 文档](https://developers.openai.com/codex/skills)，并通过 `/skills` 或显式输入 `$project-management` 核验发现情况。Codex 会自动检测本地技能变化；若更新没有出现，再重启 Codex。

确需适配层时，优先建立指向本仓库的薄入口，不复制完整行为规范。

除非发布记录明确列出真实测试的 Codex 版本、环境、发现方式和场景，否则不得宣传为已经完成 Codex 全面实测。

## 平台状态

- **Windows：** 已在 Windows 11 + PowerShell 5.1 完成代表性文件系统、路径安全、编码和安装结构验证；动态技能发现与三个模式的完整执行未运行。见 [Windows 验证记录](validation-windows.md)。
- **macOS：** 已在 macOS 26.5.2（arm64）+ Claude Code 2.1.220 完成代表性文件系统、符号链接路径安全、编码和安装结构验证；动态技能发现与三个模式的完整执行未运行。见 [macOS 验证记录](validation-macos.md)。
- **Codex：** 围绕 `AGENTS.md` 和保守的技能集成原则设计；真实环境完整验证待完成。

发现、嵌套、大小写、编码、权限或卸载残留问题见[故障排查](troubleshooting.md)。
