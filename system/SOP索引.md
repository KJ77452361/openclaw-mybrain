---
title: SOP索引
type: system
code: KB_SOP_000_v1.0.0
version: 4.7.0
created: 2026-06-27
updated: 2026-06-28
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

### 运营域（OP）— 2 个 SOP

| 编码 | 名称 | 版本 | 状态 | 适用范围 |
|------|------|------|------|---------|
| OP_SOP_001 | 工作板（Workboard）使用规范 | v1.0.0 | active | Workboard 任务管理 |
| OP_SOP_002 | 任务优先级定义 | v1.0.0 | active | P0/P1/P2/P3 优先级标准 |

---

## 3. SOP 统计

| 域 | SOP | WF | WI | 合计 |
|----|-----|----|----|------|
| KB（知识库） | 11 | 5 | 0 | **16** |
| AG（Agent） | 3 | 0 | 1 | **4** |
| QL（质量） | 3 | 0 | 0 | **3** |
| OP（运营） | 2 | 0 | 0 | **2** |
| **合计** | **19** | **5** | **1** | **25** |

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

## 6. CEO Skills（已部署）— 5 个核心 + 22 个社区 Skills

### CEO 核心技能

| Skill | 版本 | 说明 |
|-------|------|------|
| agent-coordinator | v1.0.0 | 多 Agent 协调 |
| cron-manager | v1.0.0 | Cron Job 生命周期管理 |
| knowledge-manager | v1.0.0 | 知识库维护 |
| memory-manager | v1.0.0 | 长时记忆管理 |
| standard-auditor | v1.0.0 | 标准化审计 |

### 社区 Skills（ClawHub）

| Skill | 来源 | 功能 |
|-------|------|------|
| agent-team-orchestration | arminnaimi | 多智能体团队编排 |
| self-improving-agent | pskoett | 自我改进（错误记录+持续学习） |
| self-improving-proactive-agent | yueyanc | 主动自我改进 |
| memory-pipeline | openclaw | 记忆+性能系统 |
| openclaw-super-healthcheck | openclaw | 系统健康检查 |
| task | openclaw | 任务管理 |
| copywriting | community | 文案写作 |
| lead-enrichment | community | 销售线索富化 |
| project-management-2 | community | 项目管理 |
| ciso | community | 安全审计 |
| productivity | community | 生产力框架 |
| taskflow | openclaw | 多步骤任务流 |
| clawhub | openclaw | ClawHub CLI 工具 |
| github | openclaw | GitHub CLI |
| gh-issues | openclaw | GitHub Issues 自动化 |
| healthcheck | openclaw | OpenClaw 健康检查 |
| browser-automation | openclaw | 浏览器自动化 |
| canvas | openclaw | Canvas 渲染 |
| spike | openclaw | 快速原型验证 |
| weather | openclaw | 天气查询 |
| meme-maker | openclaw | Meme 生成 |
| diagram-maker | openclaw | 图表生成 |
| feishu-doc | openclaw | 飞书文档 |
| feishu-wiki | openclaw | 飞书 Wiki |
| feishu-drive | openclaw | 飞书云盘 |
| feishu-perm | openclaw | 飞书权限 |

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
| 2026-06-28 | v4.6.0 | Cron Job 全面升级 v2.1.0：10个Job重命名符合v2.0.0规范；修复llm_wiki_weekly/memory_log_compress delivery=none；新增system/cron.md完整清单；MyBrain拆分为独立git仓库；openclaw-super-healthcheck添加YAML frontmatter | King |
| 2026-06-28 | v4.7.0 | 修复 SOP 索引 Agent Skills 表格（旧版废弃agent移除）；新增 OP 域（运营）2个SOP；更新 CEO Skills 清单为社区 Skills 架构；安全加固：channels.feishu.dmPolicy→allowlist，allowInsecureAuth→false | King |

---

*version 4.7.0 · King · 2026-06-28*