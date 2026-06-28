---
name: usage-tracking
description: OpenClaw Usage Tracking — 配额监控 / Token 统计 / 成本估算
version: 1.0.0
metadata:
  openclaw:
    emoji: 📊
    events: []
---

# Usage Tracking Skill

OpenClaw Usage Tracking 从提供商 usage endpoint 直接拉取数据，无估算，只有真实配额窗口。

## 显示位置

| 位置 | 内容 |
|------|------|
| `/status` | Session Token + 配额窗口 |
| `/usage` | 每响应 usage footer |
| `/usage cost` | 本地成本汇总 |
| `openclaw status --usage` | CLI 全提供商分解 |
| macOS menu bar | Usage section |

## Usage Footer (/usage full)

内置紧凑 footer，包含：
- Model + reasoning level
- Fast/slow mode
- Context window 使用量
- Input/output tokens
- Cache hit %
- Turn cost estimate

## 自定义模板

```json
{
  "messages": {
    "usageTemplate": "~/.openclaw/usage-footer.json"
  }
}
```

## 配额来源

| Provider | 认证方式 |
|----------|---------|
| MiniMax | OAuth → API Key fallback |
| Anthropic | OAuth auth profiles |
| OpenAI Codex | OAuth auth profiles |
| DeepSeek | API Key |

## 成本计算

`models.providers.*.models[].cost` 配置后自动计算：
```json
{
  "cost": {
    "input": 0.3,
    "output": 1.2,
    "cacheRead": 0.06,
    "cacheWrite": 0.375
  }
}
```

## CLI 查看

```bash
openclaw status --usage
openclaw channels list  # 含 usage 快照
```

## 日志成本汇总

```bash
openclaw config set commitments.enabled true
# /usage cost → 本地 OpenClaw session logs 聚合
```

---

*version 1.0.0 · King · 2026-06-27 · 基于 OpenClaw Usage Tracking v2026.6.11*
