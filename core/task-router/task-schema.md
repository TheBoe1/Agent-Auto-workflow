# 任务对象契约（Task Schema）

分类器输出统一的任务对象，供后续复杂度判断、上下文规划与工作流选择使用（分类流程见 `task-classifier.md`，类型定义见 `task-types.md`）。

## 任务对象字段

```yaml
task:
  task_type: CONTENT_PRODUCTION        # 7 种类型之一，必填
  platform: xiaohongshu                 # 内容平台，可扩展 douyin 等
  domain: general                       # 领域：general / medical / 其他垂类
  account: null                         # 账号定位，生产类必填（缺失先澄清）
  audience: null                        # 目标受众，生产类必填（缺失先澄清）
  angle: null                           # 内容方向/主题切入点，生产类必填（缺失先澄清）
  deliverable: note                     # 交付物：note(图文) / video(视频脚本) / copy(纯文案) / report(分析报告) / strategy(策略)
  source_material: null                 # 素材来源：ST-ID / 公开报道 / 用户提供 / 无
```

## 字段约束

- `task_type`：必填，且必须为 `task-types.md` 中 7 种之一。
- `platform` / `domain`：可推断时填，推断不出时留空并由下游按通用处理。
- `account` / `audience` / `angle`：`CONTENT_PRODUCTION` 与 `CONTENT_RECONSTRUCTION` 必填；缺失时触发需求澄清闸门，确认前不进入后续阶段。
- `source_material`：生产类任务在素材门禁（`workflows/content-pipeline.md` Phase 3）前必须非空，否则禁止进入文案。

## 使用方式

任务对象在分类完成后写入 `runs/{run_id}/00_input.md` 头部，供各阶段读取；后续阶段不得回改任务类型，确有需要时由主理人确认后更新。
