---
type: seed
tags: [idea-museum, anti-intuition, prompt-injection, agentic-security]
created: 2026-05-27
---

Concept: 间接提示注入（Indirect Prompt Injection）是Agentic时代最大安全威胁

Hook Insight: 传统提示注入（用户主动输入"忽略所有指令"）已被现代LLM防御。真正的危险在于"间接注入"：恶意指令隐藏在Agent检索的外部文档/邮件/网页中，Agent处理合法任务时无意间执行攻击者指令。Agent能力越强（能访问文件/API/发邮件），间接注入的"爆炸半径"越大。

反直觉点：给Agent更多工具访问权限 = 间接扩大了攻击面。HITL不只是质量控制，更是注入防线。

Wiki Link: [[Prompt_Injection]] ↔ [[Human_In_The_Loop]] ↔ [[SAP_Agent_Guardrails]]
