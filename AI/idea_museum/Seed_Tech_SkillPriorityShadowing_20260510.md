---
name: Seed_Tech_SkillPriorityShadowing
description: Enterprise 级 Skill 会静默覆盖同名的个人 Skill，没有警告、没有通知，只有行为改变
type: seed
concept: Skill Priority Shadowing
hook_insight: 你写了一个"code-review"Skill 仔细调优了三周，但如果公司 IT 也部署了同名的 Enterprise Skill，你的版本就永久失效了——Claude 不会告诉你，它只是悄悄切换
wiki_link: "[[Skill_Ecosystem]]"
---

## 技术核心逻辑

Claude Code 的 Skill 优先级层级：

```
Enterprise（管理员下发）
    ↓ 覆盖
Personal（~/.claude/skills/）
    ↓ 覆盖
Project（.claude/skills/）
    ↓ 覆盖
Plugins（marketplace 安装）
```

**静默覆盖（Silent Shadowing）**：当 Enterprise 层存在与 Personal 同名的 Skill 时：
- Claude Code **不报错**，不提示，不警告
- 用户触发请求时，Enterprise 版本执行
- 用户感知到的只是"行为变了"，不知道为什么
- `claude --debug` 才会在日志中显示加载了哪个版本

## 三类受影响场景

| 场景 | 结果 | 风险级别 |
|------|------|---------|
| 个人 `commit` Skill 被 Enterprise `commit` 覆盖 | 提交格式变化，但可能更符合公司规范 | 低（有益）|
| 个人 `security-review` Skill 被弱 Enterprise 版本覆盖 | 安全检查变弱，用户不知情 | 高 |
| 个人 `code-review` 调优版被通用 Enterprise 版覆盖 | 三周调优成果归零 | 中 |

## 正确应对策略

1. **预防**：个人 Skill 使用更具体的名字（`frontend-ts-review` 而非 `code-review`）
2. **诊断**：怀疑行为异常时运行 `"what skills are available"` 或 `claude --debug`
3. **协商**：与 IT/管理员协商重命名或在个人 Skill 中添加更精准的 description

## 工程启示

这是"约定优于配置"原则在 AI 工具层的体现——系统为了简化设计选择了静默覆盖，代价是透明度降低。企业级 AI 工具的治理策略不能依赖用户自觉，需要审计机制（参见 AgentShield）。

参见：[[Claude_Code_Skills]] 的优先级机制；[[Claude_Code_Settings]] 的权限管理；[[Multi_Agent_Architecture]] 的 drift linter（类似原理）。
