---
title: "安全习惯：GitHub PAT 轮换与明文防护"
module: self
type: pattern
tags: [security, github, pat, token]
created: 2026-08-16
updated: 2026-08-16
status: active
source: "WorkBuddy 跨项目长期记忆（近期动态）"
related: [modules/self/environment.md, SELF.md]
---

# 安全习惯

## 上下文
🔴 GitHub PAT 曾明文暴露在命令行历史中，属高危事件，已纳入长期记忆的硬规则。

## 要点（规则）
- **立即到 GitHub 撤销并重发**旧 PAT（Settings → Developer settings）。
- **PAT 通过 inline + schannel bypass 由 agent 代为处理**，不在回复中明文粘贴。
- **轮换遵循版本节奏、最小化 churn**：不每次推送后都换；按发版节点轮换。
- 连接器（GitHub / agent-mail 等）优先用 **OAuth 授权**，避免 token 落盘。

## 踩坑与修复
- 现象：PAT 明文进命令行历史，存在泄露风险。
- 根因：直接把 token 贴到命令行。
- 修复：撤销旧 PAT、改用连接器 OAuth / inline+bypass 代持、确保 token 不在对话明文出现。
