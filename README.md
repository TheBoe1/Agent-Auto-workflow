# MCN-Agent-Studio

> 一个基于 Agent Workflow 的 AI-MCN 内容运营系统，让 AI 模拟真实 MCN 公司团队完成选题、竞品分析、内容创新、生产和增长优化。

输入一个「主题 / 大方向」，系统自动完成：

**对标检索 → 真实素材 → 查重 → 内容重构 → 爆款生产 → 数据复盘**，最终产出一篇去重后、**有真实素材支撑**、可稳定发布的小红书内容；也可以为账号输出完整的**养号增长方案**。

## 核心理念

这不是「AI 帮我写文案」，而是「AI 运营一家虚拟 MCN 公司」。

| 维度 | 普通 AI 写作 | MCN-Agent-Studio |
|------|-------------|------------------|
| 输出 | 生成内容 | 管理内容全生命周期 |
| 结构 | 单 Agent | 6 名专家分工协作 |
| 真实感 | 凭空编故事 | 真实素材库驱动，无素材不落笔 |
| 可追溯 | 结果只在对话里 | 中间文件落盘，可追溯可回滚 |
| 策略 | 一次性输出 | 模拟公司决策 + 养号增长 |

## 团队架构（6 名专家）

```
             首席内容操盘手 · 甄有料（主理人）
                       │
        ┌──────────────┼───────────────┐
        │              │               │
    检索查重组       素材组          生产运营组
        │              │               │
  对标检索师(核心)  真实故事采集师   爆款文案师(含重构)
  猎同频            采真人           缪生花
                                      视觉封面师
                                      乔美设
                                      增长复盘师
                                      步得清
```

| Agent ID | 花名 | 职业 | 核心职责 |
|----------|------|------|---------|
| team-lead | 甄有料 | 首席内容操盘手 | 编排调度、决策、汇编输出 |
| competitor-scout | 猎同频 | 对标检索师 | 同类型检索、趋势、竞品、查重（核心） |
| story-collector | 采真人 | 真实故事采集师 | 真实素材采集、结构化、脱敏合规 |
| viral-copywriter | 缪生花 | 爆款文案师 | 爆款标题 + 正文 + 重构（蓝海角度） |
| visual-designer | 乔美设 | 视觉封面师 | 封面、配色、配图 prompt |
| growth-analyst | 步得清 | 增长复盘师 | 养号、冷启动、数据复盘、发布时机 |

> v1（8 Agent）→ v2（6 Agent）集成：闻风向 + 查无异 → **猎同频**；谷新裁 → 并入 **缪生花**；步渐涨 + 潘得清 → **步得清**；新增 **采真人**。

## Content Memory（长期内容资产系统）

系统由四层构成：**Orchestration**（编排层：team-lead + Workflow，负责任务调度与 SOP）+ **Agents**（5 名专业执行成员）+ **Tools**（能力层，分基础 / 整合 / 业务三层）+ **Memory**（长期内容资产，team-lead 为**记忆唯一管理者**）。其中 Tools 的三层定位见下方「工具与检索能力」；Workflow 归入 Orchestration 而非与 Agents/Tools/Memory 并列。

Memory 不是「聊天记忆」，而是团队的**内容资产库**——账号过去说过什么、用过什么角度、讲过什么故事、采用过什么表达、什么内容真正有效。它解决的是反复踩同一个重复坑的问题（今天写「用了 30 天 AI」、明天又写一遍）。

七大记忆类（详见 `memory/README.md`）：

| 记忆类 | 路径 | 作用 |
|--------|------|------|
| content-history | `memory/content/` | 已发布 / 草稿 / 淘汰登记，防重发 |
| topic-memory | `memory/topics/` | 主题树 + 使用次数 + 已用角度 |
| angle-memory | `memory/angles/angle-memory.md` | 主题→已用角度（**防角度重复，最关键**） |
| story-memory | `memory/stories/story-index.md` | 真实素材资产库（ST-ID 资产化，非一次性） |
| style-memory | `memory/account/style.md` | 账号语言 DNA（去 AI 味靠它，而非每次重猜） |
| performance-memory | `memory/performance/performance-memory.md` | 发布后数据 → 有效规律（learning loop 基础） |
| **pattern-memory** | `memory/patterns/` | **内容机制库**：`viral-patterns.md`（正文骨架 PAT-ID）+ `engagement-patterns.md`（互动触发 ENG-ID） |

