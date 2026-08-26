# bb-browser 搜索工具

> 「你的浏览器就是 API。」无需密钥，用真实浏览器登录态搜索。36 平台、103 命令。

## 环境信息

| 项 | 值 |
|---|---|
| 版本 | 0.14.2（已全局安装，npm link） |
| 项目路径 | `C:\Users\lyl\WorkBuddy\2026-08-26-15-47-34\bb-browser` |
| 适配器库 | `~/.bb-browser/bb-sites`（更新用 `bb-browser site update`） |
| daemon | `127.0.0.1:19824`（停止用 `bb-browser daemon stop`） |
| 结构化输出 | 所有命令支持 `--json`、`--jq <expr>` |

## 核心命令

```bash
bb-browser site list                  # 列出所有可用命令
bb-browser site info <name>           # 查看 adapter 参数、返回值、示例
bb-browser site update                # 更新社区 adapter 库
bb-browser site <name> [args]         # 运行 adapter
```

## 搜索用法（agent 调用示例）

```bash
# 搜索引擎
bb-browser site baidu/search "深度学习"
bb-browser site google/search "RAG framework"

# 知识百科
bb-browser site wikipedia/search "large language model"
bb-browser site wikipedia/summary "Python"

# 学术论文
bb-browser site arxiv/search "diffusion model"

# 社交媒体（含登录态）
bb-browser site zhihu/search "RAG"
bb-browser site weibo/search "关键词"
bb-browser site xiaohongshu/search "关键词"

# 技术社区
bb-browser site github search "rag-framework"
bb-browser site stackoverflow/search "async"

# 结构化输出
bb-browser site baidu/search "深度学习" --jq '.items[] | {title, url}'
```

## 本机实测最稳的适配器

- ✅ **wikipedia**（search / summary，稳定）
- ✅ **arxiv**（search，稳定）
- ✅ **baidu**（search，稳定）
- ⚠️ bing：本机地域重定向导致加载失败
- ⚠️ duckduckgo：跨子域 CORS 被拦截

> 结论：优先用 wikipedia / arxiv / baidu 三个适配器做检索。

## 平台覆盖（36 平台）

搜索引擎（Google/百度/Bing/DuckDuckGo/搜狗微信）、社交媒体（Twitter/Reddit/微博/小红书/知乎/即刻/LinkedIn/虎扑）、新闻资讯（BBC/Reuters/36氪/今日头条/东方财富）、技术开发（GitHub/StackOverflow/HackerNews/CSDN/arXiv）、视频平台（YouTube/B站）、财经股票（雪球/东方财富/Yahoo Finance）、求职招聘（BOSS直聘/LinkedIn）、知识百科（Wikipedia/知乎）、消费购物（什么值得买）等。

## 注意事项

- 搜索走真实浏览器上下文，无需 API key。
- Windows 下 CLI 会自动拉起系统 Chrome（`--remote-debugging-port=9222`）。
- 检索结果用于查重与对标时，**必须保留来源 URL**。
