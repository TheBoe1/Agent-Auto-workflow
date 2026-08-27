# engagement-analyzer · 互动内容分析工具

> 步得清（增长复盘师）的分析工具。输入一批高互动帖子，输出《互动内容分析报告》：提炼爆款规律、人群画像、可复用结构。
> 数据来源：本地文件（脱敏样本 / 用户导出）或 `content-research` 拉取的实时数据。

## 职责

- 规律挖掘：标题结构、开篇钩子、情绪落点、标签组合。
- 分层对比：高互动 vs 低互动的内容差异。
- 输出可执行建议：下一篇该写什么结构、什么钩子。

## 统一工具接口（Tool Schema）

**输入（JSON）：**

```json
{
  "posts": "path/to/posts.json | 查询词",  // 必填：本地文件路径，或交给 content-research 的查询词
  "metrics": ["likes","collects","comments"], // 分析维度
  "top_n": 10,                              // 取 top 分析
  "live": false                             // true=用 content-research 拉实时；false=读本地
}
```

**输出（JSON）：**

```json
{
  "report": {
    "sample_size": 50,
    "patterns": [                           // 爆款规律
      {"pattern":"白描叙事+克制冷静", "support_count":12, "examples":["P-001"]}
    ],
    "top_posts": [{"id":"...", "engagement":1234, "why":"..."}],
    "persona": {"gender":"女", "age":"25-35", "interests":["护肤","情感"]},
    "recommendations": ["下一篇用'时间+数字'钩子", "结尾加开放式提问"]
  },
  "status": "ok|partial|failed",
  "fallback_used": false
}
```

## 零配置运行（Demo）

- **依赖：无**（不联网、不调 LLM，纯规则分析）
- 命令：`engagement-analyzer --demo`
- 行为：读取 `samples/engagement-analyzer/posts.json`（脱敏高互动帖子），跑规则分析，输出与 `samples/engagement-analyzer/sample-report.md` 同款报告。
- 用途：clone 即见「一份互动分析报告长什么样」。

## 真实运行（最小配置）

- **方式 A（最小）**：提供本地 `posts.json`（用户从平台导出 / 脱敏），直接分析，**无需任何 key**。
- **方式 B（实时）**：`live=true`，调用 `content-research` 拉取目标账号 / 话题帖子（需 bb-browser + Chrome，见 `content-research`）。
- 可选 LLM：设 `LLM_API_KEY` 后，报告增加「深度语义归纳」段落（provider 无关，复用 humanize-writing 的抽象层）。
- 命令：`engagement-analyzer --posts data/my-posts.json --top_n 10`

## 配置管理

- `live=true` 时复用 `content-research` 的配置校验；本地模式无需任何配置。
- 缺失文件报错：`找不到 posts 文件：{path}，请检查路径或改用 --demo`。

## 错误降级（fallback）

- **MUST**：live 拉取全失败 → 停止并报错。
- **SHOULD**：live 部分失败 → 用已缓存 / 样本补全，标记 `fallback_used=true`。

## 缓存

- 实时拉取的 posts 缓存到 `.cache/engagement/{hash}.json`，TTL 24h。

## License

- 本工具定义与分析逻辑：**MIT**（TheBoe1）
- 参考的「模型无关」思路来自 lguz 项目，license 待核实，仅借鉴方法论（见 `third-party-licenses.md`）。

## 安全

- 用户导出的 posts 可能含隐私，落入 `data/` 目录（建议 gitignore）；本工具不对外发送用户数据。

## 测试

- 固定样例：`samples/engagement-analyzer/posts.json` + `sample-report.md`（输入 → 期望输出）。
- 自测：`--demo` 稳定输出 `patterns` 非空、`status=ok`。
