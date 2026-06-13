# Claude Code 开发者环境配置最佳实践

> 查询日期：2026-05-08 | 引用来源：/wiki

---

## 1. CLAUDE.md — 项目记忆文件

**三级配置架构**（加载顺序：Global → Project → Local，离工作目录越近优先级越高）

| 层级 | 路径 | 内容 |
|------|------|------|
| Global | `~/.claude/CLAUDE.md` | 个人永久偏好（所有项目自动叠加）|
| Project | `项目根目录/CLAUDE.md` | 团队标准 + 技术约束，commit 进 git |
| Local | `~/.claude/local/CLAUDE.md` | 个人小习惯，不进 git |

**写作铁律**：
- 控制在 **60–80 行**，超 150 条规则 Claude 会丢失
- 只写负面规则：`NEVER`、`IMPORTANT`、`YOU MUST` 开头
- Karpathy 4 规则：**grep first**（先搜现有模式）→ **smallest change** → **blast radius 评估** → **completion criteria（5 点验收）**

**七段核心结构**：
```markdown
## Project Overview    # 2-3 句：做什么、给谁、优先级
## Tech Stack          # framework/package manager/database
## Critical Commands   # build/test/lint/dev
## Hard Rules          # NEVER/YOU MUST 负面规则
## Architecture Map    # src//components//lib/
## Workflow Preferences
## What NOT to include
```

---

## 2. settings.json — 权限防线

**生产级完整模板**：

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
      "Read(**.env*)", "Read(**.key*)", "Read(**.secret*)",
      "Read(**/.aws/**)", "Read(**/credentials*)",
      "Bash(rm -rf *)", "Bash(rm -r *)",
      "Bash(sudo *)", "Bash(* --force)",
      "Bash(git push --force)",
      "Bash(git push origin main)",
      "Delete(**)"
    ],
    "ask": [
      "Bash(npm install)", "Bash(pnpm install)", "Write(**)"
    ]
  }
}
```

**合并规则**：`deny` 永远优先，`allow` 次之  
**团队共享**：把 `.claude/settings.json` commit 进 git，全队 clone 后自动统一权限

**三级路径**：
- `~/.claude/settings.json` — 全局安全红线
- `.claude/settings.json` — 项目级，git commit
- `.claude/settings.json.local` — 个人覆盖，不进 git

---

## 3. 环境变量与 .env 安全

**原则**：所有密钥放 `.env`，永不在 chat 中输入

**deny 全覆盖模式**（settings.json 中）：
```
"Read(**.env*)", "Read(**.env.local*)", "Read(**.env.*)",
"Read(**.pem)", "Read(**.key)", "Read(**.secret*)",
"Read(**/.aws/**)", "Read(**/credentials*)",
"Read(**.npmrc)", "Read(**.pypirc)"
```
> deny 规则在系统层强制执行——Claude 物理上无法读取 .env 文件

**Pre-commit Hook（提交前拦截密钥）**：
```sh
#!/bin/sh
if git diff --cached | grep -E 'sk-[a-zA-Z0-9]{48}|pk_live_|AKIA[0-9A-Z]{16}'; then
  echo "Secret detected! Commit blocked."
  exit 1
fi
```
安装：`chmod +x .git/hooks/pre-commit`

**测试环境隔离**：用 `.env.test` + dummy 密钥，彻底切断运行时泄露路径

---

## 4. 权限模式切换

| 模式 | 触发方式 | 适用场景 |
|------|----------|----------|
| Plan Mode | `Shift + Tab` | 复杂任务先规划后执行 |
| Auto Mode | 默认 | 安全动作自动跑，风险动作询问 |
| Bypass | 显式配置 | 全自动（仅限沙盒/可信环境）|
| Extended Thinking | `/model` 切换 Opus | 多步深度推理 |

---

## 5. Skills 与辅助文件

**结构**：
```
.claude/
  settings.json        # 权限配置
  skills/
    skill-name/
      skill.md         # YAML frontmatter + 步步规则
  rules/               # 路径/语言规则
```

**全局 Skills**：`~/.claude/skills/`（跨所有项目可用）

**辅助文件**：
| 文件 | 内容 |
|------|------|
| `AGENTS.md` | Agent 工作方式、构建测试命令 |
| `DECISIONS.md` | 架构选择、被拒绝方案、已知 Bug 模式 |

---

## 6. 每日安全检查清单

- [ ] settings.json 有 `deny **.env*` 规则？
- [ ] 测试用 `.env.test` + dummy 值？
- [ ] pre-commit hook 已安装且 `chmod +x`？
- [ ] `.env` 在 `.gitignore`？
- [ ] 生产密钥存 vault 而非文件？
- [ ] `.env` 文件放在项目文件夹外？

---

## 7. 上下文管理（防 Context Rot）

| 策略 | 触发点 | 效果 |
|------|--------|------|
| Compaction | 窗口 60–70% 时 | 保留架构决策 + 未解决 Bug |
| Observation masking | 旧 tool result | 只保留 tool calls |
| JIT retrieval | 文件读取 | 用 grep/glob 代替全文件加载 |
| Sub-agent delegation | 高输出任务 | 子代理返回 ≤2000 token 摘要 |

---

## 8. 快速上手规则（2026）

1. **Sonnet** 日常任务，**Opus** 复杂推理
2. 所有 API key 放 `.env`，永不在 chat 输入
3. 先 POC 小测试，再规模化
4. 生产环境用独立 AI 账号 + read-only key
5. 团队 settings.json commit 进 git

---

## 引用来源

- `/wiki/Claude_Code_Settings.md`
- `/wiki/CLAUDE_md_Best_Practices.md`
- `/wiki/Claude_Code_Security.md`
- `/wiki/Claude_Code_Advanced_Features.md`
- `/wiki/Agent_Harness_Engineering.md`
