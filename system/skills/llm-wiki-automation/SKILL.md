---
name: llm-wiki-automation
description: LLM Wiki 自动化引擎 — Auto-Ingest + Auto-Lint
version: 1.0.0
metadata:
  openclaw:
    emoji: 🔄
    events: []
---

# LLM Wiki Automation Skill

LLM Wiki 自动运转引擎：Auto-Ingest + Auto-Lint 双引擎驱动。

## 双引擎架构

### Auto-Ingest Engine

自动检测 MyBrain 变更并编译入 wiki：

**触发条件**：
- 每日 21:30 自动运行
- 检测 `raw/` 新增/更新文件
- 检测 `cards/` 重大变更
- 检测 `system/` SOP/模板变更

**工作流程**：
```
1. 扫描 raw/ + cards/ + system/ 变更
2. 读取变更文件
3. LLM 编译为 wiki/ 文章
4. 更新 index.md + log.md
5. 更新 .last_ingest
```

### Auto-Lint Engine

每周健康检查：

**每周五 18:00** 执行：
```
1. 扫描 wiki/ 所有文章
2. 检测断链（引用失效）
3. 检测矛盾（同一主题不同结论）
4. 修复可自动修复的断链
5. 报告无法自动修复的问题
6. 输出健康报告
```

## 变更追踪

`.last_ingest` 格式：
```json
{
  "lastIngestAt": "2026-06-27T21:30:00+08:00",
  "articlesVersion": 7,
  "sources": [
    { "path": "raw/karpathy/...", "hash": "..." }
  ]
}
```

## 与手动操作的关系

| 操作 | 手动 | 自动 |
|------|------|------|
| Ingest | ✅ llm-wiki skill | ✅ auto-ingest engine |
| Lint | ✅ llm-wiki skill | ✅ auto-lint engine |
| Query | ✅ llm-wiki skill | ❌ |

## 健康报告模板

```markdown
# LLM Wiki 健康报告 — YYYY-MM-DD

## 统计
- 文章总数：X
- 断链数：X（已修复 X，待修复 X）
- 矛盾检测：X

## 详细
[问题列表]

## 建议
[优化建议]
```

---

*version 1.0.0 · King · 2026-06-27 · OpenClaw 原生 Cron 驱动*
