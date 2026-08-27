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

## 六大记忆类

| # | 记忆类 | 目录 | 职责 |
|---|--------|------|------|
| 01 | content-history | `content/` | 已发布 / 草稿 / 淘汰内容登记，防重发 |
| 02 | topic-memory | `topics/` | 选题主题树 + 使用次数 + 已用角度 + 表现 |
| 03 | angle-memory | `angles/angle-memory.md` | 主题→已用角度登记（**比 content-history 更关键**） |
| 04 | story-memory | `stories/story-index.md` | 真实素材资产库（ST-ID 资产化，非一次性） |
| 05 | style-memory | `account/style.md` | 账号语言 DNA、禁止表达、标题/开头/结尾风格 |
| 06 | performance-memory | `performance/performance-memory.md` | 发布后数据 → 有效内容规律 |

## 唯一管理者：team-lead（甄有料）

**Memory 不是谁都能改的共享白板。** 否则 Agent A 说「用过」、Agent B 说「没用过」、Agent C 说
「用过类似」，Memory 自己会打架。

- **READ**：所有 Agent 在任务开始时**必须**读相关记忆类
  （猎同频读 `topics/`+`angles/` 防重复；缪生花读 `account/style` 对齐语气；采真人读 `stories/` 复用资产；步得清读 `performance/` 对齐打法）。
- **WRITE**：**只有 team-lead 能写 Memory**。各 Agent 的产出落盘在 `runs/{run_id}/`（执行中间文件），
  run 结束后由 team-lead 执行「记忆更新协议」（`agents/team-lead.md` 的 Phase 7），把验证过的内容
  **promote** 进 Memory（append / version，**禁止直接覆盖历史**）。

## 与 runs/ 的关系

- `runs/{run_id}/`：单次执行的**中间文件**，用完即归档（本地，已被 `.gitignore` 忽略）。
- `memory/`：跨会话的**长期资产**。`02_stories.md`（本 run 用过的故事）→ promote 进 `stories/story-index.md`（全量资产）；
  `final_bundle` 发布后 → promote 进 `content/published/`；发布数据 → promote 进 `performance/`。

> 详见 `agents/team-lead.md` 的「七、阶段说明 → Phase 7 记忆更新协议」。
