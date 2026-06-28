---
name: browser-automation
description: OpenClaw 浏览器自动化 — 快照/稳定标签/失活引用恢复
version: 1.0.0
metadata:
  openclaw:
    emoji: 🌐
    events: []
---

# Browser Automation Skill

OpenClaw 托管浏览器自动化技能。使用独立 `openclaw` profile，与个人浏览器完全隔离。

## 快速开始

```bash
openclaw browser --browser-profile openclaw doctor
openclaw browser --browser-profile openclaw status
openclaw browser --browser-profile openclaw start
openclaw browser --browser-profile openclaw open https://example.com
openclaw browser --browser-profile openclaw snapshot
```

## 核心原则

- **独立 Profile**: `openclaw` profile 是专属隔离浏览器，不触碰个人浏览器
- **稳定引用**: 使用 `snapshot` 返回的 `ref` ID 进行 click/type 操作
- **复用标签**: 用 `tabId`/label 定位标签页，避免硬编码索引
- **先快照后操作**: 任何 UI 变更后重新 `snapshot`
- **失活引用恢复**: 遇到失活 ref → 重新 snapshot 一次 → 再操作
- **登录/2FA/Captcha**: 报告为需要人工介入，不要盲目猜测

## 工作流

### 基本操作循环

```
1. browser snapshot → 获取 UI tree + ref
2. 识别目标 ref
3. browser act --ref <id> --action click
4. browser snapshot → 确认变化
5. 如遇失活 ref → 重新 snapshot → 重试
```

### 截图模式

当主模型无视觉能力时，截图自动通过视觉模型描述：
- `tools.media.image.models` 配置视觉模型
- 截图以文本描述返回，无需改变工作流

## Profile 选择

| Profile | 用途 |
|---------|------|
| `openclaw` | 托管隔离浏览器（默认）|
| `user` | 附加到用户真实 Chrome（需签入）|

## 配置

```json
{
  "browser": {
    "enabled": true,
    "defaultProfile": "openclaw",
    "headless": false,
    "profiles": {
      "openclaw": { "cdpPort": 18800, "color": "#FF4500" }
    }
  }
}
```

## 失活引用处理

```
snapshot → ref 已失效
  → 重新 snapshot
  → 找到新的 ref
  → 重试操作
  → 仍失败 → 报告 "需要人工介入"
```

## 安全注意

- 浏览器控制仅 loopback 可访问
- 不在配置文件中硬编码远程 CDP URL tokens
- 优先使用环境变量或 secrets manager

---

*version 1.0.0 · King · 2026-06-27 · 基于 OpenClaw browser tool v2026.6.11*
