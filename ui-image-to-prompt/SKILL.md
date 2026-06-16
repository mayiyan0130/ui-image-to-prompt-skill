---
name: ui-image-to-prompt
description: Convert static UI screenshots, game UI mockups, wireframes, generated UI results, or multiple UI reference images into precise image-generation/redraw prompts. Use when the user asks for UI 图转提示词, 根据 UI 图片生成提示词, 叠图提示词, 定向重绘提示词, UI overlay redesign, UI 布局优化, 从截图优化界面, 参考图重绘 UI, or asks how to adjust/redraw UI while preserving layout, characters, background, containers, buttons, icons, readability, and style. Not for UI screen-recording analysis; use video-ui-understanding for videos.
---

# UI Image to Prompt

Turn one or more UI images into a directly usable prompt for image editing, image generation, or design handoff. This skill is optimized for the repeated pattern: **current generated UI has layout/style problems; user provides screenshots/reference images; produce a strict overlay/redraw prompt that preserves assets and fixes UI layers.**

## Non-negotiable habits

1. **Establish image roles before writing.** For multiple images, explicitly label: current result / original source / target reference / partial style reference. Do not silently assume the order.
2. **Use partial references, not blind merging.** Say exactly which parts are borrowed from each reference: left buttons, bottom dialogue structure, right scroll panel, icon style, material, color, etc.
3. **Preserve first, then change.** Preserve background, aspect ratio, character identity, major architecture, and scene mood unless the user asks otherwise. Restrict changes to UI overlays when possible.
4. **Solve obstruction with anchors.** Convert “有遮挡” into anchor rules, safe text zones, non-overlap constraints, layer order, and spacing.
5. **Make prompts operational.** Avoid “更好看/高级/优化”; instead specify positions, dimensions, material, brightness, icon system, and forbidden artifacts.
6. **If corrected, regenerate cleanly.** If the user says “图1和图2顺序反了”, acknowledge and rewrite with corrected roles. Do not defend the previous prompt.
7. **Respect the user's production mode.** If they say “叠图”, write for layered overlay/editing and preserve source assets. If they say “重绘/生成”, write for image-generation redraw. If they ask “怎么给 Codex 描述”, write implementation-facing instructions instead of only visual prompts.
8. **Control text generation.** Image models often corrupt UI text. If exact wording matters, recommend leaving text areas clean/blank or adding final text in the design/game engine. Use placeholders such as “人物名/当前态度/评分点” unless the user provides final copy.

## Standard response shape

Use this structure unless the user asks for only a short prompt:

```markdown
### UI 问题判断
- ...

### 图像角色确认
- 图1：...
- 图2：...

### 定向重绘 / 叠图提示词
...

### 严格布局约束
1. ...

### Negative Prompt
...

### 短版提示词（可选）
...
```

When the user is iterating quickly, keep diagnosis short and make the prompt the main output.

## Workflow

### 1. Diagnose the visible UI problem

Look for:

- Panel overlap or UI elements squeezing each other.
- Characters placed in the center instead of anchored to sides.
- Central dialogue text blocked by character art, nameplates, rewards, or decorative frames.
- Bottom dialogue box too tall, too short, too wooden, too dark, or lacking an inner text container.
- Right evaluation/assessment panel too low, colliding with dialogue, too dirty/dark, or lacking scroll affordance.
- Left-side buttons inconsistent with reference layout.
- Small icons that look like one-off illustrations instead of reusable UI assets.
- AI artifacts: garbled text, white spreadsheet/table overlay, abnormal text blocks, non-game UI panels.

### 2. Define safe regions and anchors

Convert problems into layout rules:

- Top status bar: keep at top, do not cover it with panels.
- Left menu: vertical button stack near left side, uniform size and spacing.
- Right panel: upper-right anchor; bottom edge above the bottom dialogue box; leave visible gap.
- Bottom dialogue: wide and low; central text area must be clean.
- Characters: left character anchored near lower-left, right character anchored near lower-right; both may overlap outer dialogue frame but must not cover central text.
- Rewards/result strip: attach below or inside the lower part of the dialogue container without crowding text.

When helpful, use relative anchors instead of vague placement:

- Left character center near 12–18% of canvas width, anchored to bottom.
- Right character center near 78–88% of canvas width, anchored to bottom.
- Main text safe zone between the two characters; keep it free of portraits, nameplates, rewards, and decorative frames.
- Right panel bottom edge at least 15–30 px visually above the bottom dialogue box.
- Bottom dialogue box wide and low: increase width before increasing height.

### 3. Choose the output mode

- **Layered overlay / 叠图:** Emphasize layer order, preserved background/characters, UI container replacement, masks, opacity, and non-destructive editing.
- **Image-generation redraw / 定向重绘:** Emphasize reference-image roles, visual constraints, negative prompt, and style/material transfer.
- **Implementation handoff / 给 Codex 描述:** Emphasize UI hierarchy, component tree, anchors, masks, layout constraints, and verification checks.

### 4. Write a directed redraw prompt

Use a prompt with these parts:

