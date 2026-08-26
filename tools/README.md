# tools · 工具文件夹

> 本目录存放 agent 可调用的外部工具说明。agent 定义文件以 `工具引用` 字段声明依赖与调用权重，确保 AI 知道工具在哪、何时用、优先级多高。

## 工具清单

### 1. bb-browser — 搜索工具
- 文档：`tools/bb-browser.md`
- 能力：36 平台 103 命令，用真实浏览器登录态搜索，`--json` / `--jq` 结构化输出
- 用途：同类型内容检索、对标账号搜索、查重数据源
- 高权重 agent：闻风向（趋势选题）、查无异（查重）

### 2. research-workflow — 检索分析 skill
- 文档：`tools/research-workflow.md`
- 能力：结构化研究四阶段（Planning → Execution → Analysis → Synthesis），生成中间文件报告
- 用途：分析用户给出的平台链接、多查询检索、来源验证、综合报告
- 高权重 agent：闻风向（趋势选题）、查无异（查重）

## 协作关系

```
bb-browser（搜索能力）
      │  提供 web search capability
      ▼
research-workflow（检索方法论）
      │  输出中间文件报告
      ▼
agent 定义（闻风向 / 查无异 高权重调用）
```
