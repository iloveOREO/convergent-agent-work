# Convergent Agent Work

[English](README.md) | 简体中文

一个独立的 Codex 技能，用于让长时间、反复迭代或包含多个工作流的代理任务保持聚焦和收敛，同时不削弱完整性与必要验证。

## 为什么需要它

长任务容易逐渐演变成重复调用工具、多个审查代理检查同一范围、每次小修复后重新进行全量审查，以及上下文不断膨胀。这个技能为 Codex 提供一组紧凑的决策规则，要求每个新增动作都能减少不确定性、改变交付物，或验证实质风险。

## 它会做什么

- 维护紧凑的工作账本，记录约束、发现、改动、验证和未解决风险。
- 要求每次新增工具调用、重试、审查或子代理都产生增量价值。
- 区分委派工作流的职责范围，并复用已有代理上下文。
- 将复查范围缩小到实际变化，而不是每次都重新进行完整审查。
- 重试外部写操作前，先核对结果不明确的远端状态。
- 压缩累积上下文后继续工作，避免提前结束仍可完成的任务。

该技能不会设置固定的工具、时间、token 或代理数量上限，也不会以提高效率为由跳过必要工作。

## 适用场景

适合用于：

- 长时间的编码、调试、迁移或发布任务；
- 包含多个独立风险面的多代理工作；
- 反复进行审查、修复和验证的流程；
- 已出现重复命令、重复审查或上下文反复膨胀的工作流。

普通短任务即使使用多个工具，也不应仅凭这一点触发该技能。

## 安装

### 使用技能安装器

让 Codex 从本仓库安装技能：

```text
$skill-installer Install the skill from https://github.com/iloveOREO/convergent-agent-work
```

如果仓库是私有的，请确保运行 Codex 的环境已经通过 GitHub 身份验证。

### 手动安装

将仓库克隆到 Codex 官方文档中的用户级技能目录：

```bash
mkdir -p "$HOME/.agents/skills"
git clone https://github.com/iloveOREO/convergent-agent-work \
  "$HOME/.agents/skills/convergent-agent-work"
```

Codex 会自动检测技能变更。如果技能没有出现，请重启 Codex。

更新手动安装的版本：

```bash
git -C "$HOME/.agents/skills/convergent-agent-work" pull --ff-only
```

## 使用

显式调用技能：

```text
$convergent-agent-work Keep this multi-agent migration thorough and convergent.
```

当任务符合技能描述中的适用范围时，Codex 也可能隐式调用它。

## 安全边界

- 只读审查请求保持只读；实施修复需要相应授权。
- 外部写入结果不明确时，必须先核对权威状态，再决定是否重试。
- 不会为了效率跳过仓库要求的检查或与风险相匹配的验证。
- 只有用户要求、遇到真实阻塞，或当前环境无法安全可靠地继续时，才交接到新任务。

## 仓库结构

```text
.
├── README.md
├── README.zh-CN.md
├── SKILL.md
└── agents/
    └── openai.yaml
```

- `README.md` 和 `README.zh-CN.md` 分别提供英文与简体中文说明。
- `SKILL.md` 包含技能元数据和工作流指令。
- `agents/openai.yaml` 提供 Codex UI 元数据并启用隐式调用。

技能格式、发现位置和调用行为请参阅 [OpenAI 官方技能开发文档](https://developers.openai.com/codex/skills)。
