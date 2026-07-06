---
name: "q-character-generator"
description: "Create a consistent Q-version 3-heads-tall cartoon character package from one uploaded reference image. Use when the user asks for Q版形象生成, Q-version character generation, a chibi three-view sheet, or a consistent sticker pack: generate a three-view character sheet, generate 8 independent reaction stickers from that sheet, write a character consistency anchor Markdown file, verify the stickers against the anchor, ask the user for confirmation, then package the approved stickers and Markdown files."
---

# Q版形象生成

Use this skill when the user uploads one character reference image and wants a reusable Q-version cartoon character workflow: 3-heads-tall three-view design, 8 independent stickers, a character anchor Markdown file, consistency verification, user confirmation, and final packaging.

This skill produces bitmap assets. Use the built-in `image_gen` tool by default. Do not use CLI fallback unless the user explicitly asks for CLI/API/model control.

## Inputs

Required:

- One uploaded reference image of the character.

Optional:

- Sticker text, if the user explicitly wants text in the images.
- Output directory name.
- Sticker emotion list, if the user wants a different set than the default 8.
- Transparent background request.

Default assumptions:

- No in-image text, because generated Chinese text can be unreliable.
- Clean light background unless transparency is requested.
- The three-view sheet is the primary visual anchor for the sticker set.
- Final packaging waits for explicit user confirmation.

## Output Structure

Create a project-local output directory:

```text
output/q-character-generator-<short-slug>/
```

Use stable filenames:

```text
00-three-view.png
01-happy-wave.png
02-thumbs-up.png
03-thinking.png
04-shocked.png
05-working-hard.png
06-crying.png
07-angry-cute.png
08-salute-received.png
character-anchor.md
consistency-check.md
```

After user confirmation, create:

```text
q-character-generator-<short-slug>.zip
```

The zip must include the 8 stickers, `00-three-view.png`, `character-anchor.md`, and `consistency-check.md`.

## Workflow

1. Inspect the uploaded reference image.
2. Identify stable character traits:
   - face shape
   - hairstyle or headwear
   - eye style
   - clothing
   - accessories
   - color palette
   - age category and general identity
3. Generate one 3-heads-tall Q-version cartoon three-view sheet.
4. Save the selected three-view image as `00-three-view.png`.
5. Write `character-anchor.md` from the three-view image and original reference.
6. Generate 8 independent sticker images using the three-view and anchor rules.
7. Save each sticker with the stable filenames listed above.
8. Verify each sticker against `character-anchor.md`.
9. Write `consistency-check.md`.
10. Show the user the output directory and ask for confirmation.
11. Only after confirmation, zip the deliverables.

Do not zip before the user confirms.

## Three-View Generation Prompt Template

Use the uploaded image as identity reference only. Generate a new cartoon design sheet.

```text
Use case: stylized-concept
Asset type: character design sheet / Q-version 3-heads-tall cartoon three-view turnaround
Primary request: Based on the uploaded reference image, create a cute Q-version 3-heads-tall cartoon character, shown as a clean three-view model sheet.
Input images: Uploaded image is the character identity and outfit reference. Preserve the most recognizable facial, hairstyle/headwear, clothing, accessory, and color cues.
Subject: one stylized cartoon version of the referenced character, 3-heads-tall proportions, oversized head, compact body, small hands and feet, friendly readable expression.
Style/medium: polished 2D anime mascot / chibi character sheet, clean dark outline, soft cel shading, professional design turnaround.
Composition/framing: three full-body views placed side by side in one image: front view, strict side view, back view. Equal scale, aligned feet, generous spacing, full body visible in every view.
Scene/backdrop: clean warm off-white or very light neutral studio background, no environment.
Constraints: no text labels, no watermark, no logos, no extra characters, no props unless present in the reference and important to identity. Keep all three views consistent in outfit, proportions, hairstyle/headwear, accessories, and character identity. Strongly preserve 3-heads-tall chibi proportions; do not make the character 5-heads-tall or realistic.
```

If the generated sheet is not clearly 3-heads-tall or the three views are inconsistent, regenerate once with stricter proportion and consistency language before continuing.

## Character Anchor Markdown Template

Create `character-anchor.md` after the three-view sheet is selected.

```markdown
# Character Consistency Anchor

## Source

- Original reference image: user-uploaded image.
- Three-view anchor image: `00-three-view.png`.

## Identity

- <age/category, gender presentation if visually apparent, general character type>
- <face shape and skin tone>
- <eyes and eyebrows>
- <hair/headwear>
- <baseline expression>

## Outfit

- <top>
- <bottom>
- <shoes>
- <important accessories>
- <important colors>

## Drawing Style

- Polished 2D anime mascot / chibi sticker style.
- Clean dark outline.
- Soft cel shading.
- 3-heads-tall Q-version proportions.
- Cute, friendly, and consistent with `00-three-view.png`.

## Hard Consistency Rules

- Keep the same character identity, outfit, headwear/hairstyle, accessories, and color palette.
- Keep 3-heads-tall chibi proportions.
- Do not switch to 5-heads-tall, realistic adult anatomy, 3D toy rendering, flat vector redesign, or photorealism.
- Do not add unrelated props, extra characters, logos, watermarks, or text unless explicitly requested.
- Do not change age category, species, or role.

## Sticker Set

1. Happy wave
2. Thumbs up
3. Thinking
4. Shocked
5. Working hard
6. Crying
7. Angry but cute
8. Salute / received

## Consistency Checklist

For each generated sticker, verify:

- Same character as `00-three-view.png`.
- Same headwear/hairstyle.
- Same face shape, eye style, and color palette.
- Same outfit and important accessories.
- Same polished 2D chibi mascot style.
- No unwanted text, watermark, logo, extra character, or unrelated prop.
```

