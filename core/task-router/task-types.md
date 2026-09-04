# 任务类型定义（Task Types）

系统入口收到请求后，先分类为 7 种任务类型之一（分类流程见 `task-classifier.md`，任务对象契约见 `task-schema.md`）。

## 7 种任务类型

| 任务类型 | 含义 | 是否产出内容 | 是否建团 | 路由 |
|---|---|---|---|---|
| `CONTENT_PRODUCTION` | 从主题生产完整内容 | 是 | 建团 | `workflows/content-pipeline.md` 全流程 |
| `CONTENT_RECONSTRUCTION` | 高重复内容重新构建（蓝海角度） | 是（重构） | 不建团 | 需求澄清闸门 → viral-copywriter 直调 + 查重门禁 |
| `CONTENT_RESEARCH` | 只做内容/市场研究 | 否 | 不建团 | content-research（competitor-scout） |
| `CONTENT_ANALYSIS` | 分析已有内容/爆款机制 | 否 | 不建团 | content-research + engagement-analyzer（growth-analyst） |
| `CONTENT_REVIEW` | 对已有内容做发布前审核 | 否 | 不建团 | humanize-writing + 查重 + 平台合规核对 |
| `HUMANIZE_AUDIT` | 去 AI 味 / 人味审查 | 否（改表达） | 不建团 | humanize-writing（viral-copywriter） |
| `ACCOUNT_GROWTH` | 养号、增长、发布策略 | 否（策略） | 不建团 | `workflows/account-growth.md`（growth-analyst） |

## 各类型判定特征

### CONTENT_PRODUCTION（内容生产）

- 用户话术："帮我做一篇 / 写一篇 / 出一篇小红书（图文/视频）"、"围绕 XX 主题做内容"
- 判定要点：**要求产出可发布内容**，通常带主题或方向
- 前置：触发需求澄清闸门（账号定位 / 目标受众 / 内容方向，见 SKILL.md 编排 SOP 与 `workflows/content-pipeline.md` Phase 0）

### CONTENT_RECONSTRUCTION（内容重构）

- 用户话术："这篇和 XX 太像了，换个角度重写"、"重构这篇"
- 判定要点：已有内容 + 查重结论为高重复，需换蓝海角度重写
- **需求澄清**：重构也产出新版本，须先过需求澄清闸门（账号定位 / 目标受众 / 内容方向，见 SKILL.md 编排 SOP 与 `content-pipeline.md` Phase 0）——但重构的对象是既有内容，账号 / 受众若与原篇一致且可从原篇推断，可只确认内容方向；无法推断时仍需追问。

### CONTENT_RESEARCH（内容研究）

- 用户话术："调研一下 XX 方向"、"最近 XX 有什么热门话题"
- 判定要点：**只研究，不要求产出内容**

### CONTENT_ANALYSIS（内容分析）

- 用户话术："分析一下这篇为什么火"、"看看别人都是怎么写的"、"拆解这个爆款"
- 判定要点：**只分析/拆解，不要求产出内容**
- 与 CONTENT_PRODUCTION 的核心区分：出现"帮我写 / 做一篇"才是生产；只有"看看 / 分析 / 拆解"是分析。意图混合（先分析再写）归 `CONTENT_PRODUCTION`，但先执行研究阶段。

### CONTENT_REVIEW（发布前审核）

- 用户话术："这篇能发吗"、"帮我审一下这篇稿子"
- 判定要点：已有完整内容，做发布前安全与质量审核

### HUMANIZE_AUDIT（去 AI 味）

- 用户话术："这段话帮我改得像人写的"、"去一下 AI 味"
- 判定要点：已有文本，只改表达不改事实

### ACCOUNT_GROWTH（账号增长）

- 用户话术："帮我规划养号"、"怎么涨粉"、"账号冷启动"
- 判定要点：账号运营策略，非单篇内容

## 不确定时

分类器无法确定类型时，向用户确认任务意图，不猜测（见 `task-classifier.md` 第 4 步）。
