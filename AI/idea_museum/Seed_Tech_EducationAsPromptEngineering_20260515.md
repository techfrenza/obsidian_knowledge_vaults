---
name: EducationAsPromptEngineering
description: 为人类写的高质量教育内容（课程模块/操作手册）直接等价于最佳 Claude Skill prompt，无需"提示词工程"转换
type: seed
concept: 教育内容即 Skill Prompt
hook_insight: "最好的 Claude Skill 不是由 prompt 工程师写的——它是由课程设计师、SOP 编写者、培训讲师写的；他们花了多年打磨如何对人类清晰表达，这恰好就是对 LLM 清晰表达的最高标准"
wiki_link: "[[Claude_Code_Skills]]"
---

## 技术核心逻辑

传统 Prompt Engineering 假设：需要特殊语法、特殊结构、LLM-specific 技巧。

实际洞见：为人类助理写的操作手册 = 为 Claude 写的最佳 Skill Prompt。
- 转换步骤只需 5 分钟：取现有教育内容 → 在顶部加 `You are my [角色]. Follow the exact process below:` → 完成
- 原因：教育内容的核心是 Clarity of Thought（概念分离、步骤编号、边界条件显式化）——这恰好是 LLM 处理指令质量的关键维度

## 权衡与应用范围

**适用**：有现成高质量教育内容的领域（SOP、培训材料、Checklist）  
**不适用**：需要 Claude 执行动态推理而非遵循固定流程的任务

**反向推论**：如果一个 Skill prompt 写得很差，很可能是因为作者对任务本身没有清晰认知——问题在认知层，不在 prompt 语法层。"你写不出好 Skill，是因为你还没想清楚这件事怎么做。"

*[Source: wiki/Claude_Code_Skills.md]*
