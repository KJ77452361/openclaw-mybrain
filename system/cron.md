# Cron Job 清单

> 版本：v2.1.0
> 更新：2026-06-28
> 依据：STANDARDS.md v2.0.0 Cron Job 命名规范

---

## 命名规范（v2.0.0）

```
{domain}_{action}_{frequency}
  domain:     sys | knowledge | agent | channel | quality | memory
  action:     health | backup | sync | cleanup | check | audit | report | dreaming
  frequency:  daily | hourly | weekly | monthly

示例：
sys_health_daily          ✅ 标准
knowledge_backup_daily    ✅ 标准
sys_session_cleanup_daily ✅ 标准
quality_check_weekly      ✅ 标准
memory_log_compress       ✅ 标准（特殊 action）
```

**状态标识**
| 标识 | 含义 |
|------|------|
| ✅ | 符合 v2.0.0 规范 |
| 🔒 | 插件托管，名称不可修改 |
| ⚠️ | 历史遗留，暂保留（避免丢失运行历史） |

---

## 全部 Job 清单（v2.1.0）

| # | 名称（v2.0.0） | 域 | 频率 | 上次状态 | 说明 |
|---|---------------|---|------|---------|------|
| 1 | `sys_self_health_hourly` | sys | hourly | ✅ ok | 每 6 小时自检（cron list） |
| 2 | `sys_health_daily` | sys | daily | ✅ ok | 每日系统健康检查 |
| 3 | `sys_session_cleanup_daily` | sys | daily | ✅ ok | 每日 Session 清理（>7天） |
| 4 | `sys_report_daily_weekday` | sys | daily | ✅ ok | 工作日 08:00 报告生成 |
| 5 | `sys_dashboard_refresh_daily` | sys | daily | ✅ ok | 每日 08:00 工作板刷新 |
| 6 | `sys_audit_weekly` | sys | weekly | ✅ ok | 每周一 08:00 标准化审计 |
| 7 | `quality_check_weekly` | quality | weekly | ✅ ok | 每周质量检查 |
| 8 | `knowledge_llm_wiki_daily` | knowledge | daily | ✅ ok | 每日 21:00 LLM Wiki Auto-Ingest |
| 9 | `knowledge_backup_daily` | knowledge | daily | ✅ ok | 每日知识库备份 |
| 10 | `knowledge_sync_daily` | knowledge | daily | ✅ ok | 每日 MyBrain Git 同步 |
| 11 | `knowledge_audit_weekly` | knowledge | weekly | ✅ ok | 每周知识审计 |
| 12 | `memory_log_compress` | memory | daily | ⚠️ error | 每日日志压缩（>200行） |
| 13 | `llm_wiki_weekly` | knowledge | weekly | ⚠️ error | 每周 LLM Wiki Auto-Lint |
| 14 | `auth_store_backup` | sys | daily | ⚠️ null | 每日 Auth Store 备份 |
| 15 | `Memory Dreaming Promotion` | memory | daily | 🔒 ok | 插件托管（Dreaming System） |
| 16 | `monthly_full_audit` | sys | monthly | ⚠️ null | 每月全面审计 |

---

## 异常说明

| Job | 状态 | 原因 | 处置 |
|-----|------|------|------|
| `memory_log_compress` | ⚠️ error | Feishu 投递 target 未配置（已改 delivery=none） | 下次运行验证 |
| `llm_wiki_weekly` | ⚠️ error | Isolated session minimax auth 问题（已改 delivery=none） | 下次运行验证 |
| `auth_store_backup` | ⚠️ null | 从未运行（定时 03:10，首运行待触发） | 等待首次运行 |
| `monthly_full_audit` | ⚠️ null | 定时月末执行，尚未到达 | 正常 |

---

## Cron Expression 参考（Asia/Shanghai）

| Job | Expression | 运行时间 |
|-----|-----------|---------|
| `sys_self_health_hourly` | `0 */6 * * *` | 每 6 小时（stagger 5min） |
| `sys_health_daily` | `0 6 * * *` | 每日 06:00 |
| `sys_session_cleanup_daily` | `0 2 * * *` | 每日 02:00 |
| `sys_report_daily_weekday` | `0 8 * * 1-5` | 工作日 08:00 |
| `sys_dashboard_refresh_daily` | `0 8 * * *` | 每日 08:00 |
| `sys_audit_weekly` | `0 8 * * 1` | 每周一 08:00 |
| `quality_check_weekly` | `0 21 * * 0` | 每周日 21:00 |
| `knowledge_llm_wiki_daily` | `0 21 * * *` | 每日 21:00 |
| `knowledge_backup_daily` | `0 3 * * *` | 每日 03:00 |
| `knowledge_sync_daily` | `0 6 * * *` | 每日 06:00 |
| `knowledge_audit_weekly` | `0 22 * * 0` | 每周日 22:00 |
| `memory_log_compress` | `0 3 * * *` | 每日 03:00 |
| `llm_wiki_weekly` | `0 18 * * 5` | 每周五 18:00 |
| `auth_store_backup` | `0 3 1,15 * *` | 每月 1/15 日 03:00 |
| `monthly_full_audit` | `0 8 28 * *` | 每月 28 日 08:00 |

---

## 变更记录

| 日期 | 版本 | 变更 |
|------|------|------|
| 2026-06-27 | v1.0.0 | 初始建立，16 个 Job |
| 2026-06-28 | v2.0.0 | delivery.mode 修复（llm_wiki_weekly / memory_log_compress → none） |
| 2026-06-28 | v2.1.0 | 命名全面升级 v2.0.0（10 个 job 重命名）+ 创建本文件 |

*King · 2026-06-28*
