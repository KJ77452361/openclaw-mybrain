---
name: llm-wiki
description: Use when building or maintaining a personal LLM-powered knowledge base. Triggers: ingesting sources into a wiki, querying wiki knowledge, linting wiki quality, 'add to wiki', 'what do I know about', or any mention of 'LLM wiki' or 'Karpathy wiki'.
version: 1.0.0
metadata:
  openclaw:
    requires: []
    category: knowledge
---

# LLM Wiki — Karpathy 方法实践

基于 Karpathy LLM Wiki 模式（2026-04-04）的 OpenClaw 实现。

## 核心差异：Wiki vs RAG

| | RAG | LLM Wiki |
|--|-----|---------|
| 知识状态 | 每次查询从零检索 | **持久化积累**，编译一次，常驻最新 |
| 知识组织 | 原始文档堆砌 | 结构化页面（实体/概念/对比），互相交叉引用 |
| 维护 | 无积累，每次重推导 | LLM 自动更新所有受影响页面 |
| 适用场景 | 简单检索 | 需要综合多文档的深度分析 |

> "LLM 是全职管理员，不是临时工。" — Karpathy

## 目录结构

```
MyBrain/llm-wiki/
├── raw/                    # 原始来源（不可变）
│   └── <topic>/           # 按主题分类
│       └── YYYY-MM-DD-slug.md
├── wiki/                   # LLM 维护的知识页面
│   ├── index.md            # 全局目录（每次 ingest 更新）
│   ├── log.md              # 操作日志（append-only）
│   └── <topic>/            # 主题子目录
│       └── <article>.md    # 知识文章
└── SKILL.md               # 本文件（schema 层）
```

## 三层架构

| 层级 | 目录 | 说明 |
|------|------|------|
| 原始层 | `raw/` | 文章/论文/网页/文档，只读不修改 |
| 知识层 | `wiki/` | LLM 生成并维护的结构化页面 |
| 模式层 | `SKILL.md` | 定义工作流、格式规范、Schema |

---

## 操作一：Ingest（新知识入库）

### 触发条件
用户说"添加到 wiki"、"摄入 XXX"、"学习这个材料"

### 步骤

**Step 1 — Fetch（raw/）**

1. 获取源内容（web_fetch / read / 用户粘贴）
2. 选择主题目录：已有则复用，新主题则创建 `raw/<topic>/`
3. 保存为 `raw/<topic>/YYYY-MM-DD-descriptive-slug.md`
   - slug：标题转 kebab-case，最多 60 字符
   - 无发布日期：文件名省略日期前缀
   - 文件重名：加数字后缀 `-2`
   - 头部元数据：source URL、collected date、published date

**Step 2 — Compile（wiki/）**

内容归属判断：
- **与现有文章核心论点相同** → 合并入该文章，更新 Sources，更新受影响章节
- **新概念** → 在最相关主题目录创建新文章（文件名用概念名，不用源文件名）
- **跨多个主题** → 放在最相关目录，添加 See Also 交叉引用

**冲突处理**：新源与现有内容矛盾 → 在两处都标注分歧并互相链接。

**Step 3 — Cascade Updates（级联更新）**

单一来源可能影响 10-15 个 wiki 页面：
1. 扫描同目录文章，找到受新源影响的内容
2. 扫描 `wiki/index.md` 其他主题目录的相关概念
3. 更新每个受影响文件的 Updated 日期

**Step 4 — Post-Ingest**

更新 `wiki/index.md`：添加/更新词条，Updated 日期反映知识内容变更时间。
追加到 `wiki/log.md`：
```
## [YYYY-MM-DD] ingest | <主文章标题>
- Updated: <级联更新的文章标题>
```

---

## 操作二：Query（查询知识库）

### 触发条件
用户说"关于 X 我知道些什么"、"总结 Y 相关的一切"、"对比 A 和 B"

### 步骤

1. 读取 `wiki/index.md` 定位相关页面
2. 读取相关文章，综合答案
3. 优先使用 wiki 内容而非自身训练知识；引用时附 Markdown 链接
4. 输出答案（对话形式，不写文件，除非用户要求存档）

