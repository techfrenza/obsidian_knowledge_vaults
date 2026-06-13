---
title: Instruction Sharing Across Projects
aliases: ["Shared Instructions", "Symlink Instructions", "GitHub Copilot 团队指令共享", "Cross-project Instruction"]
tags: [github-copilot, symlink, junction, team-standards, instruction-files, shared-config]
category: claude-tooling
parent: "[[CLAUDE_md_Best_Practices]]"
created: 2026-05-06
date: "2026-05-06"
---

# Instruction Sharing Across Projects

Parent: [[CLAUDE_md_Best_Practices]]

> 核心论点：在多项目团队中，复制 Copilot 指令文件到每个仓库是维护噩梦。通过 symlink（Linux/macOS）或 NTFS junction（Windows）创建单一事实来源，所有项目指向同一中央仓库。

---

## 问题背景

GitHub Copilot 指令文件（instruction files）位于：
- 项目仓库：`.github/` 或其子目录（项目优先级高于用户目录）
- 用户主目录：`~/.github/prompts/`（限制：只能放在 prompts 文件夹，无法分层）

**痛点**：通用标准（编码规范、团队约定）若直接存各个 Project repo，更新需每仓库同步，极易漂移。

---

## 解决方案：中央仓库 + 平台链接

```
中央仓库（GitHubCopilot repo）
  └─ instructions/          ← 单一事实来源

项目仓库（任意 Project repo）
  └─ .github/
       └─ instructions/
            └─ shared/     ← symlink / junction → 指向上方
```

**`.gitignore` 必须排除** `.github/instructions/shared/`，防止链接目录被误提交到项目仓库。

---

## 平台实现

| 平台 | 机制 | 命令 |
|------|------|------|
| Linux | Symbolic link | `ln -s <source> <dest>` |
| macOS | Symbolic link（逻辑同 Linux） | `ln -s <source> <dest>` |
| Windows | NTFS Junction（避免 symlink 权限问题）| `New-Item -ItemType Junction` |

### 脚本参数（三平台通用逻辑）

| 参数 | 说明 |
|------|------|
| `--copilot` / `-GitHubCopilotRepo` | 中央仓库根目录路径 |
| `--project` / `-ProjectRepo` | 目标项目仓库根目录路径 |
| `--force` / `-Force` | 强制覆盖已存在的目标 |
| `--relative`（Linux/macOS）| 创建相对 symlink 而非绝对路径 |

### 幂等性保证
- 链接已正确指向目标 → 无操作
- 链接指向错误目标 → 删除后重建
- 目标为非链接文件 → 仅在 `--force` 时删除

---

## Windows 注意事项

- 使用 Junction 而非 Symlink，绕过企业机器的 Developer Mode 权限限制
- 新增指令文件后需 `git pull` 中央仓库才能在所有项目生效（**非自动同步**）

---

## 与 Claude Code 工作流对比

| 工具 | 指令共享机制 |
|------|-------------|
| GitHub Copilot | symlink/junction 指向中央 repo 的 `instructions/` 目录 |
| Claude Code | `~/.claude/CLAUDE.md`（全局）+ 项目级 `CLAUDE.md` + 子目录级 `CLAUDE.md` 三级分层 |

> Claude Code 的分层加载天然支持多项目复用全局规则，无需手动 symlink。

---

## 关联实体

- [[CLAUDE_md_Best_Practices]] — Claude Code 的三级指令分层架构（Global/Project/Local）
- [[AI_Team_Coding_Practice]] — 团队上下文资产（AGENTS.md/DECISIONS.md）的维护策略
- [[Agent_Harness_Engineering]] — Harness 工程中指令文件的作用（"地图"而非说明书）
- [[Claude_Code_Security]] — `.claudeignore` 和 deny 规则防止敏感文件泄露
- [[Multi_Agent_Architecture]] — drift linter 是文件系统 symlink 单源权威的 CI 层验证对等物：两者都解决 Skill 副本不一致问题

*[Source: raw/Sharing Instructions with the Team.md]*
