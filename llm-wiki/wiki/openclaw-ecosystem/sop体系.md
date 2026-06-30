---
title: SOP 与工作流体系
topic: openclaw-ecosystem
type: article
status: active
version: 1.2.0
created: 2026-06-27
updated: 2026-06-29
sources: King · MyBrain · 2026-06-29
sources: King · MyBrain · 2026-06-28
sources: King · MyBrain · 2026-06-27
tags: [sop, workflow, IATF16949, process]
---

# SOP 与工作流体系

## 总览（27+7个文件）

| 域 | SOP | WF | WI | 合计 |
|----|-----|----|----|------|
| KB（知识库） | 11 | 5 | 0 | **16** |
| AG（Agent） | 3 | 0 | 1 | **4** |
| OP（运营） | 2 | 0 | 0 | **2** |
| QL（质量） | 3 | 0 | 0 | **3** |
| **合计** | **19** | **5** | **1** | **25** |

另有 8个模板（WI） + 3个系统索引 + **7个 System Skills（v3.6.0新增）**。

> ⚠️ v1.2.0 更新：OP域 SOP 统计修正（原 24 → 25），依据 cron.md v2.3.0。

---

## KB 域 SOP（11个）

### 知识管理 SOP

| 编码 | 名称 | 核心内容 |
|------|------|---------|
| KB_SOP_001 | SOP标准格式 | frontmatter规范、编码规则、文件结构 |
| KB_SOP_002 | 卡片生命周期管理 | 概念/方法/案例卡新建→审核→发布→归档 |
| KB_SOP_003 | 文件变更控制 | patch/minor/major三级变更，6阶段审批 |
| KB_SOP_004 | 标准化审计 | Agent工作区、受控文件合规性审计 |
| KB_SOP_006 | 知识入库 | 新知识沉淀至MyBrain的完整流程 |
| KB_SOP_007 | 告警与升级路径 | P0~P3分级，升级路径，响应SLA |

### 复盘 SOP（三层闭环）

| 编码 | 名称 | 频率 | 触发时间 |
|------|------|------|---------|
| KB_SOP_005 | 每日复盘SOP | 日 | 21:30 |
| KB_SOP_010 | 每周复盘SOP | 周（周五）| 17:00 |
| KB_SOP_011 | 每月复盘SOP | 月（月末）| 18:00 |

### 自进化 SOP

| 编码 | 名称 | 核心内容 |
|------|------|---------|
| KB_SOP_008 | 自进化体系闭环 | 日常→复盘→Dreaming→洞察→行动 |
| KB_SOP_009 | Heartbeat标准化 | 30min心跳机制，多维度轮检 |

### Agent 治理 SOP

| 编码 | 名称 | 核心内容 |
|------|------|---------|
| **AG_SOP_001** | **Agent 自动标准化 SOP** | 新建 Agent 自动符合 v2.0.0 标准：8个必填文件/SOUL≥10行/自动检查清单 |

---

## KB 工作流（5个）

| 编码 | 名称 | 触发条件 |
|------|------|---------|
| KB_WF_001 | Agent协调工作流 | 多Agent协作任务 |
| KB_WF_002 | 故障响应工作流 | 系统异常/故障 |
| KB_WF_003 | 新知识入库工作流 | 外部知识引入 |
| KB_WF_004 | Dreaming System工作流 | 夜间记忆整合（22:00~06:00）|
| KB_WF_005 | 自进化评估工作流 | 季度/里程碑评估 |

---

## AG 域 SOP（3个）+ WI（1个）

| 编码 | 名称 | 生命周期阶段 |
|------|------|------------|
| AG_SOP_001 | Agent上线流程 | 创建→配置→Skill绑定→上线 |
| AG_SOP_002 | Skill上线流程 | 提案→评审→批准→发布 |
| AG_SOP_003 | Agent下线流程 | 撤销→数据归档→技能转移 |
| AG_WI_001 | Cron Job上线流程 | 需求→评估→审批→配置→验证 |

---

## QL 域 SOP（3个）

| 编码 | 名称 | 适用范围 |
|------|------|---------|
| QL_SOP_001 | 外部工具集成规范 | API调用、CLI工具 |
| QL_SOP_002 | 敏感信息脱敏规范 | API Key、Token、App Secret |
| QL_SOP_003 | 外部工具接入评审流程 | 工具引入前的安全评审 |

---

## System Skills（v3.6.0 新增，7个）

> OpenClaw v2026.6.11 全面升级引入，存放于 `MyBrain/system/skills/`

| 名称 | 核心能力 |
|------|---------|
| browser-automation | 浏览器自动化：快照/稳定标签/失活引用恢复 |
| lobster-workflow | Lobster 工作流引擎：JSON管道/审批门/可恢复token |
| active-memory | Active Memory：blocking memory sub-agent 回复前召回 |
| karpathy-llm-wiki | Karpathy LLM Wiki：持久化编译型知识积累 |
| llm-wiki-automation | **LLM Wiki 自动化引擎**：Auto-Ingest（每日21:30）+ Auto-Lint（每周五18:00）|
| taskflow | OpenClaw Task Flow：持久化多步骤流程编排 |
| usage-tracking | Usage Tracking：配额监控/Token统计/成本估算 |

> **Auto-Ingest 引擎**：本每日 ingest 即为 llm-wiki-automation Skill 的核心能力，驱动全生态知识向 wiki 的自动化沉淀。

---

## Cron Job 体系（v2.3.0 · 共 19 个 · 活跃 18 个 · 禁用 1 个）

| 域 | 频率 | Job 数 | 代表 Job |
|---|---|---|---|
| sys | hourly+daily+weekly+monthly | 10 | sys_health_hourly, sys_audit_weekly |
| knowledge | daily+weekly | 4 | knowledge_wiki_daily, knowledge_sync_daily |
| llm | weekly | 1 | llm_wiki_weekly |
| quality | weekly | 1 | quality_check_weekly |
| 插件托管 | daily | 1 | Memory Dreaming Promotion（🔒）|

> ⚠️ 已知问题：`knowledge_audit_weekly` 曾连续4次 timeout（300s不够）→ 已修复 timeout→600s，待下次运行验证。

---

## 复盘三层闭环

```
┌──────────────────────────────────────────────┐
│  每日复盘（KB_SOP_005） 21:30                │
│  → daily/日志 + 5维度检查                   │
└──────────────────────┬───────────────────────┘
                       ▼
┌──────────────────────────────────────────────┐
│  每周复盘（KB_SOP_010） 周五 17:00           │
│  → 周报_WXX + 问题模式分类                   │
└──────────────────────┬───────────────────────┘
                       ▼
┌──────────────────────────────────────────────┐
│  每月复盘（KB_SOP_011） 月末 18:00           │
│  → 月报_MM + SWOT + 生态健康度评分           │
└──────────────────────┬───────────────────────┘
                       ▼
              Dreaming System（KB_WF_004）
              → 22:00~06:00 夜间整合
                       ▼
              洞察生成 → 自进化评估（KB_WF_005）
```

---

## See Also

- [OpenClaw生态概述](生态概述.md)
- [Agent体系](agent体系.md)
- [Agent Skills](agent-skills.md)