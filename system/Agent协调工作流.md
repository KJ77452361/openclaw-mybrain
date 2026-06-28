---
title: Agent 协调工作流
type: workflow
code: KB_WF_001_v1.0.0
version: 2.0.0
updated: 2026-06-27
tags: [workflow, multi-agent, coordination, handoff, work-queue, IATF16949]
regulatory: IATF16949:2016 §7.5
owner: CEO
status: active
---

# Agent 协调工作流

> 编码：KB_WF_001_v1.0.0 | 版本：2.0.0 | 2026-06-27 | 状态：active
> 监管要求：IATF16949:2016 §7.5 成文信息控制
> 适用范围：CEO 任务派发、多 Agent 协作、跨 Agent 会话交接

---

## 1. 目的

标准化三层机制：
1. **多 Agent 路由**：不同来源的消息自动路由到对应 Agent（Multi-Agent Binding）
2. **任务派发**：CEO 统一入口，按域派发给子 Agent 执行
3. **会话交接**：子 Agent 完成后将结果汇报回 CEO，形成完整工作流

确保：任务有边界、子 Agent 上下文隔离、结果可追溯、全程自动流转。

---

## 2. 核心原则

| 原则 | 说明 |
|------|------|
| **CEO 唯一入口** | 所有外部请求经 CEO 统一受理、分解、派发 |
| **域内自治** | 子 Agent 在授权范围内独立执行，无需等待 CEO 实时参与 |
| **会话隔离** | 各 Agent 有独立 session store，不暴露内部上下文 |
| **结果汇报** | 子 Agent 完成任务后，输出结构化结果回 CEO |
| **链路可溯** | 所有协作记录在 session history 中，可随时回溯 |

---

## 3. 多 Agent 路由体系（Multi-Agent Binding）

### 3.1 当前绑定结构

```
外部消息
    │
    ▼
CEO Agent（唯一入口 / 协调层）
    │
    ├── 飞书 DM ──────────────────────────→ CEO 主会话
    ├── WebChat ──────────────────────────→ CEO 主会话
    └── 定时任务 ─────────────────────────→ 各域 Agent 独立会话
```

### 3.2 域 Agent Session Key 规范

| Agent | Session Key 格式 | 说明 |
|-------|----------------|------|
| CEO | `agent:ceo:main` | 主会话 |
| CTO | `agent:cto:main` | CTO 域主会话 |
| COO | `agent:coo:main` | COO 域主会话 |
| CA | `agent:ca:main` | CA 域主会话 |
| 各执行域 Agent | `agent:{agentId}:main` | 独立会话 |

### 3.3 路由规则

```
用户消息 → CEO 主会话（入口）
    ↓ 判断类型
    ├── 协调类（分发/汇总）→ CEO 自行处理
    ├── 技术类（代码/架构/QA）→ sessions_send → agent:cto:main 或特定 Agent
    ├── 运营类（项目/质量/销售）→ sessions_send → agent:coo:main 或特定 Agent
    ├── 战略类（规划/合规）→ sessions_send → agent:ca:main
    └── 外部工具执行 → 通过 CEO 执行（Codex++ / 其他 CLI）
```

### 3.4 Binding 配置（如需扩展）

如需将特定飞书用户/群组直接路由到某域 Agent，在 Gateway Config 中配置：

```json
{
  "agents": {
    "list": [
      { "id": "cto", "workspace": "~/.openclaw/agents/cto" },
      { "id": "coo", "workspace": "~/.openclaw/agents/coo" }
    ]
  },
  "bindings": [
    {
      "agentId": "cto",
      "match": { "channel": "feishu", "peer": { "kind": "group", "id": "群ID" } }
    }
  ]
}
```

> 当前默认所有飞书 DM → CEO。扩展路由需修改 Gateway Config。

---

## 4. 任务派发（CEO → 子 Agent）

### 4.1 派发工具

| 工具 | 适用场景 |
|------|---------|
| `sessions_spawn(mode="run")` | 独立后台任务，无需等待结果 |
| `sessions_spawn` + `sessions_yield` | 需要等待子 Agent 结果 |
| `sessions_send` | 向指定 Agent 主会话发送消息并等待回复 |

### 4.2 任务包标准格式

每次派发必须包含：

| 字段 | 说明 | 必填 |
|------|------|------|
| `task` | 清晰的任务描述和目标 | ✅ |
| `output` | 明确的输出格式要求 | ✅ |
| `verify` | 验证方式（自检步骤） | ✅ |
| `context` | 需要的上下文范围 | 建议 |
| `scope` | 不应该做什么（边界） | 建议 |
| `taskName` | 任务别名（便于追踪） | 建议 |

**标准示例：**
```
task="分析以下技术问题并给出架构建议：{问题描述}
output=结构化报告：问题分析/方案选项/推荐方案/风险点
verify=方案需包含至少2个可比较的选项
scope=仅技术评估，不涉及业务决策"
```

### 4.3 taskName 命名规范

```
{domain}_{action}_{YYYYMMDD}
示例：cto_arch_review_20260627、coo_pm_report_20260627
```

---

## 5. 会话交接模式

### 5.1 模式一：直接交接（CEO ↔ 单 Agent）

```
CEO 主会话
    │ 收到用户请求
    ▼
sessions_send → agent:{domain}:main
    │ 任务描述 + output 格式 + verify 步骤
    ▼
子 Agent 执行 → 输出结构化结果
    │
    ▼
sessions_send → agent:ceo:main
    │ 结果汇报（符合 output 格式）
    ▼
CEO 整合 → 输出给用户
```

