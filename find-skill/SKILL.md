---
name: find-skill
description: 当需要找「有没有现成的 skill 能干某件事」时使用：先上网/查本机搜现成 skill 与规范，再按格式评估、取用或新建，最后执行验证。
---

# find-skill：找、评、用、建一个 skill

> 依据 2026-11-08 用 ego 浏览器真实上网调研（GitHub anthropics/skills 官方仓库
> + 中文社区对 SKILL.md 规范的解释），材料见
> `C:\Users\梁泽贤\.dsh\bot\workspace\ego\skill-research-notes.md`。

## 目标
面对一个任务，先判断「是否已有现成 skill 可复用」；有就取用，没有就按官方规范新建，
并执行一次验证。避免重复造轮子，也避免凭空编造 skill 格式。

## 前置条件
- 上网：ego lite 浏览器自动化（见共享 skill `ego-browser-use`），能真实打开网页抓正文。
- 查本机：`list_capabilities` / `skill` 工具，看团队已挂的 skill 与 MCP。
- 不需要外部账号、不需要任何凭据。

## Skill 的官方最小结构（调研确认）
一个 skill = 一个文件夹 + 一个 `SKILL.md`。最小 frontmatter 示例：

```yaml
name: my-skill-name
description: 一句话说明做什么、何时用
```

正文建议五段：`# 标题` / `## 目标` / `## 前置条件` / `## 步骤（编号、可复制的命令/操作）` / `## 常见坑` / `## 验收标准`。

- frontmatter **必需字段只有 `name`（唯一 id）和 `description`（做什么+何时用）**。
- 规范出处 agentskills.io；官方示例仓库 github.com/anthropics/skills
  （`./spec` 规范、`./template` 模板、`./skills` 四类示例，多为 Apache 2.0）。
- 复用前审 license、在本环境测过再用于生产。

## 步骤
1. **明确需求**：一句话写清「要 skill 帮我干什么」。
2. **先找现成的（三处都查）**：
   - 本机：`list_capabilities` 看团队**全部**共享能力（没挂到自己也能看到），
     `skill` 工具/catalog 只列**已挂到本 Bot** 的、可直接加载；两者范围不同，别混。
   - 上网：用 ego 打开 `https://github.com/anthropics/skills`、`https://agentskills.io`，
     以及搜索引擎查「<任务关键词> + SKILL.md / Agent Skills / 使用说明」。
   - 官方/社区：找同主题 skill 仓库、文档、示例。
3. **评估**：对照 frontmatter 的 name/description 判断适用场景是否匹配；看正文
   目标/前置/步骤/坑/验收是否齐全；看 license 与依赖；确认和当前环境（DSH/Windows）兼容。
4. **取用或新建**：
   - 有合用的 → 直接调用：
     - DSH 里用 `skill(name)` 加载前，先确认它在**本 Bot 的 skill catalog** 里。
     - **共享 skill（`save_capability` 建的）默认只挂到创建它的那个 Bot**；其他 Bot 的
       catalog 里看不到，直接 `skill(name)` 会报 `unknown or no longer available`。
       先 `attach_capability(name)` 挂到自己（下一条消息生效、catalog 刷新），
       再 `skill(name)` 就能读到完整正文；核完可用 `detach_capability(name)` 摘回原状。
     - 也可用 `save_capability` 把自建 skill 挂成共享 skill，供全团队复用。
   - 没有 → 按上面最小结构新建，frontmatter 至少 name + description，
     正文写全五段，并写明上面的挂接步骤，才真能做到「另一个 AI 看了就能照做」。
5. **执行一次验证**：拿一个真实小任务跑一遍，确认能跑通、结果符合验收标准。
6. **沉淀**：把找到/新建的 skill、来源链接、验证结果记录归档，供下次直接复用。

## 常见坑
- 别没找就自己造——先查本机再上网，顺序别反。
- **共享 skill 加载不到**：别的 Bot 直接 `skill(name)` 报 unknown，多半是没先
  `attach_capability` 挂到自己；`list_capabilities` 看得到 ≠ `skill` 加载得到。
- 搜索引擎可能把查询误解成查单词（如「agent」被当成词典），标题全空、结果无关；
  换更具体的关键词 + 直接抓权威页面正文，用 `page.evaluate` 取 `innerText` 比只截
  语义快照前几千字更可靠（快照常先给导航菜单）。
- frontmatter 别堆多余字段，name/description 是硬性最小集。
- 别照搬生产 skill：官方仓库明确说示例仅作教学，须自测。
- 中文内容别写进 ego 的 `-e` 参数，放 `.mjs` 文件里（见 `ego-browser-use`）。

## 验收标准
- [ ] 有「先找后建」的记录：本机 + 上网两条路径都查过，附来源链接。
- [ ] 取用/新建的 skill 满足最小结构（name + description + 五段正文）。
- [ ] 真实执行过一次，有可复现的步骤与结果。
- [ ] 全程零外部账号、零凭据；复用 skill 已核 license。

> 注：正文第 4 步与「常见坑」中 `attach_capability` / `detach_capability` 字样为 WARN-1
> 可达性补句（来源 `ego/find-skill-attach-patch.md` 第 53–123 行），已由 Main Bot 经
> `save_capability` 落库（系统返回 `Saved find-skill.`）。
