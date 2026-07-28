# Windows 验证记录

本文件记录 `project-management` 技能在当前 Windows 环境中的静态质量检查和代表性文件系统验证。它只陈述实际执行过的检查，不把静态审阅、模拟验证或文件复制等同于真实 Agent 发现测试或跨平台实测。

## 验证环境

- 验证日期：2026-07-27
- 操作系统：Windows 11 Home China 10.0.26200
- 主要 shell：Windows PowerShell 5.1
- 仓库形态：本地普通文件夹；不是 Git 仓库
- 验证对象：本仓库正式行为文件、公开文档、示例、评测材料，以及临时目录中的代表性 Mode 2 和安装副本
- 未执行的外部操作：未初始化 Git，未提交，未创建远程仓库，未 push，未创建 PR 或 Release，未发布任何内容

## 状态定义

本文使用以下状态：

- **通过（passed）**：检查已实际执行，结果满足该检查的明确条件。
- **失败（failed）**：检查已执行，但命令、过程或结果未满足条件。
- **未执行（not run）**：没有执行该测试。
- **环境受限（environment-limited）**：环境不足以完成测试。
- **结果不确定（uncertain）**：已取得部分证据，但不足以支持通过或失败结论。

## 静态质量检查

| 检查 | 状态 | 证据与边界 |
|---|---|---|
| 必需文件存在 | 通过（修正验证器后） | 已按当日仓库文件清单复查 34 个入口、正式引用、双语首页、维护文件、安装与发布文档、示例和评测文件，输出 `REQUIRED_FILES: PASS (34)`。根目录 `DEVLOG.md` 不属于本仓库自身的维护文件；它是技能为受管项目生成的事实层文件，并已在示例中验证。 |
| `SKILL.md` YAML frontmatter | 通过 | 确认首行为 `---`、顶部存在闭合分隔符、包含 `name: project-management`，且 `description` 非空；输出 `FRONTMATTER: PASS`。 |
| UTF-8 文本 | 通过 | 已对 36 个适用文本文件执行严格 UTF-8 解码检查，输出 `UTF8: PASS (36)`。 |
| 无 UTF-8 BOM | 通过 | 已检查适用文本文件，输出 `NO_UTF8_BOM: PASS`。 |
| LF 行尾 | 通过 | 已检查适用文本文件，输出 `LF_ONLY: PASS`。 |
| `evals/evals.json` JSON 解析 | 通过 | 使用显式 UTF-8 读取后 JSON 解析成功，输出 `EVAL_JSON_UTF8: PASS (9)`。 |
| Markdown 相对链接 | 通过 | 枚举 Markdown 文件，排除外部 URI、图片和纯锚点，移除片段并 URL 解码后按源文件目录解析；输出 `RELATIVE_LINKS: PASS`。 |
| 正式来源边界 | 通过 | `SKILL.md`、`references/*.md`、仓库入口和人类文档一致声明：只有 `SKILL.md` 与 `references/*.md` 定义正式行为。 |
| AUDIT 标识符命名空间 | 通过（修正后） | 最终审阅发现审查轮次和建议曾同时使用 `R-` 前缀，可能造成引用歧义；现保留审查轮次 `R-<YYYYMMDD>-<序号>`，将建议稳定 ID 统一为 `REC-<YYYYMMDD>-001`，并在两份正式模板中复查，输出 `AUDIT_ID_NAMESPACES: PASS`。 |
| 公开材料敏感信息扫描 | 通过 | 对预定公开、正式及人类可读文件执行具体凭据和私钥模式扫描，输出 `SECRET_SCAN: PASS`。示例中的占位值为不可用的合成值。 |
| 作者机器绝对路径 | 通过（修正后） | 当日发现的作者机器路径已替换为 `<repo-root>` 占位符；最终复扫输出 `AUTHOR_PATH_SCAN: PASS`。 |
| 发布占位符（验证当日） | 失败（当时的发布阻断） | 验证当日扫描输出 `RELEASE_PLACEHOLDER: BLOCKED`：`LICENSE` 和 `CHANGELOG.md` 当时仍含待替换的仓库用户名占位符。该历史结果不回写为通过；2026-07-27 后续发布准备已将署名和链接替换为 `liyangbai-hub/project-management-skill`，当前状态见“当前发布阻断项”。 |

