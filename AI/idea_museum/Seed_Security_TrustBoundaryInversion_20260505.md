# Seed: Security Trust Boundary Inversion

**[Concept]** CLAUDE.md 是建议层，settings.json 是物理层——安全边界不在 prompt，在系统文件。

**[Hook Insight]** 大多数人把安全规则写进 CLAUDE.md（"不要读 .env"），但 CLAUDE.md 只能影响 Claude 的意图，无法阻止其能力。真正的安全边界在 `settings.json` 的 deny 列表——这是系统层强制执行，Claude 物理上无法绕过。安全与信任的边界不是 AI 的自我约束，而是工程化的权限门。

**[X Hook]** "你以为 CLAUDE.md 的'NEVER read .env'能保护你的密钥？不能。只有 settings.json 的 deny 规则能。"

**[Wiki Link]** [[Claude_Code_Security]] → [[Claude_Code_Settings]]

*[Source: raw/Claude Code + .env security.md | 2026-05-05]*
