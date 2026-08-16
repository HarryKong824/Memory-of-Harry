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
🔴 GitHub PAT 曾明文暴露在命令行历史中，属高危事件，已纳入长期记忆。但早期"撤销重发"规则经用户指正存在**循环逻辑缺陷**——重发=把新密钥再粘进聊天框=重复暴露，等于没修。本条已修正。

## 要点（规则）
- **任何密钥绝不通过明文对话（聊天框）传输**（最高优先级铁律）。
- 安全通道仅两类：① **浏览器 OAuth**（`gh auth login` 选 web browser，token 全程不进对话）；② **受保护凭据存储**（沙箱环境把密钥存 OS keychain / 连接器 credentials / MCP server `env`，agent 从环境变量读，不进对话）。
- **"轮换"≠"重新粘贴到对话"**：已出现在对话里的密钥，不要靠"再发一个新的到聊天框"来轮换——那会重复暴露。改为：评估对话可读性风险 → 今后只走非对话通道（本机 OAuth、沙箱凭据存储）→ 仅在对话确被共享/导出时才撤销旧密钥。
- 沙箱 agent 写仓库走 **git inline + schannel bypass**：token 由 agent 代持、固化在沙箱 `~/.gitconfig`（insteadOf），**不进对话、不落盘明文、之后不再穿越对话**。
- 连接器（GitHub / agent-mail 等）优先用 **OAuth 授权**，避免 token 落盘。

## 踩坑与修复（历史）
- 现象：PAT 明文进命令行历史 / 聊天框，存在泄露风险。
- 根因：直接把 token 贴到命令行或聊天框。
- 修复：令牌只经 OAuth 或受保护凭据存储传输；本机 `gh auth login`（浏览器）免 token；沙箱用 gitconfig 代持，绝不在对话明文重贴。
