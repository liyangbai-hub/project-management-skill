# GitHub 发布指南

本文面向第一次把本地技能发布到 GitHub 的维护者。预定仓库为 [`liyangbai-hub/project-management-skill`](https://github.com/liyangbai-hub/project-management-skill)，预定 Pages 地址为 <https://liyangbai-hub.github.io/project-management-skill/>。本文只说明发布操作，不构成第二套技能行为规范；正式行为以 [`SKILL.md`](../SKILL.md) 和 [`references/*.md`](../references/) 为准。

> **本轮状态：** 文件已在本地准备，但尚未执行 `git init`、commit、创建远程仓库、上传、push 或启用 Pages。下面的外部操作要由仓库所有者明确决定并执行。
>
> **授权边界：** 创建远程仓库、上传文件、`git push`、启用 Pages、创建 PR、Discussions、Release 和其他外部发布都会把内容发送到外部服务。只有在仓库所有者明确授权相应动作后才能执行。

## 先认准要上传的文件夹

当前仓库根目录是**直接包含以下文件的那一层**：

```text
project-management/
├── SKILL.md
├── README.md
├── README.zh-CN.md
├── index.html
├── references/
├── docs/          # 安装、发布、故障排查、验证记录、维护清单
├── examples/
└── evals/
```

GitHub 上的仓库名虽然叫 `project-management-skill`，但上传时应选择上面这一层中的**全部内容**。不要先制作一个额外的同名套娃文件夹。上传后正确地址应是：

```text
https://github.com/liyangbai-hub/project-management-skill/blob/main/SKILL.md
```

而不是：

```text
.../blob/main/project-management/SKILL.md
```

## 发布前门槛

公开发布前逐项确认：

1. `LICENSE` 的署名为 `liyangbai-hub`。
2. `CHANGELOG.md` 的仓库链接指向 `liyangbai-hub/project-management-skill`。
3. `README.md` 与 `README.zh-CN.md` 的语言跳转、本地链接和 Pages 入口有效。
4. `index.html` 的中文/English 原地切换、移动端布局和无 JavaScript 回退已经检查。
5. `SKILL.md`、`references/`、`docs/`、`examples/` 和 `evals/evals.json` 均已完成适用验证。
6. 仓库中不存在真实凭据、客户资料、内部 URL、作者机器绝对路径或私有项目内容。
7. Windows、macOS、Claude Code 和 Codex 的声明只反映真实证据；未执行项目明确写为待验证、未执行或环境受限。
8. 已确认这次发布的准确范围，并获得创建远程仓库和上传内容的明确授权。

任何一项未满足时，记录为发布阻塞项，不把仓库描述为正式可发布版本。

## 方法一：使用 GitHub 网页上传

此方法不要求安装 Git 或 GitHub CLI。

### 1. 创建仓库

1. 登录 GitHub，打开创建新仓库页面。
2. Repository name 填写 `project-management-skill`。
3. Owner 确认选择 `liyangbai-hub`。
4. Visibility 建议选择 **Public**，这样其他人才能直接查看和安装；如果暂不公开则选 **Private**：
   - Public：任何人可访问，上传前必须完成隐私和敏感信息检查。
   - Private：仅授权成员可访问，但仍属于外部上传。
5. 如果本地仓库已经包含 `README.md`、`.gitignore` 和 `LICENSE`，不要在网页上预生成这些文件，以免首次上传产生冲突。
6. 在点击创建前再次确认已获得外部发布授权。

### 2. 上传现有文件

1. 进入新仓库，选择上传现有文件。
2. 上传本地仓库根目录的**内容**，不要额外套一层 `project-management-skill/` 目录。
3. 检查上传列表至少包含：
   - `SKILL.md`
   - `references/`
   - `README.md`
   - `README.zh-CN.md`
   - `index.html`
   - `.nojekyll`
   - `.gitignore`
   - `.gitattributes`
   - `docs/`
   - `examples/`
   - `evals/`
   - `LICENSE`
4. 确认 `.env`、私钥、评测输出、缓存和本地编辑器文件未被加入。
5. 填写准确的首次提交说明，例如 `Initial public release`。
6. 提交前再次核对上传范围；网页提交会立即写入远程仓库。

### 3. 检查远程首页

上传完成后：

1. 打开默认展示的英文 `README.md`。
2. 点击“简体中文”链接，确认能打开 `README.zh-CN.md`。
3. 从中文页返回 English，确认双向链接有效。
4. 检查 Mermaid 图、相对链接、代码块和目录树的 GitHub 渲染。
5. 确认页面没有把待验证的 macOS 或 Codex 状态写成已测试。
6. 确认仓库页没有出现真实秘密、内部路径或错误署名。

## 方法二：使用 Git 命令行发布

GitHub CLI（`gh`）**不是必需工具**。以下流程使用普通 Git；每条命令都应在理解当前目录和状态后单独执行。

### 1. 在 GitHub 创建空仓库

先通过 GitHub 网页创建空的 `project-management-skill` 仓库，不预生成 README、LICENSE 或 `.gitignore`。复制准确的 HTTPS 远程地址。

### 2. 初始化和检查本地仓库

只有在所有者明确授权本地 Git 初始化和提交后，才在仓库根目录执行：

```powershell
git init
git status
git add .
git status --short
git commit -m "Initial public release"
git branch -M main
```

说明：

- `git init` 只创建本地仓库，但仍属于需要授权的结构性操作。
- 第一次 `git status --short` 必须用于检查暂存范围，避免提交秘密或无关文件。
- `git commit` 只写本地历史，不等同于外部发布。
- 不使用 `--no-verify` 绕过检查。

### 3. 连接远程并发布

确认远程 URL 和上传授权后执行：

```powershell
git remote add origin https://github.com/liyangbai-hub/project-management-skill.git
git remote -v
git push -u origin main
```

`git push` 是实际外部发布动作。执行前检查：

- 远程地址应为 `https://github.com/liyangbai-hub/project-management-skill.git`；
- 当前分支和待推送提交正确；
- 没有将私有历史或不应公开的大文件纳入提交；
- 用户明确授权本次 push。

如果 `origin` 已存在，不要直接覆盖。先运行 `git remote -v`，查明现有远程的来源和用途，再由所有者决定是沿用、改名还是修改。

## 启用双语 GitHub Pages 首页

仓库上传成功后，`index.html` 已经在根目录，但 Pages 不会自动启用。由仓库所有者在 GitHub 网页执行：

1. 打开 <https://github.com/liyangbai-hub/project-management-skill>。
2. 点击仓库顶部 **Settings**。
3. 在左侧找到 **Pages**。
4. 在 **Build and deployment** 下，把 Source 设为 **Deploy from a branch**。
5. Branch 选择 `main`，目录选择 `/ (root)`，点击 **Save**。
6. 等待 GitHub 完成部署；首次通常需要数十秒到数分钟。
7. 打开 <https://liyangbai-hub.github.io/project-management-skill/>。
8. 验证：
   - 点击“中文 / EN”时文字在当前页面原地切换；
   - 刷新后仍保持上次语言；
   - “立即安装”“查看三种模式”“打开 GitHub”链接有效；
   - 手机宽度下没有横向溢出；
   - README 中的 Pages 链接可以打开该网页。

`.nojekyll` 会让 GitHub Pages 直接发布静态文件；此页面不需要 Node.js、构建命令、主题或第三方服务。启用 Pages 是外部发布动作，本地文件准备完成不等于 Pages 已经上线。

## 仓库设置建议

以下设置都是可选项，应由仓库所有者决定。

### About

可使用简短描述：

```text
Keep project facts in local files for traceable multi-Agent handoffs, safe standardized copies, and evidence-based reviews.
```

Website 填写：

```text
https://liyangbai-hub.github.io/project-management-skill/
```

### Topics

可考虑：

```text
claude-code
agent-skills
project-management
multi-agent
ai-workflow
knowledge-handoff
```

Topics 是公开元数据。不要加入组织内部代号或敏感分类。

### Issues

适合收集：

- 可复现的模式选择问题；
- Mode 2 路径或源只读问题；
- Mode 3 证据与授权边界问题；
- 安装、发现和跨平台问题；
- 文档链接和示例问题。

Issue 模板不得要求用户上传秘密、私有仓库或客户数据。必要时让报告者创建最小合成 fixture。

### Discussions

可用于用法交流和设计讨论。启用 Discussions 会创建新的公开交互面；启用前应确认维护者愿意管理内容和社区边界。

## 可选的首个 Release

首个 Release 不是上传仓库的必需步骤。只有在以下条件全部满足并获得明确授权后，才考虑 `v1.0.0`：

1. 发布前门槛全部通过；
2. MIT 署名和仓库链接已替换；
3. 行为评测和 Windows 代表性验证已记录；
4. macOS、Codex 等未测试状态在 Release notes 中如实披露；
5. 安装文档和 ZIP 嵌套说明已验证；
6. 所有者明确授权创建 tag 和 Release。

Release notes 建议包含：

- 三种模式和本地事实层摘要；
- 已验证的平台、版本、安装级别和场景；
- 未执行或环境受限的验证；
- 已知限制和升级注意事项；
- 安装文档链接；
- 安全与授权边界。

不要仅因文件齐全就称为稳定版，也不要把静态分析写成真实设备测试。

## 从 GitHub 安装

Claude Code 的个人级和项目级路径、Git clone、ZIP 和手工复制步骤见：

- [中文安装说明](installation.zh-CN.md)
- [English installation guide](installation.en.md)
- [故障排查](troubleshooting.md)

安装后的关键结构是：

```text
<skill-directory>/SKILL.md
<skill-directory>/references/
```

如果 ZIP 解压后多出一层仓库目录，应复制直接包含 `SKILL.md` 那一层的内容，而不是依赖额外嵌套。

## 更新已安装技能

1. 确认当前生效的是个人级还是项目级安装。
2. 检查安装目录是否有用户自定义文件或本地修改。
3. 下载新 ZIP 或在明确理解工作树状态时使用 `git pull`。
4. 比较新旧内容；发现冲突或无法识别的文件时停止，不静默覆盖。
5. 只更新技能目录，不动受管项目的事实层和历史。
6. 重新验证 `SKILL.md` 直接层级、references 完整性和发现行为。

更新动作不自动授权远程 push，也不授权丢弃本地修改。

## 卸载已安装技能

删除前查看准确目标目录并确认其中没有需要保留的自定义内容。只删除明确要卸载的技能目录：

```text
~/.claude/skills/project-management/
```

或：

```text
<project-root>/.claude/skills/project-management/
```

卸载技能不代表获准删除任何受管项目，也不代表可以删除项目中的 `AGENTS.md`、`CLAUDE.md`、`README.md`、`DEVLOG.md`、`CHANGELOG.md`、需求材料或 `AUDIT.md`。

## Codex 发布说明

本仓库使用 `AGENTS.md` 作为工具中立的项目接力入口，但不猜测 Codex 的技能安装目录或自动发现行为。发布页应链接当前 OpenAI 官方 [Codex Skills 文档](https://developers.openai.com/codex/skills)，并把 Codex 状态限定为实际验证范围。

只有真实记录了 Codex 版本、操作系统、安装方式、发现结果和代表性场景后，才能写“Codex 已测试”。需要适配时优先使用指向正式规范的薄入口，不复制维护另一套行为规则。

## 发布后的维护

- 通过 PR 修改正式行为时，同步更新受影响的 examples 和 `evals/evals.json`。
- 每次 Release 记录实际验证证据和已知限制。
- 不在 Issue、Discussion、日志或 fixture 中粘贴真实秘密和私人数据。
- 外部贡献要求见 [`CONTRIBUTING.md`](../CONTRIBUTING.md)。
- 如果发现公开内容包含秘密，先按平台能力限制暴露并轮换凭据；删除当前文件不等于秘密已从 Git 历史、缓存或索引中消失。
