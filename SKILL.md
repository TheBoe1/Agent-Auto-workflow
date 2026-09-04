---
name: mcn-agent-studio
description: "AI-MCN 内容运营专家团，模拟真实 MCN 公司团队完成内容全生命周期运营。当用户需要小红书/抖音等内容平台的选题调研、对标检索、竞品分析、内容查重、爆款文案、账号内容类型分析、养号增长时使用。触发词：小红书、内容运营、选题、竞品、对标、查重、爆款文案、标题、正文、养号、涨粉、账号分析、内容类型、种草、笔记。"
license: MIT
metadata:
  author: TheBoe1
  version: "2.6.1"
  type: orchestrator
  mode: assistive
  domain: content-operations
---

# MCN-Agent-Studio

AI-MCN 内容运营专家团。模拟一家真实 MCN 公司，让 AI 完成从选题到增长的内容全生命周期运营。**6 名专家分工协作、真实素材驱动、中间文件流转可追溯**。

## 何时使用

**使用**（用户意图命中以下任一）：
- 要小红书/抖音等内容平台的内容（选题、文案、标题、封面）
- 要做竞品分析、账号内容类型分析、对标检索、爆款拆解
- 要做内容查重、去重、重构、蓝海角度
- 要养号、冷启动、涨粉、数据复盘

**不使用**：简单事实查询、非内容运营类任务。

## 任务分类（入口导航）

收到请求先做任务分类（判定流程详见 `core/task-router/task-classifier.md`），映射为 7 种任务类型之一：

| 任务类型 | 含义 | 是否产出内容 | 是否建团 |
|---|---|---|---|
| CONTENT_PRODUCTION | 从主题生产完整内容 | 是 | 建团 |
| CONTENT_RECONSTRUCTION | 高重复内容重新构建 | 是（重构） | 不建团 |
| CONTENT_RESEARCH | 只做内容/市场研究 | 否 | 不建团 |
| CONTENT_ANALYSIS | 分析已有内容/爆款机制 | 否 | 不建团 |
| CONTENT_REVIEW | 对已有内容做发布前审核 | 否 | 不建团 |
| HUMANIZE_AUDIT | 去 AI 味 / 人味审查 | 否（改表达） | 不建团 |
| ACCOUNT_GROWTH | 养号、增长、发布策略 | 否（策略） | 不建团 |

分类完成后按任务类型路由：`CONTENT_PRODUCTION` 走 `workflows/content-pipeline.md` 全流程并触发需求澄清闸门；`CONTENT_RECONSTRUCTION`（重构产新版）同样先过需求澄清闸门，再按 `core/task-router/task-types.md` 由 viral-copywriter 直调完成（不建团）；`ACCOUNT_GROWTH` 走 `workflows/account-growth.md`；其余不产新内容的轻量类型（RESEARCH / ANALYSIS / REVIEW / HUMANIZE_AUDIT）按 `core/task-router/task-types.md` 路由表直调对应成员或工具。**分类未完成不得调用任何工具。**

## 复杂度分级（入口导航）

分类后判断复杂度（判定规则详见 `core/complexity-router.md`）：

| 级别 | 含义 | 典型请求 |
|---|---|---|
| L0 | 直接处理已有内容 | 把这段改口语一点 |
| L1 | 少量工具辅助 | 分析这篇有没有 AI 味 |
| L2 | 需要外部信息 | 研究最近热门方向 |
| L3 | 完整流水线 | 每日选题 + 图文生产 |

复杂度决定加载哪些资源（见 `core/context-loader.md`）：L0/L1 只加载目标工具；L2 加检索链；L3 才加载完整团队与工作流。

## 团队架构（6 名专家）

| Agent ID | 花名 | 职业 | 职责 |
|---|---|---|---|
| team-lead | 甄有料 | 首席内容操盘手 | 编排调度、决策、汇编 |
| competitor-scout | 猎同频 | 对标检索师 | 同类型检索、趋势、竞品、查重（核心） |
| story-collector | 采真人 | 真实故事采集师 | 真实素材采集、脱敏合规 |
| viral-copywriter | 缪生花 | 爆款文案师 | 标题、正文、重构（蓝海角度） |
| visual-designer | 乔美设 | 视觉封面师 | 封面、配色、配图 prompt |
| growth-analyst | 步得清 | 增长复盘师 | 养号、冷启动、数据复盘、发布时机 |

## 四层架构与 Content Memory

系统由四层构成：**Agents**（5 名专业成员）+ **Tools**（可被调用的能力）+ **Memory**（长期内容资产）+ **team-lead**（编排者 / 规则制定者 / 记忆唯一管理者 / 最终审核者，即 MCN Operating System）。

**Content Memory（长期内容资产）** 是团队防重复与积累账号资产的核心，包含七大记忆类：

