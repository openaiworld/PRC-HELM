# RMST 质量四柱

RMST 是 PRC-HELM 的质量门禁扩展，适合严格模式、生产系统、服务上线、架构迁移和长期治理。轻量任务可只做相关项检查。

## R · Reliability 可靠性

目标：消灭系统最薄弱环节，避免单点故障和无降级依赖。

建议检查：

- 新服务上线前是否定义 SLO：可用性目标、错误率阈值、P99 延迟。
- 外部依赖调用是否有熔断、限流、超时、重试、兜底数据。
- 核心路径是否有告警，是否定义 MTTD / MTTR 目标。
- 是否定期做故障注入或恢复演练。

建议归档：

```text
.helm/quality/reliability/slo-service.md
.helm/quality/reliability/fallback-service.md
.helm/quality/reliability/alerts.md
.helm/quality/reliability/chaos-YYYY-QN.md
```

## M · Maintainability 可维护性

目标：控制复杂度，避免代码腐化、循环依赖和不可测试模块。

建议检查：

- 函数圈复杂度建议不超过 10。
- 函数长度建议不超过 50 行；如超过，说明原因或拆分。
- 不引入循环依赖。
- 核心逻辑有单测，至少覆盖 happy path 和一个关键边界。
- 新依赖说明用途、风险、替代方案和锁文件变化。
- 每个 Sprint 预留约 20% 技术债偿还预算。

建议工具：ESLint complexity、max-lines-per-function、madge、cargo-udeps、依赖审计工具等。

## S · Scalability 可扩展性

目标：让架构适应业务增长，而不是成为生产力瓶颈。

建议检查：

- 新模块是否有清晰扩展点，优先 Plugin / Strategy / Event 等低耦合方式。
- 公共 API 是否遵循语义化版本和兼容策略。
- 接口废弃是否给出 Deprecation Path，建议至少 2 个版本兼容期。
- 性能基线是否存在，变更前后是否有 benchmark 或等价验证。
- 水平扩展是否避免本地状态绑定。

建议归档：

```text
.helm/quality/scalability/extension-points.md
.helm/quality/scalability/api-versioning.md
.helm/quality/scalability/perf-baseline-YYYY-MM.md
.helm/quality/scalability/stateless-design.md
```

## T · Traceability 可溯源性

目标：让需求、决策、代码、测试和事故可以追踪，避免历史断裂。

建议检查：

- Commit 是否符合 Conventional Commits 或项目既有规范。
- 架构级变更是否有关联 RFC/ADR。
- 需求是否能追踪到任务、代码和测试。
- 关键日志是否结构化，是否有 TraceID 或请求关联字段。
- 发版是否更新 `CHANGELOG.md`。

建议归档：

```text
.helm/quality/traceability/matrix.md
.helm/quality/traceability/observability.md
.helm/decisions/adr/
CHANGELOG.md
```

## PR 前快速检查

```markdown
- [ ] R：新服务有 SLO？外部调用有降级策略？
- [ ] M：圈复杂度≤10？函数是否足够短？无循环依赖？核心逻辑有测试？
- [ ] S：接口变更有兼容/废弃路径？核心数据结构变更有影响分析？
- [ ] T：Commit/PR 关联需求或 issue？架构变更有 ADR？
```

## Review 标签

```text
[RMST-R] 可靠性问题
[RMST-M] 可维护性问题
[RMST-S] 可扩展性问题
[RMST-T] 可溯源性问题
```