**适用**：单一域任务、明确输出标准

### 5.2 模式二：并行派发 + 汇总

```
CEO 主会话
    │ 收到复杂任务
    ▼
    ├── sessions_send → agent:cto:main（技术评估）
    ├── sessions_send → agent:coo:main（运营评估）
    └── sessions_send → agent:ca:main（战略评估）
    │
    ▼（同时执行，结果汇报回 CEO 主会话）
CEO 收集三个域的结果 → 整合 → 输出
```

**适用**：需要多域评估的综合性任务

### 5.3 模式三：子 Agent 独立执行（无等待）

```
CEO 主会话
    │ 派发任务
    ▼
sessions_spawn(mode="run")
    │ task="后台任务，不等待结果"
    ▼
子 Agent 在独立 session 执行
    │ 结果写入 MyBrain/daily/ 或对应卡片
    ▼
无需汇报回 CEO（已在约定位置输出）
```

**适用**：长期后台任务、日报生成、数据采集

---

## 6. 工作队列（Work Queue）模式

### 6.1 队列架构

```
外部请求（消息/Cron/Heartbeat）
    │
    ▼
CEO 主会话（入口队列）
    │
    ├── [P0 紧急] → CEO 立即处理
    ├── [技术任务] → sessions_send → CTO Agent 队列
    ├── [运营任务] → sessions_send → COO Agent 队列
    ├── [战略任务] → sessions_send → CA Agent 队列
    └── [定时任务] → 各域 Agent 独立 session 自动运行
```

### 6.2 定时任务的队列模式

| Cron Job | 派发到 | 汇报方式 |
|---------|-------|---------|
| sys_health_daily | 各域自检 | 结果回 CEO 主会话 |
| llm_wiki_daily | 各域变更同步 | 自动写入 wiki |
| weekly_quality_check | QM Agent | 结果回 CEO |
| knowledge_backup | 自运行 | 写入 daily/ |
| job_dashboard_refresh | 自运行 | 更新工作板 |

### 6.3 优先级处理

| 优先级 | 定义 | 处理方式 |
|--------|------|---------|
| P0 | 系统异常、故障、安全事件 | CEO 立即处理，不派发 |
| P1 | 高价值任务（战略/架构决策）| CEO + CA/CTO 并行 |
| P2 | 常规任务（日报、数据处理）| 派发到对应域 Agent |
| P3 | 低优先级（知识整理）| 后台队列，闲时处理 |

---

## 7. 结果收集与验证

### 7.1 收集方式

| 方式 | 工具 | 适用 |
|------|------|------|
| 同步等待 | `sessions_yield` | 需要立即使用子 Agent 结果 |
| 异步回调 | `sessions_send` 带 reply | 子 Agent 主动回报 |
| 写入约定位置 | 文件写入 | 长期任务、定时任务 |

### 7.2 验证标准

| 验证项 | 方法 |
|--------|------|
| 输出格式 | 对照 task 中的 output 要求 |
| 内容完整性 | 对照 verify 自检步骤 |
| 准确性 | 抽样检查或交叉验证 |
| 敏感信息 | 确认输出不含凭证、脱敏正确 |

### 7.3 判定结果

| 结果 | 后续动作 |
|------|---------|
| 验证通过 | 整合进入下一环节或输出 |
| 部分通过 | 反馈给原 Agent 要求修正 |
| 失败 | 记录失败原因，重试或降级处理 |

---

## 8. 错误处理

| 错误类型 | 处理方式 |
|---------|---------|
| 子 Agent 无响应 | 检查 `sessions_list`，更换 Agent 或降级 |
| 超时 | 增加 `timeoutSeconds`，重试 ≤3 次 |
| 结果格式不符 | 反馈给原 Agent 要求重新输出 |
| 任务部分失败 | 记录失败部分，继续处理成功部分 |
| 敏感信息泄露 | 立即停止，评估影响，触发告警 SOP |

---

## 9. 禁止事项

- **禁止**在群聊中暴露子 Agent 内部上下文
- **禁止**向子 Agent 传递完整凭证（脱敏后传递）
- **禁止**让子 Agent 直接向外部服务发送消息（统一由 CEO 路由）
- **禁止**在 sessionKey 未确认时发送消息
- **禁止**在任务描述中包含其他 Agent 的私人信息

---

## 10. 记录要求

| 记录 | 保存位置 | 保存期限 |
|------|---------|---------|
| 任务派发记录 | `daily/YYYY-MM-DD.md` | 永久 |
| 子 Agent 输出摘要 | `memory/daily/` 或相关卡片 | 永久 |
| 失败任务记录 | `inbox/缺陷单_YYYY-MM-DD.md` | 永久 |

---

## 11. 相关文件

| 文件 | 编码 |
|------|------|
| Agent 上线流程 | AG_SOP_001_v1.0.0 |
| 告警与升级路径 SOP | KB_SOP_007_v1.0.0 |
| 每日复盘 SOP | KB_SOP_005_v1.0.0 |

---

## 12. 变更记录

| 日期 | 版本 | 变更内容 | 变更人 |
|------|------|---------|--------|
| 2026-06-27 | v1.0.0 | 首版发布 | King |
| 2026-06-27 | v2.0.0 | 新增多 Agent 路由体系、3种会话交接模式、工作队列（Work Queue）模式、优先级处理 | King |

---

*version 2.0.0 · King · 2026-06-27*
