# Rules

This project uses `prc-helm` as its engineering governance rule set.

- Start each non-trivial task by naming the current main contradiction in one sentence.
- For new features, clarify or draft the user story and acceptance criteria before broad implementation.
- For technology choices, use an evidence-based weighted matrix.
- For architecture, cross-module, public API, persistence, deployment, security, or long-term maintenance changes, create or propose RFC/ADR records.
- For uncertain technical paths, run or propose a `spike/` PoC and record the result.
- For refactors and migrations, prefer staged rollout with rollback paths.
- For bugs, reproduce first when possible, then fix and add regression validation.
- For severe incidents, create or propose a postmortem within 72 hours.
- When the user says “收工”, “总结一下”, “迭代结束”, “sprint 结束”, or “本轮完成”, produce an iteration health report.

# Workflows

1. Read project guidance and relevant context.
2. State the main contradiction.
3. Choose mode: light, standard, or strict.
4. Execute the task with tests or explicit validation.
5. Report risks, technical debt, and next steps.

# Response Shape

Use when applicable:

```markdown
主要矛盾：
执行模式：轻量 | 标准 | 严格
依据事实：
行动：
验证：
后续：
```
