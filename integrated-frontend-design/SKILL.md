---
name: integrated-frontend-design
description: >-
  Orchestrates three frontend skills into one workflow: frontend-design for the
  design brief and information hierarchy, design-taste-frontend for visual
  direction and anti-template critique, and web-design-engineer for
  implementation, responsive behavior and accessibility. Use when building,
  redesigning, reviewing or substantially restyling a browser UI (page, app
  screen, dashboard, prototype). Do not use for backend-only, CLI, document or
  non-visual work.
---

# Integrated Frontend Design

This is an orchestration skill. It does not copy the source skills; it decides
which one owns each decision and in what order, so they do not run as three
competing checklists.

## Dependencies

| Skill `name` | Upstream | Owns |
|---|---|---|
| `frontend-design` | anthropics/skills | The brief: subject, audience, primary job, content spine, information hierarchy |
| `design-taste-frontend` | tasteskill v2 | Visual direction: composition, type system, palette, imagery, motion, anti-template critique, rendered look-and-fix |
| `web-design-engineer` | ConardLi/garden-skills | Delivery: existing-system audit, implementation structure, tokens and components, responsive behavior, accessibility, explicit browser acceptance |

### Locating the source skills

Refer to source skills by their frontmatter `name`, not by folder name; folder
names differ between installs. To load one:

1. If the agent exposes installed skills by name (a skill tool or `$name`
   mention), invoke it by name.
2. Otherwise search the usual skill roots for a `SKILL.md` whose frontmatter
   `name` matches: the project's `.claude/skills/` and `.agents/skills/`, then
   `~/.claude/skills/`, `~/.codex/skills/` and `~/.agents/skills/`.
3. If a source skill is not installed, say which one is missing, link its
   upstream from the README, and continue with the remaining skills. Do not
   invent its rules.

Read a source skill only when its step is reached, and read its reference files
only when that skill's own instructions call for them.

## Choose a mode

- **Light mode** (default for small changes, a single component, a critique of
  an existing UI, or when time/token budget is tight): read `frontend-design`
  and the relevant sections of `web-design-engineer`. Use
  `design-taste-frontend` only for its house rules and critique, without its
  scripts.
- **Full mode** (new page or site, full redesign, or the user asks for a
  distinctive / premium / award-level result): run all steps below, including
  `design-taste-frontend`'s draw and inspect scripts when the runtime allows.
  Tell the user up front that a full run is long and token-heavy.

## Order of decisions

1. **Scope the surface.** Confirm the result is a browser-rendered visual
   artifact. Classify it as either:
   - a **page** (read and scrolled: landing page, marketing site, portfolio,
     one-pager), or
   - an **app screen** (worked in: dashboard, admin, settings, inbox, editor).

   For app screens, `design-taste-frontend` has its own run in
   `references/app-screens.md` (with `draw.mjs --app` and `inspect.mjs --app`);
   use that instead of its page run. Backend-only, CLI, document or
   source-to-article work goes elsewhere.
2. **Write the brief** (`frontend-design`). Record the subject, audience,
   primary job, required content, stack, existing design system, responsive and
   accessibility constraints, and the assets that actually exist. Build the
   content spine and information hierarchy, and drop sections with no
   user-facing claim. Do not invent proof, testimonials, logos, metrics or
   product facts. This step does not choose fonts, palette or composition.
3. **Choose the visual direction** (`design-taste-frontend`). Ground the
   direction in the subject and pick one deliberate idea; do not blend unrelated
   aesthetics. This step owns type system, palette, composition, imagery and
   motion. If an existing design system is in place, choose the direction
   *within* it.
4. **Implement** (`web-design-engineer`). Classify an existing UI as extension,
   preserve-redesign or overhaul. Build tokens, components, real content and
   assets. Cover keyboard focus, contrast, reduced motion, and
   empty/error/loading states.
5. **Verify** (see *Verification* below).
6. **Report.** State the chosen direction, important trade-offs, assets and
   their credits/licences, which checks ran (and at which viewports), and what
   remains unverified.

Do not ask the same discovery questions more than once across steps; later
steps reuse the brief from step 2.

## Verification

The source skills disagree on when to open a browser. Resolve it this way:

- **Always:** run `web-design-engineer`'s lightweight pre-delivery self-check
  (it needs no browser).
- **Full mode with the runtime available** (Node 22+ and an installed
  Chrome/Edge/Chromium): use `design-taste-frontend`'s `inspect.mjs` and its
  screenshot look-and-fix loop as the visual check. This is part of the build,
  not a separate QA pass.
- **Only when the user explicitly asks** for acceptance / 验收, QA, browser
  testing, responsive or cross-viewport checks, or visual regression: also run
  `web-design-engineer`'s `references/browser-acceptance.md` and record its
  evidence.
- If a required runtime, browser or network access is missing, say so and
  report the check as not run. Never present a single screenshot or a static
  HTML read as full acceptance.

## Precedence

When the source skills disagree, the higher item wins:

1. User requirements, existing product behavior and the project's design system.
2. Functional, accessibility, performance and licensing constraints.
3. The brief and information hierarchy from `frontend-design`.
4. The visual direction and house rules from `design-taste-frontend`.
5. Implementation conventions from `web-design-engineer`.

The verification rules above override any source skill's own rule about when
to launch a browser.

## Boundaries

- A design plan or critique does not authorize code edits; implement only when
  the user asks for implementation or the task already authorizes it.
- Do not install fonts, packages or plugins, or fetch remote assets (including
  `design-taste-frontend`'s `assets.mjs`), without checking the project's
  constraints, network permission and each asset's licence.
- Do not replace an established design system because a source skill prefers a
  different aesthetic; adapt within it unless an overhaul is requested.
