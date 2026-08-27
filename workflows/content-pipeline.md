# 工作流：内容生产流水线（Content Pipeline）

> 本工作流是 MCN-Agent-Studio 的核心，围绕「内容重复度确认」+「真实素材」设计。
> 触发条件：用户输入一个主题 / 大方向，要求产出小红书内容。

## 状态机总览

```
输入主题 / 大方向
        │
        ▼
[Phase 0] 主理人建 run 目录，写 00_input.md
        │
        ├───────────────────────────────┐
        ▼                               ▼
[Phase 1] 对标检索 + 查重          [Phase 2] 真实素材采集
 competitor-scout（串行，核心）     story-collector（可并行）
 写 01_scout.md                    写 02_stories.md
        │                               │
        └───────────────┬───────────────┘
                        ▼
        [Phase 3] 选题决策（主理人，三道门禁）
         门禁①素材门禁：02_stories 为空 → 禁止生产
         门禁②查重门禁：决定 直接生产 / 重构 / 风险确认
         门禁③记忆防重复门禁：所选角度在 angles/ 已高频使用 → 换未用/空白角度
         写 03_brief.md
                        │
        ┌───────────────┼───────────────┐
        │ 无重复         │ 有重复(70–90%)  │ 检索失败
        ▼               ▼               ▼
   直接生产        Phase 3b 重构     风险确认节点
                   viral-copywriter   等待用户确认
                   （蓝海角度）        A继续/B换主题/C参考
                        │
        ┌───────────────┴───────────────┐
        ▼                               ▼
[Phase 4] 内容生产（并行）
 viral-copywriter 写 04_copy.md
 visual-designer 写 05_visual.md
        │
        ▼
[Phase 5] 数据复盘 growth-analyst 写 06_review.md
        │
        ▼
[Phase 6] 主理人汇编 final_bundle.md → 输出稳定内容
        │
        ▼
[Phase 7] 记忆更新协议（team-lead 唯一写 Memory）
 内容发布后 promote：content/published · topics/ · angles/ · stories/ · performance/
        │
        ▼
   下一轮决策（读取 Memory 防重复）
```

## 中间文件流转

```
runs/{run_id}/
├── 00_input.md        主理人写入
├── 01_scout.md        猎同频：检索 + 趋势 + 查重（附来源 URL）
├── 02_stories.md      采真人：真实素材库（附来源/授权/脱敏）
├── 03_brief.md        主理人：选题角度（基于 01 + 02）
├── 04_copy.md         缪生花：标题 20 + 正文 + 标签（引用 ST-00X）
├── 05_visual.md       乔美设：封面方案 + 配图 prompt
├── 06_review.md       步得清：发布建议 + 预期指标 + 复盘模板
└── final_bundle.md    主理人：可发布内容包
```

## 阶段说明

### Phase 1：对标检索 + 查重（competitor-scout，核心）
**先读 Memory**：team-lead + 猎同频先读 `../memory/topics/`、`../memory/angles/angle-memory.md`（防主题/角度重复）、`../memory/stories/story-index.md`（可复用素材）。
用 content-research 检索同类型内容与对标账号，输出趋势、对标、用户画像、竞争度，并做标题/结构/关键词/语义四维查重。

查重判定：
- `similarity >= 0.90` → HIGH，拒绝重做
- `0.70 <= similarity < 0.90` → MEDIUM，转重构
- `similarity < 0.70` → 通过
- 检索失败 → 风险确认节点，禁止自动生产

### Phase 2：真实素材采集（story-collector）
采集有来源、可核实的真实故事，结构化沉淀为故事库（ST-00X 条目），脱敏并标注授权与可信度。

### Phase 3：选题决策（主理人，三道门禁）
- **门禁①素材门禁**：`02_stories.md` 为空或无适配故事 → 禁止进入文案，退回补采或请求用户提供素材。
- **门禁②查重门禁**：依据查重结论决定「直接生产 / 重构 / 风险确认」。
- **门禁③记忆防重复门禁**：所选**角度**若在 `../memory/angles/angle-memory.md` 已高频使用，必须换「未使用 / 市场空白」角度，或叠加新切口（新人群 / 新场景 / 新数据）；不得原样复用已用角度。

### Phase 4：内容生产（并行）
- **viral-copywriter**：读 `03_brief.md` + `02_stories.md`，写 `04_copy.md`。正文引用的每个故事必须指向 ST-00X；有重复则先重构（撞车点分析 + 蓝海角度 + 重新定位）。
- **visual-designer**：读主题，写 `05_visual.md`。

### Phase 5：数据复盘（growth-analyst）
读 `04_copy.md` + `05_visual.md`，写 `06_review.md`（内容体检 + 发布时机 + 预期指标 + 复盘模板）。

### Phase 6：主理人汇编
综合全部中间文件，汇编 `final_bundle.md`，标注风险点与假设值。

### Phase 7：记忆更新协议（team-lead 唯一写 Memory）
内容发布后，team-lead 把验证过的数据 **promote** 进 `memory/`（append / version，禁止覆盖）：
1. 新内容 → `memory/content/published/`（CONTENT-ID）。
2. 新主题 / 角度使用 → 更新 `memory/topics/` + `memory/angles/`（used_count+1、last_used）。
3. 新验证故事 → `memory/stories/story-index.md`（分配 ST-ID）；被使用故事 used_count+1、used_in 追加。
4. 发布数据回填 → `memory/performance/`。
5. 高表现规律 → 回灌 `angles/`、`style/`、`content-history`。

## Memory 读写（与 team-lead Phase 7 对齐）

本工作流的所有「长期记忆」只存在于 `memory/`，且**只有 team-lead 能写**。各 Agent 在对应阶段只 READ，不直接写。

- **Phase 0/1 读**：team-lead + 猎同频先读 `topics/`、`angles/`（防主题 / 角度重复）、`stories/`（可复用素材）、`account/style`（账号语言）。
- **Phase 4 读**：缪生花读 `account/style` 对齐语气、读 `angles/` 确认本篇角度未过度使用。
- **Phase 5 读**：步得清读 `performance/` 让复盘数据驱动。
- **Phase 7 写（仅 team-lead）**：见上「Phase 7」条目。

## 协作约束

1. 主理人负责编排与决策，**不代写任何成员的专业产出**。
2. 成员只读上游文件、写自己的文件，不得跨级修改。
3. 每阶段产出落盘到 `runs/{run_id}/`，所有信息流经主理人中转。
4. 未完成前序阶段，禁止跳到后续阶段。
5. 文案无素材不落笔，检索无来源不断言。
