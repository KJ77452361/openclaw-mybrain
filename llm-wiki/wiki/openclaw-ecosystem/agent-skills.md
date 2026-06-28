---
title: Agent Skills 体系
topic: openclaw-ecosystem
type: article
status: active
version: 1.0.0
created: 2026-06-27
updated: 2026-06-27
sources: King · MyBrain · 2026-06-27
tags: [skill, openclaw, capability]
---

# Agent Skills 体系

## 总览（15个 Active Skills）

| 域 | Agent | Skill | 编码 | 版本 |
|----|-------|-------|------|------|
| **CEO** | ceo | agent-coordinator | CEO_SK_AC_001 | v1.0.0 |
| **CEO** | ceo | cron-manager | CEO_SK_CM_001 | v1.0.0 |
| **CEO** | ceo | knowledge-manager | CEO_SK_KM_001 | v1.0.0 |
| **CEO** | ceo | memory-manager | CEO_SK_MM_001 | v1.0.0 |
| **CEO** | ceo | standard-auditor | CEO_SK_SA_001 | v1.0.0 |
| **CTO** | architect | architecture-review | AG_SK_ARCH_001 | v1.0.0 |
| **CTO** | qa_engineer | quality-audit | AG_SK_QA_001 | v1.0.0 |
| **CTO** | fullstack_engineer | code-review | AG_SK_FS_001 | v1.0.0 |
| **CTO** | he | hardware-ops | AG_SK_HE_001 | v1.0.0 |
| **CTO** | se | software-design | AG_SK_SE_001 | v1.0.0 |
| **COO** | pm | project-management | AG_SK_PM_001 | v1.0.0 |
| **COO** | qm | quality-metrics | AG_SK_QM_001 | v1.0.0 |
| **CA** | ca | strategic-analysis | AG_SK_CA_001 | v1.0.0 |
| **PE** | pe | product-requirements | AG_SK_PE_001 | v1.0.0 |
| **KB** | llm-wiki | karpathy-llm-wiki | KB_SK_LLM_001 | v1.0.0 |

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

### architecture-review
**触发**：架构设计评审、技术选型决策。
**核心**：架构模式、最佳实践、风险评估。

### code-review
**触发**：代码实现检查、代码质量评估。
**核心**：全栈代码评审、标准规范合规。

### quality-audit
**触发**：质量测试、QA流程、测试覆盖率评审。
**核心**：测试用例评审、缺陷分析。

### hardware-ops
**触发**：硬件设备运维、硬件问题诊断。
**核心**：设备管理、运维SOP、故障响应。

### software-design
**触发**：软件设计、建模、API设计。
**核心**：设计文档、UML建模、接口规范。

---

## COO Skills（2个）

### project-management
**触发**：项目管理、进度追踪、里程碑评审。
**核心**：任务分解、进度报告、风险识别。

### quality-metrics
**触发**：质量指标统计、质量趋势分析。
**核心**：度量体系、数据采集、报告生成。

---

## CA Skill（1个）

### strategic-analysis
**触发**：战略规划、合规评审、重大决策支持。
**核心**：SWOT分析、风险评估、战略对齐。

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
