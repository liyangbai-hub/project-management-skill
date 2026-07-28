# macOS 验证记录

本文件记录 `project-management` 技能在真实 macOS 环境中已经执行的静态质量检查和代表性文件系统验证。它不把文件复制、静态检查或临时目录结构等同于 Claude Code 动态技能发现或三个模式的完整端到端执行。

## 验证环境

- 验证日期：2026-07-27
- 操作系统：macOS 26.5.2（Build 25F84）
- 架构：arm64
- Claude Code：2.1.220
- shell：zsh
- 仓库形态：本地普通文件夹；不是 Git 仓库
- 未执行的外部操作：未初始化 Git，未提交，未创建远程仓库，未 push，未启用 GitHub Pages，未创建 PR 或 Release

## 状态定义

- **通过（passed）**：检查已实际执行，结果满足明确条件。
- **失败（failed）**：检查已执行，结果不满足条件。
- **未执行（not run）**：没有执行。
- **环境受限（environment-limited）**：当前环境不能完成。
- **结果不确定（uncertain）**：已有部分证据，但不足以判断通过或失败。

## 静态质量检查

| 检查 | 状态 | 证据与边界 |
|---|---|---|
| UTF-8 文本 | 通过 | 适用 Markdown、JSON、HTML、配置和文本文件均可按严格 UTF-8 解码。 |
| UTF-8 BOM | 通过 | 适用文本未发现 UTF-8 BOM。 |
| LF 行尾 | 通过 | 适用文本未发现 CRLF；仓库新增 `.gitattributes` 声明文本使用 LF。 |
| `evals/evals.json` | 通过 | JSON 解析成功，共 9 个评测场景；所有 `files` 引用均存在。 |
| Markdown 相对链接 | 通过 | 排除外部 URI、图片和纯锚点后，文件相对链接均解析到现有路径。 |
| 发布占位符 | 通过 | 待替换的仓库用户名发布占位符已从现行发布文件清除；仓库链接使用 `liyangbai-hub/project-management-skill`。历史设计与实施记录仍会描述当时采用过占位符，不属于现行发布配置。 |
| 作者机器绝对路径 | 通过 | 公开文件未包含当前用户目录或项目源绝对路径。 |
| 系统杂项排除 | 通过（规则） | `.gitignore` 已排除 `.DS_Store`、`Thumbs.db`、编辑器缓存和临时输出。仓库中已有的 `.DS_Store` 实际删除需要单独文件删除授权，因此未删除。 |
| 外部技能依赖 | 通过 | 全仓库扫描 `superpowers`、`subagent-driven`、`executing-plans` 均无命中；`SKILL.md` 与四份 `references/*.md` 自足，`docs/maintenance-checks.md` 的检查只需普通文本环境。 |
| 正式规范重复 | 通过 | AUDIT 记录结构、模式二交付报告定义和平台状态各自只在一处维护，其他文件通过引用指向该处。 |

## macOS 代表性 Mode 2 验证

### 目标

验证当前 macOS 文件系统上的以下性质：

- 中文和空格路径可用于源、目标、安装结构和哈希验证；
- 源等于目标、目标位于源内、源位于目标内会被拒绝；
- 目标父路径通过符号链接指向源时会被识别并拒绝；
- 独立目标路径可接受；
- 创建目标副本前后，源文件清单、大小和 SHA-256 不变；
- 目标结构完整，不包含源绝对路径或所测秘密模式。

### 实际结果

```text
PATH_REJECTION_SOURCE_EQUALS_TARGET: REJECT_SAME
PATH_REJECTION_TARGET_INSIDE_SOURCE: REJECT_TARGET_IN_SOURCE
PATH_REJECTION_SOURCE_INSIDE_TARGET: REJECT_SOURCE_IN_TARGET
PATH_ACCEPT_INDEPENDENT: OK
PATH_REJECTION_SYMLINKED_TARGET: REJECT_TARGET_IN_SOURCE
MODE2_SOURCE_INVARIANCE: PASS (2 files)
MODE2_TARGET_STRUCTURE: PASS
MODE2_TARGET_SOURCE_PATH_SCAN: PASS
MODE2_TARGET_SECRET_SCAN: PASS
```

### 结果解释

- **路径隔离：通过。** 三种直接危险关系及一项符号链接回流关系均被拒绝；独立目标被接受。
- **源项目不变性：通过。** 所测两个普通文件的相对路径、字节大小和 SHA-256 前后一致。
- **目标结构：通过。** 目标包含独立事实层、保留的旧 README 和原生 `src/main.txt`。
- **源路径扫描：通过。** 所测目标文本未发现源项目绝对路径或 `/Users/` 路径。
- **敏感模式扫描：通过。** 所测目标未发现长格式 API key、私钥头或密码赋值模式。

该验证使用合成 fixture，不覆盖全部链接类型、文件权限、超大项目、并发修改、异常中止或所有复制工具行为。

## macOS 代表性安装结构验证

将仓库复制到当前用户临时目录中的中文和空格路径后，实际检查：

```text
MACOS_INSTALL_STRUCTURE: PASS
MACOS_INSTALL_REFERENCES: PASS
FRONTMATTER_NAME: PASS
```

确认：

- `SKILL.md` 直接位于临时 `project-management` 技能目录；
- 四个正式 reference 文件存在；
- YAML frontmatter 包含精确的 `name: project-management`；
- 复制后的安装目录不包含任何外部工作流产物目录。

这只证明文件结构，不证明 Claude Code 在该临时目录中的动态发现。

## 未执行和限制

| 项目 | 状态 | 说明 |
|---|---|---|
| 临时安装位置中的 Claude Code 动态技能发现 | 未执行 | 未修改或替换用户当前安装，也未在临时 HOME 中启动新的隔离会话。 |
| 三个模式的真实 Claude Code 端到端执行 | 未执行 | 当前验证为静态检查与代表性文件系统操作。 |
| 本机旧版技能升级 | 明确不在范围 | 用户要求不处理 `~/.claude/skills/project-management`。 |
| Codex 安装、发现与代表性场景 | 未执行 | 没有真实 Codex 环境证据。 |
| Git 和 GitHub Pages 发布 | 未执行 | 用户尚未授权外部发布；当前目录也不是 Git 仓库。 |
| 根目录既有 `.DS_Store` 删除 | 未执行 | 删除工具要求对该精确文件另行授权；`.gitignore` 已保证其不会进入后续 Git 提交。 |

## 结论

本仓库在 macOS 26.5.2 上完成了代表性编码、路径、符号链接隔离、源不变证明和安装结构验证。可以表述为“macOS 代表性验证通过”，但不能表述为“macOS 全部工作流已经完整实测”或“Claude Code 动态发现已经通过”。