> **Angle ≠ Pattern**：角度是「从哪儿切入」（`angles/`），机制是「怎么展开 / 怎么引发互动」（`patterns/`）。两者正交——角度用尽可换机制，机制用老可换角度。PAT-ID 绑定 `content_fingerprint.structure`，**换机制即构成有效重构**，是撞车后成本最低的差异化手段。

> **唯一管理者 = team-lead**：Agent 只 READ Memory，不得直接写；记忆更新由 team-lead 在每轮结束的「记忆更新协议」（Phase 7）统一执行（append / version，禁止覆盖历史）。这是防止 Memory 自相矛盾的关键设计。

## 目录结构

```
MCN-Agent-Studio/
├── README.md                    # 项目说明
├── SKILL.md                     # skill 入口（触发词 + 编排 SOP + 工具依赖）
├── LICENSE                      # MIT（TheBoe1）
├── .gitignore                   # 忽略 .env / cookies / node_modules / cache / runs
├── .env.example                 # 配置模板（无真实值）
├── third-party-licenses.md      # 各上游 license 核查状态
├── registry/                    # 索引（单一事实来源，team-lead 引用而非内嵌路径）
│   ├── agents.md                #   Agent 索引表
│   ├── tools.md                #   工具索引（基础/整合/业务三层）
│   ├── memory.md               #   记忆索引（七大类）
│   └── workflows.md            #   工作流索引 + 业务调用链
├── agents/                      # 6 个专家角色定义（Markdown）
│   ├── team-lead.md             #   主理人（含 SOP 编排 + 工具清单 + 中间文件流转）
│   ├── competitor-scout.md      #   猎同频·对标检索师（高权重：content-research）
│   ├── story-collector.md       #   采真人·真实故事采集师
│   ├── viral-copywriter.md      #   缪生花·爆款文案师（含重构，humanize-writing）
│   ├── visual-designer.md       #   乔美设·视觉封面师
│   └── growth-analyst.md        #   步得清·增长复盘师（engagement-analyzer）
├── workflows/                   # 工作流定义
│   ├── content-pipeline.md      #   内容生产流水线（含中间文件流转 + 两道门禁）
│   └── account-growth.md        #   养号增长流程（三阶段）
├── tools/                       # 工具文件夹（agent 可调用）
│   ├── README.md                #   工具索引 + 协作关系 + 零/最小配置总览
│   ├── research/                #   检索能力层（基础 + 整合）
│   │   ├── bb-browser.md        #     底层搜索工具（36 平台 103 命令）
│   │   ├── research-workflow.md #     底层检索方法论（四阶段）
│   │   └── content-research.md  #     统一检索入口（Facade，封装上两者，非新增能力）
│   ├── engagement/              #   互动分析层（业务新增）
│   │   └── engagement-analyzer.md #   互动内容分析（输出报告）
│   └── writing/                 #   人格化写作层（业务新增）
│       └── humanize-writing.md  #   账号人格化写作
├── samples/                     # 内置脱敏样例（--demo 零配置运行）
│   ├── content-research/        #   对标检索报告样例
│   ├── engagement-analyzer/     #   脱敏 posts.json + 报告样例
│   └── humanize-writing/        #   拟人化 input/output 样例
├── artifacts/                   # 中间文件模板
│   └── templates/
│       └── TEMPLATES.md         #   8 个中间文件模板（含元信息头）
└── memory/                      # Content Memory 七大记忆类（跨会话持久化）
    ├── README.md                #   系统说明 + 唯一管理者(team-lead)规则
    ├── MEMORY.md                #   项目长期记忆（架构/工具/约束）
    ├── CHANGELOG.md             #   变更日志
    ├── account/                 #   账号资产
    │   ├── profile.md           #     账号画像（种子：柯医生数据）
    │   └── style.md             #     style-memory 语言 DNA
    ├── content/                 #   content-history
    │   ├── README.md            #     条目 Schema
    │   ├── published/           #     已发布
    │   ├── drafts/              #     草稿
    │   └── rejected/            #     淘汰
    ├── topics/                  #   topic-memory
    │   ├── topic-index.md       #     主题树 + 使用次数
    │   └── topic-history.md     #     选题时间序日志
    ├── angles/                  #   angle-memory
    │   └── angle-memory.md      #     主题→已用角度（防角度重复）
    ├── stories/                 #   story-memory
    │   └── story-index.md       #     真实素材资产库（ST-ID）
    ├── patterns/                #   pattern-memory（内容机制库）
    │   ├── viral-patterns.md    #     正文骨架机制（PAT-ID）
    │   └── engagement-patterns.md #   互动触发机制（ENG-ID）
    └── performance/             #   performance-memory
        └── performance-memory.md #    发布后数据 → 有效规律
```

