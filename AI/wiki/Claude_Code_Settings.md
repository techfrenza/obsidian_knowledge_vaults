---
title: Claude Code Settings
aliases: ["settings.json", "权限管理", "Claude Code 配置"]
tags: [settings, permissions, security, claude-code, hooks]
category: claude-tooling
parent: "[[index]]"
created: 2026-04-30
date: "2026-04-30"
---

# Claude Code Settings

Parent: [[index]]

> 核心论点：settings.json 是 Claude Code 的**系统层权限控制**。`deny` 规则在系统层强制执行，Claude 无法绕过——这是安全防护的唯一可靠手段，CLAUDE.md 无法替代。

---

## 三层配置架构

| 层级 | 路径 | 作用 |
|------|------|------|
| Global | `~/.claude/settings.json` | 所有项目的安全红线 |
| Project | `.claude/settings.json` | 团队共享，git 提交 |
| Local | `.claude/settings.local.json` | 个人覆盖，不进 git |

**规则合并**：`deny` 永远优先，`allow` 次之

---

## 生产级完整模板（直接复制）

```json
{
  "permissions": {
    "defaultMode": "ask",
    "allow": [
      "Read", "Glob", "Grep", "LS",
      "Bash(npm *)", "Bash(pnpm *)", "Bash(yarn *)",
      "Bash(git status)", "Bash(git diff)",
      "Bash(git add *)", "Bash(git commit -m *)",
      "Bash(vitest *)", "Bash(tsc --noEmit)",
      "Write(src/**)", "Write(components/**)", "Write(pages/**)"
    ],
    "deny": [
      "Bash(rm -rf *)", "Bash(rm -r *)",
      "Bash(sudo *)", "Bash(* --force)",
      "Bash(git push --force)",
      "Bash(git push origin main)",
      "Read(**.env*)", "Read(**.key*)", "Read(**.secret*)",
      "Read(**/.aws/**)", "Read(**/credentials*)",
      "Delete(**)"
    ],
    "ask": [
      "Bash(npm install)", "Bash(pnpm install)", "Write(**)"
    ]
  },
  "hooks": {
    "postEdit": [
      "prettier --write {file}",
      "eslint --fix {file}"
    ]
  }
}
```

---

## 模式切换

- `Shift + Tab` 循环切换：`default → acceptEdits → plan mode`
- `claude --permission-mode auto`（Team+ 计划）：第二 AI 作为安全分类器，自动放行低风险操作

---

## 安全最佳实践

### 全局 deny 红线（~/.claude/settings.json）
- `.env*` / `.pem` / `.key` / `.secret` 文件读取
- `rm -rf` / `sudo` / `git push --force`
- `Delete(**)`

### .env 安全额外防护
```bash
# Pre-commit hook：检测密钥泄露
#!/bin/sh
if git diff --cached | grep -E 'sk-[a-zA-Z0-9]{48}|pk_live_|AKIA[0-9A-Z]{16}'; then
  echo "Secret detected! Commit blocked."
  exit 1
fi
```

---

## 团队共享（Boris Cherny 实践）

把 `.claude/settings.json` commit 到 git，全队 clone 后自动统一权限，无需每次询问 "允许吗"。

---

## 关联实体

- [[Claude_Code_Hooks]] — settings.json 中的 hooks 配置
- [[CLAUDE_md_Best_Practices|CLAUDE.md Best Practices]] — 规则文件（与 settings.json 互补，非替代）
- [[Agent_Harness_Engineering]] — settings.json 在安全层的角色
- [[Claude_Code_Commands]] — `Shift+Tab` 权限模式切换

*[Source: raw/Claude Code settings.json.md, raw/Claude Code + .env security.md]*

- [[Claude_Code_MOC]] — Claude Code 生态系统学习地图
- [[Production_Reliability_MOC]] — 生产可靠性三维度（可见/结构/安全）知识地图