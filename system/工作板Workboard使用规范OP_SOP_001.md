---
title: 工作板（Workboard）使用规范
type: SOP
code: OP_SOP_001_v1.0.0
version: 1.0.0
updated: 2026-06-28
tags: [SOP, workboard, tasks, operations]
owner: CEO
status: active
---

# 工作板（Workboard）使用规范

> 编码：OP_SOP_001_v1.0.0 | 版本：1.0.0 | 2026-06-28 | 状态：active
> 适用范围：OpenClaw Workboard 任务管理

---

## 1. 目的

规范 OpenClaw Workboard 的使用，确保任务追踪清晰、优先级明确、闭环及时。

---

## 2. 适用范围

所有通过 `openclaw workboard` 或 Dashboard Workboard 标签页管理的任务卡片。

---

## 3. 卡片状态定义

| 状态 | 含义 | 何时使用 |
|------|------|---------|
| `todo` | 待处理 | 任务已识别，尚未开始 |
| `in_progress` | 进行中 | 正在执行 |
| `done` | 已完成 | 任务完成，等待验收 |
| `cancelled` | 已取消 | 放弃或不可行 |
| `lost` | 丢失 | 系统重启等导致的孤立任务 |

---

## 4. 优先级定义

| 优先级 | 含义 | 处理时限 |
|--------|------|---------|
| `high` | 高优先 | 24 小时内处理 |
| `normal` | 普通 | 3 天内处理 |
| `low` | 低优先 | 1 周内处理 |

---

## 5. 卡片创建规范

### 5.1 必须包含字段

```
标题（title）：简洁描述任务
优先级（priority）：high / normal / low
状态（status）：todo（默认）
标签（labels）：相关标签，如 github, cron, security, P1, P2 等
```

### 5.2 卡片标题命名

格式：`[领域] 简短描述`

示例：
- `GitHub Actions sync: 创建 classic PAT（repo scope）`
- `Feishu DM policy 安全加固`
- `sys_self_health_hourly isolated session 结构性限制`

### 5.3 卡片 notes 内容

每个卡片 notes 必须包含：
- **问题描述**：当前状态
- **根因分析**（如已知）：为什么会发生
- **修复路径**：如何解决
- **验收标准**：怎么确认修好了

---

## 6. 卡片生命周期

```
todo → in_progress → done
  ↓         ↓
cancelled  lost
```

---

## 7. 操作命令

```bash
# 列出所有卡片
openclaw workboard list

# 查看单个卡片
openclaw workboard show <card_id>

# 创建卡片
openclaw workboard create "<title>" --priority high --status todo --notes "<notes>"

# 派发就绪卡片（启动 worker run）
openclaw workboard dispatch
```

---

## 8. 维护要求

| 操作 | 频率 | 负责 |
|------|------|------|
| 审查所有 `todo` 卡片 | 每周 | CEO |
| 关闭已完成的 `done` 卡片 | 即时 | 执行者 |
| 清理 `lost` / `cancelled` 卡片 | 每周 | CEO |
| 更新卡片 notes（根因/验收） | 发现问题时 | 执行者 |

---

## 9. 禁止事项

- ❌ 创建无 notes 的卡片
- ❌ 长期保留 `in_progress` 状态但无进展的卡片（超过 3 天视为停滞）
- ❌ 删除已完成的历史任务（保留审计追踪）

---

## 10. 变更记录

| 日期 | 版本 | 变更内容 |
|------|------|---------|
| 2026-06-28 | v1.0.0 | 首版建立 |

---

*version 1.0.0 · King · 2026-06-28*