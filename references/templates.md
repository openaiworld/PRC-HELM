# 模板

本文件提供可直接复制的治理模板。使用时遵循两个原则：

1. **轻任务轻文档，重任务重证据**：不要为小修小补强行创建完整治理文档。
2. **先复用项目约定**：如果仓库已有 issue、PR、ADR、RFC 模板，优先沿用并补充本方法论中的关键字段。

## 用户故事

```markdown
作为 <角色>，我希望 <能力>，以便 <价值>。

验收标准：
- [ ] <可观察行为>
- [ ] <边界情况或非功能要求>
- [ ] <验证方式：测试/UAT/监控/人工验收>

不做范围：
- <本次明确不解决的问题>
```

## 需求文档

建议创建到 `docs/requirements/YYYY-MM-DD-title.md`。

```markdown
# 需求：<标题>

## 背景

## 用户故事

作为 <角色>，我希望 <能力>，以便 <价值>。

## 主要矛盾

当前最需要解决的核心问题是什么？

## 目标

- 

## 非目标

- 

## 验收标准

- [ ] 

## 约束

- 技术约束：
- 时间约束：
- 兼容性约束：
- 安全/合规约束：

## 风险与待确认问题

| 问题 | 影响 | 处理方式 |
|---|---|---|
```

## 技术选型矩阵

在选择工具、库、架构、供应商时使用。

```markdown
| 维度 | 权重 | 方案 A | 方案 B | 方案 C |
|---|---:|---:|---:|---:|
| 团队熟悉度 | 35% |  |  |  |
| 性能 / benchmark 证据 | 25% |  |  |  |
| 生态成熟度 | 20% |  |  |  |
| 社区 / 维护活跃度 | 10% |  |  |  |
| 长期维护风险 | 10% |  |  |  |
| **加权总分** | **100%** |  |  |  |

推荐结论：
证据：
风险：
回退方案：
```

打分建议：每项 1-5 分，乘以权重后求和。若某维度缺少证据，应降低置信度，而不是补想象分。

## Spike / PoC 记录

建议创建到 `spike/YYYY-MM-topic/README.md`。

```markdown
# Spike: <主题>

## 验证问题

要证明或排除什么？

## 成功标准

- [ ] 

## 实验环境

- 版本：
- 数据：
- 约束：

## 实验步骤

## 结果

## 结论

- 采用 / 不采用 / 需要继续验证：

## 后续动作

## 失败经验沉淀

如果失败，是否需要同步到 `docs/failure-kb/`？
```

## RFC

对于需要讨论的决策，创建到 `docs/rfc/YYYY-MM-title.md`。

```markdown
# RFC: <标题>

## 背景与动机

## 主要矛盾

## 目标

## 非目标

## 方案设计

## 备选方案

## 影响分析

- 性能：
- 安全：
- 兼容性：
- 运维：
- 开发体验：

## 未解决问题

## 讨论窗口

Owner：
截止时间：
```

## ADR

决策被接受后，创建到 `docs/adr/YYYY-MM-DD-title.md`。

```markdown
# ADR: <标题>

日期：YYYY-MM-DD
状态：Accepted | Superseded | Deprecated
Owner：

## 背景

## 决策

## 后果

### 正面影响

### 负面影响 / 取舍

## 被否定的备选方案

## 后续行动
```

## PR 描述

```markdown
## 主要矛盾

这个 PR 解决什么核心问题？

## 做了什么

## 为什么这样设计

## 风险与影响

## 自我批评 / 已知限制

## 测试说明

## 截图或录屏

UI 变更必填。
```

## Review 意见格式

```markdown
[MUST] <必须修改的问题与理由>
[SHOULD] <建议修改的问题与理由>
[NIT] <不阻塞的小优化>
[QUESTION] <需要作者澄清的问题>
[PRAISE] <值得保留或推广的好实践>
```

## 事后复盘

严重事故创建到 `docs/postmortem/YYYY-MM-DD-incident.md`。

