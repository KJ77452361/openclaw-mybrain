---
title: Skill上线流程SOP
type: SOP
code: AG_SOP_002_v1.0.0
version: 1.0.0
updated: 2026-06-27
tags: [SOP, skill, lifecycle, onboarding, IATF16949]
regulatory: IATF16949:2016 §7.5
owner: CEO
status: active
---

# Skill 上线流程 SOP

> 编码：AG_SOP_002_v1.0.0 | 版本：1.0.0 | 2026-06-27 | 状态：active
> 监管要求：IATF16949:2016 §7.5 成文信息控制
> 适用范围：所有新 Skill 向 OpenClaw 生态发布的流程

## 1. 目的

规范 Skill 的创建、评审、发布全流程，确保：
- 每个 Skill 有标准化的 SKILL.md 文件
- Skill 的功能边界和使用条件清晰
- Skill 可被正确加载和维护
- 上线后有完整的受控文件记录

## 2. 适用范围

所有向 OpenClaw 生态发布的 Skill。

## 3. Skill 存放位置

| Skill 类型 | 存放路径 |
|-----------|---------|
| CEO 核心 Skill | `~/.openclaw/agents/ceo/skills/{name}/SKILL.md` |
| Agent 专属 Skill | `~/.openclaw/agents/{agent-id}/skills/{name}/SKILL.md` |
| 全局共享 Skill | `~/.openclaw/skills/{name}/SKILL.md` |

## 4. SKILL.md 标准格式

```yaml
---
name: {skill-name}           # 英文、数字、下划线
description: {一句话描述}   # ≤160 字节
version: x.y.z
metadata:
  openclaw:
    requires: []             # 依赖的 skills（可选）
    tags: []                # 标签（可选）
---

# {Skill 名称}

## 描述
<!-- 详细描述：做什么、什么时候用、怎么用 -->

## 前置条件
<!-- 使用此 Skill 前必须满足的条件 -->

## 使用方法
<!-- 具体调用方式和参数说明 -->

## 输出
<!-- 执行结果的格式和含义 -->

## 限制
<!-- 使用边界和注意事项 -->

## 示例
<!-- 至少 2 个使用示例 -->
```

## 5. 上线流程

```
需求确认 ──→ SKILL.md 编制 ──→ 内部测试 ──→ 评审 ──→ 发布 ──→ 受控登记
```

### Phase 1：需求确认

| 步骤 | 操作 | 产出 |
|------|------|------|
| 1.1 | 确认 Skill 名称（kebab-case） | name |
| 1.2 | 确认功能范围 | 功能描述 |
| 1.3 | 确认存放位置 | 目标路径 |
| 1.4 | 确认依赖（是否依赖其他 Skill） | requires |
| 1.5 | 检查是否已有相似 Skill | 重用决策 |

### Phase 2：SKILL.md 编制

**执行者：** Skill 开发者

| 步骤 | 操作 |
|------|------|
| 2.1 | 创建 `skills/{name}/SKILL.md` |
| 2.2 | 填写 frontmatter（name/description/version 必填） |
| 2.3 | 编写 6 个标准章节 |
| 2.4 | 编写至少 2 个可执行示例 |
| 2.5 | 自查格式（对照 §4） |

### Phase 3：内部测试

**测试环境：** 隔离 session（非生产）

| 测试项 | 判定 |
|--------|------|
| Skill 能被正确加载 | 加载无报错 |
| 示例可执行 | 输出符合预期 |
| 边界条件 | 异常输入处理正确 |
| 与依赖 Skill 配合 | 无冲突 |

### Phase 4：上线评审

**评审者：** CEO 或域负责人

| 检查项 | 标准 |
|--------|------|
| name 唯一 | 不与现有 Skill 重名 |
| description 存在 | 非空，≤160 字节 |
| version 语义化 | 符合 semver |
| 示例可执行 | 至少 2 个 |
| 无敏感信息 | 不含真实凭证 |
| 存放位置正确 | 符合 §3 |

### Phase 5：发布

**执行者：** CEO

| 步骤 | 操作 |
|------|------|
| 5.1 | 将 Skill 状态标记为 active（如适用） |
| 5.2 | 更新 `system/SOP索引.md`，新增 Skill 条目 |
| 5.3 | 更新 `system/受控文件清单.md` |
| 5.4 | 在 `knowledge/cards/` 中创建关联卡片（可选） |
| 5.5 | 在 `daily/YYYY-MM-DD.md` 记录 Skill 上线 |

### Phase 6：受控登记

**执行者：** CEO

| 步骤 | 操作 |
|------|------|
| 6.1 | 登记至 SOP索引.md |
| 6.2 | 记录 Skill 路径、版本、创建日期 |
| 6.3 | 确认 Skill 可被目标 Agent 加载 |

## 6. Skill 撤销

见 AG_SOP_003（Agent 下线流程 SOP）。

## 7. 记录要求

| 记录 | 保存位置 | 保存期限 |
|------|---------|---------|
| 上线记录 | `daily/YYYY-MM-DD.md` | 永久 |
| Skill 列表 | `system/SOP索引.md` | 永久 |

## 8. 相关文件

| 文件 | 编码 |
|------|------|
| Agent 上线流程 SOP | AG_SOP_001_v1.0.0 |
| Agent 下线流程 SOP | AG_SOP_003_v1.0.0 |
| 技能开发指南（WI） | KB_WI_006_v1.0.0 |
| 文件变更控制 SOP | KB_SOP_003_v1.0.0 |
| SOP索引 | KB_SOP_000_v1.0.0 |

## 9. 变更记录

| 日期 | 版本 | 变更内容 | 变更人 |
|------|------|---------|--------|
| 2026-06-27 | v1.0.0 | 首版发布 | King |

---

*version 1.0.0 · King · 2026-06-27*
