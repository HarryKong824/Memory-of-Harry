# Memory Vault — 跨设备 / 多模块 / 人 + Agent 共用长期记忆库

> 设计目标：用「文件夹 + Markdown + git」做存储底座，Obsidian 做人端前端，Agent 知识沉淀 Loop 做自动读写。
> 人和 Agent 共用同一座仓库，单一事实源（single source of truth）。

## 三层角色
- **② 存储底座**：本仓库（git 版本化、跨设备 push/pull）。
- **① 人端**：用 Obsidian 打开本仓库作为 vault，写笔记、看图谱、用 Dataview 查询。
- **③ 自动化**：Agent 在任务开始「召回」、任务结束「沉淀」，读写同一批 `.md`。

## 目录结构
```
memory-vault/
├── README.md                  # 本文件
├── _agent/                    # Agent 专用区（人一般不直接手写）
│   ├── index.md               # 全局索引（Agent 任务开始先读它）
│   ├── recall-prompt.md       # 任务开始：召回模板
│   ├── precipitate-prompt.md  # 任务结束：沉淀模板
│   └── inbox/                 # 归属不明时的临时落点
├── modules/                   # 按模块分目录（人 & Agent 都写）
│   ├── career/
│   ├── tech/
│   └── project-a/
├── _templates/
│   └── learning-note.md       # Obsidian / Agent 共用的笔记骨架
└── .gitignore
```

## Frontmatter Schema（人和 Agent 都必须遵守）
每条记忆笔记头部：
```yaml
---
title: "一句话标题"
module: tech                # 对应 modules/ 下的子目录名
type: decision              # decision|mistake|pattern|code|fact|question
tags: [docker, windows]     # 检索关键词，越多越易被召回
created: 2026-08-16
updated: 2026-08-16
status: active              # active | archived
source: "任务/对话来源简述"
related: []                 # 关联笔记相对路径
---
```

## 人怎么用（Obsidian）
1. Obsidian → Open folder as vault → 选本仓库。
2. 装插件：Templates（指向 `_templates`）、Dataview（按 frontmatter 查询）、可选 Canvas。
3. 写笔记套 `_templates/learning-note.md`，存到对应 `modules/<module>/`。

## Agent 怎么用（Loop）
- 任务开始：跑 `_agent/recall-prompt.md`，把相关记忆注入上下文。
- 任务结束：跑 `_agent/precipitate-prompt.md`，萃取并追加新笔记 + 更新 index。
- 详见两个 prompt 文件。

## 冲突规避约定
- Agent 只**追加**新文件，或修改自己写的文件；**绝不改人的笔记**。
- 归属不明 → 落 `_agent/inbox/`，由人定期整理。
- 每条笔记必须带完整 frontmatter，否则召回失效。
- 一条笔记讲一个知识点（原子化），便于链接和检索。

## 跨设备同步
```
git add -A && git commit -m "..." && git push
# 另一台设备：
git pull
```
建议远端用 GitHub / Gitee 私有仓库。Obsidian 官方 Sync 可选，但 git 已够用且免费。
