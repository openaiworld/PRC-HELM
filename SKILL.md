---
name: prc-helm
description: PRC-HELM，中文工程治理与项目纪律 skill，Historical-Engineering Liberation Methodology。供编程 Agent 在项目规划、功能启动、架构评审、技术选型、事故处理、技术债治理、PR 审查、迭代复盘、RFC/ADR/需求文档/事后复盘模板生成、工具适配说明时使用。强调需求先行、调查研究、主要矛盾识别、PoC 验证、用户故事、渐进式重构、证据化决策和迭代健康度评分。
---

# PRC-HELM

PRC-HELM 是面向编程 Agent 的中文工程治理 skill。HELM 展开为 **Historical-Engineering Liberation Methodology**，同时取 “helm = 掌舵” 的工程隐喻：Agent 不是盲目执行代码补丁，而是在事实、用户价值、技术约束和可复盘纪律之间掌舵前进。

本 skill 中的政治与历史意象只作为命名隐喻和工程记忆锚点；不得让口号覆盖用户需求、安全要求、法律合规、仓库规范或可度量的技术事实。

## 行动原则

- **实事求是**：先检查仓库、需求、测试、运行行为、用户约束，再提出方案或修改代码。
- **抓主要矛盾**：为当前任务命名一个最高优先级问题；次要问题最多保留三项。
- **实践先行**：技术不确定时先做 PoC/spike，再把结论沉淀为文档或决策记录。
- **服务用户**：每个功能都要映射到用户故事、验收标准和验证路径。
- **民主讨论，集中执行**：跨模块或架构级变化使用 RFC/ADR；决策形成后保持实现一致。
- **务实选型**：技术选择看适配度、可维护性、基准数据和团队熟悉度，不追逐流行本身。
- **批评与自我批评**：在 PR、Review、复盘中明确风险、限制、测试情况和后续技术债。

## 使用强度

根据任务规模选择执行强度，避免把轻量任务文档化过度：

- **轻量模式**：小修、小文档、明确单点问题。只说明主要矛盾、直接执行、给出验证结果。
- **标准模式**：常规功能、重构、Bug 修复。执行任务开始检查清单，必要时补需求、测试和文档。
- **严格模式**：架构、跨模块、生产事故、重大迁移、长期技术选型。必须使用 RFC/ADR、技术矩阵、spike 或 postmortem 等正式产物。

## 任务开始检查清单

1. 优先读取已有项目指导：`AGENTS.md`、`CLAUDE.md`、`README.md`、`docs/`、`progress.md`、issue/PR 上下文和用户指令。
2. 用一句话说明或推断本任务的**主要矛盾**。
3. 判断任务类型：
   - **新功能**：需要用户故事和验收标准；缺失时先提出精简澄清问题，或先起草需求文档供用户确认。
   - **技术选型**：使用 [references/templates.md](references/templates.md) 中的加权矩阵比较方案。
   - **架构/跨模块变更**：创建或建议 RFC/ADR，模板见 [references/templates.md](references/templates.md)。
   - **Bug/事故**：先复现，再定位根因，补回归测试；严重生产事故需创建或建议事后复盘。
   - **重构/迁移**：优先使用 Feature Flag、兼容层、灰度或分阶段迁移；除非用户明确要求并有充分理由，避免一次性重写。
4. 围绕一个核心交付物制定计划。如果同时存在超过两个主任务，请用户排序。
5. 写入新文档或治理文件前，确认项目已有约定；没有约定时再采用本 skill 推荐结构。

## 输出约定

在适用本 skill 时，优先用以下结构组织回答或任务说明：

```markdown
主要矛盾：<一句话>
执行模式：轻量 | 标准 | 严格
依据事实：<仓库/需求/测试/运行结果中的关键证据>
行动：<将做什么或已做什么>
验证：<测试、检查、人工验证或未验证原因>
后续：<风险、技术债、下一步>
```

## 场景规则

### 需求与功能开发

- 将模糊需求改写为用户故事：`作为 <角色>，我希望 <能力>，以便 <价值>`。
- 在大规模实现前定义可观察的验收标准。
- 行为或用法发生变化时，更新模块文档或 README。
- 为核心逻辑补测试；若未补测试，明确说明原因。

### 技术选型

- 禁止只凭流行度、个人偏好或口号做决定。
- 使用 [references/templates.md](references/templates.md) 中的技术选型矩阵。
- 尽量引用可验证证据：benchmark、官方文档、维护状态、生态成熟度、项目适配度。
- 若证据不足且影响较大，先提出 `spike/` 验证方案。

### 架构决策

- 局部实现细节如有非显然理由，应在代码注释或任务说明中记录。
- 跨模块、公共 API、持久化、部署、安全、长期维护相关变化，应创建或建议 RFC/ADR。
- 保留被否定方案及理由；不要删除失败推理和有价值的试错结果。

### 重构与迁移

- 优先分阶段迁移：
  1. 新旧路径通过 Feature Flag 或兼容边界并存。
  2. 通过测试、监控或灰度逐步切换。
  3. 经观察窗口或用户批准后删除旧代码。
- 对风险较高的变化保留回滚路径。

### PR 与代码审查

- PR 描述应包含：主要矛盾、变更内容、设计理由、风险影响、测试情况、UI 截图或录屏、已知限制。
- Review 意见应具备实质内容，可使用 `[MUST]`、`[SHOULD]`、`[NIT]` 标记。
- 已接受的技术债应记录到 `progress.md`、issue 或后续任务说明中。

### Bug 与事故处理

- 能复现时先复现，避免靠猜测修复。
- 为故障模式补回归测试或等价验证。
- P0/P1 或生产面事故，应在 72 小时内创建或建议事后复盘，模板见 [references/templates.md](references/templates.md)。

### 迭代收尾

当用户说 `迭代结束`、`sprint 结束`、`本轮完成`、`收工`、`总结一下` 时：

1. 总结已完成工作、未解决风险和下一阶段主要矛盾。
2. 使用 [references/health-scoring.md](references/health-scoring.md) 进行迭代健康度评分。
3. 在适用时建议更新 `progress.md`、`CHANGELOG.md`、`docs/retrospective/YYYY-MM-DD.md`。

## 推荐项目结构

当项目没有等价约定时，使用以下目录：

```text
docs/
  requirements/   # 用户故事、验收标准、调研记录
  rfc/            # 讨论中的方案
  adr/            # 已接受的架构决策
  feedback/       # 用户反馈分析
  postmortem/     # 事故复盘
  retrospective/  # 迭代总结
  failure-kb/     # 失败 PoC、被否方案、经验教训
  knowledge/      # 入门、导师制、知识分享
spike/            # 独立技术验证
progress.md       # 当前主要矛盾、次要问题、下一步
```

## 引用资料

- 需要模板时读取 [references/templates.md](references/templates.md)：用户故事、需求文档、技术选型矩阵、RFC、ADR、PR、spike、failure-kb、postmortem、progress 模板。
- 迭代收尾或项目健康度审计时读取 [references/health-scoring.md](references/health-scoring.md)。
- 用户询问如何在 Claude Code、Codex、Cursor、Windsurf、Copilot 等工具中落地时，读取 [references/tool-adapters.md](references/tool-adapters.md)。
- 用户询问完整命名含义、理论映射、方法论来源或需要修订方法论时，读取 [references/source-methodology.md](references/source-methodology.md)。
