---
type: integration
name: content-research
role: research-facade
status: stable
capability_origin:
  - bb-browser
  - research-workflow
new_capability: false
primary_agent:
  - competitor-scout
---

# content-research · 内容研究编排入口（整合层 / Facade）

> **这是底层检索能力的业务封装层，不提供独立的新检索能力。** 它只把 `bb-browser`（真实浏览器搜索）+ `research-workflow`（结构化研究方法论）收敛成一个稳定业务接口供猎同频调用；底层工具替换（如 Firecrawl / Exa）时，上层 agent 无需改动。

> 猎同频（对标检索师）的统一检索入口。封装 `bb-browser`（真实浏览器搜索）+ `research-workflow`（结构化研究方法论），对外暴露**单一 Tool Schema**。
> 定位：把"零散的搜索命令"收敛成"一个 agent 可调用的研究工具"，让上层不必关心底层适配器差异。

## 职责

- 对标检索：找同类型账号 / 爆款笔记 / 竞品。
- 趋势与选题：多查询检索 + 综合。
- 查重：同主题多来源检索，输出相似度与来源。
- 所有结论**必须附来源 URL**；检索失败显式标注，禁止臆断「无重复」。

## 统一工具接口（Tool Schema）

**输入（JSON）：**

```json
{
  "topic": "医生IP 抗老护肤",          // 必填，研究主题
  "platform": "xiaohongshu",          // 目标平台（可选，默认全平台）
  "mode": "quick|deep",               // quick=单查询；deep=research-workflow 四阶段
  "depth": 5,                         // deep 模式查询数（默认 5）
  "demographics": "25-35 岁女性"       // 可选，用户画像约束
}
```

**输出（结构化 JSON）：**

```json
{
  "query": "医生IP 抗老护肤",
  "mode": "deep",
  "findings": [                       // 发现列表
    {"claim": "...", "consensus": "high|mid|low", "sources": ["url1","url2"]}
  ],
  "sources": [                        // 来源清单
    {"title":"...", "url":"...", "platform":"...", "retrieved_at":"ISO8601"}
  ],
  "similarity": {"score": 0.62, "basis": "..."},   // 查重（可选）
  "status": "ok|partial|failed",
  "fallback_used": false              // 是否走了降级
}
```

## 零配置运行（Demo）

- **依赖：无**（不启动浏览器、不联网）
- 命令：`content-research --demo --topic "医生IP 抗老"`
- 行为：读取 `samples/content-research/demo-brief.md`（脱敏内置样例），原样返回结构化报告。
- 用途：用户 clone 下来就能看到「一份完整的对标检索报告长什么样」，验证整条流水线能跑通。

## 真实运行（最小配置）

- 必填依赖：
  1. 已安装 `bb-browser`（见 `tools/bb-browser.md`，需 Node + 系统 Chrome）
  2. 系统 Chrome 可被发现（或设置 `CHROME_PATH`）
- 可选：`BB_BROWSER_DAEMON_URL`（默认 `127.0.0.1:19824`）
- 命令：`content-research --topic "医生IP 抗老" --mode deep`
- **小红书登录态**：如需抓小红书，在 `.env` 填 `XIAOHONGSHU_COOKIE`（见 `.env.example`）。无 cookie 时自动降级到 wikipedia / arxiv / baidu 等公开适配器，并标记 `fallback_used=true`。

## 配置管理

- 配置文件：`.env`（或 `config.yaml`）。**启动时校验**：
  - `bb-browser` 命令是否可达 → 不可达报错：`请先安装 bb-browser，方法见 tools/bb-browser.md`
  - Chrome 是否可用 → 不可用报错：`未找到 Chrome，请安装或设置 CHROME_PATH`
- 缺失配置时给出**明确可读报错**，不抛栈、不淹没在日志里。

## 错误降级（fallback）

- **MUST 工具失败**（浏览器打不开 / daemon 挂）：停止并报错，进入「风险确认节点」等待用户。
- **SHOULD 工具降级**：平台改版 / 限流 / 登录失效 → 用缓存（`.cache/`）或 `samples/` 降级继续，标记 `fallback_used=true`，输出注明「数据非实时」。

## 缓存与限流

- 搜索结果缓存到 `.cache/content-research/{hash}.json`，TTL 24h。
- 同查询 24h 内不重复拉取，降低限流风险与耗时。

## License

- 本工具定义：**MIT**（TheBoe1）
- 底层 `bb-browser`：**MIT**（epiral）✓
- 底层 `research-workflow`：来自 `jwynia/agent-skills`，**上游未声明 license（实测仓库无 LICENSE 文件、首页无 license 声明）**。⚠️ 当前仅作「方法论借鉴」记录于文档，不复制其代码；若需内置其 SKILL.md，必须先取得作者授权。详见 `third-party-licenses.md`。

## 安全

- 本工具本身不持有密钥；小红书 cookie 仅存于 `.env`（已 gitignore），不进仓库、不写进中间文件。

## 测试 / 可验证性

- 固定样例：`samples/content-research/demo-brief.md`（输入）→ 期望输出结构见上文「输出」。
- 自测：`content-research --demo` 必须稳定返回 `status=ok`、`sources` 非空、`fallback_used=false`。
