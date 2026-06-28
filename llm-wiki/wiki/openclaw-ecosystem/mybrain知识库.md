---
title: MyBrain 知识库体系
topic: openclaw-ecosystem
type: article
status: active
version: 1.0.0
created: 2026-06-27
updated: 2026-06-27
sources: King · MyBrain · 2026-06-27
tags: [mybrain, knowledge-base, LYT, IATF16949]
---

# MyBrain 知识库体系

## 定位

OpenClaw 的**第二大脑**，基于 LYT Framework 构建，采用 IATF16949 §7.5 成文信息控制规范。

路径：`~/.openclaw/workspace/MyBrain/`

---

## 目录结构

```
MyBrain/
├── inbox/               # 外来文件临时入口（非受控）
├── raw/                 # 原始来源（待处理）
├── cards/               # 知识卡片（轻量）
│   ├── 概念/            # 6张概念卡
│   ├── 方法/            # 4张方法卡
│   └── 案例/            # 3张案例卡
├── daily/               # 每日笔记（自动归档）
├── projects/            # 项目文档
├── templates/           # 8个文档模板
├── system/              # 受控文件区（SOP/WF/WI/INDEX）
│   ├── SOP索引.md       # 唯一SOP索引
│   ├── 受控文件清单.md  # IATF16949台账
│   └── *.md            # 27个SOP/WF/WI
├── llm-wiki/            # LLM Wiki（Karpathy方法）
│   ├── raw/             # 原始来源
│   └── wiki/            # LLM维护的知识文章
└── memory/
    ├── archive/         # 历史归档
    └── daily/           # 每日归档
```

---

## IATF16949 四阶文件体系

| 阶 | 定位 | 对应 MyBrain 目录 |
|----|------|-----------------|
| 一阶 | 质量手册 | `STANDARDS.md`（根目录 + CEO目录）|
| 二阶 | 程序文件 | `system/`（27个SOP/WF/WI）|
| 三阶 | 作业文件 | `cards/`（13张知识卡片）|
| 四阶 | 记录表单 | `templates/`（8个模板）+ `daily/` |

---

## 核心文件（受控台账）

### System 受控文件（27个）

| 类型 | 数量 | 编码范围 |
|------|------|---------|
| KB SOP | 11个 | KB_SOP_001~011 |
| KB WF | 5个 | KB_WF_001~005 |
| AG SOP | 3个 | AG_SOP_001~003 |
| AG WI | 1个 | AG_WI_001 |
| QL SOP | 3个 | QL_SOP_001~003 |
| 系统索引 | 3个 | INDEX/受控文件清单/导航 |

### 模板文件（8个）

| 模板 | 编码 |
|------|------|
| 每日笔记 | KB_WI_001 |
| 卡片模板 | KB_WI_002 |
| 项目文档模板 | KB_WI_003 |
| Cron报告模板 | KB_WI_004 |
| 会议记录模板 | KB_WI_005 |
| 技能开发指南 | KB_WI_006 |
| 周复盘模板 | KB_WI_007 |
| 月复盘模板 | KB_WI_008 |

### 知识卡片（13张）

| 类型 | 数量 | 编码 |
|------|------|------|
| 概念卡 | 6张 | KB_CAR_概念_001~006 |
| 方法卡 | 4张 | KB_CAR_方法_001~004 |
| 案例卡 | 3张 | KB_CAR_案例_001~003 |

---

## LLM Wiki 与 MyBrain 的关系

| 维度 | MyBrain cards | LLM Wiki |
|------|--------------|---------|
| 定位 | 轻量概念卡，快速查阅 | 深度知识文章，交叉引用 |
| 维护 | 手动 + SOP驱动 | LLM自动维护（Ingest/Query/Lint）|
| 组织 | LYT框架（概念/方法/案例）| Karpathy三层架构（raw/wiki/schema）|
| 来源 | 人工整理 | LLM编译原始材料生成 |

**互补关系**：
- `raw/` = inbox/ 的正式来源库
- `cards/` 保留（轻量速查），`wiki/` 新建（深度积累）
- 两者共同构成完整知识体系

---

## 受控文件规范

| 字段 | 要求 |
|------|------|
| frontmatter | `---` 第一行，含 title/type/version/status |
| title | 非空 |
| version | `v{x}.{y}.{z}` 格式 |
| status | `active`（正式）/ `draft`（草稿）/ `obsolete`（作废）|
| footer | 最后一行以 `*version` 开头 |

**变更分级**：
- `patch`：错别字 → v{x}.{y}.{z+1}
- `minor`：内容<20% → v{x}.{y+1}.{0}
- `major`：重写≥20% → v{x+1}.{0}.{0}

---

## See Also

- [OpenClaw生态概述](生态概述.md)
- [SOP与工作流体系](sop体系.md)
- [Agent Skills](agent-skills.md)
- [Karpathy LLM Wiki](../方法/llm-wiki/SKILL.md)
