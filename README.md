# integrated-frontend-design

[中文说明](README.zh-CN.md)

An orchestration skill for coding agents (Claude Code, Codex CLI and other
agents that read `SKILL.md`). It combines three existing frontend skills into a
single workflow:

**design brief → visual direction → implementation → verification**

It contains no copy of the source skills. It only decides which skill owns each
decision, the order they run in, and what wins when they disagree, so the agent
does not run three overlapping redesigns or three conflicting QA passes.

## Required skills

Install these first. The integrated skill finds them by their frontmatter
`name`, so the folder names do not matter.

| Skill `name` | Role | Source | License |
|---|---|---|---|
| `frontend-design` | Brief, audience, content spine, information hierarchy | [anthropics/skills](https://github.com/anthropics/skills/tree/main/skills/frontend-design) | Apache-2.0 |
| `design-taste-frontend` | Visual direction, anti-template house rules, screenshot look-and-fix loop | [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill) (v2) | MIT |
| `web-design-engineer` | Implementation, responsive layout, accessibility, explicit browser acceptance | [ConardLi/garden-skills](https://github.com/ConardLi/garden-skills/tree/main/skills/web-design-engineer) | MIT |

If one is missing, the skill tells you which one and continues with the rest.

## Install

Copy the `integrated-frontend-design/` folder into your skills directory:

| Agent | User-level | Project-level |
|---|---|---|
| Claude Code | `~/.claude/skills/` | `.claude/skills/` |
| Codex CLI | `~/.codex/skills/` | `.agents/skills/` |

```bash
git clone https://github.com/Kwz112/integrated-frontend-design.git
cp -r integrated-frontend-design/integrated-frontend-design ~/.claude/skills/
```

## Usage

The skill triggers automatically on frontend design/build/redesign requests.
To call it directly:

- Claude Code: `/integrated-frontend-design build a landing page for ...`
- Codex: `$integrated-frontend-design build a landing page for ...`

### Modes

- **Light** (default for small changes, single components, critiques): uses
  `frontend-design` plus `web-design-engineer`, and `design-taste-frontend`'s
  house rules without its scripts.
- **Full** (new sites, full redesigns, "make it look premium"): runs all three,
  including `design-taste-frontend`'s draw/inspect scripts. Needs Node 22+ and
  Chrome/Edge/Chromium. Expect a long, token-heavy run.

### Verification rules

- Always: the lightweight pre-delivery self-check from `web-design-engineer`.
- Full mode with the runtime available: `design-taste-frontend`'s inspector and
  screenshot loop.
- Only when you explicitly ask for acceptance / QA / responsive testing:
  `web-design-engineer`'s browser acceptance harness.

## Precedence

1. Your requirements, existing product behavior and design system
2. Functional, accessibility, performance and licensing constraints
3. `frontend-design` (brief and hierarchy)
4. `design-taste-frontend` (visual direction)
5. `web-design-engineer` (implementation conventions)

## Credits

This project only coordinates other people's work. All design guidance comes
from:

- **frontend-design** by [Anthropic](https://github.com/anthropics/skills), Apache-2.0
- **tasteskill v2 (`design-taste-frontend`)** by [Leonxlnx](https://github.com/Leonxlnx/taste-skill), MIT
- **web-design-engineer** by [ConardLi](https://github.com/ConardLi/garden-skills), MIT

None of their files are redistributed here. Install them from their own
repositories and follow their licenses.

## License

[MIT](LICENSE). Applies only to the files in this repository.
