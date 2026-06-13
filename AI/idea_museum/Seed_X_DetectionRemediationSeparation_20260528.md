---
name: 最终审查员有修复权是工程反模式
description: 给 AI Validator Agent 写权限是直觉但也是生产事故来源——只读 Validator 强制缺陷回流到正确责任方
type: seed
concept: 发现与修复必须分离（Separation of Detection and Remediation）
hook_insight: 你让 AI Code Reviewer 同时能修复它发现的问题——听起来效率翻倍。但这是最常见的 AI Agent 架构错误：发现者和修复者是同一个 Agent，绕过了所有中间验证步骤，把"最后一分钟热修复"自动化了。这不是效率，这是绕过质量门控的自动化
wiki_link: "[[Seven_Agent_Software_Factory]]"
---

# 最终审查员有修复权是工程反模式

## Hooks 草稿

Hook 1（实践冲突）：
你的 AI Code Review Agent 发现问题自动修复——效率翻倍。Seven-Agent Factory 的铁律：Validator 只能写报告，绝对不能动代码。原因：有写权限的 Validator 会产生"最后一分钟热修复"，绕过所有 QA 门控，这是最常见的回归 bug 来源。效率不是目标，可追溯性才是。

Hook 2（普遍原则）：
软件工程中，越靠近发布的修改越危险。给 AI Validator 写权限，等于把最危险的修改时机自动化。把发现和修复分给两类 Agent，每个 bug 都有明确责任方——这不是过度工程，这是生产系统的基线要求。

[Source: wiki/Seven_Agent_Software_Factory]