1. Source preservation.
2. Role of each reference image.
3. UI module changes by region: left / right / bottom / top.
4. Material and color system.
5. Text handling policy: exact text, placeholder text, or blank text areas for later composition.
6. Icon reuse system.
7. Layer order.
8. Strict non-overlap constraints.

## Text handling rules

Use one of these policies explicitly:

- **Exact text required:** Tell the model to preserve existing text areas and typography positions, but recommend adding final text manually after generation if the image model cannot render accurate Chinese.
- **Placeholder text acceptable:** Use short labels like “人物名”, “当前态度”, “评分点”, “任务说明”, “奖励” and ask for clean readable placeholder blocks rather than long generated copy.
- **No text needed:** Ask for blank clean text containers, clear hierarchy, and no random/garbled characters.

Add to negative prompts when relevant: “乱码文字, 错误汉字, 随机外语, 假 UI 文本, 文字糊成一团”.

## Prompt templates from prior UI work

### A. Single-image layout repair prompt

Use when a generated UI has obstruction, wrong character placement, or cramped panels:

```text
基于当前 UI 截图进行 UI overlay redesign，只调整 UI 层级、容器位置和人物立绘摆放，不改变背景场景、人物身份、人物脸部特征、服装、整体画风和横版比例。

请重新规划界面安全区域，解决当前 UI 遮挡问题。右侧信息/评定面板锚定在画面右上区域，整体上移，面板底部必须高于底部主对话框的上沿，与底部对话框之间保留清晰间距，避免任何重叠。若内容较长，可在右侧面板边缘加入一条非常细、克制的竖向滚动条，颜色为浅棕或淡金色，不要过于现代。

底部主任务/对白对话框横向拉长，放在画面底部中央，宽度覆盖大部分底部区域，但高度适当降低，中央文本区域必须完整、干净、无遮挡。对话框不要使用厚重木质边框，改为平滑的浅米色纸张、绢布或淡玉色容器，边缘使用细金色/淡铜色描边，整体更明亮、更轻、更干净。

人物立绘分别锚定在底部对话框左右两端：左侧人物靠近左下角，右侧人物靠近右下角。两个人物只允许轻微压在外层对话框前方形成叠层关系，不能遮挡中央主要文字区域。右侧人物不能站在画面中间，也不能遮挡右侧信息/评定面板。

整体 UI 提高亮度，减少灰暗、脏旧和厚重木质感，使用暖白、浅米色、淡杏色、浅金色、淡铜色、低饱和棕色。小图标统一替换为可复用的细线古风图标系统，统一线宽、尺寸、边距和底座样式。文本区域保持干净，可使用短占位文字或留白，避免生成乱码。目标是层级清晰、无遮挡、可读、可复用、适合游戏 UI 落地。
```

### B. Two-image targeted redraw prompt

Use when one image is the current result and another is a target/reference. Replace bracketed fields:

```text
基于图A当前画面进行定向 UI 重绘和修正，保留图A的[背景/人物/人物脸部特征/服装/横版比例/主要构图]，移除图A中错误生成的[乱码表格/白色遮挡/异常文字块/错误 UI]。图B仅作为局部 UI 参考，用于[左侧按钮布局/底部双层对话框/右侧卷轴评定栏/图标风格]，不要把图B所有元素生硬混入图A，也不要继承图B中过暗、脏旧或厚重的材质。

请按照图B的布局逻辑重新叠加 UI：顶部状态栏保持清晰；左侧采用图B那种纵向功能按钮布局，按钮垂直排列、尺寸统一、间距一致，带统一细线古风图标和浅色装饰边框。

底部主对话框参考图B的“容器上再叠一层容器”结构：外层是横向拉长的大容器，覆盖底部主要区域；内层是更浅、更干净的文本内容容器，用来承载对白文字。外层容器承载人物立绘、姓名牌和边框装饰；内层文本框必须完整、干净、无遮挡。

右侧评定栏目参考图B的卷轴/布条结构，但整体比图B更明亮、更干净，不要脏旧暗沉。右侧评定栏做成悬挂式浅色布卷或纸卷，顶部和底部可有细卷轴横杆，边缘可有轻微垂坠布边、细绳或浅金装饰。材质使用暖白、浅米色、淡杏色，边缘为淡金、浅铜或浅棕描边。

人物立绘参考图B的摆放方式，分别锚定在底部左右两端。人物可以略微压在外层对话框前方形成叠图层次，但不能遮挡中央对白文本区；右侧人物不要站到画面中央，也不要遮挡右侧评定栏目。整体 UI 颜色更明亮、更清爽，避免灰暗、脏旧和厚重木质感。
```

### C. Corrected image-order prompt

Use this when the previous prompt assigned image roles backwards:

```text
修正图像角色：图1是需要被修正的当前结果，图2是目标 UI 布局和风格参考。基于图1进行定向 UI 重绘，保留图1的背景、人物素材、横版比例和主要构图；参考图2的左侧按钮结构、底部双层对话容器、右侧卷轴/布条评定栏。移除图1中的错误遮挡、乱码表格、白底表格、异常文字块和所有不属于游戏 UI 的内容。不要改变背景建筑和人物身份。
```

Then continue with template B.

