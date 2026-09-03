---
type: integration
name: content-research
role: research-facade
status: stable
capability_origin:
  - bb-browser
  - research-workflow
  - dxy-crawler
new_capability: false
primary_agent:
  - competitor-scout
---

# content-research · 内容研究编排入口（整合层 / Facade）

> **这是底层检索能力的业务封装层，不提供独立的新检索能力。** 它把 `bb-browser`（真实浏览器搜索）+ `research-workflow`（结构化研究方法论）+ `dxy-crawler`（丁香园医疗热榜专项源，仅医疗垂类启用）收敛成一个稳定业务接口供猎同频调用；底层工具替换（如 Firecrawl / Exa）时，上层 agent 无需改动。

> 猎同频（对标检索师）的统一检索入口。封装 `bb-browser`（真实浏览器搜索）+ `research-workflow`（结构化研究方法论）+ `dxy-crawler`（医疗垂类专项热榜源），对外暴露**单一 Tool Schema**。
> 定位：把"零散的搜索命令"收敛成"一个 agent 可调用的研究工具"，让上层不必关心底层适配器差异。

## 职责

- 对标检索：找同类型账号 / 爆款笔记 / 竞品。
- 趋势与选题：多查询检索 + 综合。
- 查重：同主题多来源检索，输出相似度与来源。
- **医疗垂类趋势（专项源）**：`domain=medical` 时，优先读取 `dxy-crawler` 热榜快照做医疗话题风向与选题参考（见下方「医疗垂类专项源」）。
- 所有结论**必须附来源 URL**；检索失败显式标注，禁止臆断「无重复」。

## 统一工具接口（Tool Schema）

**输入（JSON）：**

```json
{
  "topic": "医生IP 抗老护肤",          // 必填，研究主题
  "platform": "xiaohongshu",          // 目标平台（可选，默认全平台）
  "mode": "quick|deep",               // quick=单查询；deep=research-workflow 四阶段
  "depth": 5,                         // deep 模式查询数（默认 5）
  "demographics": "25-35 岁女性",       // 可选，用户画像约束
  "domain": "medical"                 // 可选，=medical 时启用医疗专项源（dxy 热榜），默认关闭
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

## 医疗垂类专项源（dxy-crawler）

> 医疗话题的**专项风向源**，非通用检索。仅 `domain=medical` 时启用；服务由独立仓 `dxy-crawler`（`https://github.com/Jaye-520/dxy-crawler.git`）部署，默认 `127.0.0.1:8000`。

### 数据用途边界（**硬约束**）

dxy 热榜是**论坛 UGC 帖**。按 `core/quality-gates.md` 素材分级，UGC = **C 级**：

- ✅ **只服务 Phase 1（趋势 / 选题 / 查重）**：读取热榜标题、热度、讨论方向，做医疗话题风向与选题参考。
- ❌ **不得服务 Phase 2（ST-ID 素材采集）**：热榜帖正文 / 评论**禁止直接当素材写进文案**。论坛 UGC 商用生产与「大众点评诉百度 / 丁香园诉医学界」判例属同一风险模式（实质性替代）。确需引用作者原文，须先取得原始作者授权（升级为 A 级授权素材）并标注来源。
- 引用结论（如"医生圈近期在热议 XX"）用**转述 + 第三人称**，禁止伪装成自有亲历。

### source_type 与 snapshot_time

- `source_type: live`：实时调用 dxy `POST /api/crawl/trigger` 采集（受 5 分钟合规间隔约束）。
- `source_type: snapshot`：读已落盘快照（`GET /api/latest` 或 `GET /api/snapshots/{date}/{filename}`），**默认推荐**——采集是低频操作，绝大多数场景读快照即可，避免触发合规冷却。
- `snapshot_time`：快照的 `crawl_time`（ISO8601）。任何用到快照的结论必须携带 `snapshot_time` 标注数据时间，禁止当实时数据呈现。

### 调用方式（REST，agent 经 curl / 脚本调）

| 用途 | 调用 | 说明 |
|---|---|---|
| 最新快照 | `GET /api/latest?fill_detail=true` | 含热榜 Top20 + 可回填最近详情 |
| 历史快照 | `GET /api/snapshots?date=&limit=&offset=` | 元信息分页 |
| 单帖趋势 | `GET /api/posts/{post_id}` | rank/热度/评论时间序列 |
| 统计 | `GET /api/stats` | 快照数 / 成功率 |
| 手动采集 | `POST /api/crawl/trigger` | body `{"with_detail":false}`；间隔不足返回 **409** |

> 服务**无鉴权**，仅监听 `127.0.0.1`（生产勿对外暴露）。详情采集约 2-3 分钟，必须异步（`with_detail=true` 时不阻塞同步调用）。

### 状态判定（并入 `core/tool-status.md` 四态）

