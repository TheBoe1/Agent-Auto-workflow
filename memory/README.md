# Content Memory · 内容资产记忆系统

> 这是 MCN-Agent-Studio 的**长期状态层**。它不是「聊天记忆」，而是团队的**内容资产库**：
> 账号过去说过什么、用过什么角度、讲过什么故事、采用过什么表达、什么内容真正有效。

## 为什么需要它

没有记忆的系统会反复犯同一个错误：今天写「我用了 30 天 AI」，明天又写「我用了 30 天 AI」，
后天写「我用了一个月 AI」——语义不同，实则重复。Content Memory 让系统知道：

- 这个**主题**写过几次、哪些**角度**已用
- 这个**故事**讲过没有、被哪篇用过
- 账号的**语言风格**是什么（去 AI 味靠它，不是每次重新猜）
- 什么**内容真的有效**（learning loop 的数据基础）

## 七大记忆类

| # | 记忆类 | 目录 | 职责 |
|---|--------|------|------|
| 01 | content-history | `content/` | 已发布 / 草稿 / 淘汰内容登记，防重发 |
| 02 | topic-memory | `topics/` | 选题主题树 + 使用次数 + 已用角度 + 表现 |
| 03 | angle-memory | `angles/angle-memory.md` | 主题→已用角度登记（**比 content-history 更关键**） |
| 04 | story-memory | `stories/story-index.md` | 真实素材资产库（ST-ID 资产化，非一次性） |
| 05 | style-memory | `account/style.md` | 账号语言 DNA、禁止表达、标题/开头/结尾风格 |
| 06 | performance-memory | `performance/performance-memory.md` | 发布后数据 → 有效内容规律 |
| 07 | **pattern-memory** | `patterns/` | **内容机制库**：`viral-patterns.md`（正文骨架 PAT-ID）+ `engagement-patterns.md`（互动触发 ENG-ID） |

> **Angle ≠ Pattern**：角度是「从哪儿切入」（存 `angles/`），机制是「怎么展开 / 怎么引发互动」（存 `patterns/`）。
> 两者正交——角度用尽可换机制，机制用老可换角度。详见 `patterns/viral-patterns.md` 开篇。
> Pattern 与 `content_fingerprint.structure` 绑定：**换 PAT-ID 即构成有效重构**，是撞车后成本最低的差异化手段。

## 唯一管理者：team-lead（甄有料）

**Memory 不是谁都能改的共享白板。** 否则 Agent A 说「用过」、Agent B 说「没用过」、Agent C 说
「用过类似」，Memory 自己会打架。

- **READ**：所有 Agent 在任务开始时**必须**读相关记忆类
  （猎同频读 `topics/`+`angles/` 防重复；缪生花读 `account/style` 对齐语气、**`patterns/` 选正文骨架与互动钩子**；采真人读 `stories/` 复用资产；步得清读 `performance/`+**`patterns/`** 对齐打法）。
- **WRITE**：**只有 team-lead 能写 Memory**。各 Agent 的产出落盘在 `runs/{run_id}/`（执行中间文件），
  run 结束后由 team-lead 执行「记忆更新协议」（`agents/team-lead.md` 的 Phase 7），把验证过的内容
  **promote** 进 Memory（append / version，**禁止直接覆盖历史**）。

## 与 runs/ 的关系

- `runs/{run_id}/`：单次执行的**中间文件**，用完即归档（本地，已被 `.gitignore` 忽略）。
- `memory/`：跨会话的**长期资产**。`02_stories.md`（本 run 用过的故事）→ promote 进 `stories/story-index.md`（全量资产）；
  `final_bundle` 发布后 → promote 进 `content/published/`；发布数据 → promote 进 `performance/`。

## Memory 写入协议（Write Protocol）

Agent 的产出只落盘 `runs/{run_id}/`，**不得直接写 `memory/`**。一轮结束后，team-lead 按以下协议把候选记忆提升（promote）进长期 Memory：

```
Agent 产出（runs/）
   ↓ 提交候选记忆（含 evidence + confidence）
team-lead 验证
   ↓
Memory Conflict Check（查重：该主题/角度/故事是否已记录）
   ↓
判定：APPEND / MERGE / UPDATE / REJECT
   ↓
写入 Memory（append / version，禁止覆盖历史）
   ↓
记录 CHANGELOG（谁、何时、为何）
```

- **APPEND**：全新条目，分配 ID（CONTENT-ID / ST-ID）。
- **MERGE**：与既有条目互补，合并字段。
- **UPDATE**：既有条目新增表现/使用计数。
- **REJECT**：重复或低置信度（附理由）。

> 即使 team-lead 自身，也**不得凭感觉写 Memory**——每一条写入都必须经过冲突检查与 CHANGELOG 留痕。

## 候选记忆层（memory-candidates）

为避免「执行中间文件」直接污染长期资产，引入候选层：

```
runs/{run_id}/          # 单次执行中间文件（用完即归档，gitignore）
   ↓ 甄有料审核
memory-candidates/      # 候选记忆（带 candidate_id / run_id / evidence / action）
   ↓ APPEND / MERGE / UPDATE / REJECT
memory/                 # 长期资产（七大记忆类）
```

候选记忆示例：

```yaml
candidate_id: MC-20260827-001
run_id: 20260827-001
type: angle
candidate:
  topic: 医疗人文
  angle: 临终前的细微动作
evidence:
  source: final_bundle
  confidence: high
action:
  type: APPEND
```

> 详见 `agents/team-lead.md` 的「七、阶段说明 → Phase 7 记忆更新协议」。