### D. Implementation handoff prompt for Codex/game UI

Use when the user asks how to describe the UI to Codex or how to implement/check it:

```text
请按分层 UI 实现这个界面，不要把所有内容烘焙成一张不可维护的大图。保留背景图和人物立绘为独立层；UI 容器、按钮、右侧评定栏、底部对话框、文本、图标、奖励条分别作为可复用组件。

推荐层级：Background → Characters/Illustrations → TopStatusBar → LeftMenuButtons → BottomDialogueOuter → BottomDialogueInnerTextPanel → Nameplates → RewardStrip → RightAssessmentScrollPanel → Text/Icon overlays。

检查点：人物不能遮挡中央对白文本；右侧评定栏不能压到底部对话框；底部对话框必须有外层容器和内层文本容器；左侧按钮尺寸和间距统一；图标来自同一套可复用图标系统；文本不要依赖图片生成，最终文案应由 UI 文本组件渲染。
```

## Region-specific guidance

### Bottom dialogue box: double-layer container

Use this when the user asks for “容器上再叠一层容器”:

- Outer container: long horizontal base across the bottom, light beige / silk / smooth paper, thin gold/copper outline, soft shadow.
- Inner container: inset lighter text card, cleaner and smoother, with subtle transparency, used only for dialogue text.
- Nameplate: small, near character-dialogue intersection; do not let it dominate.
- Characters: overlap outer container only; never cover central text.
- Avoid: one flat box, thick wood, dirty parchment, over-decorated frames.

### Right evaluation panel / scroll panel

Use for “评定栏目 / 右侧容器 / 卷轴布条”:

- Anchor upper-right; keep above bottom dialogue box.
- Make it a hanging cloth-paper scroll: top/bottom thin rods, slight cloth edges, fine cords, light gold details.
- Keep it bright and clean: warm white, light beige, pale apricot; not black-brown, not stained, not old parchment.
- Internal sections: name, current attitude/status, score points, keywords, influence/explanation.
- Add a very thin vertical scrollbar only when scrollability is requested.

### Left-side buttons

Use for “参考左侧按钮布局”:

- Four vertical buttons unless the image clearly needs a different count.
- Uniform size, spacing, padding, border, and icon style.
- Light paper/silk base, fine gold/copper/brown outline.
- Icons: reusable thin-line ancient-style symbols, not unique mini illustrations.

### Reusable icon system

When icons look non-reusable, state:

```text
将零散复杂的小图标统一替换为一套可复用图标系统：统一线宽、统一尺寸、统一边距、统一底座形状。图标风格为单色细线古风图标、印章式符号或半填充古风符号，适用于顶部状态栏、左侧菜单、底部奖励栏和小按钮。避免每个按钮都使用完全不同的复杂小插画。
```

## Layer order for overlay prompts

Choose the order based on the intended visual overlap.

For **image-editing overlay prompts** where portraits should sit on top of the bottom frame:

```text
画面层级顺序：背景原图 → 顶部状态栏 → 左侧按钮 → 右侧评定卷轴栏 → 底部外层对话容器 → 底部内层文本容器 → 左右人物立绘 → 姓名牌 → 底部奖励/结果条 → 细节装饰与统一提亮层。
```

For **game implementation handoff**, keep assets independent and control z-index explicitly:

```text
实现层级：Background → CharacterSprites → TopStatusBar → LeftMenu → BottomDialogueOuter → BottomDialogueInnerTextPanel → Nameplates → RewardStrip → RightAssessmentPanel → Text/IconOverlay。
```

If characters should appear behind some UI pieces, adjust the order explicitly. If the right panel must visually sit above all lower UI, place it after the bottom containers and state that it cannot overlap the text safe zone.

## Negative Prompt bank

Pick only relevant terms:

```text
乱码文字，错误文字，错误汉字，随机外语，假 UI 文本，文字糊成一团，白色 Excel 表格，电子表格遮挡，异常文字块，非游戏 UI 的表格，画面中央大面积白底遮挡，修改人物脸部，人物五官变形，人物遮挡对白文字，右侧人物位于画面中央，右侧评定栏遮挡底部对话框，UI 元素互相挤压，布局拥挤，厚重木质边框，脏旧暗沉羊皮纸，黑褐色破旧卷轴，发黑发黄的旧纸，高饱和霓虹色，现代科幻 UI，复杂不可复用小图标，按钮尺寸不统一，图标线宽不统一，边框风格混乱，底部容器只有一层，缺少内层文本卡片，过度装饰，中央文字区不可读
```

## Quality checklist before final answer

Before replying, ensure the answer includes:

- Correct image roles, especially current result vs target reference.
- Preservation instructions for background, characters, aspect ratio, and mood.
- Concrete layout moves: up/down/left/right, anchor, gap, width/height tendency.
- Non-overlap rules for right panel, bottom dialogue, characters, and text area.
- Material/color changes with brightness and dirtiness controlled.
- Text handling policy: exact text vs placeholder vs blank text boxes.
- Reusable icon system if icons are mentioned.
- Negative Prompt with the likely failure modes.
- Short version if the user likely needs to paste into a model quickly.
