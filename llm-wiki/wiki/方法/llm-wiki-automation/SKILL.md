---
name: llm-wiki-automation
description: LLM Wiki 自动运转引擎。每日 21:30 自动 Ingest MyBrain 变更；每周五 18:00 自动 Lint。触发：cron 自动调度。
version: 1.0.0
metadata:
  openclaw:
    always: true
    category: knowledge
---

# LLM Wiki Automation — 自动运转引擎

让 LLM Wiki **自己跑起来**，无需人工触发。

---

## 核心设计

| 组件 | 频率 | 触发 | 职责 |
|------|------|------|------|
| **Auto-Ingest** | 每日 21:30 | cron | 扫描 MyBrain 变更 → 自动编译入 wiki |
| **Auto-Lint** | 每周五 18:00 | cron | Wiki 质量检查 → 自动修复 + 报告 |
| **Change Marker** | 实时 | 事件 | 跟踪上次处理时间戳 |

---

## Change Marker（变更追踪）

文件：`MyBrain/llm-wiki/.last_ingest`

```
{
  "last_ingest": "2026-06-27T21:30:00+08:00",
  "last_lint": "2026-06-27T21:30:00+08:00",
  "articles": {
    "生态概述": "v1.0.0",
    "agent体系": "v1.0.0",
    ...
  }
}
```

每次 Auto-Ingest/Lint 开始前读取，完成后更新。

---

## Auto-Ingest 流程（每日 21:30）

### 触发条件
cron job `llm_wiki_daily` 触发（isolated session）

### 输入
- `.last_ingest` — 上次处理时间戳
- `MyBrain/` — 知识库所有变更

### Step 1 — 变更扫描

扫描以下路径，筛选 `updated: > last_ingest` 或新建的文件：

| 路径 | 内容类型 | Wiki 动作 |
|------|---------|---------|
| `MyBrain/system/*.md` | SOP/WF/WI | 有变更 → 更新对应 wiki 文章 |
| `MyBrain/cards/*/` | 知识卡片 | 有新增 → 更新概念/方法/案例 wiki 文章 |
| `MyBrain/templates/*.md` | 模板 | 有新增 → 记录到 index |
| `MyBrain/projects/*.md` | 项目文档 | 有变更 → 更新生态 article |
| `MyBrain/llm-wiki/raw/` | raw 来源 | 有新增 → 自动 ingest 入 raw/（如果尚未入库）|
| `~/.openclaw/agents/*/AGENTS.md` | Agent 工作区 | 有变更 → 更新 agent体系 wiki 文章 |
| `~/.openclaw/agents/*/skills/*/SKILL.md` | Skills | 有新增/变更 → 更新 agent-skills wiki 文章 |

### Step 2 — 分级响应

**自动执行（低风险）**：
- raw/ 新来源文件 → 追加到 `raw/<topic>/`（未入库的）
- index.md 缺漏条目 → 自动补全
- log.md 追加记录

**AI 编译（需分析）**：
- 新 SOP（`system/*.md` 新增）→ 编译为 wiki 新文章
- 新 Agent/Skill → 更新 `agent体系.md` 和 `agent-skills.md`
- MyBrain 重大版本升级 → 更新 `生态概述.md`
- 新 kb_card → 更新 `mybrain知识库.md`

### Step 3 — Cascade Updates

1. 扫描 `wiki/openclaw-ecosystem/` 所有文章
2. 检查哪些文章的知识需要更新（基于 Step 1 的变更）
3. 刷新 `updated` 日期

### Step 4 — Post-Ingest

1. 更新 `wiki/index.md`（所有变更文章的 Updated 日期）
2. 追加 `wiki/log.md`：
   ```
   ## [YYYY-MM-DD] ingest | Auto-Ingest: <N> changes detected
   - Scanned: MyBrain/*, agents/*
   - Updated: <article1>, <article2>
   - Compiled: <new articles>
   ```
3. 更新 `.last_ingest` 时间戳

### 输出
- wiki 文章已更新
- 操作日志已追加
- 摘要报告写入 `MyBrain/daily/llm_wiki_digest_YYYY-MM-DD.md`

---

## Auto-Lint 流程（每周五 18:00）

### 触发条件
cron job `llm_wiki_weekly` 触发（isolated session）

### Step 1 — 确定性检查（自动修复）

| 检查项 | 策略 |
|--------|------|
| `wiki/index.md` vs 实际文件 | 缺 → 补 `(no summary)`；多 → 标记 `[MISSING]` |
| wiki 内链断裂 | 搜索同名词条：唯一 → 自动修复路径；多/无 → 报告 |
| raw 引用断裂 | 搜索 raw/ 同名：唯一 → 修复；多/无 → 报告 |
| See Also 缺漏 | 自动补充明显相关的交叉引用 |

### Step 2 — 启发式检查（报告）

| 检查项 | 报告内容 |
|--------|---------|
| 事实矛盾 | 跨文章矛盾点 + 来源对比 |
| 过时断言 | 被新文件取代的内容 |
| 孤立页面 | 无其他文章引入链接的页面 |
| 缺失交叉引用 | 相关但未互相链接的文章对 |
| 概念缺失 | 频繁提及但无专属页面的概念 |

### Step 3 — Post-Lint

1. 追加 `wiki/log.md`：
   ```
   ## [YYYY-MM-DD] lint | <N> issues found, <M> auto-fixed
   ```
2. 生成健康报告写入 `MyBrain/daily/llm_wiki_lint_YYYY-WXX.md`

---

## 目录结构（完整）

```
MyBrain/llm-wiki/
├── .last_ingest          ← 变更追踪（自动维护）
├── raw/                   # 原始来源
│   └── karpathy/
└── wiki/                  # LLM 维护的知识
    ├── index.md           # 全局索引（自动维护）
    ├── log.md            # 操作日志（append-only）
    ├── openclaw-ecosystem/  # 生态文章
    ├── 方法/llm-wiki/     # Skill 版方法论
    ├── 概念/              # 概念文章（未来扩展）
    ├── 案例/              # 案例文章（未来扩展）
    └── .last_lint         ← Lint 追踪（自动维护）
```

---

## 与其他系统的关系

| 系统 | 关系 | 整合点 |
|------|------|-------|
| **每日复盘** | 同一时间触发（21:30）| 每日复盘结果 → MyBrain → Auto-Ingest 原料 |
| **Dreaming System** | 夜间整合 | Dreaming 洞察 → Auto-Ingest 入库 |
| **knowledge_backup** | 备份范围包含 | llm-wiki/ 在备份范围内 |
| **weekly_knowledge_audit** | 互补 | Wiki 质量由 Auto-Lint 负责 |

---

## 触发条件总结

| 触发 | 来源 | 动作 |
|------|------|------|
| cron `llm_wiki_daily` | 每日 21:30 | Auto-Ingest |
| cron `llm_wiki_weekly` | 周五 18:00 | Auto-Lint |
| SOP/Agent/Skill 新增 | 人工/子 Agent | 自动检测并 Ingest |
| Dreaming 结果 | 夜间 job | Auto-Ingest |

---

*v1.0.0 · King · 2026-06-27 · LLM Wiki 自动运转体系*
