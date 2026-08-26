# 中间文件模板（8 个）

> 每个文件头带元信息（run_id / agent / created_at / input_files）。
> 使用时把对应模板复制为 `runs/{run_id}/` 下的文件。

---

## 00_input.md（主理人）

```markdown
---
run_id: {run_id}
agent: team-lead
input_files: []
---

# 任务输入

- 主题 / 大方向：
- 账号定位（人设 / 垂类 / 目标用户）：
- 对标参考（可选）：
- 目标（涨粉 / 变现 / 立人设）：
```

---

## 01_scout.md（猎同频）

```markdown
---
run_id: {run_id}
agent: competitor-scout
input_files: [00_input.md]
---

# 对标检索报告

## 同类型内容清单（附 URL + 标题 + 数据）
1. [标题](URL) — 点赞 / 收藏
2. ...

## 对标账号（2–3 个）
- 账号：类型 / 可借鉴模式

## 趋势判断
- 生命周期（上升 / 成熟 / 衰退）＋ 依据

## 查重结论
```json
{ "duplicate": false, "similarity": 0.62, "risk": "LOW", "reference": [], "verdict": "通过" }
```
```

---

## 02_stories.md（采真人）

```markdown
---
run_id: {run_id}
agent: story-collector
input_files: [00_input.md]
---

# 真实故事素材库

### 故事 ID：ST-001
- 来源：一手采访 / 评论区 UGC / 公开报道（附链接）
- 授权：已授权 / 需脱敏 / 不可用
- 人物：……（已脱敏）
- 场景：……
- 冲突：……
- 细节 / 金句：……
- 情绪落点：……
- 可信度：一手 / 二手 / 无法核实

### 故事 ID：ST-002
...
```

---

## 03_brief.md（主理人）

```markdown
---
run_id: {run_id}
agent: team-lead
input_files: [01_scout.md, 02_stories.md]
---

# 选题角度决策

- 选定角度：
- 依据（检索 + 素材）：
- 选用的故事条目：ST-00X
- 是否通过查重门禁：是 / 否（附 similarity）
```

---

## 04_copy.md（缪生花）

```markdown
---
run_id: {run_id}
agent: viral-copywriter
input_files: [03_brief.md, 02_stories.md, 01_scout.md]
---

# 文案

## 标题（20 个）
- 好奇型：...
- 数据型：...
- 冲突型：...

## 正文
（黄金 3 秒开头 → 痛点 → 经历 → 方法论 → 行动建议 → 互动提问）
（每个故事段落标注来源 ST-00X）

## 标签
#...

## 互动提问
...
```

---

## 05_visual.md（乔美设）

```markdown
---
run_id: {run_id}
agent: visual-designer
input_files: [04_copy.md]
---

# 视觉方案

- 封面主标题（≤10 字）：
- 副标：
- 配图 prompt：
- 配色（主 / 辅 / 文字，hex）：
```

---

## 06_review.md（步得清）

```markdown
---
run_id: {run_id}
agent: growth-analyst
input_files: [04_copy.md, 05_visual.md, 00_input.md]
---

# 发布与复盘建议

- 内容体检（优势 / 风险）：
- 最佳发布时间：
- 预期指标（预估）：
- 复盘模板（24h / 72h）：
- 下一轮迭代方向：
```

---

## final_bundle.md（主理人）

```markdown
---
run_id: {run_id}
agent: team-lead
input_files: [01_scout.md, 02_stories.md, 03_brief.md, 04_copy.md, 05_visual.md, 06_review.md]
---

# 可发布内容包

- 标题：
- 正文：
- 标签：
- 封面 / 配图：
- 发布建议：
```
