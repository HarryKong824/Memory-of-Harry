---
title: "ima-mcp 连接器能力边界与变通方案"
module: tooling
type: fact
tags: [ima, knowledge-base, connector, limitation]
created: 2026-08-16
updated: 2026-08-16
status: active
source: "WorkBuddy 跨项目长期记忆（连接器能力备注）"
related: [SELF.md]
---

# ima-mcp 连接器能力边界

## 要点
- 当前环境只暴露**读取/搜索**类工具：`search_knowledge`、`get_knowledge_list`、`fetch_media_content`、`get_knowledge_base_list`。**没有写入/导入接口**（无法新建笔记、上传文档）。
- **变通方案**：要「存到 ima」时，生成本地 Markdown 文件，由用户在 ima 客户端手动「导入」。
- 用户个人知识库名「孔皓冉的知识库」，知识库 ID `0019ed3e4040718b`（8 个条目，均已验证 `can_fetch_content=true`；MD/笔记/脑图可拉全文，PDF/PPT 可拉解析文本）。

## 知识库内容盘点（2026-08-04）
1. `Dify排障与KB-QA经验.md` — 简历项目1技术底稿，含 RAG 检索污染治理实战。
2. `rag_pipeline_kb.png` — RAG 管线图。
3. `liudao-ai-roadmap.pdf` — **威海柳道机械 AI 引进路线图·八大部门真实案例**，对标准备投递的威海制造业 B 类岗，可融入简历补「真实交付」短板。
4. `周五行业认知日学习指南.md`
5. `企业架构日学习指南.md`
6. `Transformer.pptx`（52MB）
7. `Transformer论文解读.xmind`
8. `LLM核心技术速记笔记`