Keep this file specific. Replace placeholders with concrete observations from the generated three-view sheet and original reference image.

## Sticker Generation

Generate one image per sticker. Do not generate a contact sheet. Each sticker must be independent.

Default sticker set:

1. Happy wave
2. Thumbs up
3. Thinking
4. Shocked
5. Working hard
6. Crying
7. Angry but cute
8. Salute / received

Use the same base prompt for all 8, changing only the pose/expression line.

```text
Use case: stylized-concept
Asset type: independent Q-version chibi reaction sticker
Primary request: Generate sticker <N> of 8: <emotion/action>, using `00-three-view.png` and `character-anchor.md` as strict character consistency references.
Input images: The uploaded original image and `00-three-view.png` are identity references. Preserve the same character from the three-view sheet.
Subject: same Q-version 3-heads-tall cartoon character described in `character-anchor.md`; preserve headwear/hairstyle, face shape, eye style, outfit, accessories, color palette, and polished 2D anime mascot style.
Pose/expression: <emotion/action-specific instruction>.
Composition/framing: single standalone sticker centered on a clean light background, generous padding, full body if it preserves the 3-heads-tall design; otherwise upper body is acceptable when it better preserves identity.
Style/medium: polished 2D anime mascot / chibi sticker, clean dark outline, soft cel shading, consistent with `00-three-view.png`.
Constraints: no text, no watermark, no logo, no extra character, no unrelated props, no scene background. Do not change outfit, headwear/hairstyle, accessories, or color palette. Do not make it 5-heads-tall, realistic, 3D, or flat vector.
```

Pose/expression lines:

- Happy wave: cheerful smile, one hand raised and waving.
- Thumbs up: confident gentle smile, one hand giving a clear thumbs-up.
- Thinking: thoughtful expression, one hand touching chin, eyes looking slightly upward.
- Shocked: wide eyes, small open mouth, hands raised near cheeks.
- Working hard: determined focused expression, one fist clenched near chest, small sweat drop allowed.
- Crying: watery eyes, tears down cheeks, trembling mouth, hands near chest.
- Angry but cute: cute angry pout, cheeks slightly puffed, brows lowered, arms crossed.
- Salute / received: attentive friendly expression, one hand saluting at the brim or temple, slight smile.

## Consistency Verification

Inspect every generated sticker against `character-anchor.md`.

Create `consistency-check.md`:

```markdown
# Sticker Consistency Check

Anchor file: `character-anchor.md`
Reference image: `00-three-view.png`

## Result

<Approved for user review / Needs regeneration>

## Checked Files

| File | Intended emotion/action | Character consistency | Notes |
| --- | --- | --- | --- |
| `01-happy-wave.png` | Happy wave | Pass/Fail | <short note> |
| `02-thumbs-up.png` | Thumbs up | Pass/Fail | <short note> |
| `03-thinking.png` | Thinking | Pass/Fail | <short note> |
| `04-shocked.png` | Shocked | Pass/Fail | <short note> |
| `05-working-hard.png` | Working hard | Pass/Fail | <short note> |
| `06-crying.png` | Crying | Pass/Fail | <short note> |
| `07-angry-cute.png` | Angry but cute | Pass/Fail | <short note> |
| `08-salute-received.png` | Salute / received | Pass/Fail | <short note> |

## Checklist

- Same character identity: pass/fail.
- Same headwear/hairstyle: pass/fail.
- Same outfit and important accessories: pass/fail.
- Same color palette: pass/fail.
- Same 2D chibi mascot style: pass/fail.
- No unwanted text, watermark, logo, extra character, or unrelated prop: pass/fail.

## Packaging Status

Not packaged yet. Waiting for user confirmation before creating the final archive.
```

If any sticker fails a hard consistency rule, regenerate only that sticker with a targeted prompt. Do not regenerate the entire set unless multiple images fail for the same systemic reason.

## User Confirmation

After verification, show:

- output directory path
- preview links or filenames for the 8 stickers
- `character-anchor.md`
- `consistency-check.md`

Ask the user to confirm before packaging.

Use concise wording:

```text
我已生成并按锚点检查完成，暂时还没打包。请确认这 8 张表情包和三视图是否可以作为最终版；你确认后我再压缩交付。
```

## Packaging

Only after explicit user confirmation:

1. Ensure the final folder contains:
   - `00-three-view.png`
   - `01-happy-wave.png`
   - `02-thumbs-up.png`
   - `03-thinking.png`
   - `04-shocked.png`
   - `05-working-hard.png`
   - `06-crying.png`
   - `07-angry-cute.png`
   - `08-salute-received.png`
   - `character-anchor.md`
   - `consistency-check.md`
2. Create a zip beside the output folder.
3. Report the zip path.

Do not delete source images or generated originals unless the user explicitly asks.

## Transparent Background Requests

If the user requests transparent stickers, follow the `imagegen` skill's built-in-first chroma-key workflow:

- Generate on a perfectly flat chroma-key background.
- Remove the background locally using the installed chroma-key helper.
- Validate alpha before packaging.

Do not switch to CLI native transparency without explicit user confirmation.
