---
name: karpathy-llm-wiki
description: Karpathy LLM Wiki 方法论 — 持久化编译型知识积累
version: 1.0.0
metadata:
  openclaw:
    emoji: 🧠
    events: []
---

# Karpathy LLM Wiki Skill

基于 Andrej Karpathy 的 LLM Wiki 方法论：持久化编译型知识积累 vs RAG 临时检索。

## 核心理念

**LLM Wiki = 编译，RAG = 解释**

每次 RAG 重新推导相同答案（浪费）；LLM Wiki 知识积累复利（知识直接持有）。

## 三层架构

```
raw/     → 不可变来源（Gist/文档/代码）
  ↓ Ingest
wiki/    → LLM 编译知识（结构化文章）
  ↓ Lint
schema/  → SKILL.md 规范
```

## 三操作

### 1. Ingest（入库）

将原始来源编译为 wiki 文章：
- 读取 raw/ 中的原始文档
- LLM 提取核心概念、关系、方法
- 输出为结构化 markdown 到 wiki/
- 更新 .last_ingest 时间戳

### 2. Query（查询）

知识召回：
- 先查 wiki/ 已有知识
- wiki/ 无 → raw/ 推导
- 始终返回编译后的知识

### 3. Lint（校验）

质量维护：
- 检查断链
- 检测矛盾
- 更新 index/log

## 目录结构

```
llm-wiki/
├── raw/              # 不可变来源
│   └── karpathy/     # Karpathy 相关
├── wiki/             # LLM 编译知识
│   ├── openclaw-ecosystem/
│   └── 方法/
├── 方法/
│   └── llm-wiki/
│       └── SKILL.md  # 本技能
├── .last_ingest      # 变更追踪
├── index.md          # 总索引
└── log.md            # 变更日志
```

## 与 MyBrain 的互补

| 层 | MyBrain | LLM Wiki |
|----|---------|---------|
| 入口 | inbox/ raw/ | raw/ |
| 深度 | cards/ 轻量 | wiki/ 重量 |
| 结构 | 原子卡片 | 编译文章 |

两者并存：raw/ = inbox/ 正式版；wiki/ = cards/ 深度版

## 自动化

- `llm_wiki_daily`: 每日 21:30，扫描变更 → AI 编译 → 更新
- `llm_wiki_weekly`: 每周五 18:00，断链修复 + 矛盾检测 + 健康报告

---

*version 1.0.0 · King · 2026-06-27 · Karpathy LLM Wiki 方法论*
