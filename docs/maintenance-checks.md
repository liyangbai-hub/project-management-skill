# 维护检查清单

本文件面向维护者，列出修改本仓库后应执行的检查。它不是行为规范；正式行为以 [`SKILL.md`](../SKILL.md) 和 [`references/*.md`](../references/) 为准。

本清单不依赖任何外部工作流、插件或额外技能，只需要一个能读写 UTF-8 文本的环境。可用的脚本语言（Python、PowerShell、Shell 等）都可以，用哪种由当前环境决定。

## 一、静态检查

修改任何文件后适用：

- 仓库必需文件存在：`SKILL.md`、四份 `references/*.md`、两份 README、`index.html`、`LICENSE`、`AGENTS.md`、`CLAUDE.md`、`CHANGELOG.md`、`CONTRIBUTING.md`、`docs/`、`examples/`、`evals/evals.json`。
- `SKILL.md` 的 YAML frontmatter 可解析，且包含 `name: project-management` 与非空 `description`。
- `SKILL.md` 引用的每个 `references/*.md` 都存在。
- 全部 Markdown 相对链接和 `index.html` 的本地链接可解析。
- 文本为严格 UTF-8、无 BOM、LF 行尾。
- 关键文件大小写精确：`SKILL.md`、`AGENTS.md`、`CLAUDE.md`、`README.md`、`DEVLOG.md`、`CHANGELOG.md`、`AUDIT.md`。
- 只有 `SKILL.md` 与 `references/*.md` 声明正式行为，人类文档没有变成第二套执行规范。
- 没有作者机器绝对路径、用户名、盘符或私有环境变量。
- 没有真实凭据、令牌、私钥或个人数据。
- 没有发布阻断占位符（例如未替换的仓库所有者名）。
- 核心行为没有新增对 Python、Node.js、Bash、PowerShell、Git、GitHub CLI、`rsync`、`robocopy`、符号链接或任何外部技能的硬依赖。
- 模式二文本中没有“原地重组”“移动源文件”等与源只读矛盾的语义。
- DEVLOG 历史规则、Git 授权规则在各文件间没有冲突。
- 兼容性与验证声明没有超出 `docs/validation-windows.md`、`docs/validation-macos.md` 记录的证据。

## 二、行为评测

`evals/evals.json` 覆盖九个场景：

| ID | 场景 |
|---|---|
| E-01 | 模式一新建代码项目 |
| E-02 | 模式一新建研究项目 |
| E-03 | 材料索引与历史 `[已读]` 兼容 |
| E-04 | 模式二独立标准化副本 |
| E-05 | 模式二危险路径关系 |
| E-06 | 模式三只读审查 |
| E-07 | 模式三授权范围内修复 |
| E-08 | 无聊天记录的多 Agent 接力 |
| E-09 | 不应触发完整流程的小请求 |

修改正式行为时，同步复核受影响场景的断言、`examples/` 和 fixture；每项结果记录为通过、失败、未执行、环境受限或结果不确定。

## 三、多 Agent 接力验收

Agent A 建立事实层、完成部分工作并更新“当前状态”；Agent B 只接收项目文件，不获得任何聊天记录。Agent B 应能说明：

- 项目目的；
- 当前阶段；
- 最近完成；
- 下一步；
- 阻塞项；
- 关键验证命令；
- 后续记录应写在哪里。

Agent B 还须回到实际代码与配置核验关键结论，不把文档声明当作运行事实。

## 四、跨平台检查

在目标平台实际执行时记录：安装方式（Git clone、ZIP、手工复制）、安装层级（个人级或项目级）、中文与空格路径、大小写与行尾表现、模式二源项目内容哈希不变、README 与 Pages 链接、是否依赖符号链接。

没有真实设备时保留“未执行”状态，不用静态分析或模拟替代实测。现有证据见 [Windows 验证](validation-windows.md) 与 [macOS 验证](validation-macos.md)。

## 五、新电脑可用性

交付给他人前确认：

- 仓库自身包含全部必需文件；
- 不依赖作者的其他个人技能、插件或工作流；
- 不依赖作者的环境变量或私有路径；
- 没有 Git 时可用 ZIP 或手工复制完成安装；
- 用户清楚应复制哪一层目录（直接包含 `SKILL.md` 的那层）；
- 安装验证、更新和卸载步骤可独立执行。

## 六、仓库元数据建议

发布时可用的 Topics：

```text
claude-code
agent-skills
project-management
multi-agent
ai-workflow
knowledge-handoff
ai-coding
developer-tools
```

首个正式版本可考虑 `v1.0.0`，但只在适用验证完成并获得所有者明确授权后创建 tag 和 Release。发布流程见 [GitHub 发布指南](github-publishing.zh-CN.md)。
