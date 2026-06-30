---
title: Agent Skills 体系
topic: openclaw-ecosystem
type: article
status: active
version: 1.1.0
created: 2026-06-27
updated: 2026-06-29
sources: King · MyBrain · 2026-06-29
sources: King · MyBrain · 2026-06-27
tags: [skill, openclaw, capability]
---

# Agent Skills 体系

## 总览（17个 Active Skills）

| 域 | Agent | Skill | 来源 | 版本 |
|----|-------|-------|------|------|
| **CEO** | ceo | agent-coordinator | 内部 | v1.0.0 |
| **CEO** | ceo | cron-manager | 内部 | v1.0.0 |
| **CEO** | ceo | knowledge-manager | 内部 | v1.0.0 |
| **CEO** | ceo | memory-manager | 内部 | v1.0.0 |
| **CEO** | ceo | standard-auditor | 内部 | v1.0.0 |
| **CTO** | cto | github-sync | bradvin/openclaw-github-sync | — |
| **CTO** | cto | healthcheck | pettercc2024/openclaw-super-healthcheck | — |
| **CTO** | he | hardware-ops | 内部 | v1.0.0 |
| **CTO** | se | software-design | 内部 | v1.0.0 |
| **CTO** | pe | product-requirements | 内部 | v1.0.0 |
| **COO** | coo | project-management-2 | jk-0001 | — |
| **COO** | coo | task | amirbrooks | — |
| **COO** | coo | productivity | ivangdavila | — |
| **COO** | pm | project-management | 内部 | v1.0.0 |
| **COO** | qm | quality-metrics | 内部 | v1.0.0 |
| **CA** | ca | ciso | ivangdavila | — |
| **KB** | llm-wiki | karpathy-llm-wiki | 内部 | v1.0.0 |

> ⚠️ v1.1.0 更新：同步 CEO/COO/CTO/CA 域 Agent AGENTS.md v2.1.0，Skills 来源更新。

---

## SKILL.md 格式规范

每个 Skill 包含：

```yaml
---
name: <skill-name>
description: "触发描述（<160字节）"
version: v1.0.0
metadata:
  openclaw:
    requires: []    # 依赖的其他Skills
    category: <类别>  # knowledge/operation/quality等
---
# Skill 主体
## 核心操作
## 格式规范
## 与其他Skill的关系
```

---

## CEO Skills（5个）

### agent-coordinator
**触发**：多Agent任务协调、任务派发、结果汇总。
**核心**：CEO向各域Agent派发任务→收集结果→汇总报告。

### cron-manager
**触发**：Cron Job 创建/更新/删除/验证。
**核心**：IATF16949定时任务生命周期管理，v2.0.0命名体系。

### knowledge-manager
**触发**：知识库操作、MyBrain维护、新知识入库。
**核心**：KB编码、受控文件台账、新知识沉淀评审。

### memory-manager
**触发**：MEMORY.md更新、记忆同步、每日归档。
**核心**：长时记忆与短时记忆的同步归档机制。

### standard-auditor
**触发**：合规审计、工作区标准化检查。
**核心**：frontmatter合规性、跨文件引用、版本一致性。

---

## CTO Skills（5个）

### github-sync
**来源**：bradvin/openclaw-github-sync
**触发**：GitHub 仓库同步、备份、私有仓库创建。
**核心**：仓库状态监控、自动化备份、PR管理。

### healthcheck
**来源**：pettercc2024/openclaw-super-healthcheck
**触发**：OpenClaw 系统深度健康检查与修复。
**核心**：多维度健康扫描、自动修复建议。

### hardware-ops
**触发**：硬件设备运维、硬件问题诊断。
**核心**：设备管理、运维SOP、故障响应。

### software-design
**触发**：软件设计、建模、API设计。
**核心**：设计文档、UML建模、接口规范。

### product-requirements
**触发**：产品需求分析、PRD评审、需求优先级。
**核心**：用户故事、需求拆分、优先级评估。

---

## COO Skills（4个）

### project-management-2
**来源**：jk-0001
**触发**：项目管理、Eisenhower 矩阵、周/日规划 ritual、多阶段项目管理。
**核心**：任务分解、进度追踪、风险识别。

### task
**来源**：amirbrooks
**触发**：任务状态跟踪、跨会话任务管理、任务优先级。
**核心**：任务生命周期、状态可视化、跨会话持久化。

### productivity
**来源**：ivangdavila
**触发**：生产力框架、ADHD/燃烧殆尽/习惯养成等针对性方案。
**核心**：个性化生产力提升方案。

### project-management（PM）
**触发**：项目管理、进度追踪、里程碑评审。
**核心**：任务分解、进度报告、风险识别。

### quality-metrics（QM）
**触发**：质量指标统计、质量趋势分析。
**核心**：度量体系、数据采集、报告生成。

---

## CA Skill（1个）

### ciso
**来源**：ivangdavila
**触发**：首席信息安全官框架、合规审计、风险评估、供应商安全、事件管理。
**核心**：合规性评估、风险矩阵、安全策略。

---

## PE Skill（1个）

### product-requirements
**触发**：产品需求分析、PRD评审、需求优先级。
**核心**：用户故事、需求拆分、优先级评估。

---

## KB Skill（1个）

### karpathy-llm-wiki
**触发**：添加到wiki、知识查询、wiki质量检查。
**核心**：Ingest（入库）/Query（查询）/Lint（质量检查）三操作。

---

## Skill 存放规范

| Skill 类型 | 存放路径 |
|-----------|---------|
| CEO Skills | `~/.openclaw/agents/ceo/skills/{skill}/SKILL.md` |
| 域 Skills | `~/.openclaw/agents/{agent}/skills/{skill}/SKILL.md` |
| LLM Wiki | `MyBrain/llm-wiki/wiki/方法/llm-wiki/SKILL.md` |

> Skills 不得存放于 `workspace/MyBrain/system/`

---

## See Also

- [Agent体系](agent体系.md)
- [SOP与工作流体系](sop体系.md)