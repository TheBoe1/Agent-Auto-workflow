# tools · 工具文件夹

> 本目录存放 agent 可调用的外部工具说明。每个工具文件内含：**统一 Tool Schema（输入/输出契约）** + **零配置 Demo 运行** + **真实最小配置** + 配置校验 / 降级 / 缓存 / License / 安全 / 测试。
> agent 定义文件以「工具引用」字段声明依赖与调用权重，确保 AI 知道工具在哪、何时用、优先级多高。

## 工具清单（5 个）

### 应用工具（agent 直接调用，均支持 `--demo` 零配置）
| 工具 | 文档 | 服务 agent | 能力 |
|---|---|---|---|
| content-research | `tools/content-research.md` | 猎同频（对标检索师） | 统一检索入口，封装 bb-browser + research-workflow |
| humanize-writing | `tools/humanize-writing.md` | 缪生花（爆款文案师） | 文本拟人化，LLM 抽象层（provider 无关） |
| engagement-analyzer | `tools/engagement-analyzer.md` | 步得清（增长复盘师） | 互动内容分析，输出《互动内容分析报告》 |

### 底层工具（被应用工具依赖）
| 工具 | 文档 | 能力 |
|---|---|---|
| bb-browser | `tools/bb-browser.md` | 36 平台 103 命令，真实浏览器登录态搜索 |
| research-workflow | `tools/research-workflow.md` | 结构化研究四阶段，生成中间文件报告 |

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

## 协作关系
```
bb-browser（搜索能力）──┐
                        ├──▶ content-research（统一检索入口，猎同频调用）
research-workflow（方法论）┘
                                      │
humanize-writing（拟人化，缪生花）────┤
                                      ├──▶ 6 名专家按 content-pipeline 协作
engagement-analyzer（复盘，步得清）───┘
```

## 安全与合规
- `.env.example` 为配置模板，真实密钥存 `.env`（已 gitignore，见 `.gitignore`）。
- `third-party-licenses.md` 记录各上游 license 核查状态；`research-workflow` 上游未声明 license，仅借鉴方法论、不复制代码。