## 工具与检索能力

每个工具文件内含**统一 Tool Schema（输入/输出契约）+ 零配置 Demo + 真实最小配置 + 配置校验/降级/缓存/License/安全/测试**。两个业务工具（`humanize-writing` / `engagement-analyzer`）均支持 `--demo` 零配置运行（用内置脱敏样例），真实场景仅需最小配置。

| 层级 | 工具 | 状态 | 能力 / 高权重 agent |
| -------- | --------------------- | ------ | ------- |
| 基础工具 | `bb-browser` | 已有 | 浏览器检索（36 平台 103 命令） |
| 基础方法 | `research-workflow` | 已有 | 结构化研究四阶段 |
| **整合层** | `content-research` | 已有/封装 | 统一研究入口（**Facade，非新增能力**），猎同频 |
| **业务工具** | `engagement-analyzer` | **新增** | 分析互动机制，输出报告，步得清 |
| **业务工具** | `humanize-writing` | **新增** | 账号人格化写作（绑定 `memory/account/style.md`），缪生花 |

> **层级关系**：`content-research` 是**能力整合层（Facade / Adapter）**——只把底层 `bb-browser` + `research-workflow` 收敛成稳定业务接口，**不提供独立的新检索能力**；真正本次新增的业务能力是 `engagement-analyzer` 与 `humanize-writing`。

**业务调用链（Business Call Chain）：**

```
competitor-scout ──▶ engagement-analyzer ──▶ viral-copywriter ──▶ humanize-writing ──▶ final content
```

`engagement-analyzer` 负责「分析为什么火」，结论交给 `viral-copywriter` 生产；`humanize-writing` 在最后做账号人格化收尾（绑定 `account/style.md`），而非随机润色。

检索/分析/润色类 agent 在定义文件中通过「工具引用」小节声明工具的**调用权重**，主理人在 `tools/` 索引中完成调度派发。

### 零配置与最小配置
- **Demo 零配置**：`content-research --demo` / `humanize-writing --demo` / `engagement-analyzer --demo` 均读取 `samples/` 内置脱敏数据，无需浏览器、无需 LLM key，clone 即跑。
- **真实最小配置**：小红书抓取需填 `XIAOHONGSHU_COOKIE`；拟人化/深度归纳需填 `LLM_API_KEY` + `LLM_BASE_URL` + `LLM_MODEL`（provider 无关）。详见 `.env.example`。

### 安全与合规
- 密钥仅存 `.env`（已被 `.gitignore` 忽略），不进仓库、不写进中间文件。
- `third-party-licenses.md` 记录上游 license 核查：`bb-browser` 为 MIT（可安全使用）；`research-workflow` 上游**未声明 license**，仅借鉴方法论、不复制代码；`humanizer`/`lguz` 系列待核实，禁止在核实前复制。

## 中间文件流转（核心机制）

每次任务一个独立 `run_id` 目录，每个成员产出落盘为固定命名的中间文件，头部带元信息，保证**可追溯、可回滚**。

```
runs/{run_id}/
├── 00_input.md        主理人写入
├── 01_scout.md        猎同频：检索 + 趋势 + 查重（附来源 URL）
├── 02_stories.md      采真人：真实素材库（附来源/授权/脱敏）
├── 03_brief.md        主理人：选题角度（两道门禁）
├── 04_copy.md         缪生花：标题 20 + 正文 + 标签（引用 ST-00X）
├── 05_visual.md       乔美设：封面方案 + 配图 prompt
├── 06_review.md       步得清：发布建议 + 预期指标 + 复盘模板
└── final_bundle.md    主理人：可发布内容包
```

## 核心工作流：内容生产流水线

围绕「内容重复度确认」+「真实素材」设计（详见 `workflows/content-pipeline.md`）：

