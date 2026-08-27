# MCN-Agent-Studio · 项目记忆

> 本文件是项目的长期记忆，记录架构决策、工具清单与目录约定。agent 与后续会话据此对齐，避免偏离方向。

## 项目定位

基于 Agent Workflow 的 AI-MCN 内容运营系统，让 AI 模拟真实 MCN 公司团队完成选题、竞品分析、内容创新、生产与增长优化。

## 团队架构（6 Agent · v2）

- **甄有料**（主理人）：编排调度、决策、汇编输出。
- **猎同频**（对标检索师，核心）：同类型检索、趋势、竞品、用户画像、查重。
- **采真人**（真实故事采集师）：真实素材采集、结构化、脱敏合规。
- **缪生花**（爆款文案师）：爆款标题 + 正文 + 重构（蓝海角度）。
- **乔美设**（视觉封面师）：封面、配色、配图 prompt。
- **步得清**（增长复盘师）：养号、冷启动、数据复盘、发布时机。

> v1（8）→ v2（6）：闻风向 + 查无异 → 猎同频；谷新裁 → 并入缪生花；步渐涨 + 潘得清 → 步得清；新增采真人。

## 工具清单

**应用工具（agent 直接调用，均支持 `--demo` 零配置）**
| 工具 | 位置 | 能力 | 高权重 agent |
|---|---|---|---|
| content-research | `tools/content-research.md` | 统一检索入口（封装 bb-browser + research-workflow） | 猎同频 |
| humanize-writing | `tools/humanize-writing.md` | 文本拟人化，LLM 抽象层（provider 无关） | 缪生花 |
| engagement-analyzer | `tools/engagement-analyzer.md` | 互动内容分析，输出报告 | 步得清 |

**底层工具（被应用工具依赖）**
| 工具 | 位置 | 能力 |
|---|---|---|
| bb-browser | `tools/bb-browser.md` | 36 平台 103 命令，真实浏览器登录态搜索 |
| research-workflow | `tools/research-workflow.md` | 结构化检索分析（Planning→Execution→Analysis→Synthesis） |

> 零配置原则：Demo 用 `samples/` 内置脱敏数据，无需浏览器/key；真实场景最小配置填 cookie / LLM key（见 `.env.example`）。

## 关键约束（铁律）

1. 检索结论必须**附来源 URL**，检索失败显式标注，禁止臆断「无重复」。
2. 文案取材须来自**真实素材**，禁止 AI 凭空编故事。
3. 任何生产变更前**先 commit，基线先行**，可追溯、可回滚。
4. 每个 agent 产出落盘中间文件 `runs/{run_id}/*.md`。

## 环境信息

- bb-browser 版本 0.14.2，已全局安装。
- 项目路径：`C:\Users\lyl\WorkBuddy\2026-08-26-15-47-34\bb-browser`
- 适配器库：`~/.bb-browser/bb-sites`（更新用 `bb-browser site update`）
- daemon：`127.0.0.1:19824`，停止用 `bb-browser daemon stop`
- 最稳适配器：wikipedia / arxiv / baidu（bing、duckduckgo 本机有环境问题）
