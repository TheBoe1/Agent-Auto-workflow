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
| L2 | L1 + 检索工具链（`tools/research/*`） |
| L3 | L2 + `agents/team-lead.md` + `workflows/content-pipeline.md` + 相关成员定义（`agents/*.md`）+ 相关 `memory/` |

> **分类与复杂度无关，属入口流程**：SKILL.md 内置任务分类简表与路由简表（第 27-54 行），所有任务在入口即完成分类与路由，**不随复杂度加载分类器**。
> 仅当 SKILL.md 简表无法判定（混合意图 / 类型特征冲突，见 `core/task-router/task-classifier.md` 第 3-4 步）或需要类型全表路由（`core/task-router/task-types.md`）时，才按需细读这两个文件。L0 轻量任务若分类无争议，不读 task-classifier / task-types。

> **`references/` 按需加载**：本表不含 references，它不按复杂度触发，而按**具体知识点需求**触发。
> 当需要领域细节时（如平台规则、医疗合规细则、去 AI 味手法清单），查 `references/README.md` 的登记表，命中才读。**无需求不读**。

## 按任务类型加载（agent / memory）

| 任务类型 | 主调 agent | 读 memory |
|---|---|---|
| CONTENT_PRODUCTION | 全员 | topics/ angles/ stories/ account/style/ content/ **patterns/** |
| CONTENT_RECONSTRUCTION | viral-copywriter | angles/ content/ **patterns/**（换 PAT-ID 优先于换角度） |
| CONTENT_RESEARCH | competitor-scout | topics/ |
| CONTENT_ANALYSIS | growth-analyst | performance/ |
| CONTENT_REVIEW | team-lead | content/ |
| HUMANIZE_AUDIT | viral-copywriter | account/style/ |
| ACCOUNT_GROWTH | growth-analyst | performance/ account/ **patterns/**（机制表现回测） |

## 加载纪律

1. **不相关 Agent 不加载**：SKILL.md 的团队表格只作导航，成员详细定义按需进入 `agents/*.md`。
2. **不相关工具不加载**：工具说明在 `tools/` 与 `registry/tools.md`，按任务类型选择。
3. **memory 只读**：所有读取遵循"team-lead 唯一写"协议（见 SKILL.md 与 `memory/README.md`）。
4. **分级判断错误时**：按 `complexity-router.md` 的升级 / 降级规则修正后重新规划。
5. **references 按知识点触发**：`references/` 不参与复杂度分级，只在需要领域细节时按 `references/README.md` 登记表命中才读；无需求不读。
