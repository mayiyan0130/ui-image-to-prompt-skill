# ui-image-to-prompt

Portable OpenClaw AgentSkill for converting static UI screenshots, game UI mockups, generated UI results, and multi-image UI references into production-ready overlay/redraw prompts and implementation handoff instructions.

## What it does

- Turns UI screenshots into precise prompt text for image editing or redraw.
- Handles multi-image roles: current result, source image, target reference, and partial style reference.
- Produces constraints for layered overlay workflows, generated redraw workflows, and Codex/game-UI implementation handoff.
- Includes safeguards for common production failures: overlapping panels, misplaced character sprites, corrupted UI text, dirty/dark inherited materials, inconsistent icons, and non-reusable UI assets.

## Install

### Option A: copy the skill folder

Copy `ui-image-to-prompt/` into your OpenClaw skills directory, for example:

```text
~/.openclaw/workspace/skills/ui-image-to-prompt/
```

Then restart or refresh the OpenClaw session so the skill list reloads.

### Option B: use the packaged skill

Download:

```text
dist/ui-image-to-prompt.skill
```

The `.skill` file is a zip archive containing:

```text
ui-image-to-prompt/SKILL.md
```

If your OpenClaw install supports importing `.skill` files, import it directly. Otherwise, unzip it into the skills directory.

## Verify

After installation, ask naturally:

```text
这张游戏 UI 有遮挡，帮我写一个叠图提示词。
```

The agent should load `ui-image-to-prompt` and return a structured answer with image roles, layout constraints, a redraw/overlay prompt, and a Negative Prompt.

## Package integrity

SHA256:

```text
c2f3800e89a865f53f2337fde26e4df069643acaed6e4254fd9a70331fb1621f  dist/ui-image-to-prompt.skill
```

## Compatibility

- Designed for static UI screenshots and UI reference images.
- Not for UI screen-recording analysis; use a video UI understanding skill for videos.
- Contains no machine-specific paths, secrets, or external dependencies.
