---
title: SOP索引
type: system
code: KB_SOP_000_v1.0.0
version: 4.5.0
created: 2026-06-27
updated: 2026-06-27
tags: [SOP, index, registry, IATF16949]
regulatory: IATF16949:2016 §7.5
owner: CEO
status: active
---

# MyBrain 全生态 SOP 索引

> 编码：KB_SOP_000_v1.0.0 | 版本：4.5.0 | 2026-06-27 | 状态：active
> 监管要求：IATF16949:2016 §7.5 | 保存期限：永久

## 1. 目的

本文档是 OpenClaw 生态内所有 SOP、WI、工作流、Agent 全生命周期标准的唯一索引，对应 IATF16949 §7.5 对受控文件清单的要求。

---

## 2. SOP 总览

### 知识库管理域（KB）— 11 个 SOP + 5 个工作流

| 编码 | 名称 | 版本 | 状态 | 适用范围 |
|------|------|------|------|---------|
| KB_SOP_001 | SOP标准格式 | v1.0.0 | active | 所有 SOP/WI 编制 |
| KB_SOP_002 | 卡片生命周期管理 | v1.0.0 | active | cards/ 下所有卡片 |
| KB_SOP_003 | 文件变更控制 | v1.0.0 | active | 所有受控文件 |
| KB_SOP_004 | 标准化审计 | v1.0.0 | active | 所有 Agent 工作区 |
| KB_SOP_005 | **每日复盘SOP** | v1.0.0 | active | CEO 及所有 Agent |
| KB_SOP_006 | 知识入库 | v1.0.0 | active | 所有知识沉淀操作 |
| KB_SOP_007 | 告警与升级路径 | v1.0.0 | active | 异常/故障响应 |
| KB_SOP_008 | 自进化体系闭环 | v1.0.0 | active | OpenClaw 自进化机制 |
| KB_SOP_009 | Heartbeat 标准化 | v1.0.0 | active | Heartbeat 配置与执行 |
| KB_SOP_010 | **每周复盘SOP** | v1.0.0 | active | 周维度复盘 |
| KB_SOP_011 | **每月复盘SOP** | v1.0.0 | active | 月/季度深度复盘 |

### 工作流域（WF）— 5 个工作流

| 编码 | 名称 | 版本 | 状态 | 适用范围 |
|------|------|------|------|---------|
| KB_WF_001 | Agent 协调工作流 | v1.0.0 | active | 多 Agent 协作任务 |
| KB_WF_002 | 故障响应工作流 | v1.0.0 | active | 系统异常/故障 |
| KB_WF_003 | 新知识入库工作流 | v1.0.0 | active | 外部知识沉淀 |
| KB_WF_004 | Dreaming System 工作流 | v1.0.0 | active | 夜间记忆整合与洞察生成 |
| KB_WF_005 | 自进化评估工作流 | v1.0.0 | active | 自进化效果定期评估 |

### Agent 域（AG）— 3 个 SOP + 1 个 WI

| 编码 | 名称 | 版本 | 状态 | 适用范围 |
|------|------|------|------|---------|
| AG_SOP_001 | Agent 上线流程 | v1.0.0 | active | 新 Agent 创建 |
| AG_SOP_002 | Skill 上线流程 | v1.0.0 | active | 新 Skill 创建 |
| AG_SOP_003 | Agent 下线流程 | v1.0.0 | active | Agent 撤销 |
| AG_WI_001 | Cron Job 上线流程 | v1.0.0 | active | Cron Job 生命周期 |

### 质量域（QL）— 3 个 SOP

| 编码 | 名称 | 版本 | 状态 | 适用范围 |
|------|------|------|------|---------|
| QL_SOP_001 | 外部工具集成规范 | v1.0.0 | active | 外部工具接入 |
| QL_SOP_002 | 敏感信息脱敏规范 | v1.0.0 | active | 所有含敏感信息的文件 |
| QL_SOP_003 | 外部工具接入评审流程 | v1.0.0 | active | 外部工具接入前评审 |

