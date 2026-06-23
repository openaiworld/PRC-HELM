# PRC-HELM

**PRC-HELM** 是一个面向编程 Agent 的中文工程治理 skill。它把“调查研究、主要矛盾、实践检验、用户路线、集中决策、复盘改进”等方法转译为软件项目中可执行的工作流、模板和检查清单，帮助 Agent 在规划、开发、评审、复盘时更稳、更有证据、更可追踪。

HELM 展开为 **Historical-Engineering Liberation Methodology**。项目名有四层含义：第一层是政治与历史意象，“舵手”呼应中文语境中“掌舵前进”的经典表达；第二层是英文展开，强调从历史经验中抽象工程方法；第三层是工程彩蛋，Kubernetes 社区熟悉的 Helm 也是“管理、编排、交付”的象征；第四层是项目含义，**Helm = 掌舵**，让 Agent 成为驾驶项目前进的执行框架，而不是只会套补丁的代码机器。

> 说明：本项目中的历史、政治与文化意象主要作为命名隐喻和工程记忆锚点，不用于替代用户需求、仓库规范、安全合规或可度量的技术事实。

## 特性

- **主要矛盾识别**：每个非平凡任务先明确当前最核心的问题，避免“眉毛胡子一把抓”。
- **分级执行模式**：支持轻量、标准、严格三种模式，避免小任务过度文档化，也避免大任务缺少治理。
- **需求先行**：新功能先形成用户故事、验收标准、目标与非目标。
- **证据化技术选型**：通过加权矩阵比较技术方案，避免只凭流行度或个人偏好决策。
- **RFC / ADR 决策流**：跨模块、架构级、长期维护相关变更有讨论、有结论、有留痕。
- **Spike / PoC 验证**：对不确定技术先验证再实现，并保留成功或失败经验。
- **渐进式重构**：鼓励 Feature Flag、兼容层、灰度切换和可回滚路径。
- **PR 与 Review 自检**：强调风险、测试、已知限制和技术债记录。
- **事故复盘机制**：严重 Bug 或生产事故要求根因分析、时间线和改进措施。
- **迭代健康度评分**：从生产力、RMST 质量四柱、工程文化、用户价值四个维度做收尾评估。
- **RMST 质量门禁**：在严格模式下检查可靠性、可维护性、可扩展性、可溯源性。
- **`.ai_helm/` 隔离目录**：长期治理产物可统一归档到 `.ai_helm/`，避免污染项目源码目录。
- **多 Agent Loop**：支持 Orchestrator、Planner、Researcher、Architect、Coder、Reviewer、Sentinel、Historian 八类角色契约。
- **多工具适配**：内置 Claude Code、AGENTS.md / Codex、Cursor、Windsurf、GitHub Copilot 的适配片段。

## 适用场景

适合在以下任务中显式调用或自动触发：

- 项目启动、里程碑规划、迭代收尾
- 新功能需求澄清与验收标准制定
- 技术选型、架构评审、跨模块改造
- 重大重构、迁移、技术债治理
- Bug 修复、生产事故处理、postmortem 编写
- PR 描述、代码审查、自检清单生成
- 编程 Agent 项目规范落地
- 多 Agent 协同、自动队列、`.ai_helm/` 状态目录治理
- SLO、降级策略、技术债、追踪矩阵等 RMST 质量门禁

## 目录结构

