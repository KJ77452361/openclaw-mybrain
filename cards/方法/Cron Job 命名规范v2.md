---
title: Cron Job 命名规范v2
type: 方法
code: KB_CAR_方法_001_v1.0.0
version: 1.0.0
created: 2026-06-27
updated: 2026-06-27
tags: [方法, cron, naming, convention]
regulatory: IATF16949:2016 §7.5
owner: CEO
status: active
---

# Cron Job 命名规范v2

> 编码：KB_CAR_方法_001_v1.0.0 | 版本：1.0.0 | 2026-06-27 | 状态：active
> 监管要求：IATF16949:2016 §7.5 成文信息控制
> 详细流程见：`system/CronJob上线流程SOP.md`（AG_WI_001_v1.0.0）

## 摘要

定义 Cron Job 的标准化命名格式，确保所有 Job 可快速识别其域、功能和执行频率。

## 命名格式（v2.0.0）

```
{domain}_{action}_{frequency}
```

| 字段 | 合法值 | 说明 |
|------|--------|------|
| domain | sys, knowledge, agent, channel, quality, memory | 所属域 |
| action | health, backup, sync, cleanup, check, audit, report, dreaming | 执行动作 |
| frequency | daily, hourly, weekly, monthly | 执行频率 |

**命名字符：** 仅使用小写字母和下划线（`_`），不得使用其他字符。

## 正确命名示例

| 正确命名 | 域 | 动作 | 频率 | 说明 |
|---------|-----|------|------|------|
| `sys_health_daily` | sys | health | daily | 系统每日健康检查 |
| `knowledge_backup_weekly` | knowledge | backup | weekly | 知识库每周备份 |
| `agent_daily_sync` | agent | sync | daily | Agent 每日同步 |
| `quality_audit_monthly` | quality | audit | monthly | 质量每月审计 |
| `dreaming_daily` | — | dreaming | daily | 每日梦境处理 |
| `channel_feishu_health_daily` | channel | health | daily | 飞书通道每日健康 |

## 错误命名示例

| 错误命名 | 错误原因 | 正确命名 |
|---------|---------|---------|
| `daily_report_weekday` | 违反格式（daily 是 frequency，不是 action）| `report_daily_weekday` |
| `healthCheck` | 使用驼峰而非小写+下划线 | `health_check_daily` |
| `sys.health.daily` | 使用点号而非下划线 | `sys_health_daily` |
| `check` | 缺失 domain 和 frequency | `quality_check_daily` |

## Job ID vs Job 名称

| 字段 | 说明 | 示例 |
|------|------|------|
| **Job 名称** | 用户可读的命名 | `sys_health_daily` |
| **Job ID** | 系统分配的 UUID | `job_abc123def456` |

**注意：** 插件托管的 Job（如 Dreaming System）使用插件分配的名称，不可改名。

## 相关文件

| 文件 | 编码 |
|------|------|
| Cron Job 上线流程 SOP | AG_WI_001_v1.0.0 |
| 告警与升级路径 SOP | KB_SOP_007_v1.0.0 |
| Cron Job（概念卡） | KB_CAR_概念_002 |

---

*version 1.0.0 · King · 2026-06-27*