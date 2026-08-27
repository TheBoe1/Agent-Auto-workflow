# Agent Registry · 成员索引

> **单一事实来源**：agent 的完整职责、契约与 Memory 规则见各 `../agents/*.md`。本表只做索引，调度前必须读取对应 .md，不要凭记忆猜测。

| Agent ID | 花名 | 职业 | 路径 | 核心职责 | 触发条件 |
|----------|------|------|------|---------|---------|
| team-lead | 甄有料 | MCN Orchestrator（OS） | `../agents/team-lead.md` | 编排、调度、门禁、记忆唯一管理者、最终审核 | 总入口 / 所有任务 |
| competitor-scout | 猎同频 | 对标检索师 | `../agents/competitor-scout.md` | 同类型检索、趋势、竞品、用户画像、四维查重 | 查重 / 竞品 / 趋势 / 选题 |
| story-collector | 采真人 | 真实故事采集师 | `../agents/story-collector.md` | 真实素材采集、结构化、脱敏合规 | 真实故事 / 素材 |
| viral-copywriter | 缪生花 | 爆款文案师 | `../agents/viral-copywriter.md` | 标题、正文、重构（蓝海角度）、humanize 收尾 | 文案 / 标题 / 重构 |
| visual-designer | 乔美设 | 视觉封面师 | `../agents/visual-designer.md` | 封面、配色、配图 prompt | 封面 / 配图 |
| growth-analyst | 步得清 | 增长复盘师 | `../agents/growth-analyst.md` | 养号、冷启动、内容体检、数据复盘、发布时机 | 养号 / 涨粉 / 复盘 |

## 调度铁律（与 team-lead 一致）
1. **团队创建只能由 team-lead 执行**（TeamCreate），其余成员不得自组团队。
2. **调用前必须读取对应 .md**，完整职责与契约以该文件为准。
3. 成员只产出自己职责内的文件；跨成员信息流必须经 team-lead 中转，不得互相直连。
4. Agent 契约标准化见各 agent 文件的「## Agent Contract（标准化契约）」段。
