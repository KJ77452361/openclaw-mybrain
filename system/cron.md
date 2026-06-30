---
title: Cron Job 清单
type: system
code: KB_SYS_CRON
version: 2.3.0
updated: 2026-06-29
---

# Cron Job 清单

> 版本：v2.3.0
> 更新：2026-06-29
> 依据：STANDARDS.md v1.4.0 Cron Job 命名规范

---

## 命名规范（v2.0.0）

```
{domain}_{action}_{frequency}
  domain:     sys | knowledge | quality | llm
  frequency:  hourly | daily | weekly | monthly

示例：
sys_health_daily          ✅ 标准
knowledge_backup_daily    ✅ 标准
sys_session_cleanup_daily ✅ 标准
quality_check_weekly      ✅ 标准
```

**状态标识**
| 标识 | 含义 |
|------|------|
| ✅ | 符合 v2.0.0 规范 |
| 🔒 | 插件托管，名称由插件控制，不可手动修改 |
| ⚠️ | 历史遗留，暂保留 |
| ⏳ | 已禁用或新建未运行 |

---

## 全部 Job 清单（v2.3.0 · 共 19 个 · 活跃 18 个 · 禁用 1 个）

| # | 名称 | 域 | 频率 | 调度（CST） | 上次状态 | 说明 |
|---|------|----|------|-----------|---------|------|
| 1 | `sys_self_health_hourly` | sys | hourly | 每6h | ✅ ok | 每6小时自检 |
| 2 | `knowledge_wiki_daily` | knowledge | daily | 06:00 | ✅ ok | 每日 Wiki 同步 |
| 3 | `sys_session_cleanup_daily` | sys | daily | 02:00 | ⏳ null | 每日 Session 清理 |
| 4 | `knowledge_backup_daily` | knowledge | daily | 03:00 | ✅ ok | 每日知识库备份 |
| 5 | `Memory Dreaming Promotion` | — | daily | 03:00 | 🔒 ok | 插件托管（memory-core） |
| 6 | `Memory_Dreaming_Promotion` | — | daily | 03:00 | ⛔ 已禁用 | 与插件版重复，用户创建，已禁用 |
| 7 | `sys_memory_compress_daily` | sys | daily | 03:00 | ✅ ok | 每日记忆压缩 |
| 8 | `sys_auth_backup_daily` | sys | daily | 03:30 | ✅ ok | 每日 Auth Store 备份 |
| 9 | `knowledge_sync_daily` | knowledge | daily | 06:00 | ✅ ok | 每日 MyBrain Git 同步 |
| 10 | `sys_report_daily_weekday` | sys | daily | 08:00（工作日）| ✅ ok | 工作日早报告 |
| 11 | `sys_dashboard_refresh_daily` | sys | daily | 08:00 | ✅ ok | 每日工作板刷新 |
| 12 | `sys_health_daily` | sys | daily | 09:00 | ✅ ok | 每日健康检查 |
| 13 | `sys_audit_monthly` | sys | monthly | 月末21:00 | ⏳ null | 月末全面审计 |
| 14 | `quality_check_weekly` | quality | weekly | 周三08:00 | ⏳ null | 每周质量检查 |
| 15 | `llm_wiki_weekly` | llm | weekly | 周五18:00 | ✅ ok | 每周 LLM Wiki Lint |
| 16 | `sys_workboard_archive_weekly` | sys | weekly | 周日03:00 | ⏳ null | 每周工作板归档 |
| 17 | `sys_tasks_maintenance_weekly` | sys | weekly | 周一02:00 | ✅ ok | 每周任务维护 |
| 18 | `sys_audit_weekly` | sys | weekly | 周一08:00 | ✅ ok | 每周标准化审计 |
| 19 | `knowledge_audit_weekly` | knowledge | weekly | 周一08:00 | ⚠️ error | 审计超时→已修复600s |

---

## 异常追踪

| Job | 问题 | 修复 | 状态 |
|-----|------|------|------|
| `knowledge_audit_weekly` | 连续4次 timeout（300s不够）| timeout→600s | ⏳ 下次运行验证 |
| `Memory_Dreaming_Promotion` | 与插件版重复 | 已禁用 | ✅ |

---

## 变更记录

| 日期 | 版本 | 变更 |
|------|------|------|
| 2026-06-27 | v1.0.0 | 初始建立，16 个 Job |
| 2026-06-28 | v2.0.0 | delivery.mode 修复 |
| 2026-06-28 | v2.1.0 | 命名全面升级 v2.0.0 |
| 2026-06-29 | v2.2.0 | 新增 job 命名修正 |
| 2026-06-29 | v2.3.0 | Dreaming 重复处理 + 18 jobs 全面对齐 |

*King · 2026-06-29*