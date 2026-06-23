# PRC-HELM

本项目采用 `prc-helm` 作为工程治理规则。

## 核心规则

- 任务开始先识别一句话“主要矛盾”。
- 新功能先明确用户故事与验收标准。
- 技术选型必须使用证据化加权矩阵。
- 跨模块、公共 API、持久化、部署、安全或长期维护变化应使用 RFC/ADR。
- 不确定技术先做 `spike/` 或 PoC，并记录结论。
- 重构和迁移优先分阶段推进，保留回滚路径。
- Bug 修复先复现再修复，核心故障补回归验证。
- 严重事故在 72 小时内输出 postmortem。
- 严格模式下执行 RMST 四柱检查：可靠性、可维护性、可扩展性、可溯源性。
- 项目存在 `.ai_helm/` 或用户要求多 Agent 协同时，读取 `.ai_helm/HELM.md`、`.ai_helm/agents.md`、`.ai_helm/state/progress.md`、`.ai_helm/state/queue.json`。
- 用户说“收工 / 总结一下 / 迭代结束 / sprint 结束 / 本轮完成”时，输出迭代健康度报告。

## 输出格式

适用时使用：

```markdown
主要矛盾：
执行模式：轻量 | 标准 | 严格
依据事实：
行动：
验证：
后续：
```

## 推荐目录

轻量模式：

```text
docs/requirements/
docs/rfc/
docs/adr/
docs/postmortem/
docs/retrospective/
docs/failure-kb/
spike/
progress.md
```

严格 / Loop 模式：

```text
.ai_helm/HELM.md
.ai_helm/agents.md
.ai_helm/config.json
.ai_helm/state/progress.md
.ai_helm/state/queue.json
.ai_helm/decisions/{rfc,adr}/
.ai_helm/quality/{reliability,maintainability,scalability,traceability}/
.ai_helm/failure-kb/
```