---

## 3. SOP 统计

| 域 | SOP | WF | WI | 合计 |
|----|-----|----|----|------|
| KB（知识库） | 10 | 5 | 0 | **15** |
| AG（Agent） | 3 | 0 | 1 | **4** |
| QL（质量） | 3 | 0 | 0 | **3** |
| **合计** | **16** | **5** | **1** | **22** |

---

## 4. 工作指导（WI）— 8 个模板

| 编码 | 名称 | 版本 | 状态 | 适用范围 |
|------|------|------|------|---------|
| KB_WI_001 | 每日笔记模板 | v1.0.0 | active | 每日记录 |
| KB_WI_002 | 卡片模板 | v1.0.0 | active | 新建卡片 |
| KB_WI_003 | 项目文档模板 | v1.0.0 | active | 项目建立 |
| KB_WI_004 | Cron报告模板 | v1.0.0 | active | Cron 输出 |
| KB_WI_005 | 会议记录模板 | v1.0.0 | active | 会议记录 |
| KB_WI_006 | 技能开发指南 | v1.0.0 | active | 新 Skill 编制 |
| KB_WI_007 | **周复盘模板** | v1.0.0 | active | 每周复盘 |
| KB_WI_008 | **月复盘模板** | v1.0.0 | active | 每月复盘 |

---

## 5. System Skills（workspace/MyBrain/system/skills/）— 7 个新增

> v3.6.0 OpenClaw v2026.6.11 全面升级引入

| 名称 | 版本 | 说明 |
|------|------|------|
| browser-automation | v1.0.0 | 浏览器自动化：快照/稳定标签/失活引用恢复 |
| lobster-workflow | v1.0.0 | Lobster 工作流引擎：JSON管道/审批门/可恢复token |
| active-memory | v1.0.0 | Active Memory 插件：blocking memory sub-agent 回复前召回 |
| karpathy-llm-wiki | v1.0.0 | Karpathy LLM Wiki 方法论：持久化编译型知识积累 |
| llm-wiki-automation | v1.0.0 | LLM Wiki 自动化引擎：Auto-Ingest + Auto-Lint |
| taskflow | v1.0.0 | OpenClaw Task Flow：持久化多步骤流程编排 |
| usage-tracking | v1.0.0 | Usage Tracking：配额监控/Token统计/成本估算 |

## 6. Agent Skills（已部署）— 14 个 + 2 个 KB Skills

| Agent | Skill | 版本 | 说明 |
|-------|-------|------|------|
| architect | architecture-review | v1.0.0 | 架构设计与评审 |
| qa_engineer | quality-audit | v1.0.0 | 质量审计与测试评审 |
| fullstack_engineer | code-review | v1.0.0 | 代码评审与实现检查 |
| pm | project-management | v1.0.0 | 项目管理与进度追踪 |
| qm | quality-metrics | v1.0.0 | 质量指标统计与分析 |
| ca | strategic-analysis | v1.0.0 | 战略分析与规划 |
| he | hardware-ops | v1.0.0 | 硬件设备运维支持 |
| se | software-design | v1.0.0 | 软件设计与建模 |
| pe | product-requirements | v1.0.0 | 产品需求分析 |
| llm-wiki | karpathy-llm-wiki | v1.0.0 | LLM Wiki 持久化知识库（Ingest/Query/Lint）|
| llm-wiki | llm-wiki-automation | v1.0.0 | LLM Wiki 自动运转引擎（每日 Auto-Ingest + 每周 Auto-Lint）|
| ceo | agent-coordinator | v1.0.0 | 多 Agent 协调 |
| ceo | cron-manager | v1.0.0 | Cron Job 生命周期管理 |
| ceo | knowledge-manager | v1.0.0 | 知识库维护 |
| ceo | memory-manager | v1.0.0 | 长时记忆管理 |
| ceo | standard-auditor | v1.0.0 | 标准化审计 |
| KB | karpathy-llm-wiki | v1.0.0 | LLM Wiki 持久化知识库（Ingest/Query/Lint）|
| KB | llm-wiki-automation | v1.0.0 | LLM Wiki 自动运转引擎（每日 Auto-Ingest + 每周 Auto-Lint）|

