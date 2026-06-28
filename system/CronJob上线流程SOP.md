---
title: CronJob上线流程SOP
type: SOP
code: AG_WI_001_v1.0.0
version: 1.0.0
updated: 2026-06-27
tags: [SOP, cron, job, lifecycle, IATF16949]
regulatory: IATF16949:2016 §7.5
owner: CEO
status: active
---

# Cron Job 上线流程 SOP

> 编码：AG_WI_001_v1.0.0 | 版本：1.0.0 | 2026-06-27 | 状态：active
> 监管要求：IATF16949:2016 §7.5 成文信息控制
> 适用范围：所有新建 Cron Job 的定义、评审、上线、监控、下线

## 1. 目的

规范 Cron Job 从需求到上线的全流程，确保：
- 每个 Cron Job 有明确的定义和验收标准
- 命名符合 v2.0.0 规范（`{domain}_{action}_{frequency}`）
- 上线前经过评审
- 上线后有监控和告警
- 下线有规范的回收流程

## 2. 适用范围

所有在 `cron/definitions/` 下创建的 Cron Job 定义文件。

## 3. Cron Job 分类

| 类型 | 说明 | 管理方 |
|------|------|--------|
| 系统级 | sys_health_daily、session_cleanup 等 | CEO |
| 知识库级 | knowledge_backup、weekly_knowledge_audit 等 | CEO |
| 业务级 | daily_mybrain_sync、daily_report 等 | 对应 Agent |
| 插件托管 | Dreaming System 等 | 插件自身（不可改名） |

## 4. 上线流程

```
需求确认 ──→ 定义编写 ──→ 评审 ──→ 上线 ──→ 监控配置 ──→ 受控登记
```

### Phase 1：需求确认

| 步骤 | 操作 | 产出 |
|------|------|------|
| 1.1 | 确认触发场景和频率 | 需求描述 |
| 1.2 | 确认 job 类型（系统/知识库/业务） | 类型定义 |
| 1.3 | 确认 sessionTarget（isolated/current/main） | 绑定模式 |
| 1.4 | 确认交付方式（announce/webhook/none） | 交付模式 |

### Phase 2：定义编写

**按 `cron/definitions/{name}.md` 模板编写：**

```markdown
> 版本：v1.0.0
> 状态：✅|⚠️历史|🔒插件
> 标准名：`{name}`

# {Job 名称}

## 基本信息

| 字段 | 值 |
|------|---|
| Job ID | {jobId} |
| 类型 | sys/knowledge/agent/channel |
| 执行频率 | {cron表达式} |
| sessionTarget | {isolated/current/main} |
| 负责人 | {owner} |

## 功能描述
{描述这个 Job 做什么}

## 输入
<!-- 执行所需的输入参数 -->

## 输出
<!-- 执行的输出产物 -->

## 错误处理
{失败时的处理方式}

## 执行记录
<!-- 上线后由系统自动填写 -->
```

**命名规范（v2.0.0）：**
```
{domain}_{action}_{frequency}
```

| 字段 | 合法值 |
|------|--------|
| domain | sys, knowledge, agent, channel, quality, memory |
| action | health, backup, sync, cleanup, check, audit, report, dreaming |
| frequency | daily, hourly, weekly, monthly |

**示例：**
- `sys_health_daily` ✅
- `daily_report_weekday` ✅
- `healthCheck` ❌（不符合格式）

### Phase 3：评审

**评审内容：**

| 检查项 | 标准 |
|--------|------|
| 命名规范 | 符合 `{domain}_{action}_{frequency}` |
| cron 表达式 | 合法，且与描述频率一致 |
| sessionTarget | 正确（推荐 isolated） |
| 定义完整性 | 含基本信息/功能描述/错误处理 |
| 重复性 | 不与现有 Job 重名 |
| 权限 | sessionTarget/main 需要确认是否必要 |

**评审者：** CEO 或域负责人

### Phase 4：上线

**执行者：** CEO

| 步骤 | 操作 |
|------|------|
| 4.1 | 使用 cron add 命令创建 Job |
| 4.2 | 在 definitions/ 目录确认定义文件存在 |
| 4.3 | 记录 jobId |
| 4.4 | 配置 failureAlert（如为 P1+ Job） |
| 4.5 | 在 `knowledge/system/cron.md` 新增条目 |

### Phase 5：监控配置

| 步骤 | 操作 |
|------|------|
| 5.1 | 设置 failureAlert（连续失败 ≥2 次触发） |
| 5.2 | 配置 delivery 方式（announce/none） |
| 5.3 | 记录到 `cron/runs/README.md` |
| 5.4 | 首次执行后验证输出是否符合预期 |

### Phase 6：受控登记

| 步骤 | 操作 |
|------|------|
| 6.1 | 更新 `system/受控文件清单.md`（如适用） |
| 6.2 | 更新 `knowledge/system/cron.md` |
| 6.3 | 在 `daily/YYYY-MM-DD.md` 记录上线事件 |

## 5. 下线流程

### 5.1 下线条件

- Job 目的已实现，不再需要
- Job 有 bug 且无法修复
- 被新 Job 替代

### 5.2 下线步骤

1. 使用 `cron remove {jobId}` 删除 Job
2. 将定义文件加 `_obsolete_{日期}` 后缀，移入 `memory/archive/cron/`
3. 更新 `knowledge/system/cron.md`（标注为 obsolete）
4. 在 `daily/` 记录下线事件

## 6. 记录要求

| 记录 | 保存位置 | 保存期限 |
|------|---------|---------|
| Job 定义 | `cron/definitions/{name}.md` | 永久 |
| 执行历史 | `cron/runs/` | 1 年 |
| 上线记录 | `daily/YYYY-MM-DD.md` | 永久 |
| 下线记录 | `daily/YYYY-MM-DD.md` | 永久 |

## 7. 相关文件

| 文件 | 编码 |
|------|------|
| Cron Job（概念卡） | KB_CAR_概念_002 |
| Cron Job 命名规范v2（方法卡） | KB_CAR_方法_001 |
| 告警与升级路径 SOP | KB_SOP_007_v1.0.0 |
| 受控文件清单 | KB_REC_001_v1.0.0 |

## 8. 变更记录

| 日期 | 版本 | 变更内容 | 变更人 |
|------|------|---------|--------|
| 2026-06-27 | v1.0.0 | 首版发布 | King |

---

*version 1.0.0 · King · 2026-06-27*