```
输入主题 / 大方向
        │
        ▼
Phase 0  主理人建 run 目录，写 00_input.md
        │
        ├──────────────────────────────┐
        ▼                              ▼
Phase 1  对标检索 + 查重          Phase 2  真实素材采集
        猎同频（核心）                  采真人（可并行）
        │                              │
        └──────────────┬───────────────┘
                       ▼
        Phase 3  选题决策（主理人，两道门禁）
                 门禁①素材：无素材禁止生产
                 门禁②查重：直接生产 / 重构 / 风险确认
                       │
        ┌──────────────┼───────────────┐
        │ 无重复        │ 有重复(70–90%)  │ 检索失败
        ▼              ▼               ▼
    直接生产       重构（缪生花）    风险确认节点
                   蓝海角度         等待用户确认
                       │
        ┌──────────────┴───────────────┐
        ▼                              ▼
Phase 4  内容生产（缪生花 + 乔美设，并行）
Phase 5  数据复盘（步得清）
Phase 6  主理人汇编 → 输出「可直接发布」的内容包
```

### 查重判定规则

| 相似度 | 风险 | 处置 |
|--------|------|------|
| ≥ 90% | HIGH | 拒绝重做 |
| 70%–90% | MEDIUM | 重构优化 |
| < 70% | LOW | 通过 |
| 无法检索 | — | 风险确认节点，等待用户确认 |

## 参考的开源项目

设计时调研了以下 GitHub 项目的思路（均围绕「AI Agent 模拟内容运营团队」）：

- **SocialFlow** — 6-Agent 自主内容管线（Scout→Planner→Creator→Reviewer→Publisher→Analyst），「品牌 Kit + 审核门禁」思路。
- **multi-agent-social-media-automation** — 7 专家（Researcher→Marketer→Copywriter→Designer→Moderator→Scheduler→Analytical），n8n + LangGraph 混合架构。
- **Agentic_Social_Media_AI** — 多 Agent 社媒自动化（research / copy / media / publish），强调「审核与授权发布」。
- **redbook（lucasygu）** — 小红书搜索、竞品分析、爆款拆解、内容建议的 CLI 工具。
- **xiaohongshu-ops-skill** — 把 OpenClaw 变成小红书运营助手，覆盖分析、选题、创作、复盘、复刻。
- **bb-browser（epiral）** — 浏览器即 API 的搜索工具，本项目的搜索能力来源。
- **research-workflow（jwynia/agent-skills）** — 结构化研究方法论，本项目的检索分析来源。

## 路线图

- [x] V0.1 选题 → 查重 → 重构 → 文案（8 名专家定义 + 工作流）
- [x] V0.2 集成精简为 6 名专家 + 真实素材采集 + 中间文件流转 + 工具层（bb-browser + research-workflow）
- [x] V0.2.1 工具工程化：3 个应用工具（content-research / humanize-writing / engagement-analyzer）+ 统一 Tool Schema + Demo 零配置 + 安全/license 基线
- [x] V0.3 Content Memory 六大记忆类（account/content/topics/angles/stories/performance）+ team-lead 升格为 MCN 操作系统（注册表 + Mermaid 主流程 + 5 条调度铁律 + 记忆唯一管理者 + Phase 7 记忆更新协议）
- [x] V0.4 架构固化：content-research 整合层定位写死（commit 9817a58）；Tools 三层；team-lead 升格 MCN Orchestrator；Memory 写入协议 + 候选记忆层
- [x] V0.4 Agent Contract 标准化（6 agent 统一 Input/Tools/Memory READ/Output/Gate/Failure）；Registry 索引（agents/tools/memory/workflows 四表）；content_fingerprint 七维防重升级；业务调用链 + Cron 调度协议（本次 commit）
- [ ] V0.5 自动发布、数据采集、增长优化闭环（先验证 10–20 轮选题/角度不重复，再接 Cron→团队→人工审核，最后才自动发布）

## 技术落地建议

Agent 定义与工作流是平台无关的；若要落地成可运行系统：

- **Agent 框架**：LangGraph / CrewAI / Spring AI（状态流转密集）
- **存储**：关系库（内容/账号）+ 向量库（查重 Embedding）+ 文件（中间文件流转）
- **前端**：Dashboard（任务状态、内容审核、数据分析）

## License

MIT
