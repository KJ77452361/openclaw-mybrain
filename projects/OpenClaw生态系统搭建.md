---
title: OpenClaw 生态系统搭建
type: project
code: KB_PROJ_001
version: 2.1.0
created: 2026-06-27
updated: 2026-06-27
tags: [project, openclaw]
status: active
owner: CEO
lastReview: 2026-06-27
dashboardRef: MyBrain/system/工作板.md
---
# OpenClaw 生态系统搭建

> 项目
> 创建：2026-06-27
> 状态: 🟢 进行中（重大升级完成）

## 项目目标

搭建完整的 OpenClaw 第二大脑系统，实现全系统自进化。

## 背景

用户在 Windows 机器上运行 OpenClaw，通过 WSL2 + Linux 开发环境，连接 Windows 本地 AI 模型（Hermes/MiniMax）和 Codex++ 编程工具。

## 架构设计

```
CEO（唯一入口）
├─ COO ─── PM / QM / RS / SA
├─ CTO ─── PE / HE / SE / architect / fullstack_engineer / qa_engineer
├─ CA ─── 合规总监
└─ main ── 系统入口

业务执行层（workspace/agents/）
└─ sales_executive / sales_operator / sales_orchestrator / translator_zh
```

## 关键路径

- Codex++ 执行：`ssh -i ~/.ssh/id_ed25519 Administrator@172.21.112.1 "cmd /c codex_task.bat \"prompt\""`
- Codex++ 结果：`C:\Users\Administrator\codex_result.txt`
- Hermes 重启：SSH→taskkill→java -jar

## 外部工具

| 工具 | 定位 | 状态 |
|------|------|------|
| 豆包 | Windows 桌面 AI | ✅ |
| Hermes | 本地大模型（深度分析） | ✅ |
| Codex++ | 开发者编程工具 | ✅ |

## 当前版本

**v2.1.0**（2026-06-27 夜）— OpenClaw v2026.6.11 全面升级完成

实时状态面板：`MyBrain/system/工作板.md`

## 里程碑

| 日期 | 里程碑 | 版本 | 状态 |
|------|---------|------|------|
| 2026-06-26 | 基础架构搭建完成 | v0.9.0 | ✅ |
| 2026-06-27 AM | MyBrain 标准化，13 张卡片 | v1.0.0 | ✅ |
| 2026-06-27 PM-A | IATF16949 §7.5 引入，受控文件台账建立 | v2.0.0 | ✅ |
| 2026-06-27 PM-B | 架构重组 v3.0.0，VP1/VP2 撤销，Skills 体系 | v3.0.0 | ✅ |
| 2026-06-27 晚A | Karpathy LLM Wiki 引入（raw + wiki + Skill）| v3.1.0 | ✅ |
| 2026-06-27 晚B | LLM Wiki 全生态首次 Ingest（7 篇生态文章）| v3.2.0 | ✅ |
| 2026-06-27 晚C | LLM Wiki 自动运转体系建立（2 个 cron jobs）| v3.2.0 | ✅ |
| 2026-06-27 夜 | **工作板建立**，llm_wiki_daily/weekly 上线 | v2.0.0 | ✅ |
| 2026-06-27 🔴夜 | **OpenClaw v2026.6.11 全面升级** | v2.1.0 | ✅ |

### v2.1.0 升级内容（2026-06-27）

**配置升级（openclaw.json v2026.6.11）**：
- ✅ 移除 vp1/vp2（v3.0.0 归档）
- ✅ 修复 `channels.feishu.dmPolicy=open` → `allowlist`（P0 安全漏洞）
- ✅ 启用 `active-memory` 插件（blocking memory sub-agent）
- ✅ 启用 `browser` 插件（OpenClaw 托管浏览器）
- ✅ 启用 `commitments`（推断式跟进记忆）
- ✅ 配置 `compaction.memoryFlush`（会话压缩前内存 flush）
- ✅ 配置 `subagents.maxConcurrent=8`，`maxSpawnDepth=2`（Orchestrator 模式）
- ✅ 启用 `hooks.internal`（session-memory/command-logger/compaction-notifier）
- ✅ 配置 `skills.load.watch` + extraDirs
- ✅ 配置 `auth.cooldowns`（billing/overloaded/retry 策略）
- ✅ `tools.alsoAllow: sessions_spawn, sessions_yield, subagents, browser, lobster`

**System Skills（7个新增）**：
- ✅ `browser-automation` — 快照/稳定标签/失活引用恢复
- ✅ `lobster-workflow` — JSON 管道/审批门/可恢复 token
- ✅ `active-memory` — 回复前 blocking memory sub-agent
- ✅ `karpathy-llm-wiki` — 持久化编译型知识积累
- ✅ `llm-wiki-automation` — Auto-Ingest + Auto-Lint 引擎
- ✅ `taskflow` — 持久化多步骤流程编排（OpenClaw 原生）
- ✅ `usage-tracking` — 配额监控/Token 统计/成本估算

**Hook Bundles（已有）**：
- ✅ `session-memory` — `/new`/`/reset` 时保存会话上下文
- ✅ `command-logger` — 所有命令事件审计
- ✅ `compaction-notifier` — 压缩开始/完成时可见通知
- ✅ `boot-md` — Gateway 启动时运行 BOOT.md
- ✅ `bootstrap-extra-files` — 额外 bootstrap 文件注入

## 下一步

| 优先级 | 任务 | 负责 |
|--------|------|------|
| ~~🔴 P0~~ | ~~飞书 dmPolicy=open → allowlist~~ | ~~CEO~~ ✅ |
| 🟡 P1 | 飞书 failure destination 配置 | CEO |
| 🟡 P1 | MyBrain/external/ 目录建立 | CEO |
| 🟡 P1 | Model Failover 配置（fallbacks 增强）| CEO |
| 🟡 P1 | Active Memory + MEMORY.md 联动机制 | CEO |
| 🟢 P2 | LLM Wiki 引入外部来源（nanoGPT/llm.c等）| CEO |
| 🟢 P2 | ClawHub 发布工作流建设 | CEO |
| 🟢 P2 | Codex++ 集成到 cron | CEO |
| 🟢 P2 | MiniMax MiniMax-M2.7 详细用量分析 | CEO |

---

---

*version 2.1.0 · King · 2026-06-27 · OpenClaw v2026.6.11 全面升级*
