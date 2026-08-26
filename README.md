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

## 目录结构

```
MCN-Agent-Studio/
├── README.md                    # 项目说明
├── agents/                      # 6 个专家角色定义（Markdown）
│   ├── team-lead.md             #   主理人（含 SOP 编排 + 工具清单 + 中间文件流转）
│   ├── competitor-scout.md      #   猎同频·对标检索师（高权重：bb-browser + research-workflow）
│   ├── story-collector.md       #   采真人·真实故事采集师
│   ├── viral-copywriter.md      #   缪生花·爆款文案师（含重构）
│   ├── visual-designer.md       #   乔美设·视觉封面师
│   └── growth-analyst.md        #   步得清·增长复盘师
├── workflows/                   # 工作流定义
│   ├── content-pipeline.md      #   内容生产流水线（含中间文件流转 + 两道门禁）
│   └── account-growth.md        #   养号增长流程（三阶段）
├── tools/                       # 工具文件夹（agent 可调用）
│   ├── README.md                #   工具索引 + 协作关系
│   ├── bb-browser.md            #   搜索工具（36 平台 103 命令）
│   └── research-workflow.md     #   检索分析 skill（结构化研究 + 中间文件报告）
├── artifacts/                   # 中间文件模板
│   └── templates/
│       └── TEMPLATES.md         #   8 个中间文件模板（含元信息头）
└── memory/                      # 记忆文件（跨会话持久化）
    ├── MEMORY.md                #   项目长期记忆（架构/工具/约束）
    └── CHANGELOG.md             #   变更日志
```

## 工具与检索能力

| 工具 | 位置 | 能力 | 高权重 agent |
|------|------|------|-------------|
| bb-browser | `tools/bb-browser.md` | 36 平台 103 命令，真实浏览器登录态搜索，`--json`/`--jq` 结构化输出 | 猎同频 |
| research-workflow | `tools/research-workflow.md` | 结构化研究四阶段（Planning→Execution→Analysis→Synthesis），生成中间文件报告 | 猎同频 |

检索类 agent（猎同频）在定义文件中通过「工具引用」小节声明了工具的**调用权重**，主理人在 `tools/` 索引中完成调度派发。

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
- [ ] V0.3 竞品分析、账号画像、爆款拆解自动化
- [ ] V0.4 自动发布、数据采集、增长优化闭环

## 技术落地建议

Agent 定义与工作流是平台无关的；若要落地成可运行系统：

- **Agent 框架**：LangGraph / CrewAI / Spring AI（状态流转密集）
- **存储**：关系库（内容/账号）+ 向量库（查重 Embedding）+ 文件（中间文件流转）
- **前端**：Dashboard（任务状态、内容审核、数据分析）

## License

MIT