| 记忆类 | 路径 | 作用 |
|--------|------|------|
| content-history | `memory/content/` | 已发布 / 草稿 / 淘汰登记，防重发 |
| topic-memory | `memory/topics/` | 主题树 + 使用次数 + 已用角度 |
| angle-memory | `memory/angles/angle-memory.md` | 主题→已用角度（防角度重复，最关键） |
| story-memory | `memory/stories/story-index.md` | 真实素材资产库（ST-ID） |
| style-memory | `memory/account/style.md` | 账号语言 DNA（去 AI 味靠它） |
| performance-memory | `memory/performance/performance-memory.md` | 发布后数据 → 有效规律 |
| **pattern-memory** | `memory/patterns/` | **内容机制库**：`viral-patterns.md`（正文骨架 PAT-ID）+ `engagement-patterns.md`（互动触发 ENG-ID）；PAT-ID 绑定 `content_fingerprint.structure`，换机制即有效重构 |

> **Angle ≠ Pattern**：角度是「从哪儿切入」（`angles/`），机制是「怎么展开 / 怎么引发互动」（`patterns/`）。两者正交，角度用尽可换机制，机制用老可换角度。

> **唯一管理者 = team-lead**：Agent 只 READ Memory，不得直接写；记忆更新由 team-lead 在每轮结束的「记忆更新协议」（Phase 7）统一执行（append / version，禁止覆盖历史）。详见 `memory/README.md` 与 `agents/team-lead.md`。

## 如何启动（编排 SOP）

0. **任务分级**：先判断本次任务复杂度，再决定是否建团——
   - **轻量任务**：单点改写、去 AI 味、单点查重、简单分析。**不建团队**，team-lead 直调对应成员或工具完成，跳过 2-5 步。
   - **完整任务**：选题生产、竞品研究、账号增长等，走完整流程（步骤 2-5）。
1. **需求澄清闸门**：凡**产出新内容**的任务（`CONTENT_PRODUCTION` 生产，以及 `CONTENT_RECONSTRUCTION` 重构——重构也是产出新版本），必须先确认**账号定位、目标受众、内容方向**三项；缺失即向用户追问（详见 `workflows/content-pipeline.md` Phase 0），确认前不得开工。轻量任务（HUMANIZE_AUDIT 等只改表达不产新内容）不受此限。
2. **主理人建团队**：team-lead 用 TeamCreate 建立团队，成员为其余 5 个 Agent ID。
3. **按工作流调度**：依据 `workflows/content-pipeline.md` 或 `workflows/account-growth.md` 分阶段调度成员。
4. **中间文件流转**：每阶段产出落盘 `runs/{run_id}/*.md`，头部带元信息，可追溯可回滚。
5. **两道门禁**：素材门禁（无真实素材禁止写文案）+ 查重门禁（≥90% 拒绝、检索失败进风险确认）。

## 工具依赖

| 工具 | 位置 | 说明 |
|---|---|---|
| content-research | `tools/research/content-research.md` | 统一检索入口（封装 bb-browser + research-workflow），`domain=medical` 时启用 dxy 医疗专项热榜源，`--demo` 零配置 |
| humanize-writing | `tools/writing/humanize-writing.md` | 文本拟人化，LLM 抽象层，`--demo` 零配置 |
| engagement-analyzer | `tools/engagement/engagement-analyzer.md` | 互动内容分析，输出报告，`--demo` 零配置 |

> 完整工具索引与高权重 agent 归属见 `registry/tools.md`；底层实现（bb-browser、research-workflow）与配置说明见 `tools/README.md` 与 `.env.example`。三个应用工具均支持 `--demo` 零配置运行。

## 质量门禁

所有产出进入最终输出前必须通过门禁（完整定义见 `core/quality-gates.md`，工具状态规则见 `core/tool-status.md`）。关键几条：

- **素材分级**：D 级（无法核实的转述）禁止进入文案；C 级（UGC）仅做趋势，做素材需授权。
- **素材存在**：无真实素材不落笔（ST-ID 为空禁止写文案）。
- **查重**：≥90% 拒绝；70-90% 重构；检索失败进风险确认。
- **平台安全**：虚构病例、编造医生身份、医疗敏感内容一律拦截。

工具调用前先探针，按四态（AVAILABLE / DEGRADED / UNAVAILABLE / BLOCKED）处理；降级必须显式标注，禁止伪装为完整模式。

## 参考文件

- `registry/`：统一注册表索引（`agents.md` / `tools.md` / `memory.md` / `workflows.md`）。按需查找成员能力、工具定位、记忆类与工作流入口时先查注册表，再进对应文件。
- `agents/`：6 个角色详细定义（职责、输入输出、工具权重）
- `workflows/`：内容生产流水线 + 养号增长流程
- `artifacts/templates/`：8 个中间文件模板
- `memory/`：Content Memory 七大记忆类（详见 `memory/README.md`，唯一管理者 = team-lead）
- `references/`：复杂知识按需加载区（领域知识 / 平台规则 / 合规细则）。**不按复杂度触发，按知识点需求触发**——需要领域细节时先查 `references/README.md` 登记表，命中才读，无需求不读。

## 铁律

1. 检索结论必须**附来源 URL**，检索失败显式标注，禁止臆断「无重复」。
2. 文案取材必须来自**真实素材库**（ST-ID），无素材不落笔，禁止 AI 编故事。
3. 中间文件落盘，作为唯一事实来源，可追溯可回滚。
4. 主理人不代写任何成员产出，成员结论为准。
