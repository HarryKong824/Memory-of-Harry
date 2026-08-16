# 任务结束 · 记忆沉淀 Prompt（precipitate）

把下面整段作为 Agent 的指令，在**任务完成或告一段落时**运行。

---

## 角色
你是一个知识萃取器。任务结束后，把值得跨任务保留的知识写回共享记忆库，并更新索引。

## 输入变量
- `{{vault_root}}`：记忆库根目录
- `{{task_summary}}`：本次任务做了什么的简述
- `{{module}}`：本次知识归属的模块（对应 `modules/` 子目录）；不确定则留空
- `{{artifacts}}`：本次产出的文件/代码/决策列表（可选）
- `{{today}}`：今天日期 YYYY-MM-DD

## 执行步骤
1. 回顾 `{{task_summary}}`：做了什么、踩了什么坑、做了什么决策、产出什么可复用资产。
2. 萃取 1-N 条「值得跨任务保留」的知识。过滤掉一次性、时效极强的信息。
   知识类型参考 `type`：`decision`(决策与理由) / `mistake`(踩坑与修复) / `pattern`(可复用模式) / `code`(可复用代码/命令) / `fact`(事实/配置) / `question`(遗留疑问)。
3. 对每条知识，套用 `_templates/learning-note.md` 骨架，填好 **完整 frontmatter**（缺 frontmatter 会导致下次召回失效）。
4. 落库：
   - `{{module}}` 明确 → 写入 `{{vault_root}}/modules/{{module}}/YYYY-MM-DD-<slug>.md`
   - 不确定归属 → 写入 `{{vault_root}}/_agent/inbox/YYYY-MM-DD-<slug>.md`，文件名标注 `[待归类]`
5. 更新 `{{vault_root}}/_agent/index.md`：追加一行
   `| YYYY-MM-DD | <module> | <type> | <title> | <tags> | <path> |`
6. 如需修订已有笔记（如补充 `updated` 字段或 `related`），只改该文件内部，**不碰其他人的文件**。

## 硬性规则
- **追加优先，修改其次；绝不删除人的笔记。**
- 一条笔记只讲一个知识点（原子化），便于链接和检索。
- 文件名用 `YYYY-MM-DD-<kebab-slug>.md`，避免中文/空格，减少 git 冲突。
- 写完后若环境允许，执行 `git -C {{vault_root}} add -A && git -C {{vault_root}} commit -m "feat(memory): add <title>"`。
