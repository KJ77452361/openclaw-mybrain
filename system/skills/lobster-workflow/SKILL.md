---
name: lobster-workflow
description: Lobster typed workflow runtime — JSON 管道 + 审批门 + 可恢复状态
version: 1.0.0
metadata:
  openclaw:
    emoji: 🦞
    events: []
---

# Lobster Workflow Skill

Lobster 是 OpenClaw 的类型化工作流运行时，支持多步骤管道、审批门和可恢复 token。

## 核心概念

- **JSON 管道**: 命令通过 stdin/stdout 传递 JSON
- **审批门**: 副作用操作（发邮件/发消息）暂停等待审批
- **可恢复**: 暂停的工作流返回 token，审批后恢复不重新运行
- **内嵌运行**: 工作流在 Gateway 进程内执行，无需外部 CLI

## 基本用法

### JSON 管道模式

```json
{
  "action": "run",
  "pipeline": "inbox list --json | inbox categorize --json | inbox apply --json",
  "timeoutMs": 30000
}
```

### 工作流文件模式

```json
{
  "action": "run",
  "pipeline": "/path/to/workflow.lobster",
  "argsJson": "{\"tag\":\"family\"}"
}
```

### 审批恢复

```json
{
  "action": "resume",
  "token": "<resumeToken>",
  "approve": true
}
```

## Workflow 文件格式 (.lobster)

```yaml
name: inbox-triage
args:
  tag:
    default: "family"
steps:
  - id: collect
    command: inbox list --json
  - id: categorize
    command: inbox categorize --json
    stdin: $collect.stdout
  - id: approve
    command: inbox apply --approve
    stdin: $categorize.stdout
    approval: required
  - id: execute
    command: inbox apply --execute
    stdin: $categorize.stdout
    condition: $approve.approved
```

## 集成 LLM Task

对于需要结构化 LLM 步骤的工作流：

```yaml
steps:
  - id: classify
    command: llm-task --json
    stdin: $collect.json
```

## 输出信封

| Status | 含义 |
|--------|------|
| `ok` | 成功完成 |
| `needs_approval` | 暂停，需审批 token 恢复 |
| `cancelled` | 拒绝或取消 |

## 适用场景

✅ 适合 Lobster：
- 多步骤数据处理管道
- 需要人工审批的外部操作
- 定期运行的可恢复工作流
- AI 判断 + 确定性规则的混合流程

❌ 不适合 Lobster：
- 简单单步操作（直接用 tool）
- 完全 AI 判断无规则流程
- 需要实时交互的操作

## 在 Cron 中使用

```bash
openclaw cron add \
  --name "Inbox Triage" \
  --cron "0 8 * * 1-5" \
  --session session:inbox-triage \
  --message "Run inbox-triage Lobster workflow" \
  --announce \
  --channel feishu
```

---

*version 1.0.0 · King · 2026-06-27 · 基于 OpenClaw Lobster v2026.6.11*
