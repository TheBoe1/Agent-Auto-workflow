# Tool Registry · 工具索引

> **单一事实来源**：工具的完整 Schema / Demo / 配置 / 降级见各 `../tools/*.md`。本表只做索引与分层。

| 层级 | 工具 | 路径 | 服务 Agent | 作用 | 是否新增能力 |
|------|------|------|-----------|------|------------|
| 基础能力层 | bb-browser | `../tools/research/bb-browser.md` | competitor-scout | 真实浏览器检索（36 平台 103 命令） | 已有 |
| 基础能力层 | research-workflow | `../tools/research/research-workflow.md` | competitor-scout | 结构化研究方法论（四阶段） | 已有 |
| 基础能力层（医疗专项源） | dxy-crawler | `../tools/research/content-research.md#医疗垂类专项源dxy-crawler` | competitor-scout | 丁香园医疗热榜（独立服务 `127.0.0.1:8000`，仅 `domain=medical` 启用，只服务趋势/查重） | 已有（外部独立仓） |
| **整合层（Facade）** | content-research | `../tools/research/content-research.md` | competitor-scout | 统一检索入口，封装上三者 | **非新增（封装壳）** |
| 业务能力层 | engagement-analyzer | `../tools/engagement/engagement-analyzer.md` | growth-analyst | 互动内容分析，输出《互动内容分析报告》 | **本次新增** |
| 业务能力层 | humanize-writing | `../tools/writing/humanize-writing.md` | viral-copywriter | 账号人格化写作（绑定 `../memory/account/style.md`） | **本次新增** |

## 分层与依赖关系
```
基础能力层：bb-browser  +  research-workflow  +  dxy-crawler(医疗专项源)
                    │
                    ▼
整合层（Facade）：content-research   ← 只封装，不创造新能力；底层替换时上层 agent 无需改动
                    │
                    ▼
业务能力层：engagement-analyzer  +  humanize-writing   ← 本次真正新增的能力
```

- 整合层是 **Facade / Adapter**：competitor-scout 只依赖 `content-research` 的单一 Schema，不直接关心 bb-browser / research-workflow / dxy-crawler 的调用细节。
- 底层工具（bb-browser / research-workflow）**无 License 风险**（bb-browser = MIT；research-workflow 仅借鉴方法论，不复制代码，见 `../third-party-licenses.md`）。
- `dxy-crawler` 为**外部独立仓**（MIT，Jaye-520），非本 skill 内置；作为医疗垂类专项源只服务 Phase 1（趋势/查重），**不得服务素材采集**（UGC 商用风险，详见 content-research.md「医疗垂类专项源」）。
- 三个可调用的工具（content-research / engagement-analyzer / humanize-writing）均支持 `--demo` 零配置（内置脱敏样例）。

> **重点**：`content-research` 不提供独立的新检索能力，仅是底层检索能力的业务封装层。详见 `../tools/research/content-research.md` 元信息（`type: integration` / `role: research-facade` / `new_capability: false`）。
