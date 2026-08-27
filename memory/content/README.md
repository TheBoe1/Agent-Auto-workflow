# 内容历史 · Content History

> 记录已发布 / 草稿 / 淘汰内容，防止已发布内容被再次生产。

## 条目 Schema（每个内容一个文件，命名 `CONTENT-YYYYMMDD-NNN.md`）

```yaml
content_id: CONTENT-20260826-001
title: "病房里最安静的病人"
topic: "医疗人文/科室故事"
published_at: 2026-08-26
status: published        # published | draft | rejected
url: "https://www.xiaohongshu.com/..."

summary: "..."
core_viewpoint: "..."

structure:
  hook: "..."
  ending: "..."

keywords: [...]

used_story_ids: [ST-0001, ST-0002]

similarity_fingerprint: "sha256:..."   # 用于快速重复检测
```

## 目录
- `published/`：已发布内容（复盘数据来自这里）
- `drafts/`：已生产未发布
- `rejected/`：被门禁淘汰或表现差的内容（留档防重犯）

## 写入者
仅 team-lead（记忆更新协议）。各 Agent 不直接写。