```text
PRC-HELM/
├── SKILL.md                         # skill 主入口，供 Agent 读取
├── README.md                        # 项目说明、特性和使用方法
├── agents/
│   └── openai.yaml                  # UI 元数据与默认提示词
├── references/
│   ├── templates.md                 # 需求、技术选型、RFC、ADR、PR、复盘、.ai_helm 状态模板
│   ├── health-scoring.md            # 迭代健康度评分规则
│   ├── rmst-quality.md              # RMST 质量四柱与门禁
│   ├── helm-loop.md                 # .ai_helm 状态目录与多 Agent Loop 协议
│   ├── agent-roles.md               # 八类 Agent 角色契约
│   ├── tool-adapters.md             # Claude/Codex/Cursor/Windsurf/Copilot 适配说明
│   └── source-methodology.md        # 命名含义、方法论来源与工程映射
└── assets/
    └── adapters/
        ├── CLAUDE.md                # Claude Code 项目级适配文件
        ├── AGENTS.md                # Codex / AGENTS.md 风格工具适配文件
        ├── prc-helm.mdc
        ├── .windsurfrules
        └── copilot-instructions.md
```

## 安装

### 方式一：通过 skill-installer / skills 体系安装

如果你的环境提供 `skill-installer` 或等价的 `skills` 安装能力，推荐直接从 GitHub 安装：

```text
使用 $skill-installer 安装 https://github.com/openaiworld/PRC-HELM/tree/main
```

也可以直接运行安装脚本。安装到 Codex 默认目录：

```bash
python3 ~/.zcode/skills/skill-installer/scripts/install-skill-from-github.py \
  --repo openaiworld/PRC-HELM \
  --path . \
  --name prc-helm
```

安装到 `~/.agents/skills`：

```bash
CODEX_HOME="$HOME/.agents" \
python3 ~/.zcode/skills/skill-installer/scripts/install-skill-from-github.py \
  --repo openaiworld/PRC-HELM \
  --path . \
  --name prc-helm
```

> 安装脚本在目标目录已存在时会中止。更新时请先备份或删除旧目录，例如 `~/.agents/skills/prc-helm`。

### 方式二：本地复制安装到 `~/.agents`

适用于本仓库已经 clone 到本地的情况：

```bash
mkdir -p ~/.agents/skills/prc-helm
cp SKILL.md README.md LICENSE ~/.agents/skills/prc-helm/
cp -R agents references assets ~/.agents/skills/prc-helm/
```

安装后可通过以下方式显式调用：

```text
使用 $prc-helm 为这个项目制定迭代计划。
```

或：

```text
使用 $prc-helm 审查这个技术选型，并输出矩阵、风险和回退方案。
```

## 项目级适配

如果目标工具不会自动发现 skill，可把 `assets/adapters/` 中的文件复制到项目根目录。

### Claude Code

```bash
cp assets/adapters/CLAUDE.md /path/to/your-project/CLAUDE.md
```

### Codex / AGENTS.md 风格工具

```bash
cp assets/adapters/AGENTS.md /path/to/your-project/AGENTS.md
```

### Cursor

```bash
mkdir -p /path/to/your-project/.cursor/rules
cp assets/adapters/prc-helm.mdc \
  /path/to/your-project/.cursor/rules/prc-helm.mdc
```

### Windsurf

```bash
cp assets/adapters/.windsurfrules /path/to/your-project/.windsurfrules
```

### GitHub Copilot

```bash
mkdir -p /path/to/your-project/.github
cp assets/adapters/copilot-instructions.md \
  /path/to/your-project/.github/copilot-instructions.md
```

## 使用方法

### 1. 规划任务

```text
使用 $prc-helm 为“用户登录重构”制定实施计划，要求包含主要矛盾、执行模式、风险、验证方式、RMST 检查和后续文档。
```

推荐输出结构：

```markdown
主要矛盾：
执行模式：轻量 | 标准 | 严格
依据事实：
行动：
验证：
后续：
```

### 2. 编写需求文档

```text
使用 $prc-helm 把这个想法整理成需求文档：用户希望导出账单 CSV。
```

可生成用户故事、目标 / 非目标、验收标准、约束与风险、待确认问题。

### 3. 做技术选型

```text
使用 $prc-helm 比较 Pinia、Redux Toolkit、Zustand 在当前项目中的适用性。
```

将使用 `references/templates.md` 中的加权矩阵，从团队熟悉度、性能、生态、维护活跃度和长期风险等维度比较。

