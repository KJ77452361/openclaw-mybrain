---
name: active-memory
description: Active Memory 插件 — 回复前内存召回 blocking sub-agent
version: 1.0.0
metadata:
  openclaw:
    emoji: 🧩
    events: []
---

# Active Memory Skill

Active Memory 是一个 blocking memory sub-agent，在主回复**之前**运行，将相关记忆注入对话。

## 工作原理

```
用户消息 → Active Memory Blocking Sub-Agent → 记忆召回 → 隐藏上下文 → 主回复
```

## 与 MEMORY.md 的区别

| 特性 | Active Memory | MEMORY.md |
|------|--------------|-----------|
| 触发 | 每次对话自动运行 | 显式调用 memory_search |
| 内容 | 最近偏好/习惯 | 长期结构性记忆 |
| 时机 | 回复**前**注入 | 任意时刻召回 |
| 存储 | 临时（可配置持久化）| 持久文件 |

## 配置

```json
{
  "plugins": {
    "entries": {
      "active-memory": {
        "enabled": true,
        "config": {
          "enabled": true,
          "agents": ["ceo", "coo", "cto", "ca"],
          "allowedChatTypes": ["direct"],
          "modelFallback": "minimax/MiniMax-M2.7",
          "queryMode": "recent",
          "promptStyle": "balanced",
          "timeoutMs": 15000,
          "maxSummaryChars": 220,
          "persistTranscripts": false,
          "logging": true
        }
      }
    }
  }
}
```

## Query Mode

| Mode | 上下文量 | 延迟 | 适用场景 |
|------|---------|------|---------|
| `message` | 仅最新消息 | 最快 | 快速偏好召回 |
| `recent` | 最新+少量对话尾 | 中等 | 平衡速度与上下文 |
| `full` | 完整对话 | 较慢 | 需要最强召回质量 |

## Prompt Style

| Style | 召回倾向 |
|-------|---------|
| `strict` | 最低召回，仅明显匹配 |
| `balanced` | 平衡（默认）|
| `contextual` | 最强上下文友好 |
| `recall-heavy` | 更愿意召回 |
| `precision-heavy` | 高度精确 |
| `preference-only` | 仅偏好/习惯 |

## 调试

```
/verbose on   → 显示 Active Memory 状态行
/trace on     → 显示 Active Memory Debug 信息
/trace raw    → 显示原始隐藏上下文
```

## 使用场景

✅ 适合 Active Memory：
- 稳定偏好（喜欢的食物/工作方式）
-  recurring habits（每周一开会）
- 长期上下文（项目背景）
- 需要自然浮现的记忆

❌ 不适合 Active Memory：
- 自动化任务
- 内部 worker agent
- 一次性 API 任务

---

*version 1.0.0 · King · 2026-06-27 · 基于 OpenClaw active-memory plugin v2026.6.11*
