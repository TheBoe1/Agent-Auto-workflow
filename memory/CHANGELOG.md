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

## 2026-08-27（Content Memory + team-lead OS · V0.3）

- **Content Memory 六大记忆类**：把 `memory/` 从单一 MEMORY.md 升格为长期内容资产系统
  （`memory/README.md` 系统说明 + 唯一管理者规则）。新增：
  - `account/`（profile.md 账号画像 + style.md 语言 DNA）
  - `content/`（已发布/草稿/淘汰 + 条目 Schema）
  - `topics/`（topic-index + topic-history）
  - `angles/`（angle-memory，防角度重复，最关键）
  - `stories/`（story-index 真实素材资产库 ST-ID）
  - `performance/`（performance-memory，发布后数据→规律）
  - 种子数据：用实测「柯医生」账号做 profile/style/topic/angle 初始条目，系统建好即非空可用。
- **team-lead 升格为 MCN 操作系统**：`agents/team-lead.md` 重写为四层架构 + Agent Registry + Tool Registry
  + Memory 索引 + Mermaid 主流程 + 5 条调度铁律 + Phase 7 记忆更新协议（learning loop）。
- **Memory 唯一管理者 = team-lead**：5 个 agent 各加「Memory 访问规则」小节（只 READ，不 WRITE；
  记忆更新由 team-lead 在 Phase 7 统一 promote，append/version 禁止覆盖）。
- **门禁③记忆防重复门禁**：选题决策新增「角度在 angles/ 已高频使用 → 换未用/空白角度」。
- 更新 SKILL.md（四层架构 + Content Memory 段）、README.md（Content Memory 说明 + 目录结构 + 路线图 V0.3）、
  `workflows/content-pipeline.md`（状态机补读 Memory 起点与 Phase 7、门禁③、Memory 读写小节）。
