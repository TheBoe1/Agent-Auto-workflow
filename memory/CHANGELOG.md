# 变更日志

## 2026-08-26

- 新增 `tools/`：bb-browser 搜索工具、research-workflow 检索分析 skill。
- 新增 `memory/`：项目记忆文件（MEMORY.md + CHANGELOG.md）。
- 在检索类 agent（闻风向、查无异）中标注工具引用与高权重。
- git 备份：main 已与远程同步（提交 `848617d`）。

## 2026-08-26（6 Agent 集成）

- 团队从 8 Agent 集成精简为 6 Agent：闻风向 + 查无异 → 猎同频；谷新裁 → 并入缪生花；步渐涨 + 潘得清 → 步得清；新增采真人。
- 新增 `artifacts/templates/TEMPLATES.md`（8 个中间文件模板）。
- 更新 `workflows/content-pipeline.md`（6 Agent + 中间文件流转 + 两道门禁）、`account-growth.md`（growth-analyst）。
- 更新 README 与 memory/MEMORY.md 反映 6 Agent 架构。

## 2026-08-27（工具工程化 v0.2.1）

- 新增 3 个应用工具（agent 直接调用，均支持 `--demo` 零配置）：
  - `tools/content-research.md`：统一检索入口，封装 bb-browser + research-workflow（猎同频）
  - `tools/humanize-writing.md`：文本拟人化，LLM 抽象层 provider 无关（缪生花）
  - `tools/engagement-analyzer.md`：互动内容分析，输出报告（步得清）
- 每个工具文件含：统一 Tool Schema（输入输出契约）+ 零配置 Demo + 真实最小配置 + 配置校验/降级/缓存/License/安全/测试。
- 新增 `samples/`（4 文件，脱敏内置样例，保证 clone 即跑）。
- 新增 `.gitignore`、`.env.example`、`LICENSE`（MIT, TheBoe1）、`third-party-licenses.md`。
- 更新 tools/README、SKILL.md 工具表、3 个 agent 工具引用（高权重）。
- git 备份：推送至 `Agent-Auto-workflow` main（提交 `b179279`）。
- License 核查：bb-browser = MIT ✓；research-workflow（jwynia/agent-skills）= 未声明 license（仓库无 LICENSE，仅借鉴方法论）；humanizer/lguz 系列待核实。
