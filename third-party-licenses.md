# 第三方依赖 License 核查

> 本仓库（Agent-Auto-workflow）自身声明 **MIT**（见 `LICENSE`）。
> 但本仓库**借鉴/依赖**了若干第三方项目，其 license 状态如下，**引用前必须确认**，禁止在 license 不明时复制代码或再分发。

| 项目 | 用途 | 上游 License | 核查状态 | 处置 |
|---|---|---|---|---|
| `epiral/bb-browser` | 真实浏览器搜索（content-research 底层） | **MIT** ✅ | 已核实（raw LICENSE 为 MIT） | 可安全使用 + 保留署名 |
| `jwynia/agent-skills` → `research-workflow` | 结构化研究方法论（content-research 底层） | **未声明** ⚠️ | 已核实：仓库无 LICENSE 文件、首页无 license 声明（GitHub API 亦 404） | **仅借鉴方法论，不复制其代码/SKILL.md**；若要内置，须先取得作者书面授权 |
| `blader/humanizer` | 去 AI 痕迹方法论参考（humanize-writing） | **待核实** | 未核查 | 禁止复制代码；借鉴前须确认 license |
| `lguz/humanize-writing-skill` | 拟人化 skill 参考（humanize-writing） | **待核实** | 未核查 | 禁止复制代码；借鉴前须确认 license |
| `lguz`（模型无关思路） | engagement-analyzer 参考 | **待核实** | 未核查 | 仅借鉴方法论；借鉴前须确认 license |

## 核查方法（复盘）

- bb-browser：访问 `raw.githubusercontent.com/epiral/bb-browser/main/LICENSE` → 返回 MIT 全文。
- research-workflow：访问 `raw.githubusercontent.com/jwynia/agent-skills/main/LICENSE` → 404；`api.github.com/repos/jwynia/agent-skills/license` → 404；仓库首页无 license 字段 → 判定为「未声明」。
- humanizer / lguz 系列：本轮未联网核查，状态标记为「待核实」，**在核实前一律按「保留所有权利」对待**。

## 红线

1. 上游 license 未核实前，**不**将其代码/SKILL.md 直接复制进本仓库。
2. 仅以「方法论」形式借鉴（文字描述思路），并在对应工具文件内注明来源。
3. 一旦内置第三方代码，必须在其文件头保留原始 copyright + license 全文。
