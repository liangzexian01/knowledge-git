# find-skill（提交就绪包）

> 来源：DSH 团队共享 skill `find-skill`（创建者：总规划师；Main Bot：Chief 应用 WARN-1 补丁）
> 用途：本仓库是阶段 2「能力沉淀闭环」的提交就绪包，准备推送到私有 GitHub
> 打包时间：2026-11-08
> LICENSE：Apache 2.0（依官方仓库 github.com/anthropics/skills）

## 结构

```
find-skill-repo/
├── SKILL.md              # find-skill 完整五段正文 + 真实 frontmatter（含 WARN-1 补句）
├── README.md             # 本文件
├── LICENSE               # Apache 2.0
└── references/
    └── README.md         # 研究 / 验证来源路径引用（归总规划师线，未复制入包）
```

- `SKILL.md`：内容取自 `ego/find-skill-attach-patch.md` 第 53–123 行（五段正文 + 真实
  frontmatter），已在共享 capability 本体落库（`save_capability` → `Saved find-skill.`）。
- `references/`：链 `ego/skill-research-notes.md`（调研笔记）与 `ego/find-skill-verification.md`
  （验证记录），这两个文件归总规划师线（`ego/`），本包只引用路径、不复制内容。

## 推送前置清单（最后一步，待用户提供）

1. **私有 GitHub 仓库 URL**：由用户提供（例如 `https://github.com/<owner>/find-skill.git`）。
2. **GitHub PAT**：必须走 `request_secret`（零明文，绝不入文件 / 对话）。变量名 `GITHUB_PAT`。
3. **关联远程并推送**：
   ```bash
   git -C <本目录> remote add origin <私有仓库 URL>
   git -C <本目录> push -u origin main
   ```
   PAT 作为 basic auth 密码（用户名任意，如 `x-access-token`）：
   ```bash
   git -C <本目录> push https://x-access-token:${GITHUB_PAT}@github.com/<owner>/find-skill.git main
   ```
4. **零明文纪律**：PAT 只经 `request_secret` 进 shell 环境变量，不写文件、不打印、不进对话。

## 状态

- ✅ 本地 `git init` + `add` + `commit` 已完成（提交就绪，未 push）。
- ⏳ 推送待：私有仓库 URL + PAT(`request_secret`)。
- 不启用设计师 bot、不落地 22:00 定时任务、不解锁 1.3/1.4（阶段 2 与 1.2 闸门解耦）。
