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
  - `tools/research/content-research.md`：统一检索入口，封装 bb-browser + research-workflow（猎同频）
  - `tools/writing/humanize-writing.md`：文本拟人化，LLM 抽象层 provider 无关（缪生花）
  - `tools/engagement/engagement-analyzer.md`：互动内容分析，输出报告（步得清）
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

## 2026-08-27（架构固化 + Skill 化 · V0.4-prep，提交 `9817a58`）

- **`content-research` 整合层定位写死**：明确它是 Facade / Adapter（非新增能力），只把 bb-browser + research-workflow 收敛为稳定业务接口。
  - `tools/research/content-research.md`：加 YAML 元信息头（`type: integration` / `role: research-facade` / `new_capability: false` / `primary_agent: competitor-scout`），首行澄清「不提供独立的新检索能力」。
  - `tools/README.md`：工具清单改为三层分类表（基础工具 / 基础方法 / **整合层** / **业务工具**）+ 分层依赖图，标注整合层非新增能力。
  - `README.md`（根）：四层架构升级（Orchestration 含 Workflow；Tools 分 基础/整合/业务）+ 工具表 + 路线图 V0.4。
  - `agents/team-lead.md`：架构图改为四块（Orchestration / Agents / Tools[3 层] / Memory[6 类]）+ Tool Registry 三层重排 + 升格 **MCN Orchestrator**。
- **Memory 写入协议 + 候选记忆层**（`memory/README.md`）：新增 Write Protocol（候选→冲突检查→APPEND/MERGE/UPDATE/REJECT→写→CHANGELOG）与 `memory-candidates/` 候选层概念，杜绝 team-lead 凭感觉写 Memory。
- 业务调用链定稿：competitor-scout → engagement-analyzer → viral-copywriter → humanize-writing → final content。

## 2026-08-27（Registry + Agent Contract + content_fingerprint + Cron · V0.4，提交 `98b810a`）

- **Registry 索引（#27）**：新增 `registry/` 四表，team-lead 改为「引用索引」而非内嵌相对路径，便于自动化加载。
  - `registry/agents.md`：6 成员索引（触发条件 + 调度铁律）。
  - `registry/tools.md`：工具三层索引 + 依赖图，明确标注 content-research = 封装壳（非新增能力）。
  - `registry/memory.md`：六大记忆类索引 + 写入协议摘要。
  - `registry/workflows.md`：工作流索引 + 业务调用链图。
  - 同步 `team-lead.md` 二/三/四节、`README.md` 目录树新增 `registry/`。
- **Agent Contract 标准化（#29）**：5 个执行 Agent 各加统一契约段（Input required/optional / Tools / Memory READ / Output must_contain / Gate / Failure），消除 Agent 互相猜。
- **content_fingerprint 七维防重（#30）**：
  - `angle-memory.md` 新增七维指纹（topic / angle / narrative / story / hook / structure / expression）+ 冲突规则。
  - `competitor-scout.md` 查重判定改「七维全同 → 拒绝」优先，输出 JSON 加 `content_fingerprint`。
  - `content-pipeline.md` Phase1 改 similarity + fingerprint 双轴判定。
- **业务调用链 + Cron 协议（#31）**：
  - `team-lead.md` 新增「十二、业务调用链」与「十三、定时任务（Cron）调度规则」（三阶段演进 + 调度协议 + 门禁不可绕过约束）。
  - `registry/workflows.md` 补调用链图。
- 待做：**#32 tools/ 子文件夹重组织**（`research/` `engagement/` `writing/` 三分）——因改 20+ 相对路径，按铁律待用户确认后再做。
