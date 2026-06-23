# 工具适配指南

PRC-HELM 是通用工程治理 skill，真正落地时需要放到不同编程工具的“自动读取入口”中。优先使用工具原生入口；不要假设所有工具都会自动发现 `SKILL.md`。

## ZCode / Agents Skills

推荐目录：

```text
~/.agents/skills/prc-helm/
  SKILL.md
  agents/openai.yaml
  references/
  assets/
```

显式调用示例：

```text
使用 $prc-helm 为这个重构任务制定计划，并给出风险与验证方式。
```

## 通过 skills / skill-installer 安装

如果环境提供 `skill-installer` 或等价的 `skills` 安装能力，推荐从 GitHub 安装：

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

安装后重启对应编程工具，以便重新加载 skill 列表。

## Claude Code

Claude Code 通常读取项目根目录或用户目录中的 `CLAUDE.md`。

项目级推荐（从 PRC-HELM skill 根目录执行或替换为实际路径）：

```bash
cp assets/adapters/CLAUDE.md ./CLAUDE.md
```

如果没有适配文件，可在 `CLAUDE.md` 中加入：

```markdown
# PRC-HELM

进行项目规划、功能开发、技术选型、架构变更、事故处理和迭代收尾时，遵循 prc-helm 的规则：

- 先识别当前主要矛盾。
- 新功能先写用户故事与验收标准。
- 技术选型使用加权矩阵。
- 架构/跨模块变化使用 RFC/ADR。
- 事故修复后补回归验证，严重事故写 postmortem。
- 用户说“收工/总结一下/迭代结束”时输出健康度评分。
```

## OpenAI Codex CLI / AGENTS.md 风格工具

Codex 类工具通常读取 `AGENTS.md`。

项目级推荐（从 PRC-HELM skill 根目录执行或替换为实际路径）：

```bash
cp assets/adapters/AGENTS.md ./AGENTS.md
```

简化片段：

```markdown
# Rules

- 遵循 prc-helm：需求先行、主要矛盾、实践验证、用户价值、证据化决策。
- 大任务先计划；跨模块变化先 RFC/ADR；技术选型先矩阵；事故先复现再修复。

# Workflows

1. 读取项目文档和上下文。
2. 说明主要矛盾。
3. 选择轻量/标准/严格模式。
4. 执行并验证。
5. 记录风险、技术债和后续事项。
```

## Cursor

Cursor 新版推荐 `.cursor/rules/*.mdc`，旧版可用 `.cursorrules`。

```bash
mkdir -p .cursor/rules
cp assets/adapters/prc-helm.mdc .cursor/rules/prc-helm.mdc
```

规则应尽量短，保留核心约束即可：主要矛盾、需求先行、技术矩阵、RFC/ADR、测试验证、迭代收尾。

## Windsurf

Windsurf 可使用 `.windsurfrules`：

```bash
cp assets/adapters/.windsurfrules ./.windsurfrules
```

## GitHub Copilot

Copilot 项目指令通常放在：

```text
.github/copilot-instructions.md
```

建议写入核心规则，而不是完整方法论文档，避免上下文过重。

## 适配原则

- **短入口，长引用**：自动读取入口只写硬规则和触发词；完整模板保留在 `references/`。
- **项目级优先**：不同项目的约束不同，优先把规则安装到项目根目录，而非全局强制所有项目使用。
- **显式调用最可靠**：如果工具不支持自动 skill 路由，直接在提示词中写“使用 prc-helm”。
- **不要覆盖仓库既有规范**：已有 `AGENTS.md`、`CLAUDE.md`、PR 模板时，合并关键规则，不要直接替换。
