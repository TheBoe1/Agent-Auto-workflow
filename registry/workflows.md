# Workflow Registry · 工作流索引

> **单一事实来源**：工作流完整 SOP 见各 `../workflows/*.md`。本表只做索引。
> Workflow 属于 **Orchestration 编排层**（与 team-lead 同层），不单独与 Agents / Tools / Memory 并列。

| Workflow | 路径 | 触发 | 编排 |
|----------|------|------|------|
| 内容生产流水线 | `../workflows/content-pipeline.md` | 输入主题 / 大方向，产出小红书内容 | Phase1 检索+查重 → Phase2 素材 → Phase3 选题(三道门禁) → Phase4 生产(并行) → Phase5 复盘 → Phase6 汇编 → Phase7 记忆更新 |
| 养号增长方案 | `../workflows/account-growth.md` | 问养号 / 涨粉 / 冷启动 | growth-analyst(串行) → 落盘 → 汇编 → Phase7 |
| 单点查重 / 重构 | （Workflow 1 子集） | 只带一段文案 / 标题要求查重或重构 | competitor-scout(查重) → viral-copywriter(重构) → 二次查重 → 结论 |

## 业务调用链（能力编排，Cron 自动运行核心）
```
competitor-scout  ──检索热门内容──▶  engagement-analyzer
   （查重/趋势）                        （分析为什么火，输出规律）
        │                                     │
        │                                     ▼
        │                              viral-copywriter
        │                                （按规律生产正文）
        │                                     │
        │                                     ▼
        └──────────────────────────▶  humanize-writing
                                          （账号人格化收尾）
                                                │
                                                ▼
                                          final content
```
- engagement-analyzer 在 Phase 1 检索后、Phase 4 生产前显式介入，把「竞品为什么火」的规律交给 viral-copywriter；同时在 Phase 5 复盘时由 growth-analyst 调用（双重角色）。
- humanize-writing 在 Phase 4 由 viral-copywriter 收尾调用，绑定 `../memory/account/style.md` 做账号人格化。

> 定时任务（Cron）驱动整套链路的规则见 `../agents/team-lead.md` 的「定时任务（Cron）调度规则」段。