### 4. 处理架构变更

```text
使用 $prc-helm 为“从 REST 迁移到 GraphQL”生成 RFC 草案。
```

适用于跨模块变更、公共 API 调整、数据持久化方案变化、部署、安全、长期维护策略变化。

### 5. 启用 `.ai_helm/` 严格治理

```text
使用 $prc-helm 为当前项目初始化 .ai_helm 状态目录，包含 HELM.md、agents.md、config.json、state/progress.md 和 state/queue.json。
```

适用于长期项目、多 Agent 协同、自动队列、质量门禁和复盘归档。

### 6. 迭代收尾

当用户说：

```text
收工，总结一下。
```

Agent 应输出本轮完成情况、未解决风险、下一阶段主要矛盾、迭代健康度评分，以及建议更新的文档，如 `progress.md`、`CHANGELOG.md`、轻量模式的 `docs/retrospective/YYYY-MM-DD.md` 或严格模式的 `.ai_helm/retrospective/YYYY-MM-DD.md`。

## 核心工作流

```text
调查研究
  ↓
识别主要矛盾
  ↓
选择执行模式：轻量 / 标准 / 严格
  ↓
形成需求、矩阵、RFC、ADR、spike、RMST 门禁或实施计划
  ↓
编码 / 验证 / Review
  ↓
记录风险、技术债、失败经验
  ↓
迭代健康度评分与下一轮主要矛盾
```

## 推荐项目结构

当目标项目没有等价约定时，可使用：

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

严格模式 / Loop 模式可使用 `.ai_helm/` 隔离结构：

```text
.ai_helm/
  HELM.md
  agents.md
  config.json
  state/
    progress.md
    queue.json
  decisions/{rfc,adr}/
  quality/{reliability,maintainability,scalability,traceability}/
  failure-kb/
  postmortem/
  retrospective/
  knowledge/
```

## 资源说明

| 文件 | 用途 |
|---|---|
| `SKILL.md` | 主入口，控制触发、执行原则、场景规则和引用导航 |
| `references/templates.md` | 常用治理模板：需求、技术选型、spike、RFC、ADR、PR、复盘、`.ai_helm` 状态等 |
| `references/health-scoring.md` | 迭代健康度评分规则与报告格式 |
| `references/rmst-quality.md` | RMST 可靠性、可维护性、可扩展性、可溯源性门禁 |
| `references/helm-loop.md` | `.ai_helm` 状态目录、多 Agent Loop、队列与终止条件 |
| `references/agent-roles.md` | 八类 Agent 的职责、触发条件和输出约定 |
| `references/tool-adapters.md` | 各类编程工具的落地方式 |
| `references/source-methodology.md` | 命名含义、方法论来源与工程映射说明 |
| `assets/adapters/*` | 可复制到项目中的工具适配文件 |

## 设计原则

- **短入口，长引用**：`SKILL.md` 保持精炼，详细模板放入 `references/`。
- **先项目规范，后方法论默认值**：如果目标仓库已有规范，优先合并而不是覆盖。
- **证据优先**：没有调查、测试或上下文证据时，不做过度断言。
- **可复盘**：重要决策、失败 PoC、事故和技术债都应留下记录。
- **不过度治理**：小任务用轻量模式，大任务才使用完整文档流。

## 验证

可使用 skill creator 的验证脚本检查 `SKILL.md` frontmatter：

```bash
python3 /Users/bing/.zcode/skills/skill-creator/scripts/quick_validate.py .
```

如果本地 Python 环境缺少 `PyYAML`，可先安装依赖或使用项目环境中的验证方式。

## 默认提示词

`agents/openai.yaml` 中配置的默认提示词为：

```text
使用 $prc-helm 以规范的软件工程流程规划、审查或收尾这个任务。
```

## License

This project is licensed under the AGPL-3.0 License.
