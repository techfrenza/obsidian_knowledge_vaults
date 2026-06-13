---
title: Human-in-the-Loop (HITL)
aliases: ["HITL", "人类闸门", "工具拦截"]
tags: [hitl, agent, safety, tool-interception, approval]
category: agent-engineering
parent: "[[Agent_Harness_Engineering]]"
created: 2026-05-15
date: "2026-05-15"
---

# Human-in-the-Loop (HITL)

Parent: [[Agent_Harness_Engineering]]

> HITL 是代理系统中确保关键操作必须经人类确认的程序化"门禁"机制。[Source: raw/Human-in-the-loop(HITL).md]

---

## 核心机制：工具调用拦截

- **拦截时机**：`tool_use` 请求发出后、实际执行逻辑之前。
- **挂起行为**：SDK 进入 "Hang" 状态，等待外部信号（人类 approve/reject）。
- **拒绝后处理**：钩子将拒绝原因返回给代理，代理在循环中反思并寻找替代方案。

### 为什么用钩子而非提示词？

| 方式 | 保证类型 | 失败率 |
|------|----------|--------|
| 提示词指令 | 概率性 | 存在故障率 |
| 拦截钩子（Hook） | **确定性** | 物理阻断 |

---

## 高风险操作识别

需设置 HITL 拦截的工具类型：
- 删除文件 / 数据库表
- 部署代码到生产环境
- 高额退款（如金额 > $500）
- 权限修改 / 账户操作
- `rm -rf` / 服务器重启等危险 Bash 命令

---

## 典型应用场景

### 财务风控（客服代理）
```python
# 当 process_refund 金额超过 $500 时强制人工审核
if tool_name == "process_refund" and tool_args["amount"] > 500:
    pause_and_await_human_approval(reasoning, tool_args)
```

### Claude Code 系统安全
通过 `settings.json` deny 列表实现程序化拦截：
```json
"deny": ["Bash(rm -rf *)", "Bash(git push --force)", "Delete(**)"]
```

---

## 设计原则

1. **识别高风险工具列表** → 不可逆操作、外部可见操作、高成本操作。
2. **配置拦截钩子**（PostToolUse / PreToolUse）捕获传出调用。
3. **展示上下文给人类**：显示代理推理过程 + 工具参数，而非只要求 yes/no。
4. **明确 approve/reject 路径**：拒绝时返回结构化错误信息供代理反思。

---

## 相关链接

- [[Claude_Code_Hooks]] — Hooks 事件驱动执行层
- [[Claude_Code_Security]] — settings.json deny 规则
- [[Agent_Harness_Engineering]] — Harness 控制平面全景
- [[Claude_Code_Settings]] — 权限管理架构

- [[Production_Reliability_MOC]] — 生产可靠性三维度（可见/结构/安全）知识地图
- [[Multi_Agent_Architecture]] — Outcomes/Rubric 自动质量门控与 HITL 人工门控形成互补的两层质量体系
- [[Agent_Governance_Layers]] — HITL 是 Layer 4 Escalation Protocol 的执行机制；Governance 定义何时触发 HITL