---
title: "SELF — 用户档案（WorkBuddy 上的『我』）"
module: self
type: profile
tags: [profile, ai-architect, collaboration, habits]
created: 2026-08-16
updated: 2026-08-16
status: active
source: "WorkBuddy 跨项目长期记忆（~/.workbuddy/MEMORY.md）与本次对话萃取"
related: [modules/self/collaboration-style.md, modules/self/environment.md, modules/self/security.md]
---

# SELF — 用户档案

> 本文件是记忆库的「主人公」档案，由 WorkBuddy 上沉淀的长期记忆与协作习惯萃取而来。

## 身份
- **AI 架构师**，兼具技术视角与商业视角。
- 对**事实准确性**要求极高：要求 AI 对照真实代码/文档核实，先确认状态再执行；拿不准就停问，禁止编造。

## 四条并行主线
1. **AI 资讯精选服务**：曾运营每日 2 篇高质量 AI 行业文章（已不活跃但框架在跑）。格式严格：日期标题 + 两篇资讯（来源/发布时间/200-300字摘要/原文链接）+ 一句话速评。真实性底线：禁止杜撰、伪造链接、编造数据；须 24h 内、权威来源、主题差异化、排除营销与未证实传闻。
2. **seller-agent LangGraph 系统**：多仓结构（backend=FastAPI+LangGraph+MCP ChatAgent；frontend=Next.js+Tailwind v4；root=docs）。MCP stdio JSON-RPC 2.0 架构。工作区 `E:/agent学习`，GitHub 身份 HarryKong824，Watt Toolkit 代理访问 GitHub。
3. **AI 架构师简历准备/面试**：作为面试官考察候选人的场景分析、技术边界、ROI、变更管理四维度；自身也在备考（桌面 `ai-500-questions-answers.txt` 题库）。
4. **seller-agent 功能设计文档**：将用户痛点映射到已实现特性，支撑后续优化与规划。

## 当前阶段与关注
- seller-agent 进入 **P3-3**（geofence + reverse geocoding + auto-notification，跨后端/前端/文档三仓推进）。
- **Phase F 完成**：对话历史持久化落库 + JWT username 隔离；LangGraph Agent F4+F5（ChunkSearchResult.source + 多轮改写/分解/引用节点）已交付。
- 流失预警阈值拍板 **60 天**。
- 路线图：线 B③–⑦（Pipeline/Playbook/知识沉淀/话术个性化/引用溯源）+ 线 C（每日报表+管理者模块，销售动作须可量化）。

## 待拍板事项（历史）
- embedding model 两端配置不一致（root `.env`=`qwen3.7-text-embedding` vs backend `.env`=`text-embedding-v2`）待统一。
- 线 B② 前端 3 个 commit（`3ad97cd1..9979cd2e`）未推 GitHub，后端已推；文档根 origin 误配为后端 URL 待修正。
- 🔴 GitHub PAT 曾明文暴露命令行历史，需到 GitHub 撤销重发（见 `modules/self/security.md`）。

## 语言与输出
- **中文输出为强制要求**；技术指令同样用中文。
- 结构化对比优先用表格；状态摘要清晰；用 ⚠️🔴 等强调风险；下一步行动优先置顶。
- 混合中英沟通自然流畅，仅一句「继续」即可交还 initiative。