### 未计为通过的静态检查尝试

1. 一次组合式 Bash/Python 验证命令以退出码 `49` 结束，且没有产生足以判断各子检查结果的有效输出。该次尝试记为**失败**，未被转换为成功结论。后续使用独立 PowerShell 检查替代了适用项目。
2. 早期 frontmatter 正则检查曾输出 `FRONTMATTER: FAIL`。复核发现该检查器的多行匹配方式不可靠；随后使用确定性的逐行条件重新验证并得到 `FRONTMATTER: PASS`。因此，早期结果是验证器误报，不是对 `SKILL.md` 缺陷的确认。
3. 早期宽泛的敏感信息扫描命中了当日维护文档中的扫描规则示例。人工区分规则文本与真实敏感值后，改为扫描预定公开文件及相关内容；未发现真实凭据。规则示例本身不作为泄露证据。
4. 一次必需文件验证器把根目录 `DEVLOG.md` 错列为本技能仓库发布文件，输出 `REQUIRED_FILES: FAIL`。`DEVLOG.md` 是技能为受管项目生成的事实层文件，本仓库自身不要求在根目录维护它。修正验证器边界后，按当日仓库文件清单重新检查。
5. Windows PowerShell 5.1 中，一次使用 `Get-Content -Raw | ConvertFrom-Json` 的检查因默认解码导致中文变为乱码，并输出 JSON 解析失败。随后改用 `[System.IO.File]::ReadAllText($path, [System.Text.Encoding]::UTF8)` 读取同一文件，得到 `EVAL_JSON_UTF8: PASS (9)`；早期失败属于检查器编码问题。

## Windows 代表性 Mode 2 验证

### 验证目标

验证以下可观察性质：

- 源项目在整个独立副本构建过程中保持不变；
- 危险的源/目标路径关系被拒绝；
- 标准化目标是独立目录，并包含事实层和原生项目内容；
- 目标不残留源绝对路径；
- 目标不包含所测凭据或密钥模式；
- 中文和空格路径不会破坏代表性文件复制及结构检查。

### 过程

1. 在当前用户临时目录下创建带中文和空格的 GUID 唯一目录。
2. 将 `evals/fixtures/legacy-project` 复制为合成源项目。
3. 对源项目建立前置快照，记录每个文件的：
   - 相对路径；
   - 文件大小；
   - SHA-256 内容哈希。
4. 测试并拒绝四种危险情况：
   - 源等于目标；
   - 目标位于源内；
   - 源位于目标内；
   - 目标非空。
5. 在独立目标目录中构建标准化副本：
   - 保留原生 `src/`；
   - 复制已核对为安全模板的 `.env.example`；
   - 将旧 README 保留为 `LEGACY-README.md`；
   - 生成 `README.md`、`AGENTS.md`、`CLAUDE.md`、`DEVLOG.md` 和 `CHANGELOG.md`。
6. 再次计算源项目快照，并要求前后相对路径、大小和 SHA-256 完全一致。
7. 验证目标结构，并扫描源绝对路径和所测敏感模式。

### 第一次尝试中的失败

第一次 PowerShell 尝试在重建固定临时目录前调用了删除操作，安全控制返回：

```text
Remove-Item on system path '\' is blocked. This path is protected from removal.
```

该次尝试记为**失败**，没有把它纳入成功证据。修正方法不是放宽删除权限，而是改用新的 GUID 唯一目录并取消前置删除步骤。

### 修正后的结果

```text
MODE2_SOURCE_INVARIANCE: PASS
MODE2_SOURCE_FILES: 3
PATH_REJECTION_SOURCE_EQUALS_TARGET: PASS
PATH_REJECTION_TARGET_INSIDE_SOURCE: PASS
PATH_REJECTION_SOURCE_INSIDE_TARGET: PASS
PATH_REJECTION_NON_EMPTY_TARGET: PASS
MODE2_TARGET_STRUCTURE: PASS
MODE2_TARGET_SOURCE_PATH_SCAN: PASS
MODE2_TARGET_SECRET_SCAN: PASS
WINDOWS_INSTALL_STRUCTURE: PASS
FILE_ONLY_HANDOFF: PASS
TEMP_FIXTURE: <user-temp>\项目 管理 验证 a0df6fd099ce40a5b098794d740361d0
```

