# PRC-HELM Loop 协议

本文件描述 PRC-HELM 的严格模式 / Loop 模式。仅当用户要求多 Agent 协同、自动迭代、严格治理、项目长期运行，或目标项目已经采用 `.helm/` 目录时加载。轻量任务不要强制启用 Loop。

## 启用条件

使用 Loop 模式的典型信号：

- 用户要求“持续迭代”“自动跑队列”“多 Agent 协同”“严格治理”。
- 项目根目录已存在 `.helm/HELM.md`、`.helm/state/progress.md` 或 `.helm/state/queue.json`。
- 任务属于架构迁移、生产稳定性治理、长期技术债治理、跨团队项目管理。

若只是小修、小文档、单点 bug，使用 SKILL.md 中的轻量或标准模式即可。

## 目录隔离原则

PRC-HELM 产物优先归档到 `.helm/`，避免污染项目原生源码与业务文档结构。唯一常见例外是项目根目录的 `CHANGELOG.md`，用于发版记录。

```text
.helm/
  HELM.md
  agents.md
  config.json
  state/
    progress.md
    queue.json
  decisions/
    rfc/
    adr/
  quality/
    reliability/
    maintainability/
    scalability/
    traceability/
  failure-kb/
  postmortem/
  retrospective/
  knowledge/
  feedback/
```

## 启动读取顺序

```text
Step 1: 读取 .helm/HELM.md                # 项目级 PRC-HELM 宪法，如存在
Step 2: 读取 .helm/agents.md              # Agent 角色契约，如存在
Step 3: 读取 .helm/state/progress.md      # 当前主要矛盾、Sprint 上下文
Step 4: 读取 .helm/state/queue.json       # 待执行任务队列
Step 5: 读取 .helm/config.json            # Loop、重试、质量门禁参数
```

若某文件不存在，不要臆造当前状态；应建议创建对应模板，或在任务输出中明确“未启用 .helm 状态目录”。

## Loop 流程

```text
OrchestratorAgent
  1. 重新评估主要矛盾。
  2. 从 .helm/state/queue.json 取最高优先级可执行任务。
  3. 按任务 type 路由给对应 Agent。
  4. 汇总执行结果与质量门禁。
  5. 更新 queue.json 与 progress.md。
  6. 遇到 blocked、超过重试次数、主要矛盾转化或达到 max_iterations 时暂停。
```

## 任务路由

| type | Agent | 说明 |
|---|---|---|
| `orchestrate` | OrchestratorAgent | 主要矛盾、队列调度、阻塞裁决 |
| `plan` | PlannerAgent | Sprint 规划、任务拆解、依赖排序 |
| `research` | ResearcherAgent | 用户调研、需求澄清、RFC 草稿 |
| `rfc` | ArchitectAgent | 方案讨论、影响分析 |
| `adr` | ArchitectAgent | 决策记录、技术选型矩阵 |
| `spike` | CoderAgent | PoC 验证 |
| `implement` | CoderAgent | 正式实现 |
| `refactor` | CoderAgent | 渐进式重构 |
| `review` | ReviewerAgent | PR 审查、可维护性门禁 |
| `slo` | SentinelAgent | SLO、告警、降级策略 |
| `chaos` | SentinelAgent | 混沌演练、可靠性验证 |
| `archive` | HistorianAgent | ADR、failure-kb、追踪矩阵归档 |
| `postmortem` | HistorianAgent | 事故复盘 |

建议并行类型：`research`、`spike`、`archive`、`slo`。建议顺序类型：`rfc`、`adr`、`implement`、`review`。

## 队列状态

任务状态使用：`pending`、`in_progress`、`done`、`failed`、`blocked`。

阻塞任务必须包含：

```markdown
⚠️ [BLOCKED] PRC-HELM Loop 暂停
任务：<task_id> - <task_title>
阻塞原因：<blocked_reason>
需要人类决策：<decision_needed>
相关文件：<context_files>
建议操作：[A] <option_a> | [B] <option_b>
```

## 推荐配置

```json
{
  "loop": {
    "max_iterations": 20,
    "iteration_timeout_minutes": 30,
    "parallel_task_limit": 3,
    "auto_continue_on_success": true,
    "pause_on_blocked": true,
    "pause_on_primary_contradiction_shift": true
  },
  "retry": {
    "max_retries_per_task": 2,
    "archive_to_failure_kb_on_final_failure": true
  },
  "quality_gates": {
    "enable_rmst_check": true,
    "block_on_circular_dependency": true,
    "block_on_missing_slo": true,
    "block_on_missing_test": true,
    "cyclomatic_complexity_max": 10,
    "function_lines_max": 50,
    "test_coverage_min_percent": 80
  }
}
```

## 终止条件

满足任一条件时暂停 Loop 并向用户报告：

- 队列为空，正常完成。
- 任务进入 `blocked`，需要人类决策。
- `iteration_count` 超过 `max_iterations`。
- 主要矛盾发生转化，需要重新规划。
- 质量门禁连续失败，超过最大重试次数。