- **409（合规冷却）→ BLOCKED**：距上次采集不足 5 分钟。**属常态非故障**，不重试；改用最近快照 `source_type=snapshot`。对上层报告须写"专项源在合规冷却中，已用 N 分钟前快照"，**不得报"检索失败"**。
- **连接失败 / 无快照 → UNAVAILABLE**：走降级链（见下）。
- **机器人验证（"检测到异常流量"）→ BLOCKED**：站点限流，等待数十分钟，不重试。

### 降级链（medical 检索）

```
dxy-crawler（专项医疗热榜）
   │ 不可用 / 无快照
   ▼
bb-browser 公开检索（baidu / wikipedia 等适配器，domain=medical 补充查询）
   │ 仍失败
   ▼
RESEARCH_UNAVAILABLE → 停止，或经用户确认后降级生产（须标注限制，见 core/tool-status.md 规则 4）
```

- 降级后必须显式标注：`primary_tool`（dxy）、`fallback_tool`（bb-browser）、`confidence`、数据时间。
- **禁止伪装**：dxy 挂了走 bb-browser，不得宣称"已全面检索医疗热榜"。

### 环境要求

- dxy 服务需独立部署（见上游 README：`pip install -r requirements.txt && playwright install chromium`，或 Docker `docker compose up -d`）。
- 本 skill 不内置 dxy 进程 / 端口 / 依赖管理；dxy 是独立 git 仓，保持独立。

## 零配置运行（Demo）

- **依赖：无**（不启动浏览器、不联网）
- 命令：`content-research --demo --topic "医生IP 抗老"`
- 行为：读取 `samples/content-research/demo-brief.md`（脱敏内置样例），原样返回结构化报告。
- 用途：用户 clone 下来就能看到「一份完整的对标检索报告长什么样」，验证整条流水线能跑通。

## 真实运行（最小配置）

- 必填依赖：
  1. 已安装 `bb-browser`（见 `tools/research/bb-browser.md`，需 Node + 系统 Chrome）
  2. 系统 Chrome 可被发现（或设置 `CHROME_PATH`）
- 可选：`BB_BROWSER_DAEMON_URL`（默认 `127.0.0.1:19824`）
- 命令：`content-research --topic "医生IP 抗老" --mode deep`
- **小红书登录态**：如需抓小红书，在 `.env` 填 `XIAOHONGSHU_COOKIE`（见 `.env.example`）。无 cookie 时自动降级到 wikipedia / arxiv / baidu 等公开适配器，并标记 `fallback_used=true`。
- **医疗模式（`domain=medical`）**：额外依赖 dxy 服务可达（默认 `http://127.0.0.1:8000`），失败走「医疗垂类专项源」降级链。

## 配置管理

- 配置文件：`.env`（或 `config.yaml`）。**启动时校验**：
  - `bb-browser` 命令是否可达 → 不可达报错：`请先安装 bb-browser，方法见 tools/research/bb-browser.md`
  - Chrome 是否可用 → 不可用报错：`未找到 Chrome，请安装或设置 CHROME_PATH`
- 缺失配置时给出**明确可读报错**，不抛栈、不淹没在日志里。

## 错误降级（fallback）

- **MUST 工具失败**（浏览器打不开 / daemon 挂）：停止并报错，进入「风险确认节点」等待用户。
- **SHOULD 工具降级**：平台改版 / 限流 / 登录失效 → 用缓存（`.cache/`）或 `samples/` 降级继续，标记 `fallback_used=true`，输出注明「数据非实时」。
- **BLOCKED ≠ 失败**（见 `core/tool-status.md`）：dxy 返回 409（合规冷却）时，**不得报"检索失败"**，改用最近快照并标注 `snapshot_time`；机器人验证同理，等待不重试。

## 缓存与限流

- 搜索结果缓存到 `.cache/content-research/{hash}.json`，TTL 24h。
- 同查询 24h 内不重复拉取，降低限流风险与耗时。

## License

- 本工具定义：**MIT**（TheBoe1）
- 底层 `bb-browser`：**MIT**（epiral）✓
- 底层 `dxy-crawler`：**MIT**（Jaye-520）✓，独立仓 `https://github.com/Jaye-520/dxy-crawler.git`
- 底层 `research-workflow`：来自 `jwynia/agent-skills`，**上游未声明 license（实测仓库无 LICENSE 文件、首页无 license 声明）**。⚠️ 当前仅作「方法论借鉴」记录于文档，不复制其代码；若需内置其 SKILL.md，必须先取得作者授权。详见 `third-party-licenses.md`。

## 安全

- 本工具本身不持有密钥；小红书 cookie 仅存于 `.env`（已 gitignore），不进仓库、不写进中间文件。

## 测试 / 可验证性

- 固定样例：`samples/content-research/demo-brief.md`（输入）→ 期望输出结构见上文「输出」。
- 自测：`content-research --demo` 必须稳定返回 `status=ok`、`sources` 非空、`fallback_used=false`。
