# integrated-frontend-design

[English](README.md)

一个给编程 Agent（Claude Code、Codex CLI 等读取 `SKILL.md` 的工具）用的编排技能，把三个现有前端技能串成一条流程：

**设计需求 → 视觉方向 → 实现 → 验收**

仓库里不包含原技能的任何内容，只规定每个决策由哪个技能负责、按什么顺序执行、冲突时听谁的，避免 Agent 跑三遍重复的设计或三套互相冲突的验收。

## 依赖技能

请先安装以下技能。整合技能按 frontmatter 里的 `name` 查找，文件夹叫什么都可以。

| 技能 `name` | 职责 | 来源 | 许可证 |
|---|---|---|---|
| `frontend-design` | 需求、受众、内容主线、信息层级 | [anthropics/skills](https://github.com/anthropics/skills/tree/main/skills/frontend-design) | Apache-2.0 |
| `design-taste-frontend` | 视觉方向、反模板规则、截图检查与修正循环 | [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill)（v2） | MIT |
| `web-design-engineer` | 实现、响应式、无障碍、按需浏览器验收 | [ConardLi/garden-skills](https://github.com/ConardLi/garden-skills/tree/main/skills/web-design-engineer) | MIT |

缺少某个技能时，会提示缺的是哪个，并用剩下的技能继续。

## 安装

把 `integrated-frontend-design/` 文件夹复制到技能目录：

| Agent | 用户级 | 项目级 |
|---|---|---|
| Claude Code | `~/.claude/skills/` | `.claude/skills/` |
| Codex CLI | `~/.codex/skills/` | `.agents/skills/` |

## 使用

遇到前端设计、搭建、改版类请求时会自动触发，也可以手动调用：

- Claude Code：`/integrated-frontend-design 帮我做一个……的落地页`
- Codex：`$integrated-frontend-design 帮我做一个……的落地页`

### 两种模式

- **轻量模式**（小改动、单个组件、设计评审时默认使用）：用 `frontend-design` 和 `web-design-engineer`，`design-taste-frontend` 只用它的规则，不跑脚本。
- **完整模式**（新站点、整体改版、要求"高级感"时使用）：三个技能全部启用，包括 `design-taste-frontend` 的 draw/inspect 脚本。需要 Node 22+ 和 Chrome/Edge/Chromium，耗时长、token 消耗大。

### 验收规则

- 始终执行：`web-design-engineer` 的交付前轻量自检。
- 完整模式且环境满足时：`design-taste-frontend` 的 inspector 和截图循环。
- 仅在你明确要求验收 / QA / 响应式测试时：`web-design-engineer` 的浏览器验收流程。

## 优先级

1. 你的需求、现有产品行为和设计系统
2. 功能、无障碍、性能、许可证约束
3. `frontend-design`（需求与层级）
4. `design-taste-frontend`（视觉方向）
5. `web-design-engineer`（实现规范）

## 致谢

本项目只做编排，设计内容全部来自：

- **frontend-design**，作者 [Anthropic](https://github.com/anthropics/skills)，Apache-2.0
- **tasteskill v2（`design-taste-frontend`）**，作者 [Leonxlnx](https://github.com/Leonxlnx/taste-skill)，MIT
- **web-design-engineer**，作者 [ConardLi](https://github.com/ConardLi/garden-skills)，MIT

本仓库不再分发它们的任何文件，请从各自仓库安装并遵守其许可证。

## 许可证

[MIT](LICENSE)，仅适用于本仓库中的文件。
