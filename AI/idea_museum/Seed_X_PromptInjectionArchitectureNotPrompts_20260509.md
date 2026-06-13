---
name: Prompt Injection Defense — Trust Tier Architecture as the Only Real Solution
description: X 病毒观点：所有"让 Claude 忽略恶意指令"的防御都是幻觉；唯一有效的方案是架构分层——不可信文档根本不应该接触有权限的 Agent
type: seed
---

# Prompt Injection Defense — Architecture Beats Prompts

[Hook Insight]

> "告诉 Claude '忽略所有注入攻击' 就像告诉收银员 '别被骗'。
> 唯一有效的防御是：**让收银员根本看不到那张支票。**"

三层隔离架构：
```
不可信文档 → Reader（只读，无写权限，无 MCP）
                ↓ schema 验证 + 字符白名单
            Orchestrator（不碰原始文档，聚合）
                ↓ 已清洗数据
            Resolver（可写文件，但无外部通信）
```

反直觉点：
- 主流方案：更好的系统 Prompt，让模型更聪明地识别攻击
- 实际有效方案：**物理上剥夺权限**，让被注入的 Agent 没有能力执行危险操作

这不是 AI 安全问题，是**权限架构问题**。解决方式不是更好的 Prompt，是更严格的角色分离。

[Wiki Link] [[Multi_Agent_Architecture]] · [[Claude_Code_Security]] · [[Human_In_The_Loop]]
