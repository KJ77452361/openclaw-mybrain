---
name: taskflow
description: OpenClaw Task Flow — 持久化多步骤流程编排，跨 Gateway 重启
version: 1.0.0
metadata:
  openclaw:
    emoji: 🔮
    events: []
---

# Task Flow Skill

Task Flow 是 OpenClaw 的流程编排层，运行于 Background Tasks 之上，管理持久化多步骤流程。

## vs Background Tasks

| 场景 | 用什么 |
|------|--------|
| 单个后台任务 | Plain Task |
| 多步骤管道（A→B→C）| Task Flow |
| 观察外部创建的任务 | Task Flow（mirrored）|
| 一次性提醒 | Cron Job |

## 两种同步模式

### Managed Mode

Task Flow 端到端拥有生命周期：创建任务→驱动完成→推进状态

```
Flow: weekly-report
  Step 1: gather-data    → task created → succeeded
  Step 2: generate-report → task created → succeeded
  Step 3: deliver        → task created → running
```

### Mirrored Mode

观察外部创建的任务并同步流程状态，不拥有任务创建

```
三个独立 cron jobs → 形成 "morning ops" routine
Mirrored Flow → 统一视图跟踪进度
```

## CLI 命令

```bash
# 列出活跃和最近的 flows
openclaw tasks flow list

# 显示特定 flow 详情
openclaw tasks flow show <lookup>

# 取消运行中的 flow 及其任务
openclaw tasks flow cancel <lookup>
```

## 可靠定时工作流模式

对于定期工作流（市场情报简报等）：

```bash
openclaw cron add \
  --name "Market Intelligence Brief" \
  --cron "0 7 * * 1-5" \
  --tz "America/New_York" \
  --session session:market-intel \
  --message "Run market-intel Lobster workflow. Verify source freshness before summarizing." \
  --announce \
  --channel feishu
```

**架构**：
1. Cron → 定时
2. Persistent Session → 累积上下文
3. Lobster → 确定性步骤+审批门
4. Task Flow → 跨步骤进度跟踪

## 可靠性检查

在 LLM 总结步骤前做前置检查：
- 浏览器可用性 + profile 选择
- API 凭证和配额
- 网络可达性
- 必需工具可用性

## 取消行为

`openclaw tasks flow cancel` 设置粘性取消意图：
- 活跃任务被取消
- 不启动新步骤
- 取消意图跨重启持久化

## 与 Lobster 的关系

| 特性 | Lobster | Task Flow |
|------|---------|-----------|
| 审批门 | ✅ | ❌ |
| 可恢复状态 | ✅ | ✅ |
| 持久化 | ✅ | ✅ |
| 跨步骤编排 | ❌ | ✅ |
| 多任务协调 | ❌ | ✅ |

## 适用场景

✅ Task Flow：
- 跨多天的定期报告
- 需要跟踪进度的复杂项目
- 多团队工作流协调
- 需要在 Gateway 重启后恢复的工作

---

*version 1.0.0 · King · 2026-06-27 · 基于 OpenClaw Task Flow v2026.6.11*
