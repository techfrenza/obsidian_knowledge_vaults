---
name: Seed_X_SkillsMarketplaceAlreadyCompromised
description: AI Skills marketplace 在 2026 年 1 月已有 12% 恶意软件——App Store 的安全问题在 AI 时代提前重演
type: seed
concept: Skills Marketplace Security Crisis
hook_insight: 2026 年 1 月，某 Skills marketplace 上 12% 的 Skill 是恶意软件——2,857 个 Skill 里有 341 个是攻击载体；这不是 App Store 的 2012 年，这是 AI 代理时代的 2026 年
wiki_link: "[[Skill_Ecosystem]]"
---

## X Hook 草稿

**Hook 1（数据震撼型）：**
> 2026 年 1 月的某个 AI Skills marketplace：
>
> - 2,857 个社区 Skill
> - 341 个是恶意软件（12%）
> - CVSS 8.8 CVE：一次点击，17,500 个实例可被 RCE
> - Moltbook breach：150 万个 API token 泄露
>
> 你用 Claude Code 安装第三方 Skill 了吗？

**Hook 2（历史对比型）：**
> App Store 用了 4 年才认真对待恶意 App 问题（2008-2012）。
>
> AI Skills marketplace 用了几个月就走完了同样的路。
>
> 原因：SKILL.md 不需要执行权限审批，它只需要让 Claude 相信这是合法工作流。
> 提示注入 + 壳工具 = 零用户感知的攻击链。

**Hook 3（行动指引型）：**
> 你的 Claude Code 配置扫过吗？
>
> `npx ecc-agentshield scan`
>
> 1,282 个测试，扫：CLAUDE.md 硬编码 key、settings.json 权限漏洞、MCP server CVE、Hooks 注入向量。
>
> 这不是偏执，这是 2026 年 AI 工程的基本卫生。
