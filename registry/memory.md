# Memory Registry · 记忆索引

> **单一事实来源**：记忆系统说明与写入协议见 `../memory/README.md`。本表只做索引。
> **唯一管理者 = team-lead**（Agent 只 READ，team-lead 才 WRITE，Phase 7 统一 promote）。

| 记忆类 | 路径 | 作用 |
|--------|------|------|
| account（画像） | `../memory/account/profile.md` | 账号定位、人设、目标用户 |
| account（语言 DNA） | `../memory/account/style.md` | 账号语言风格，humanize-writing 绑定此文件做人格化 |
| content-history | `../memory/content/` | 已发布 / 草稿 / 淘汰登记，防重发 |
| topic-memory | `../memory/topics/` | 主题树 + 使用次数 + 已用角度 |
| angle-memory | `../memory/angles/angle-memory.md` | 主题→已用角度（**防角度重复，最关键**） |
| story-memory | `../memory/stories/story-index.md` | 真实素材资产库（ST-ID） |
| performance-memory | `../memory/performance/performance-memory.md` | 发布后数据 → 有效规律 |

## 写入协议（Write Protocol，摘要）
1. Agent 产出落 `runs/{run_id}/`，**不直接写 memory/**。
2. team-lead 在 Phase 7 审核 → 判定 `APPEND / MERGE / UPDATE / REJECT`。
3. 经冲突检查（content_fingerprint 比对）后写入，并追加 `memory/CHANGELOG.md`。
4. 新结果一律 append / version，**禁止覆盖**历史。

> 候选记忆层：runs 产出经甄有料审核后 promote 进 memory/。详见 `../memory/README.md` 的 Write Protocol。
