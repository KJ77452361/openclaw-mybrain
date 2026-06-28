---
title: Agent 协调协议
type: 概念
version: 1.0.0
created: 2026-06-27
updated: 2026-06-27
tags: [#概念, #多Agent, #协作]
status: active
---
# Agent 协调协议

> 类型：概念
> 创建：2026-06-27
> 标签：#Agent协调协议

## 摘要

通过 sessions_spawn / sessions_send / sessions_yield 实现多 Agent 协作。

## 核心要点

- sessions_spawn：派发独立子任务（推荐）
- sessions_send：向指定 Agent 发送消息
- sessions_yield：等待子 Agent 完成
- 禁止在群聊中暴露子 Agent 私人上下文

## 相关概念

- [[自进化系统]]
- [[知识卡片（LYT框架）]]
- [[第二大脑]]

---

*2026-06-27 · King*

*version 1.0.0 · King · 2026-06-27*
