# tools · 工具文件夹

> 本目录存放 agent 可调用的外部工具说明。每个工具文件内含：**统一 Tool Schema（输入/输出契约）** + **零配置 Demo 运行** + **真实最小配置** + 配置校验 / 降级 / 缓存 / License / 安全 / 测试。
> agent 定义文件以「工具引用」字段声明依赖与调用权重，确保 AI 知道工具在哪、何时用、优先级多高。

## 工具分层（3 层）

> **核心区分**：`content-research` 不是新能力，是**整合层（Facade / Adapter）**——只把底层 `bb-browser` + `research-workflow` 收敛成稳定业务接口；`humanize-writing` 与 `engagement-analyzer` 才是本次**新增业务能力**。

| 层级 | 工具 | 状态 | 作用 |
| -------- | --------------------- | ------ | ------- |
| 基础工具 | `bb-browser` | 已有 | 浏览器检索 |
| 基础方法 | `research-workflow` | 已有 | 结构化研究 |
| **整合层** | `content-research` | 已有/封装 | 统一研究入口（Facade，非新增能力） |
| **业务工具** | `engagement-analyzer` | **新增** | 分析互动机制 |
| **业务工具** | `humanize-writing` | **新增** | 账号人格化写作 |

### 分层依赖

```
基础工具 / 基础方法
        │
        ▼
   能力封装（整合层）
   content-research（Facade）
        │
        ▼
   业务 Agent
   competitor-scout ──▶ engagement-analyzer ──▶ viral-copywriter ──▶ humanize-writing
```

- **基础能力层**：`bb-browser`、`research-workflow` —— 会话早期已建，提供底层检索/搜索能力。
- **能力整合层（非新增）**：`content-research` —— 封装上述两个底层工具，对外单一 Tool Schema。底层工具替换（如 Firecrawl / Exa）时，上层 agent 无需改动。
- **业务能力层（本次新增）**：`engagement-analyzer`、`humanize-writing` —— 真正新增的业务能力，分别服务步得清与缪生花。

> 业务调用链：**competitor-scout → engagement-analyzer → viral-copywriter → humanize-writing → final content**。
> `engagement-analyzer` 负责「分析为什么火」，结论交给 `viral-copywriter` 生产；`humanize-writing` 在最后做账号人格化收尾（绑定 `account/style.md`），而非随机润色。

## 零配置 / 最小配置 总览
| 工具 | Demo（零配置） | 真实最小配置 |
|---|---|---|
| content-research | `--demo`，读 `samples/` | 安装 bb-browser + 系统 Chrome（小红书需 cookie） |
| humanize-writing | `--demo`，读预计算样例 | `LLM_API_KEY` + `LLM_BASE_URL` + `LLM_MODEL` |
| engagement-analyzer | `--demo`，读脱敏 `posts.json` | 本地 posts 文件（无需 key）/ 或 live 拉取 |

## 内置样例数据
`samples/` 目录存放三个工具的 `--demo` 数据（已脱敏），保证 clone 即跑：
- `samples/content-research/demo-brief.md`
- `samples/engagement-analyzer/posts.json` + `sample-report.md`
- `samples/humanize-writing/example.json`

## 协作关系（分层）
```
基础能力层                      能力整合层                  业务层
bb-browser ──┐
             ├──▶ content-research（Facade）──▶ 猎同频
research-workflow ┘

humanize-writing ──▶ 缪生花
engagement-analyzer ──▶ 步得清

业务调用链：
competitor-scout ──▶ engagement-analyzer ──▶ viral-copywriter ──▶ humanize-writing ──▶ final content
```

## 安全与合规
- `.env.example` 为配置模板，真实密钥存 `.env`（已 gitignore，见 `.gitignore`）。
- `third-party-licenses.md` 记录各上游 license 核查状态；`research-workflow` 上游未声明 license，仅借鉴方法论、不复制代码。
