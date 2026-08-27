# humanize-writing · 文本拟人化工具

> 缪生花（爆款文案师）的润色工具。把「AI 味」明显的文案改写成自然、有人味的中文。
> 依赖：LLM（**provider 无关**）。

## 职责

- 去除 AI 痕迹：过度连接词、排比、夸张象征、破折号滥用、三段式法则等。
- 适配平台语气：小红书口语 / 公众号沉稳 / 短视频节奏。
- 保留原意与事实，不编造、不改数据。

## 统一工具接口（Tool Schema）

**输入（JSON）：**

```json
{
  "text": "原始文案（AI 生成或草稿）",     // 必填
  "tone": "xiaohongshu|wechat|shortvideo", // 平台语气
  "intensity": "light|medium|strong",       // 改写强度
  "keep_facts": true                        // 是否严格保留事实（默认 true）
}
```

**输出（JSON）：**

```json
{
  "text": "拟人化后文本",
  "changes": [                              // 改动说明（可追溯）
    {"type":"删连接词", "before":"首先…其次…", "after":"…"}
  ],
  "confidence": 0.9,                        // 拟人化置信度
  "status": "ok|partial|failed",
  "fallback_used": false
}
```

## 零配置运行（Demo）

- **依赖：无**（不调 LLM）
- 命令：`humanize-writing --demo`
- 行为：读取 `samples/humanize-writing/example.json`（含 `input` + 预计算 `output`），直接返回样例结果，演示「AI 文本 → 人味文本」的差异。
- 用途：无 key 也能看到工具能力边界，验证接口契约。

## 真实运行（最小配置）

- 必填（**LLM 抽象层，provider 无关**）：
  - `LLM_API_KEY`
  - `LLM_BASE_URL`（如 `https://api.openai.com/v1` 或本地 Ollama `http://localhost:11434/v1`）
  - `LLM_MODEL`（如 `gpt-4o-mini` / `claude-3-5` / `qwen2.5:14b`）
- 配置见 `.env.example`。**换模型只需改这三个变量，不改工具代码。**
- 命令：`humanize-writing --text "..." --tone xiaohongshu`

## LLM 抽象层

- 工具只认 **OpenAI-compatible** 的 `/v1/chat/completions` 接口。
- 支持：OpenAI / Azure / Claude(兼容网关) / Gemini(兼容网关) / 本地 Ollama / 阿里云百炼 DashScope(兼容网关)。
- 用户只需填 `endpoint + key + model`，满足「换模型不改代码」。

## 配置管理

- 启动时校验 `LLM_API_KEY` / `LLM_BASE_URL` / `LLM_MODEL` 三者齐全，缺失则报错：`请配置 LLM_API_KEY / LLM_BASE_URL / LLM_MODEL，方法见 .env.example`，不抛栈。

## 错误降级（fallback）

- **MUST**：LLM 不可达且为关键发布节点 → 停止并报错。
- **SHOULD**：非关键润色 → 返回原文 + warning（`fallback_used=true`），由缪生花决定是否人工润色。

## 缓存

- 相同 `(text + tone + intensity)` 的 LLM 结果缓存到 `.cache/humanize/{hash}.json`，避免重复烧钱。

## License

- 本工具定义：**MIT**（TheBoe1）
- 借鉴参考：`blader/humanizer`、`lguz/humanize-writing-skill`。**二者 license 尚未核实，禁止直接复制其代码**，仅借鉴「去 AI 痕迹」的方法论。核实前状态见 `third-party-licenses.md`。

## 安全

- `LLM_API_KEY` 仅存 `.env`（gitignore），不进仓库、不打印到中间文件。

## 测试

- 固定样例：`samples/humanize-writing/example.json`（input → expected output）。
- 自测：`--demo` 稳定返回 `changes` 非空、`status=ok`。