---

## 7. 复盘 SOP 体系（完整闭环）

```
┌─────────────────────────────────────────────────────────┐
│  每日复盘（KB_SOP_005）  ←  每日 21:30 执行            │
│  输出：daily/YYYY-MM-DD.md                             │
└────────────────────┬────────────────────────────────────┘
                     │ 每周五聚合
                     ▼
┌─────────────────────────────────────────────────────────┐
│  每周复盘（KB_SOP_010）  ←  每周五/六 执行              │
│  输出：daily/周报_YYYY-WXX.md                          │
└────────────────────┬────────────────────────────────────┘
                     │ 每月末聚合
                     ▼
┌─────────────────────────────────────────────────────────┐
│  每月复盘（KB_SOP_011）  ←  每月末执行                  │
│  输出：daily/月报_YYYY-MM.md                           │
│         + 季度末：daily/季报_YYYY-QN.md                │
└─────────────────────────────────────────────────────────┘
```

---

## 8. SOP 状态定义

| 状态 | 含义 | 可执行 | 需评审 |
|------|------|--------|--------|
| **active** | 正式发布使用 | ✅ | ✅ |
| **draft** | 草稿/讨论中 | ❌ | — |
| **obsolete** | 已作废 | ❌ | — |

---

## 9. SOP 评审周期

| 评审频率 | 覆盖范围 | 负责角色 |
|---------|---------|---------|
| 每季度 | 全部 active SOP | CEO |
| 每半年 | 全部 SOP（含 draft/obsolete） | CEO |
| 每年 | 全生态 SOP 体系评估 | CEO |

---

## 10. 新增 SOP 流程

1. 确认需求 → 填写 `inbox/SOP申请`
2. 分配编码（查本清单避免重号）
3. 按 `KB_SOP_001` 格式编制
4. 提交评审（owner + 相关方）
5. 批准发布 → 更新本清单

---

## 11. 缺失 SOP 登记

暂无。所有已识别 SOP 均已创建。

---

## 12. 变更记录

| 日期 | 版本 | 变更内容 | 变更人 |
|------|------|---------|--------|
| 2026-06-27 | v1.0.0 | 首版建立 SOP 索引 | King |
| 2026-06-27 | v2.0.0 | 新增 10 个 SOP/WF | King |
| 2026-06-27 | v3.0.0 | 新增自进化体系 SOP、Dreaming WF、自进化评估 WF、9 个 Agent Skills | King |
| 2026-06-27 | v4.0.0 | 新增复盘 SOP 体系（KB_SOP_005/010/011）、review 模板组（KB_WI_007/008）；SOP 总计 22 个，模板 8 个 | King |
| 2026-06-27 | v4.1.0 | 新增 llm-wiki-automation skill；新增 llm_wiki_daily + llm_wiki_weekly 两个 cron jobs | King |
| 2026-06-27 | v4.2.0 | 新增工作板（KB_SYS_DASHBOARD）：9个面板分区，实时状态，开放问题追踪 | King |
| 2026-06-27 | v4.3.0 | 工作板接入自动刷新：job_dashboard_refresh（每日08:00），Cron Job 总计13个 | King |
| 2026-06-27 | v4.4.0 | Agent协调工作流 → v2.0.0（Multi-Agent路由/会话交接/工作队列）；工作板 → v1.2.0（Section 3 工作队列面板）| King |
| 2026-06-27 | v4.5.0 | openclaw.json v2026.6.11 升级：新增7个System Skills（browser-automation/lobster-workflow/active-memory/taskflow/usage-tracking等）；修复飞书dmPolicy P0安全漏洞；移除vp1/vp2；启用active-memory/browser/commitments等原生插件 | King |

---

*version 4.5.0 · King · 2026-06-27*