---
title: "Agent 接手本记忆库指南（HANDOFF）"
module: self
type: handbook
tags: [handoff, agent, memory-vault, protocol]
created: 2026-08-16
updated: 2026-08-16
status: active
source: "用户要求整合给未来 Agent 的使用提示词"
related: [README.md, _agent/recall-prompt.md, _agent/precipitate-prompt.md, SELF.md]
---

# Agent 接手本记忆库指南（HANDOFF）

> 把下面整段（或本文件）直接粘贴给**任何新会话 / 其他 AI 工具**，它就能正确接手这个长期记忆库。

---

## 这是什么

`Memory-of-Harry` 是一个**纯 Markdown + git** 的跨设备长期记忆库（GitHub 私有仓库）。
- **人**用 Obsidian 主动读写、看图谱。
- **Agent** 自动读写：任务前召回、任务后沉淀。
- 单一事实源：人和 Agent 共用同一批 `.md`，靠约定好的目录与 frontmatter 互不踩踏。

**仓库**：`https://github.com/HarryKong824/Memory-of-Harry`

## 目录结构（遵守，勿乱建）

```
README.md                      # 架构 + frontmatter schema + 冲突规避约定（先读）
SELF.md                        # 用户「主人公」档案（身份/主线/阶段/习惯）
_agent/
  recall-prompt.md             # 任务前·召回（只读）
  precipitate-prompt.md        # 任务后·沉淀（萃取→落库→更新索引）
  index.md                     # 全局索引表，人和 Agent 共用；Agent 任务开始先读这里
  inbox/                       # 归属不明时 Agent 的临时落点
_templates/learning-note.md    # 笔记骨架（frontmatter 必填）
modules/<module>/              # 按模块分目录（self / tech / career / tooling / project-a ...）
AGENT-HANDOFF.md               # 本文件
```

## frontmatter schema（每条笔记必须填）

```yaml
---
title: "笔记标题"
module: <模块名>          # self / tech / career / tooling / project-a ...
type: <类型>              # profile / pattern / fact / decision / mistake / code / question
tags: [a, b]              # 便于检索
created: YYYY-MM-DD
updated: YYYY-MM-DD
status: active
source: "记忆来源"
related: [path/to/other.md]   # 双向链接，用相对路径
---
```

## Agent Loop（核心流程）

**任务开始前 — 召回（只读）**
1. 读 `_agent/index.md` 全局索引。
2. 用当前任务的 `active_modules` 与描述，按 `module / tags / title` 匹配相关条目。
3. 读取命中的 `.md`，输出 3-5 条「记忆简报」注入本次任务。
4. **无相关记忆就明说"无"，绝不编造。**
> 基础版靠 index + grep；如需语义召回，可对 `modules/` 做 embedding 检索（扩展口）。

**任务结束后 — 沉淀**
1. 萃取：`decision / mistake / pattern / code / fact / question` 五类知识。
2. 套 `_templates/learning-note.md` 骨架，填好 frontmatter。
3. 明确模块 → 写 `modules/<module>/YYYY-MM-DD-<slug>.md`；归属不明 → 写 `_agent/inbox/`。
4. 在 `_agent/index.md` 表格末尾追加一行（保持列顺序）。
5. 提交并推送（见下）。

## 冲突规避（硬规则）

- **Agent 只追加 / 新建自己的文件，绝不删改人的笔记。**
- Agent 写入范围限定：`modules/<module>/` 与 `_agent/inbox/`。
- 索引 `index.md` 只能追加行，不改动已有行的内容。
- 人与 Agent 共用 frontmatter schema，否则 Obsidian 图谱 / Agent 检索同时失效。

## 如何读写仓库

**本机（人 / 有浏览器的环境）**
```bash
git clone https://github.com/HarryKong824/Memory-of-Harry.git
# gh auth login 后，commit + git push 全自动（无需 PAT）
```

**沙箱 / 无浏览器的 Agent 环境（Windows schannel 注意）**
- Windows 的 schannel 在 TLS 握手时做证书吊销检查（CRL/OCSP），沙箱常因此失败。
- 推送必须绕过：`git -c http.schannelCheckRevoke=false ... push`。
- Token 由 **agent 代持**（inline 注入，不进对话、不落盘明文）：
  ```bash
  git -c http.schannelCheckRevoke=false \
      -c url."https://x-access-token:<PAT>@github.com/HarryKong824/Memory-of-Harry.git".insteadOf="https://github.com/HarryKong824/Memory-of-Harry.git" \
      push -u origin main
  ```
- 长期固化：把上面两项写入沙箱 `~/.gitconfig`（`http.schannelCheckRevoke=false` + 该仓库的 insteadOf），之后 `git push` 全自动。

## 安全铁律（来自用户）

- **GitHub PAT 绝不出现在本对话明文里**——由 agent 通过 inline + schannel bypass 代持。
- PAT 曾明文泄露即视为高危：到 GitHub 撤销重发，按版本节奏轮换，最小化 churn。
- 连接器（WorkBuddy GitHub MCP）默认**只读**，写操作走 `gh` CLI 或 git+token 兜底，不要依赖连接器写仓库。

## 一句话给新 Agent

「这是 Harry 的 Markdown 长期记忆库，先读 `README.md` 和 `_agent/index.md`，按 `_agent/recall-prompt.md` 召回、按 `_agent/precipitate-prompt.md` 沉淀，只追加不删改，无记忆就明说无。」

<!-- 新增条目追加在表格末尾，保持列顺序一致 -->