```markdown
# 事后复盘：<事故名称>

## 摘要

- 开始时间：
- 结束时间：
- 持续时间：
- 影响范围：
- 严重级别：

## 根本原因

使用 5 Why 或其他明确根因分析方法。

## 时间线

## 主要矛盾

是什么底层张力让事故成为可能？

## 做得好的地方

## 做得不好的地方

## 改进措施

| 措施 | 负责人 | 截止时间 | 状态 |
|---|---|---|---|

## 经验沉淀
```

## 失败知识库条目

建议创建到 `docs/failure-kb/YYYY-MM-topic.md`。

```markdown
# 失败经验：<主题>

## 背景

## 原计划

## 失败表现

## 根因分析

## 已尝试方案

## 最终结论

## 下次如何避免重复踩坑

## 可复用证据

- 日志：
- benchmark：
- issue/PR：
- spike：
```

## 迭代复盘

建议创建到 `docs/retrospective/YYYY-MM-DD.md`。

```markdown
# 迭代复盘：<迭代名称>

## 本轮主要矛盾

## 完成情况

## 未完成事项

## 做得好的地方

## 做得不好的地方

## 经验沉淀

## 下一轮主要矛盾候选

## 健康度评分

见 `references/health-scoring.md`。
```

## `.helm/state/progress.md`

严格模式 / Loop 模式下使用。

```markdown
# PRC-HELM · 迭代状态

## 当前主要矛盾

> 本迭代唯一核心目标，一句话。

## 次要矛盾（≤3条）

1.
2.
3.

## Sprint 信息

| 字段 | 值 |
|---|---|
| Sprint ID | sprint-001 |
| 开始日期 | YYYY-MM-DD |
| 结束日期 | YYYY-MM-DD |
| 当前 Loop 迭代 | 0 / 20 |
| 上次更新 | YYYY-MM-DDTHH:mm:ssZ |
| 更新 Agent | OrchestratorAgent |

## RMST 状态快照

| 质量柱 | 当前状态 | 最近异常 |
|---|---|---|
| 可靠性（R） | ✅ 正常 | - |
| 可维护性（M） | ✅ 正常 | - |
| 可扩展性（S） | ✅ 正常 | - |
| 可溯源性（T） | ✅ 正常 | - |

## 待关注风险

## 上一迭代结论

健康度得分：— / 100  
复盘报告：`.helm/retrospective/YYYY-MM-DD.md`
```

## `.helm/state/queue.json`

```json
{
  "version": "2.0",
  "sprint": {
    "id": "sprint-001",
    "primary_contradiction": "当前主要矛盾",
    "started": "YYYY-MM-DD",
    "ends": "YYYY-MM-DD",
    "iteration_count": 0,
    "max_iterations": 20
  },
  "queue": [
    {
      "id": "t001",
      "title": "简洁动宾短语描述任务",
      "agent": "ResearcherAgent",
      "type": "research",
      "status": "pending",
      "priority": "primary",
      "input_files": [],
      "output_path": ".helm/decisions/rfc/YYYY-MM-title.md",
      "depends_on": [],
      "estimated_hours": 2,
      "retry_count": 0,
      "max_retries": 2,
      "blocked_reason": null,
      "created_at": "YYYY-MM-DDTHH:mm:ssZ",
      "completed_at": null
    }
  ],
  "in_progress": null,
  "completed": [],
  "failed": [],
  "blocked": []
}
```

## `.helm/config.json`

```json
{
  "version": "2.0",
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

## SLO 文档

建议创建到 `.helm/quality/reliability/slo-service.md`。

```markdown
# SLO: <服务名称>

## SLI

- 可用性：<目标>%（滚动 30 天）
- P99 延迟：<N>ms
- 错误率：<N>%

## 降级策略

- 触发条件：
- 降级行为：
- 兜底数据：

## 告警配置

- 通道：
- 接收人：
- 升级路径：
```

## 需求追踪矩阵

建议创建到 `.helm/quality/traceability/matrix.md`。

```markdown
# 需求追踪矩阵

| 需求 | RFC | ADR | 任务 | 代码/PR | 测试 | 状态 |
|---|---|---|---|---|---|---|
|  |  |  |  |  |  |  |
```

