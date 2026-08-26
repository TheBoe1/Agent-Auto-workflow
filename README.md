# MCN-Agent-Studio

> 一个基于 Agent Workflow 的 AI-MCN 内容运营系统，让 AI 模拟真实 MCN 公司团队完成选题、竞品分析、内容创新、生产和增长优化。

输入一个「主题 / 大方向」，系统自动完成：

**选题调研 → 全网查重 → 内容重构 → 爆款生产 → 数据复盘**，最终产出一篇去重后、可稳定发布的小红书内容；也可以为账号输出完整的**养号增长方案**。

## 核心理念

这不是「AI 帮我写文案」，而是「AI 运营一家虚拟 MCN 公司」。

| 维度 | 普通 AI 写作 | MCN-Agent-Studio |
|------|-------------|------------------|
| 输出 | 生成内容 | 管理内容全生命周期 |
| 结构 | 单 Agent | 8 名专家分工协作 |
| 反馈 | 无 | 数据闭环、持续迭代 |
| 策略 | 一次性输出 | 模拟公司决策 + 养号增长 |

## 团队架构（8 名专家）

```
                首席内容操盘手 · 甄有料（主理人）
                          │
        ┌─────────────────┼─────────────────┐
        │                 │                 │
   市场研究组          内容生产组          增长运营组
        │                 │                 │
   趋势选题分析师      查重检测师(核心)    养号增长师(重点)
   闻风向             查无异             步渐涨
                      内容重构师(核心)    数据复盘师
                      谷新裁             潘得清
                      爆款文案师
                      缪生花
                      视觉封面师
                      乔美设
```

| Agent ID | 花名 | 职业 | 核心职责 |
|----------|------|------|---------|
| team-lead | 甄有料 | 首席内容操盘手 | 编排调度、决策、汇编输出 |
| trend-analyst | 闻风向 | 趋势选题分析师 | 趋势、竞品、用户画像、竞争度 |
| similarity-analyst | 查无异 | 查重检测师 | 全网查重、重复度打分（核心） |
| reframe-strategist | 谷新裁 | 内容重构师 | 重构、蓝海角度挖掘（核心） |
| viral-copywriter | 缪生花 | 爆款文案师 | 爆款标题 + 小红书正文 |
| visual-designer | 乔美设 | 视觉封面师 | 封面、配色、配图 prompt |
| growth-specialist | 步渐涨 | 养号增长师 | 养号、冷启动、涨粉（重点） |
| data-analyst | 潘得清 | 数据复盘师 | 发布时机、指标、复盘迭代 |

## 目录结构

```
MCN-Agent-Studio/
├── README.md                    # 项目说明
├── agents/                      # 8 个专家角色定义（Markdown）
│   ├── team-lead.md             #   主理人（含 SOP 编排 + 工具清单）
│   ├── trend-analyst.md         #   趋势选题分析师（高权重：bb-browser + research-workflow）
│   ├── similarity-analyst.md    #   查重检测师（高权重：bb-browser + research-workflow）
│   ├── reframe-strategist.md    #   内容重构师
│   ├── viral-copywriter.md      #   爆款文案师
│   ├── visual-designer.md       #   视觉封面师
│   ├── growth-specialist.md     #   养号增长师
│   └── data-analyst.md          #   数据复盘师
├── workflows/                   # 工作流定义
│   ├── content-pipeline.md      #   内容生产流水线（方案一/方案二状态机）
│   └── account-growth.md        #   养号增长流程（三阶段）
├── tools/                       # 工具文件夹（agent 可调用）
│   ├── README.md                #   工具索引 + 协作关系
│   ├── bb-browser.md            #   搜索工具（36 平台 103 命令）
│   └── research-workflow.md     #   检索分析 skill（结构化研究 + 中间文件报告）
└── memory/                      # 记忆文件（跨会话持久化）
    ├── MEMORY.md                #   项目长期记忆（架构/工具/约束）
    └── CHANGELOG.md             #   变更日志
```

## 工具与检索能力

| 工具 | 位置 | 能力 | 高权重 agent |
|------|------|------|-------------|
| bb-browser | `tools/bb-browser.md` | 36 平台 103 命令，真实浏览器登录态搜索，`--json`/`--jq` 结构化输出 | 闻风向、查无异 |
| research-workflow | `tools/research-workflow.md` | 结构化研究四阶段（Planning→Execution→Analysis→Synthesis），生成中间文件报告 | 闻风向、查无异 |

检索类 agent（闻风向 / 查无异）在定义文件中通过「工具引用」小节声明了工具的**调用权重**，主理人在 `tools/` 索引中完成调度派发。

## 核心工作流：内容生产流水线

围绕「内容重复度确认」设计成两条分支（详见 `workflows/content-pipeline.md`）：

```
输入主题 / 大方向
        │
        ▼
Phase 1  趋势选题（闻风向）
        │
        ▼
Phase 2  全网查重（查无异）
        │
        ├─ 有重复 ──────────────► 【方案一】
        │                          重构（谷新裁）
        │                          蓝海搜索：类似主题但未充分覆盖的角度
        │                          二次查重（查无异）
        │                          ├─ ≥90% → 拒绝重做
        │                          ├─ 70%–90% → 继续优化
        │                          └─ <70% → 通过
        │
        ├─ 无重复 ──────────────► 【方案二·A】跳过重构，直接进入内容生产
        │
        └─ 检测失败（搜索不可用）► 【方案二·B】风险确认节点
                                   A 继续 / B 换主题 / C 人工提供参考
                                   等待用户确认，禁止自动生产
        │
        ▼
Phase 4  内容生产（缪生花 + 乔美设，并行）
Phase 5  数据复盘（潘得清）
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
- **Postiz / AiToEarn** — 社媒排期 + 内容工厂 + 数据复盘闭环。

## 路线图

- [x] V0.1 选题 → 查重 → 重构 → 文案（8 名专家定义 + 工作流）
- [ ] V0.2 竞品分析、账号画像、爆款拆解自动化
- [ ] V0.3 自动发布、数据采集、增长优化闭环

## 技术落地建议

Agent 定义与工作流是平台无关的；若要落地成可运行系统：

- **Agent 框架**：LangGraph / CrewAI / Spring AI（状态流转密集）
- **存储**：关系库（内容/账号）+ 向量库（查重 Embedding）
- **前端**：Dashboard（任务状态、内容审核、数据分析）

## License

MIT