### 结果解释

- **源项目不变性：通过。** 三个源文件的相对路径、大小和 SHA-256 在构建前后完全一致。
- **路径隔离拒绝：通过。** 四种所测危险情况均被拒绝。
- **目标结构：通过。** 独立目标包含要求的事实层文件和原生内容。
- **源路径扫描：通过。** 在所测目标内容中未发现源项目绝对路径。
- **敏感模式扫描：通过。** 在所测目标内容中未发现测试规则覆盖的凭据或密钥值。
- **中文与空格路径：通过。** 所测复制、哈希、结构验证和内容扫描在该临时路径下正常完成。

这些结果证明当前 Windows 环境中的代表性文件系统行为，不证明所有复制工具、链接类型、权限模型、超大项目或异常中断场景都已覆盖。

## Windows 代表性安装结构验证

将仓库复制到另一个带中文和空格的临时安装路径后，检查：

- `SKILL.md` 直接位于 `project-management` 技能目录；
- 以下正式引用文件存在：
  - `references/project-templates.md`
  - `references/migration-copy-mode.md`
  - `references/audit-checklist.md`
  - `references/cross-platform.md`

结果：`WINDOWS_INSTALL_STRUCTURE: PASS`。

该结果只证明文件复制后的安装结构。当前验证没有在该临时安装位置启动新的隔离 Claude Code 会话，因此不能把它表述为动态技能发现通过。

## 文件接力示例验证

对 `examples/new-project` 执行文件层检查，确认示例提供：

- `AGENTS.md` 中的接力阅读顺序；
- `README.md` 中的项目目标、边界和原生布局说明；
- `DEVLOG.md` 中的当前状态、下一步、阻断项、验证命令及稳定工作单元记录；
- 可供接手者核对的原生项目文件。

结果：`FILE_ONLY_HANDOFF: PASS`。

这证明示例具备规定的文件导航和当前状态信息，不证明任意 Agent 仅凭文档就能完全理解所有运行时事实。关键声明仍应与代码、配置和适用命令交叉核验。

## 未执行和待验证项目

| 项目 | 状态 | 说明 |
|---|---|---|
| 临时安装位置中的 Claude Code 动态技能发现 | 未执行 | 未针对该复制位置启动新的隔离会话或使用版本特定的技能列表机制。 |
| 三个模式的真实 Claude Code 端到端执行 | 未执行 | 当前结果主要来自静态检查、合成 fixture 和代表性文件系统操作。 |
| macOS 真实设备安装与行为 | 未执行 | 没有 macOS 设备、版本、安装层级、发现方式或场景证据。 |
| Codex 真实环境安装与发现 | 未执行 | 没有真实 Codex 版本、官方目录下的安装、发现结果或代表性场景证据。 |
| 特殊链接完整验证 | 未执行 | 当前 fixture 未建立符号链接、junction 或其他 reparse point；正式规范要求遇到此类对象时先解析并重新验证隔离。 |
| Git 工作流 | 未执行 | Git 是可选项，当前目录也不是 Git 仓库；未执行 `git init`、commit 或任何远程操作。 |

## 当前发布阻断项

1. 仓库链接与 MIT 署名已经配置为 `liyangbai-hub/project-management-skill`；原发布占位符阻断已解除。
2. macOS 代表性验证已经在 macOS 26.5.2（arm64）执行并记录于 `docs/validation-macos.md`；动态技能发现和完整端到端仍未执行。
3. Codex 真实环境验证未执行；不得宣传为全面实测。
4. Claude Code 临时安装副本的动态发现未执行；Windows 结论只能表述为代表性安装结构和文件系统验证。
5. 尚未获得初始化 Git、提交、创建远程仓库、push、PR、Release、启用 Pages 或其他外部发布授权。

当前没有署名或仓库链接占位符阻止上传。动态发现、完整端到端和 Codex 实测可作为明确披露的预发布限制保留；外部发布操作仍须另行获得明确授权。

## 临时 fixture 与清理边界

验证产生的临时 fixture 位于：

```text
<user-temp>\项目 管理 验证 a0df6fd099ce40a5b098794d740361d0
```

本轮没有删除该目录。删除临时目录不影响上述证据成立；如需清理，应先精确核对该路径，并把清理视为独立的删除操作，不使用宽泛路径或递归匹配。
