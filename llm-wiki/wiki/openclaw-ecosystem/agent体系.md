---
title: Agent 体系 v3.0.0
topic: openclaw-ecosystem
type: article
status: active
version: 1.0.0
created: 2026-06-27
updated: 2026-06-27
sources: King · MyBrain · 2026-06-27
tags: [agent, multi-agent, architecture, v3.0.0]
---

# Agent 体系 v3.0.0

## 架构原则

OpenClaw Agent 体系遵循**层级汇报制**：
- CEO 为唯一对外入口
- 各域（CTO/COO/CA）向 CEO 汇报
- 域内 Agent 向域负责人汇报
- 域间协作通过 CEO 协调

---

## 三层架构

```
┌─────────────────────────────────────────┐
│         CEO（智能体助手/King）            │  ← 唯一对外入口
│   5个Skills：协调/记忆/知识/定时/审计     │
└──────────────┬──────────────────────────┘
               │ 汇报
       ┌───────┼───────┐
       ▼       ▼       ▼
    CTO     COO     CA（合规总监）
   技术      运营     战略
    │        │        │
  6个     4个+2个   ca（单一）
  Agent   Agent    （撤销VP1/VP2）
```

---

## 各域 Agent 详情

### CTO 域（技术负责人）

| Agent | 代号 | 角色 | Skill | 工作目录 |
|-------|------|------|-------|---------|
| architect | — | 架构设计与评审 | architecture-review | `~/.openclaw/agents/architect/` |
| fullstack_engineer | — | 全栈工程实现检查 | code-review | `~/.openclaw/agents/fullstack_engineer/` |
| qa_engineer | — | 质量审计与测试评审 | quality-audit | `~/.openclaw/agents/qa_engineer/` |
| he | hardware | 硬件运维支持 | hardware-ops | `~/.openclaw/agents/he/` |
| se | software | 软件设计与建模 | software-design | `~/.openclaw/agents/se/` |
| pe | product | 产品需求分析 | product-requirements | `~/.openclaw/agents/pe/` |

### COO 域（运营负责人）

| Agent | 代号 | 角色 | Skill | 工作目录 |
|-------|------|------|-------|---------|
| pm | project | 项目管理与进度追踪 | project-management | `~/.openclaw/agents/pm/` |
| qm | quality | 质量指标统计与分析 | quality-metrics | `~/.openclaw/agents/qm/` |
| rs | research | 研究调研 | — | `~/.openclaw/agents/rs/` |
| sa | sales | 销售分析 | — | `~/.openclaw/agents/sa/` |

### CA 域（合规总监）

| Agent | 代号 | 角色 | Skill | 工作目录 |
|-------|------|------|-------|---------|
| ca | compliance | 战略分析与规划 | strategic-analysis | `~/.openclaw/agents/ca/` |

### 已撤销 Agent

| Agent | 撤销日期 | 原因 | 归档状态 |
|-------|---------|------|---------|
| vp1（战略VP） | 2026-06-27 | 功能合并至 CA 域 | v3.0.0 标注撤销 |
| vp2（运营VP） | 2026-06-27 | 功能合并至 CA 域 | v3.0.0 标注撤销 |

### Workspace Agents（业务执行层）

| Agent | 角色 |
|-------|------|
| sales_executive | 销售执行 |
| sales_operator | 销售操作 |
| sales_orchestrator | 销售编排 |
| translator_zh | 中英翻译 |

---

## Agent 工作区标准化

每个 Agent 工作区统一格式：

| 文件 | 必须字段 | 版本 |
|------|---------|------|
| `AGENTS.md` | name, version, establish, reporting-line, standards-compliance | v2.0.0 |
| `MEMORY.md` | identity, reporting-line, knowledge-base, Skills-table, responsibilities | v2.0.0 |

---

## Skill 路径规范

Skills 存放于 `~/.openclaw/agents/{agent}/skills/{skill}/SKILL.md`
> 不得存放于 `workspace/MyBrain/system/`

---

## See Also

- [OpenClaw生态概述](生态概述.md)
- [Agent Skills](agent-skills.md)
- [SOP与工作流体系](sop体系.md)