### 归档（用户要求时）

用户明确要求"存档这个答案"时：
1. 写为新 wiki 页面（用 archive 模板）
2. Sources：引用 wiki 文章的 Markdown 链接
3. 无 Raw 字段（内容来自综合答案，非原始材料）
4. 文件名反映查询主题（如 `transformer-architectures-overview.md`）
5. 放在最相关主题目录
6. 永远创建新页面，不合并入现有文章
7. 更新 `wiki/index.md`，Summary 前加 `[Archived]`
8. 追加 `wiki/log.md`：`## [YYYY-MM-DD] query | Archived: <page title>`

---

## 操作三：Lint（质量检查）

### 触发条件
用户说"检查 wiki 健康度"、"lint wiki"

### 确定性检查（自动修复）

| 检查项 | 修复策略 |
|--------|---------|
| index 中有但文件不存在 | 标记为 `[MISSING]`，不删除 |
| 文件存在但 index 无记录 | 添加条目，Summary 留空 `(no summary)` |
| 内部链接目标不存在 | 搜索同名词条：唯一匹配→自动修复；多个/无→报告用户 |
| Raw 引用链接失效 | 搜索 raw/ 同名文件：唯一匹配→自动修复；多个/无→报告用户 |
| See Also 缺失 | 补充明显相关的交叉引用，移除已删除文件链接 |

### 启发式检查（仅报告）

- 跨页面的事实矛盾
- 被新源取代的过时断言
- 源之间有分歧但缺少冲突标注
- 孤立页面（无其他页面引入链接）
- 缺少跨主题引用
- 频繁提及但无专属页面的概念
- 引用源文章已大幅更新的归档页

### Post-Lint

追加 `wiki/log.md`：`## [YYYY-MM-DD] lint | <N> issues found, <M> auto-fixed`

---

## 格式规范

### 文章模板（wiki/）

```markdown
---
title: <文章标题>
topic: <主题>
type: article
status: active
version: 1.0.0
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: <来源信息>
tags: [<tag1>, <tag2>]
---

# <标题>

## 摘要
<50字以内概括本文核心>

## 正文

## 关键洞察
- <洞察1>
- <洞察2>

## 来源
<引用格式：作者/机构 + 日期>

## See Also
- [<相关文章>](<相对路径>)
```

### index.md 格式

```markdown
---
title: Knowledge Base Index
type: index
status: active
version: 1.0.0
---

# Knowledge Base Index

## <主题>

| 文章 | Summary | Updated |
|------|---------|---------|
| [文章名](wiki/<topic>/<file>.md) | 一句话概括 | YYYY-MM-DD |

## 无主题（orphan pages）
```

### log.md 格式

```markdown
---
title: Wiki Log
type: log
status: active
version: 1.0.0
---

# Wiki Log

## [YYYY-MM-DD] ingest | <标题>
## [YYYY-MM-DD] query | Archived: <标题>
## [YYYY-MM-DD] lint | <N> issues found, <M> auto-fixed
```

---

## 与 MyBrain 现有体系的关系

| 概念 | MyBrain | LLM Wiki |
|------|---------|---------|
| 原始材料 | `inbox/` | `raw/` |
| 知识卡片 | `cards/` | `wiki/`（article 类型）|
| 概念卡 | `cards/概念/` | `wiki/`（concept 类型）|
| 案例卡 | `cards/案例/` | `wiki/`（entity 类型）|
| 每日笔记 | `daily/` | `log.md` 的一部分 |
| 索引 | `INDEX.md`（system） | `wiki/index.md` |
| SOP | `system/` | 独立维护，引用到 sources |

**LLM Wiki 与 MyBrain 的关系**：
- `raw/` = inbox/ 的正式版（inbox 仍是临时过渡区）
- `wiki/` 与 `cards/` 并存：cards 是轻量概念卡，wiki 是深度知识文章
- 两者互补，不替代

---

*v1.0.0 · King · 2026-06-27 · 基于 Karpathy 2026-04-04 gist + Astro-Han 实现*
