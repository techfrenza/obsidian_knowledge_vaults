---
title: Claude Code Security & .env Hardening
aliases: ["Claude Code 安全", "env Hardening", "settings deny"]
tags: [claude-code, security, env, settings, hardening]
category: claude-tooling
parent: "[[Claude_Code_Settings]]"
created: 2026-05-15
date: "2026-05-15"
---

# Claude Code Security & .env Hardening

Parent: [[Claude_Code_Settings]]

> 将安全控制前移到系统层，而非依赖提示词。[Source: raw/Claude Code + .env security.md]

---

## 核心原则

- **settings.json deny 规则**是唯一可靠防线；CLAUDE.md 仅作建议，无法阻止读取。
- 生产凭证永不放项目文件夹，用环境变量或 vault 注入。
- 测试环境用 `.env.test` + dummy 密钥，彻底切断运行时泄露路径。

---

## settings.json 全局安全配置（立即可用）

```json
{
  "permissions": {
    "defaultMode": "ask",
    "allow": [
      "Read", "Glob", "Grep", "LS",
      "Bash(npm *)", "Bash(pnpm *)", "Bash(yarn *)",
      "Bash(git status)", "Bash(git diff)", "Bash(git add *)", "Bash(git commit -m *)",
      "Bash(vitest *)", "Bash(tsc --noEmit)",
      "Write(src/**)", "Write(components/**)"
    ],
    "deny": [
      "Read(**.env*)", "Read(**.env.local*)", "Read(**.env.*)",
      "Read(**.pem)", "Read(**.key)", "Read(**.secret*)",
      "Read(**/.aws/**)", "Read(**/credentials*)",
      "Read(**.npmrc)", "Read(**.pypirc)",
      "Bash(rm -rf *)", "Bash(rm -r *)", "Bash(sudo *)", "Bash(* --force)",
      "Bash(git push --force)",
      "Delete(**)"
    ],
    "ask": ["Bash(npm install)", "Bash(pnpm install)", "Write(**)"]
  }
}
```

> **deny 规则在系统层强制执行，Claude 物理上无法读取 .env 文件。**

---

## Pre-commit Hook（提交前拦截密钥）

```sh
#!/bin/sh
if git diff --cached | grep -E 'sk-[a-zA-Z0-9]{48}|pk_live_|AKIA[0-9A-Z]{16}'; then
  echo "Secret detected! Commit blocked."
  exit 1
fi
```

安装：`chmod +x .git/hooks/pre-commit`

---

## 容器隔离（最高安全等级）

```dockerfile
FROM your-base
COPY . /app
RUN rm -f /app/.env*   # 真实 .env 永不进入容器
```

---

## 每日安全检查清单

- [ ] settings.json 有 `deny **.env*` 规则？
- [ ] 测试用 `.env.test` + dummy 值？
- [ ] pre-commit hook 已安装且 `chmod +x`？
- [ ] `.env` 在 `.gitignore`？
- [ ] 生产密钥存 vault 而非文件？
- [ ] `.env` 文件放在项目文件夹外？

---

## 相关链接

- [[Claude_Code_Settings]] — allow/deny/ask 权限架构
- [[CLAUDE_md_Best_Practices]] — CLAUDE.md 硬性规则段写法
- [[Human_In_The_Loop]] — 高风险操作拦截钩子
- [[Agent_Harness_Engineering]] — Harness 安全控制平面
- [[Prompt_Injection]] — 提示注入攻击（settings deny 规则的核心防御对象）

- [[Claude_Code_MOC]] — Claude Code 生态系统学习地图
- [[Production_Reliability_MOC]] — 生产可靠性三维度（可见/结构/安全）知识地图