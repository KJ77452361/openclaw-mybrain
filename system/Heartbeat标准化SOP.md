---
title: Heartbeat标准化SOP
type: SOP
code: KB_SOP_009_v1.0.0
version: 1.0.0
updated: 2026-06-27
tags: [SOP, heartbeat, monitoring, IATF16949]
regulatory: IATF16949:2016 §7.5
owner: CEO
status: active
---

# Heartbeat 标准化 SOP

> 编码：KB_SOP_009_v1.0.0 | 版本：1.0.0 | 2026-06-27 | 状态：active
> 监管要求：IATF16949:2016 §7.5 成文信息控制
> 适用范围：OpenClaw Heartbeat 的配置、执行、监控全流程

## 1. 目的

规范 OpenClaw Heartbeat 的运行标准，确保：
- Heartbeat 的检查内容有明确定义
- 检查频率合理（不浪费资源，也不遗漏问题）
- 异常能被及时发现并触发告警
- 检查结果有记录、可追溯

## 2. Heartbeat 定义

Heartbeat 是 OpenClaw 的轻量级定期检查机制，与 Cron Job 的区别：

| 维度 | Heartbeat | Cron Job |
|------|----------|---------|
| 触发方式 | 心跳 poll（30min） | 定时（精确 cron） |
| 用途 | 快速状态检查 | 复杂任务执行 |
| 阻塞 | 非阻塞 | 非阻塞 |
| 典型场景 | 收件箱/日历/通知 | 备份/健康检查/报告 |

## 3. Heartbeat 检查清单

每次 Heartbeat 触发时，依次执行以下检查：

### 3.1 系统基础检查

| 检查项 | 方法 | 异常阈值 | 触发告警 |
|--------|------|---------|---------|
| OpenClaw Gateway | `gateway status` | 连接失败 | P1 |
| Session 活跃度 | `sessions_list` | 无活跃 session >8h | P3 |
| 磁盘空间 | `df -h` | `< 1GB` | P2 |
| Cron Job 运行状态 | `cron status` | scheduler 停止 | P1 |

### 3.2 知识库检查

| 检查项 | 方法 | 异常阈值 | 触发告警 |
|--------|------|---------|---------|
| 受控文件合规率 | frontmatter 扫描 | `< 95%` | P2 |
| 新增待处理文件 | 检查 inbox/ | inbox > 10 项 | P3 |
| 知识库备份 | 检查 backup 状态 | 备份失败 | P1 |

### 3.3 质量检查

| 检查项 | 方法 | 异常阈值 | 触发告警 |
|--------|------|---------|---------|
| 敏感信息 | grep 扫描 | 任何泄露 | P0 |
| frontmatter 完整率 | 扫描 | `< 100%` | P2 |

### 3.4 消息通道检查

| 检查项 | 方法 | 异常阈值 | 触发告警 |
|--------|------|---------|---------|
| 飞书发送失败 | 检查最近发送 | 失败率 >10% | P2 |
| 待处理消息 | 检查飞书 inbox | > 5 条未处理 | P3 |

## 4. 执行规范

### 4.1 触发频率

| 场景 | 频率 | 说明 |
|------|------|------|
| 标准检查 | 每 30 分钟 | 默认配置 |
| 夜间（23:00-08:00） | 降低频率 | 每 2 小时 |
| 紧急事件后 | 提高频率 | 每 10 分钟（持续 1 小时） |

### 4.2 执行时间

- 每次 Heartbeat 执行时间应 < 60 秒
- 如检查耗时 > 60 秒，分批执行（分到多次 heartbeat）

### 4.3 记录规范

每次 Heartbeat 完成后，在 `memory/heartbeat-state.json` 记录：

```json
{
  "lastCheck": "2026-06-27T13:30:00+08:00",
  "results": {
    "gateway": "ok",
    "sessions": "ok",
    "mybrain": "ok",
    "quality": "ok",
    "feishu": "ok"
  },
  "alerts": [],
  "nextScheduled": "2026-06-27T14:00:00+08:00"
}
```

## 5. 异常处理

| 检查结果 | 处理方式 |
|---------|---------|
| P0 异常 | 立即触发告警（KB_SOP_007），不等下一个 heartbeat |
| P1 异常 | 触发 KB_SOP_007 P1 告警流程 |
| P2 异常 | 记录到 `inbox/问题单`，下次 heartbeat 复查 |
| P3 提示 | 记录到 heartbeat-state.json，下次 routine 处理 |

## 6. Heartbeat 配置

### 6.1 HEARTBEAT.md 格式

在 CEO 工作区维护 `HEARTBEAT.md`：

```markdown
# Heartbeat Check List

## 控制

- 频率：每 30 分钟（默认）
- 夜间：每 2 小时（23:00-08:00）
- 超时：60 秒

## 检查清单

### 系统
- [x] Gateway 连接
- [x] Session 活跃度
- [x] 磁盘空间

### 知识库
- [x] 受控文件合规率
- [x] inbox 待处理

### 质量
- [x] 敏感信息扫描
- [x] frontmatter 完整率

### 消息通道
- [x] 飞书发送状态
- [x] 待处理消息
```

## 7. 禁止事项

- **禁止**在 Heartbeat 中执行复杂任务（用 Cron Job）
- **禁止**Heartbeat 检查结果不记录
- **禁止**Heartbeat 超时影响主 session 响应
- **禁止**在 Heartbeat 中发送外部消息（只检查，不主动发）

## 8. 记录要求

| 记录 | 保存位置 | 保存期限 |
|------|---------|---------|
| 检查状态 | `memory/heartbeat-state.json` | 7 天 |
| 异常记录 | `inbox/问题单_YYYY-MM-DD.md` | 永久 |
| 告警记录 | `daily/YYYY-MM-DD.md` | 永久 |

## 9. 相关文件

| 文件 | 编码 |
|------|------|
| 告警与升级路径 SOP | KB_SOP_007_v1.0.0 |
| 敏感信息脱敏规范 | QL_SOP_002_v1.0.0 |
| Cron Job 上线流程 SOP | AG_WI_001_v1.0.0 |

## 10. 变更记录

| 日期 | 版本 | 变更内容 | 变更人 |
|------|------|---------|--------|
| 2026-06-27 | v1.0.0 | 首版发布 | King |

---

*version 1.0.0 · King · 2026-06-27*
