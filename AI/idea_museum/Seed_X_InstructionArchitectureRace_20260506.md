---
type: seed
source: wiki_scan
date: 2026-05-06
---

# Seed: AI 工具的真实竞争维度是"指令加载架构"，不是"模型能力"

**[Hook Insight]**

你以为在选 AI 编码工具时比的是模型质量。
实际上，你在比的是**它怎么加载你的团队规则**。

Claude Code：`~/.claude/CLAUDE.md`（全局）→ 项目 `CLAUDE.md` → 子目录 `CLAUDE.md`，三级自动分层，跨项目规则零维护。

GitHub Copilot：没有分层加载。想跨项目共享规则？你需要自己写 PowerShell 脚本创建 NTFS junction，然后确保每个开发者都运行过这个脚本，然后在 `.gitignore` 里排除链接目录，然后祈祷新人不会忘记。

这不是功能差距，这是架构设计理念的差距。
**一个把"团队知识持续化"当成 first-class feature，另一个让你用 1990 年代的文件系统 hack 绕过它。**

反直觉结论：选 AI 工具时，问的第一个问题不应该是"它有多聪明"，而应该是"它能不能记住我告诉它的事，跨越所有项目、所有会话、所有团队成员"。

**[Wiki Link]** [[Instruction_Sharing]] — symlink/junction 方案技术细节  
**[Wiki Link]** [[CLAUDE_md_Best_Practices]] — Claude Code 三级分层架构
