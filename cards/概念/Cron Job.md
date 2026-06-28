---
title: Cron Job
type: 概念
version: 1.0.0
created: 2026-06-27
updated: 2026-06-27
tags: [#Cron, #定时任务]
status: active
---
# Cron Job

> 类型：概念
> 创建：2026-06-27
> 标签：#CronJob

## 摘要

OpenClaw 定时任务系统，在指定时间自动触发 Agent 行为。

## 核心要点

- 命名规范：{{domain}}_{{action}}_{{frequency}}
- sessionTarget：isolated（独立）/ current（当前会话）
- 插件 Job 由插件托管，不可改名
- 健康检查 → sys_health_daily，每日 09:00 执行

## 相关概念

- [[自进化系统]]
- [[知识卡片（LYT框架）]]
- [[第二大脑]]

---

*2026-06-27 · King*

*version 1.0.0 · King · 2026-06-27*
