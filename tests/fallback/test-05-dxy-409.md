# Test 05 · dxy 409 合规冷却用快照（不报失败）

**验证点**：dxy 医疗专项源返回 **409（合规冷却）**时，按 BLOCKED 处理——**改读快照、标注 snapshot_time，禁止报"检索失败"**。
**对应阶段**：阶段 5（接入 dxy-crawler 医疗专项源）
**状态**：✅ 通过（阶段 5 验证，文档级链路复核）

## 输入（场景）

医疗垂类内容生产，`domain=medical`，team-lead / competitor-scout 在 Phase 1 调用 dxy 热榜做趋势：

```
距上次采集不足 5 分钟 → POST /api/crawl/trigger 返回 409
```

## 通过标准

| # | 标准 | 判定方式 |
|---|---|---|
| 1 | 409 判定为 **BLOCKED**，而非 UNAVAILABLE / 失败 | 引用 `core/tool-status.md` 四态表 |
| 2 | 自动切换 `source_type=snapshot`，读最近快照 | 引用 content-research 专项源小节 |
| 3 | 结论**携带 `snapshot_time`** 标注数据时间 | 引用 snapshot_time 规则 |
| 4 | 报告措辞为「专项源在合规冷却中，已用 N 分钟前快照」，**不得出现"检索失败 / 无数据"** | 引用 tool-status 专项源传导第 3 条 |
| 5 | **不重试** | 引用 tool-status 专项源传导第 4 条 |
| 6 | 若快照也不存在 → 按 UNAVAILABLE 走降级链（dxy → bb-browser 公开检索 → RESEARCH_UNAVAILABLE 停止） | 引用降级链 |

## 实测记录

### 验证（阶段 5，文档级链路复核）
两条判定链路均完整可追溯：

**DoD 1 · 409 用快照不报错**
1. `core/tool-status.md` 四态表 → BLOCKED = 被合规/策略限制（非故障）→ 不重试，使用现有资源
2. `core/tool-status.md`「专项源的 BLOCKED 传导」→ 第 1 条 **BLOCKED ≠ 失败**；第 2 条自动改读快照；第 3 条报告措辞；第 4 条不重试
3. `tools/research/content-research.md` 错误降级段 → **「BLOCKED ≠ 失败：dxy 返回 409 时，不得报"检索失败"，改用最近快照并标注 snapshot_time」**

**DoD 2 · dxy 挂降级公开检索**
1. `tools/research/content-research.md` 降级链图 → dxy → bb-browser 公开检索 → RESEARCH_UNAVAILABLE 停止
2. `core/tool-status.md` 专项源传导第 5 条 → 快照不存在时按 UNAVAILABLE 走降级链
3. 降级须显式标注 `primary_tool` / `fallback_tool` / `confidence` / 数据时间，**禁止伪装**（不得宣称"已全面检索医疗热榜"）

- 判定：✅ 通过

## dxy 专项源关键约束速查

| 项 | 值 |
|---|---|
| 服务地址 | `127.0.0.1:8000`（**无鉴权**，勿对外暴露） |
| 合规间隔 | 5 分钟硬限制，间隔不足返回 **409** |
| 详情采集 | 约 2-3 分钟，**必须异步** |
| 数据用途 | **只服务 Phase 1（趋势/查重），不得服务 Phase 2 素材**（UGC = C 级，商用生产有判例风险） |
| source_type | `live`（实时触发，受 5 分钟约束）/ `snapshot`（读已落盘快照，**默认推荐**） |

## 回归提示

若后续改动触及以下任一项，须重跑本用例：
- `tools/research/content-research.md` 的「医疗垂类专项源」小节或错误降级段
- `core/tool-status.md` 的四态表或「专项源的 BLOCKED 传导」
- `workflows/content-pipeline.md` Phase 1 的医疗垂类引用块
- `registry/tools.md` 的 dxy-crawler 登记行
