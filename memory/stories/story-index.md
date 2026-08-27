# 故事记忆 · Story Memory（Asset Library）

> 真实素材是 MCN 的**内容资产**，不是一次性中间文件。每个故事一旦采集并验证，永久进入本资产库。

## 资产条目 Schema

```yaml
ST-ID: ST-0001
主题: "医疗人文/科室故事"
人物: "心衰大爷（已脱敏）"
场景: "夜班急诊"
冲突: "一个人扛着两个人的日子"
关键细节: "泡面凉了/女儿局促/旧布袋/瞟时钟"
可用角度: [人间真实, 医者仁心]
不可用角度: [具体病情诊断细节]
used_count: 0
used_in: []
sensitivity: normal     # normal | need-deid | restricted
来源: "作者历史笔记（一手）"
授权: "已授权"
```

## 与 runs/ 的关系
- 单次 run 中，采真人把本 run 用到的故事写在 `runs/{run_id}/02_stories.md`。
- run 结束后，team-lead 把**新采集且验证过**的故事登记进本表（分配 ST-ID、初始化 used_count=0），
  并把**被使用**的故事 `used_count +1`、`used_in` 追加 content_id。

## 写入者
仅 team-lead（记忆更新协议）。采真人只写 runs/ 中间文件。

## 当前资产
（运行后由 team-lead 填充；首次运行前为空，采真人从用户/公开源采集）
