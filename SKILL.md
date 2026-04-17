---
name: svg-cover-studio
description: Create or refine local SVG cover images, article headers, social cards, and tech-style visual assets. Use when the task is to generate an SVG cover, convert a title into a polished visual, adapt an existing SVG into another aspect ratio, or validate/fix SVG quality for Codex, OpenClaw, Hermes, Claude, Gemini, or other local agents.
---

# SVG Cover Studio

Use this skill when the user wants a cover image, article header, social card, visual title graphic, or other code-generated SVG asset that should be produced locally and edited as files.

This skill is a local, agent-agnostic adaptation of the upstream `svg-architect` project. Keep the parts that are durable:

- clear scene sizing
- high-readability title composition
- tech-dark and light visual specs
- SVG validation and repair scripts

Do not assume Gemini CLI, extension installs, or any fixed machine-specific output path.

## Quick workflow

1. Identify the scene:
   - `wechat_cover`: `1800x766`
   - `article_16_9`: `1920x1080`
   - `x_card` or generic card: `1200x630`
   - custom ratio if the user explicitly asks
2. Identify the visual mode:
   - `tech_dark` for dark, neon, engineering style
   - `apple_light` for clean, bright, restrained style
3. Distill the content:
   - one main title
   - optionally one short subtitle
   - at most one compact supporting panel or data motif
4. Generate or edit the SVG locally.
5. Validate if needed with `scripts/svg_validator.py`.
6. If exporting to PNG is requested, use `scripts/optimize_and_convert.py` or a local conversion path.

## Design rules

- Prioritize thumbnail readability over decorative detail.
- Prefer one strong idea per graphic.
- Keep at least roughly 35% to 40% visual breathing room.
- For cover art, large title text should dominate.
- Avoid cluttered grids, dense labels, or tiny explanatory copy unless the user asked for an infographic.
- Use ASCII in file names unless the user explicitly wants otherwise.

## Layout defaults

### WeChat cover

- Size: `1800x766`
- Use a wide composition with generous side margins.
- Keep important text away from the far left and far right edges.

### 16:9 article cover

- Size: `1920x1080`
- Use a two-zone layout:
  - left or center-left for the headline
  - right side for one compact visual panel, chart, or motif

### 4:3 cover

- Common working size: `1600x1200`
- Allow more vertical stacking than 16:9.

## Style selection

Read the matching reference only when needed:

- Tech dark: [references/tech_dark.md](/Users/paul/.codex/skills/svg-cover-studio/references/tech_dark.md)
- Apple light: [references/apple_light.md](/Users/paul/.codex/skills/svg-cover-studio/references/apple_light.md)

Use `tech_dark` by default for topics like:

- AI
- coding
- agents
- APIs
- architecture
- developer tools

Use `apple_light` for topics like:

- product explainers
- strategy
- editorial visuals
- calm premium brand tone

## Reusable assets

- Template starter: [templates/article_cover_tech.svg](/Users/paul/.codex/skills/svg-cover-studio/templates/article_cover_tech.svg)
- Prompt structure reference: [references/prompt_template.md](/Users/paul/.codex/skills/svg-cover-studio/references/prompt_template.md)
- Platform sizes: [resources/platform_profiles.json](/Users/paul/.codex/skills/svg-cover-studio/resources/platform_profiles.json)

## Validation and repair

Use these scripts when the asset is important, reused, or exported broadly:

- Doctor environment:
  - `python3 /Users/paul/.codex/skills/svg-cover-studio/scripts/setup_doctor.py`
- Validate SVG:
  - `python3 /Users/paul/.codex/skills/svg-cover-studio/scripts/svg_validator.py --svg <file> --json`
- Repair common issues:
  - `python3 /Users/paul/.codex/skills/svg-cover-studio/scripts/svg_fixer.py --svg <file> --errors E_A11Y_MISSING`
- Optimize and optionally export PNG:
  - `python3 /Users/paul/.codex/skills/svg-cover-studio/scripts/optimize_and_convert.py <file.svg> --format all`

## Important local adaptation notes

- Never write files to a hard-coded author-specific path.
- Save outputs in the current working directory unless the user requested another path.
- Do not require Gemini extensions or any specific agent runtime.
- These scripts are helpers; direct SVG editing is still allowed and often faster for one-off cover generation.

## When adapting an existing SVG

- Preserve the title hierarchy first.
- Reflow supporting panels instead of scaling everything uniformly.
- If converting ratios, keep the headline block optically balanced rather than mathematically centered at all costs.
- Remove details before shrinking type.

## Deliverables

When you complete a request with this skill, prefer delivering:

- the SVG source file
- a PNG export if asked
- one short note on what visual direction was chosen
