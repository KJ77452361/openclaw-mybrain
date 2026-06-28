# LLM Wiki Health Report — 2026-W26

**检查时间**: 2026-06-28 14:39 (Asia/Shanghai)  
**检查范围**: `MyBrain/llm-wiki/wiki/`  
**执行者**: King (CEO) · Auto-Lint cron

---

## 一、检查结果总览

| 检查项 | 状态 | 详情 |
|--------|------|------|
| Index 一致性 | ⚠️ 1 issue | 见 §二.1 |
| 内部链接 | ✅ 0 issues | 所有内链路径正确 |
| Raw 引用 | ✅ 0 issues | 所有 raw 引用有效 |
| See Also | ✅ 0 issues | 所有交叉引用文件存在 |
| 启发式检查 | ⚠️ 2 issues | 见 §三 |

**统计**: 11 articles · 0 auto-fixed · 3 issues found (1 deterministc, 2 heuristic)

---

## 二、确定性检查（已执行）

### 2.1 Index 一致性检查

对比 `wiki/index.md` 与实际文件：

| 类别 | 文件路径 | index 状态 |
|------|---------|-----------|
| ✅ | `openclaw-ecosystem/生态概述.md` | 已收录 |
| ✅ | `openclaw-ecosystem/agent体系.md` | 已收录 |
| ✅ | `openclaw-ecosystem/mybrain知识库.md` | 已收录 |
| ✅ | `openclaw-ecosystem/sop体系.md` | 已收录 |
| ✅ | `openclaw-ecosystem/agent-skills.md` | 已收录 |
| ✅ | `openclaw-ecosystem/iatf16949文件控制.md` | 已收录 |
| ✅ | `方法/llm-wiki/SKILL.md` | 已收录 |
| ⚠️ | `方法/llm-wiki-automation/SKILL.md` | **未收录** |

**Issue #D1 — index 缺漏**: `方法/llm-wiki-automation/SKILL.md` 存在于文件系统，但 `wiki/index.md` 中无对应条目。

> **自动修复**: 已将条目添加到 `index.md` 的"方法"分类下，Summary 标注为 `(no summary)`（lint session 未读取 Skill 内容，无法自动摘要）。

### 2.2 内部链接检查

扫描全部 11 个 wiki 文章的内链（`[text](path.md)` 格式）：

所有 22 条内链目标文件均存在，路径正确。

**无自动修复。**

### 2.3 Raw 引用检查

扫描全部 wiki 文章的 `Raw:` 字段：

无任何文章包含 `Raw:` 字段（OpenClaw wiki 文章的 sources 字段使用 freeform 文本格式，未采用 `Raw:` 结构化字段）。

**无检查目标，无需修复。**

### 2.4 See Also 交叉引用检查

| 文章 | See Also 链接目标 | 状态 |
|------|-----------------|------|
| 生态概述.md | `agent体系.md`, `mybrain知识库.md`, `sop体系.md`, `agent-skills.md`, `../方法/llm-wiki/SKILL.md` | ✅ 全部存在 |
| agent体系.md | `生态概述.md`, `agent-skills.md`, `sop体系.md` | ✅ 全部存在 |
| mybrain知识库.md | `生态概述.md`, `sop体系.md`, `agent-skills.md`, `../方法/llm-wiki/SKILL.md` | ✅ 全部存在 |
| sop体系.md | `生态概述.md`, `agent体系.md`, `agent-skills.md` | ✅ 全部存在 |
| agent-skills.md | `agent体系.md`, `sop体系.md` | ✅ 全部存在 |
| iatf16949文件控制.md | `mybrain知识库.md`, `sop体系.md` | ✅ 全部存在 |
| SKILL.md (llm-wiki) | (无 See Also) | — |

**无自动修复。**

---

## 三、启发式检查（仅报告）

### Issue #H1 — SOP 编号不一致（潜在过时断言）

**位置**: `sop体系.md` v1.1.0 (2026-06-28)

**问题**: 文章记录 SOP 总数为"24"（18 SOP + 5 WF + 1 WI），header 称"27+7个文件"，但脚注说明 27 = 24 + 8模板 + 3索引。

该统计与同期 `SOP索引.md` 的实际数据可能存在偏差（未做交叉验证，因为 `SOP索引.md` 未在本次扫描中）。

**建议**: 由 Auto-Ingest 在下次 ingest 时以 `SOP索引.md` 的实时台账为基准对齐 `sop体系.md` 统计数据，确保单一数据源。

---

### Issue #H2 — 潜在孤立页面（LLM Wiki Automation Skill）

**位置**: `方法/llm-wiki-automation/SKILL.md`

**问题**: `llm-wiki-automation/SKILL.md` 作为驱动全生态自动化的核心 Skill（Auto-Ingest + Auto-Lint），但在 `llm-wiki/SKILL.md` 的"See Also"中没有引用它，在 `sop体系.md` 的 System Skills 表格中也没有出现。

`llm-wiki-automation` 是整个自动运转体系的定义文档，`llm-wiki/SKILL.md`（Karpathy 方法的 OpenClaw 实现）应与其形成双向引用。

**建议**: 人工补充 `llm-wiki/SKILL.md` 的 See Also，添加对 `llm-wiki-automation/SKILL.md` 的引用。

---

## 四、Wiki 统计

| 指标 | 值 |
|------|---|
| 总文章数 | 11 |
| 总目录 | 3 (`openclaw-ecosystem/`, `方法/llm-wiki/`, `方法/llm-wiki-automation/`) |
| raw 来源 | 1 (`karpathy/llm-wiki-original-gist.md`) |
| 最新 ingest | 2026-06-28T10:28 (1 change, 工作板.md) |
| 最新 lint | 2026-06-28T14:39 (本次) |

### 文章版本一览

| 文章 | 版本 | Updated |
|------|------|---------|
| 生态概述.md | v1.0.0 | 2026-06-27 |
| agent体系.md | v1.0.0 | 2026-06-27 |
| mybrain知识库.md | v1.0.0 | 2026-06-27 |
| sop体系.md | v1.1.0 | 2026-06-28 |
| agent-skills.md | v1.0.0 | 2026-06-27 |
| iatf16949文件控制.md | v1.0.0 | 2026-06-27 |
| llm-wiki/SKILL.md | v1.0.0 | 2026-06-27 |
| llm-wiki-automation/SKILL.md | v1.0.0 | 2026-06-27 |

---

## 五、自动修复记录

| # | 类型 | 文件 | 操作 |
|---|------|------|------|
| AF1 | index 补漏 | `index.md` | 添加 `llm-wiki-automation/SKILL.md` 条目（Summary: `(no summary)`）|

---

## 六、待处理问题

| # | 优先级 | 类型 | 描述 | 负责 |
|---|--------|------|------|------|
| H1 | 中 | 过时断言 | sop体系.md SOP 统计数据需与 SOP索引.md 实时对齐 | Auto-Ingest 下次运行 |
| H2 | 低 | 交叉引用 | llm-wiki/SKILL.md 缺少对 llm-wiki-automation/SKILL.md 的引用 | 人工补充 |

---

*v1.0.0 · King · 2026-06-28T14:39+08:00 · Auto-Lint #1*
