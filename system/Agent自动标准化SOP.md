---
title: Agent 自动标准化 SOP
type: system
code: AG_SOP_AUTO_001
version: 1.0.0
created: 2026-06-28
owner: CEO
status: active
---

# Agent 自动标准化 SOP

## 目的

确保新创建的 Agent 自动符合 v2.0.0 标准，无需手动补充。

## 新建 Agent 必填文件清单

| 文件 | 版本 | 说明 |
|------|------|------|
| AGENTS.md | v2.0.0 | 工作区说明 |
| SOUL.md | v2.0.0 | 人格定义（≥10行） |
| MEMORY.md | v2.0.0 | 记忆规范 |
| IDENTITY.md | v2.0.0 | 身份定义 |
| HEARTBEAT.md | — | 心跳配置 |
| USER.md | — | 使用者信息 |
| TOOLS.md | — | 工具说明 |
| STANDARDS.md | v1.0.0 | 标准化规范 |

## 目录结构

```
{agent_id}/
├── AGENTS.md      ← 必填
├── SOUL.md        ← 必填（≥10行）
├── MEMORY.md      ← 必填
├── IDENTITY.md    ← 必填
├── HEARTBEAT.md   ← 必填
├── USER.md        ← 必填
├── TOOLS.md       ← 必填
├── STANDARDS.md   ← 必填
├── knowledge/     ← 必有 README.md
│   └── README.md
└── memory/        ← 自动创建
    └── dreaming/  ← memory-core 插件自动生成
```

## 自动检查清单

创建后立即检查：
- [ ] AGENTS.md 包含 `> 版本：v2.0.0`
- [ ] SOUL.md 内容 ≥ 10 行
- [ ] 所有 8 个必填文件存在
- [ ] knowledge/README.md 存在
- [ ] 运行 `standard_audit_weekly` 验证

## 违规处理

发现违规 → 立即通知 CEO → 由 CEO 派发子 Agent 修复