---
title: Agent上线流程SOP
type: SOP
code: AG_SOP_001_v1.0.0
version: 1.0.0
created: 2026-06-27
updated: 2026-06-27
tags: [SOP, agent, lifecycle, onboarding, IATF16949]
regulatory: IATF16949:2016 §7.5
owner: CEO
status: active
---

# Agent 上线流程 SOP

> 编码：AG_SOP_001_v1.0.0 | 版本：1.0.0 | 2026-06-27 | 状态：active
> 监管要求：IATF16949:2016 §7.5 成文信息控制
> 适用范围：所有新 Agent 加入 OpenClaw 生态的上线流程

## 1. 目的

规范新 Agent 加入 OpenClaw 生态的标准流程，确保：
- 新 Agent 有完整的工作区文件结构
- 符合标准化要求（5个必备文件）
- 有明确的汇报线和职责定义
- 可被 CEO 正确调度

## 2. 适用范围

新 Agent 创建并加入 `agents/` 目录。

## 3. Agent 分层

| 层级 | 说明 | 创建审批 |
|------|------|---------|
| CEO | 最高决策层 | 不适用 |
| COO | 运营管理层 | CEO 批准 |
| CTO | 技术管理层 | CEO 批准 |
| CA | 战略层 | CEO 批准 |
| 执行层（pe/he/se等） | 执行单元 | CTO 批准 |
| 业务层（sales_*） | 业务执行 | CEO 批准 |

## 4. 上线流程

```
需求确认 ──→ 工作区建立 ──→ 标准化填充 ──→ 上线评审 ──→ 上线 ──→ 调度配置
```

### Phase 1：需求确认

| 步骤 | 操作 | 产出 |
|------|------|------|
| 1.1 | 确认 Agent 名称（英文，简洁） | Agent ID |
| 1.2 | 确认 Agent 职责和边界 | 职责文档 |
| 1.3 | 确认汇报线（向谁汇报） | 汇报关系 |
| 1.4 | 确认所属层级（CEO/COO/CTO/CA/executive） | 层级定义 |
| 1.5 | 确认使用的工具和资源 | 工具清单 |

### Phase 2：工作区建立

**操作：**
1. 创建目录：`~/.openclaw/agents/{agent-id}/`
2. 创建子目录：
   ```
   agents/{agent-id}/
   ├── AGENTS.md          ← 必填
   ├── SOUL.md            ← 必填
   ├── IDENTITY.md        ← 必填
   ├── USER.md            ← 必填
   ├── MEMORY.md          ← 必填
   ├── skills/            ← 可选
   ├── knowledge/         ← 可选
   ├── memory/            ← 可选
   └── cron/              ← 可选
   ```
3. 分配资源（根据需求）

### Phase 3：标准化填充

**执行者：** Agent 创建者（发起人）

必须创建 5 个标准文件（见 §5）：

| 文件 | 最低内容要求 |
|------|------------|
| AGENTS.md | 角色定义 + 核心职责 + 汇报线 + 行为准则 |
| SOUL.md | 核心人格 + 沟通风格 + 禁止事项 |
| IDENTITY.md | 版本 + 更新日期 |
| USER.md | 用户信息（可引用） |
| MEMORY.md | 初始记忆（身份 + 角色描述） |

### Phase 4：上线评审

**评审者：** CEO 或对应域负责人

| 检查项 | 标准 |
|--------|------|
| 5 个标准文件存在 | 全部存在 |
| AGENTS.md | 含角色定义、职责、汇报线 |
| SOUL.md | 含人格描述 |
| frontmatter | 无需（工作区文件无强制要求） |
| 汇报线 | 明确向谁汇报 |
| 名称 | 符合英文小写规范 |
| 重复 | 无与现有 Agent 职能重复 |

**通过标准：** 全部检查项通过

### Phase 5：上线

**执行者：** CEO

| 步骤 | 操作 |
|------|------|
| 5.1 | 在 `knowledge/system/agents.md` 新增 Agent 条目 |
| 5.2 | 如 Agent 需要 Skill → 按 AG_SOP_002 创建 Skill |
| 5.3 | 如 Agent 需要 Cron → 创建 cron 定义 |
| 5.4 | 在 `daily/YYYY-MM-DD.md` 记录 Agent 上线 |
| 5.5 | 向相关方通知新 Agent 上线（飞书/企业微信） |

### Phase 6：调度配置

| 步骤 | 操作 |
|------|------|
| 6.1 | 确认 Agent 的 session key |
| 6.2 | 配置必要的 channel 权限 |
| 6.3 | 测试 Agent 响应（发送测试消息） |
| 6.4 | 记录 Agent session key（仅 CEO 知道） |

## 5. 标准文件模板

### AGENTS.md（必填内容）

```markdown
# {Agent Name}

## 角色

{一句话角色描述}

## 核心职责

1. {主要职责1}
2. {主要职责2}
3. {主要职责3}

## 汇报线

- **上级**：{汇报对象}
- **同级协作**：{相关 Agent}
- **调度方式**：sessions_spawn / sessions_send

## 行为准则

- {行为准则1}
- {行为准则2}

## 禁止事项

- {禁止事项1}
- {禁止事项2}
```

### SOUL.md（必填内容）

```markdown
# SOUL.md — {Agent Name} 人格

## 核心人格

{描述 Agent 的性格特点}

## 沟通风格

{描述沟通风格}

## 禁止表述

- {禁止表述1}
- {禁止表述2}
```

### IDENTITY.md（必填内容）

```markdown
# IDENTITY.md

> 版本：v1.0.0
> 创建：YYYY-MM-DD
> 更新：YYYY-MM-DD
```

### USER.md（必填内容）

```markdown
# USER.md

## 用户信息

- 名称：{名称}
- 称呼：{如何称呼}
```

### MEMORY.md（必填内容）

```markdown
# MEMORY.md — {Agent Name}

## 身份

我是 {Agent Name}，归属于 {域} 域，汇报给 {上级}。

## 角色

{角色描述}
```

## 6. 记录要求

| 记录 | 保存位置 | 保存期限 |
|------|---------|---------|
| 上线记录 | `daily/YYYY-MM-DD.md` | 永久 |
| Agent 档案 | `knowledge/system/agents.md` | 永久 |

## 7. 相关文件

| 文件 | 编码 |
|------|------|
| Agent 下线流程 SOP | AG_SOP_003_v1.0.0 |
| Skill 上线流程 SOP | AG_SOP_002_v1.0.0 |
| 文件变更控制 SOP | KB_SOP_003_v1.0.0 |
| Agent 协调工作流 | KB_WF_001_v1.0.0 |

## 8. 变更记录

| 日期 | 版本 | 变更内容 | 变更人 |
|------|------|---------|--------|
| 2026-06-27 | v1.0.0 | 首版发布 | King |

---

*version 1.0.0 · King · 2026-06-27*
