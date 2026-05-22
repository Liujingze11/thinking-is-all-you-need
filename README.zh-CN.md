# Thinking Is All You Need

[![agentskills.io](https://img.shields.io/badge/agentskills.io-compatible-4B32C3)](https://agentskills.io)

> [Read in English →](README.md)

> *灵感来自 "Attention Is All You Need" (Vaswani et al., 2017) 与爱因斯坦："如果给我一小时解决问题，我会花 55 分钟思考，5 分钟行动。"*

一个**模型无关的推理技能**，在生成答案前强制执行结构化思考。

**对于 LLM，Attention Is All You Need。对于 AI Agent，Thinking Is All You Need。**

如果说 Transformer 证明了 Attention 是架构的核心，那么这个技能证明了 Thinking 是方法论的核心。

> 📖 **阅读项目背后的故事：** [我给 Agent 加上"思考流程"后，准确率突然提升了](STORY.zh-CN.md)

## 问题

大推理模型会在内部思考，小模型则直接跳到输出，导致：

- 代码中因未检查约束而产生逻辑错误
- JSON/配置文件字段缺失或自相矛盾
- 计划在第一个边界情况就崩溃

## 解决方案

**Thinking Is All You Need** 在生成任何答案前增加一个四阶段推理门：

```
Deconstruct → Plan → Trace → Verify → Output
```

每个阶段捕获前一阶段遗漏的错误。`---正式输出---` 标记将思考过程与最终答案分离。

## 四阶段

| 阶段 | 做什么 | 产出 |
|------|--------|------|
| **Deconstruct（拆解）** | 拆解为原子化的需求、约束和边界情况 | 穷举清单 |
| **Plan（规划）** | 列举每一个输出单元 | 完整骨架 |
| **Trace（追踪）** | 遍历每一条执行路径，标记漏洞 | 修正后的计划 |
| **Verify（验证）** | 对照 Phase 1 的约束逐项检查 | 验证通过的输出 |

然后：`---正式输出---` → 干净、正确的答案。

## 安装

```bash
# Claude Code
mkdir -p ~/.claude/skills/thinking-is-all-you-need
cp SKILL.md ~/.claude/skills/thinking-is-all-you-need/
```

## 使用场景

| 场景 | 使用？ |
|------|--------|
| 带约束的代码生成 | 是 |
| 结构化输出（JSON、YAML、配置） | 是 |
| 多步骤架构/工作流设计 | 是 |
| 小模型/弱模型（Haiku 等） | 是 — 效果最显著 |
| 简单事实查询 | 否 |
| 休闲对话 | 否 |

## 模型效果

| 模型 | 不使用时 | 使用时 | 提升 |
|------|---------|--------|------|
| Haiku | 基线错误率 | 显著改善 | ★★★★★ |
| Sonnet | 偶尔疏忽 | 捕获边界情况 | ★★★ |
| Opus | 已经很强 | 复杂任务仍有帮助 | ★ |

**模型越小，收益越大。** 显式结构弥补了隐式推理的不足。

## 命名由来

致敬两个思想：

1. **"Attention Is All You Need"** — 2017 年改变 NLP 的论文。Attention 是 LLM 处理信息的方式；**Thinking 是 AI Agent 应如何行动的方式。**
2. **爱因斯坦的 55/5 法则** — 花 55 分钟思考，5 分钟行动。

## 对比

| 技能 | 范围 | 风格 |
|------|------|------|
| **Thinking Is All You Need** | 通用 — 任意任务 | 四阶段内部独白 |
| Systematic Debugging | 仅限 Bug | 领域特定的追踪 |
| Brainstorming | 创意设计 | 与用户协作 |
| Verification Before Completion | 完成声明 | 基于命令的检查 |

唯一一个对所有答案类型进行推理门控、且输出可机械提取的技能。

## 文件结构

```
thinking-is-all-you-need/
├── SKILL.md         # 技能文件
├── STORY.md         # 项目故事（英文）
├── STORY.zh-CN.md   # 项目故事（中文）
├── README.md        # 本文件（英文）
├── README.zh-CN.md  # 本文件（中文）
└── LICENSE          # MIT
```

## License

MIT — 自由使用、修改、分发。
