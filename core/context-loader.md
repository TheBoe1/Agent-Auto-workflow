# 上下文规划（Context Loader）

按"任务类型 × 复杂度"两级判断，决定本次任务加载哪些资源。原则：**只加载本次任务需要的**，不全量加载（渐进式披露）。分类与分级判断见 `task-router/task-classifier.md` 与 `complexity-router.md`。

## 必载

- `SKILL.md`：入口、任务分类简表、复杂度简表、编排 SOP、铁律
- `core/complexity-router.md`：复杂度判定依据
- `core/context-loader.md`：本加载策略（执行按需加载前读取）

## 按复杂度加载

| 级别 | 额外加载 |
|---|---|
| L0 | 目标工具文档（如 `tools/writing/humanize-writing.md`） |
| L1 | L0 + `registry/tools.md`（工具索引） |
| L2 | L1 + 检索工具链（`tools/research/*`）+ 分类存疑时 `core/task-router/task-classifier.md` 细判 |
| L3 | L2 + `agents/team-lead.md` + `workflows/content-pipeline.md` + 相关成员定义（`agents/*.md`）+ 相关 `memory/` |

## 按任务类型加载（agent / memory）

| 任务类型 | 主调 agent | 读 memory |
|---|---|---|
| CONTENT_PRODUCTION | 全员 | topics/ angles/ stories/ account/style/ content/ |
| CONTENT_RECONSTRUCTION | viral-copywriter | angles/ content/ |
| CONTENT_RESEARCH | competitor-scout | topics/ |
| CONTENT_ANALYSIS | growth-analyst | performance/ |
| CONTENT_REVIEW | team-lead | content/ |
| HUMANIZE_AUDIT | viral-copywriter | account/style/ |
| ACCOUNT_GROWTH | growth-analyst | performance/ account/ |

## 加载纪律

1. **不相关 Agent 不加载**：SKILL.md 的团队表格只作导航，成员详细定义按需进入 `agents/*.md`。
2. **不相关工具不加载**：工具说明在 `tools/` 与 `registry/tools.md`，按任务类型选择。
3. **memory 只读**：所有读取遵循"team-lead 唯一写"协议（见 SKILL.md 与 `memory/README.md`）。
4. **分级判断错误时**：按 `complexity-router.md` 的升级 / 降级规则修正后重新规划。
