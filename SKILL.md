---
name: "q-character-generator"
description: "Create a consistent Q-version 3-heads-tall cartoon character delivery folder from one uploaded reference image, with a normal cute head size and no over-enlarged head. Use when the user asks for Q版形象生成, Q-version character generation, a chibi three-view sheet, or a consistent sticker pack: generate a balanced 3-heads-tall three-view character sheet, generate 8 independent reaction stickers from that sheet, write a character consistency anchor Markdown file, verify the stickers against the anchor, ask the user for confirmation, then organize the approved stickers and Markdown files into one ordinary delivery folder."
---

# Q版形象生成

Use this skill when the user uploads one character reference image and wants a reusable Q-version cartoon character workflow: balanced 3-heads-tall three-view design, 8 independent stickers, a character anchor Markdown file, consistency verification, user confirmation, and final folder delivery.

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
- Use balanced 3-heads-tall Q-version proportions. The head should be about one third of the total height, cute but not oversized; keep enough torso and leg length so the character does not look like a giant-head mascot.
- Preserve the reference character's recognizable body silhouette, outfit fit, and identity cues within the 3-heads-tall constraint.
- Final delivery waits for explicit user confirmation.
- "Package" means collect the final files into one ordinary folder. Do not create a `.zip` or other compressed archive unless the user explicitly asks for a compressed file.

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

After user confirmation, deliver the same output directory as the final delivery folder. It must contain the 8 stickers, `00-three-view.png`, `character-anchor.md`, and `consistency-check.md`.

## Workflow

1. Inspect the uploaded reference image.
2. Identify stable character traits:
   - face shape
   - hairstyle or headwear
   - eye style
   - clothing
   - accessories
   - color palette
   - body silhouette and outfit fit
   - age category and general identity
3. Generate one balanced 3-heads-tall Q-version cartoon three-view sheet.
4. Save the selected three-view image as `00-three-view.png`.
5. Write `character-anchor.md` from the three-view image and original reference.
6. Generate 8 independent sticker images using the three-view and anchor rules.
7. Save each sticker with the stable filenames listed above.
8. Verify each sticker against `character-anchor.md`.
9. Write `consistency-check.md`.
10. Show the user the output directory and ask for confirmation.
11. Only after confirmation, finalize the output directory as the delivery folder.

Do not create a zip before or after user confirmation unless the user explicitly asks for a compressed archive.

## Three-View Generation Prompt Template

Use the uploaded image as identity reference only. Generate a new cartoon design sheet.

```text
Use case: stylized-concept
Asset type: character design sheet / Q-version 3-heads-tall cartoon three-view turnaround with balanced proportions
Primary request: Based on the uploaded reference image, create a cute Q-version 3-heads-tall cartoon character with a normal cute head size, shown as a clean three-view model sheet.
Input images: Uploaded image is the character identity, outfit, body silhouette, and color reference. Preserve the most recognizable facial, hairstyle/headwear, clothing, accessory, body silhouette, outfit fit, and color cues.
Subject: one stylized cartoon version of the referenced character, balanced 3-heads-tall proportions, head about one third of total height, compact but readable torso and legs, friendly readable expression.
Style/medium: polished 2D anime mascot / chibi character sheet, clean dark outline, soft cel shading, professional design turnaround.
Composition/framing: three full-body views placed side by side in one image: front view, strict side view, back view. Equal scale, aligned feet, generous spacing, full body visible in every view.
Scene/backdrop: clean warm off-white or very light neutral studio background, no environment.
Constraints: no text labels, no watermark, no logos, no extra characters, no props unless present in the reference and important to identity. Keep all three views consistent in outfit, proportions, hairstyle/headwear, accessories, and character identity. Strongly preserve balanced 3-heads-tall proportions; the head should not be oversized beyond one third of total height, and the body should not be compressed into a tiny torso. Do not make the character 5-heads-tall, realistic, 3D, or photorealistic.
```

If the generated sheet is not clearly balanced 3-heads-tall, has an oversized head, or the three views are inconsistent, regenerate once with stricter proportion and consistency language before continuing.

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
- Balanced 3-heads-tall Q-version proportions.
- Head is cute but normal for 3-heads-tall design, about one third of total height, not oversized.
- Cute, friendly, and consistent with `00-three-view.png`.

## Hard Consistency Rules

- Keep the same character identity, outfit, headwear/hairstyle, accessories, and color palette.
- Keep balanced 3-heads-tall proportions from `00-three-view.png`.
- Keep the head size normal for a 3-heads-tall character; do not enlarge the head beyond about one third of total height.
- Preserve the reference character's recognizable body silhouette and outfit fit within the 3-heads-tall design.
- Do not switch to 5-heads-tall, giant-head mascot proportions, realistic adult anatomy, 3D toy rendering, flat vector redesign, or photorealism.
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
- Same balanced 3-heads-tall proportions with normal head size.
- Same recognizable body silhouette and outfit fit.
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
Subject: same Q-version 3-heads-tall cartoon character described in `character-anchor.md`; preserve headwear/hairstyle, face shape, eye style, outfit, accessories, balanced 3-heads-tall proportions, normal cute head size, body silhouette, color palette, and polished 2D anime mascot style.
Pose/expression: <emotion/action-specific instruction>.
Composition/framing: single standalone sticker centered on a clean light background, generous padding, full body if it preserves the balanced 3-heads-tall design; otherwise upper body is acceptable when it better preserves identity.
Style/medium: polished 2D anime mascot / chibi sticker, clean dark outline, soft cel shading, consistent with `00-three-view.png`.
Constraints: no text, no watermark, no logo, no extra character, no unrelated props, no scene background. Do not change outfit, headwear/hairstyle, accessories, balanced 3-heads-tall proportions, normal head size, body silhouette, or color palette. Do not make the head too large, do not compress the body into a tiny torso, and do not make it 5-heads-tall, realistic, 3D, or flat vector.
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
- Same balanced 3-heads-tall proportions: pass/fail.
- Normal cute head size, not oversized: pass/fail.
- Same color palette: pass/fail.
- Same 2D chibi mascot style: pass/fail.
- No unwanted text, watermark, logo, extra character, or unrelated prop: pass/fail.

## Delivery Status

Not finalized yet. Waiting for user confirmation before declaring the output folder as the final delivery folder.
```

If any sticker fails a hard consistency rule, regenerate only that sticker with a targeted prompt. Do not regenerate the entire set unless multiple images fail for the same systemic reason.

## User Confirmation

After verification, show:

- output directory path
- preview links or filenames for the 8 stickers
- `character-anchor.md`
- `consistency-check.md`

Ask the user to confirm before final folder delivery.

Use concise wording:

```text
我已生成并按锚点检查完成，暂时还没作为最终交付文件夹确认。请确认这 8 张表情包和三视图是否可以作为最终版；你确认后我会把这些文件整理在同一个文件夹里交付，不生成压缩包。
```

## Final Folder Delivery

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
2. Keep these files together in the same output directory as the final delivery folder.
3. Report the final folder path.

Do not create `.zip`, `.tar`, `.7z`, `.rar`, or any other compressed archive unless the user explicitly asks for a compressed file.

Do not delete source images or generated originals unless the user explicitly asks.

## Transparent Background Requests

If the user requests transparent stickers, follow the `imagegen` skill's built-in-first chroma-key workflow:

- Generate on a perfectly flat chroma-key background.
- Remove the background locally using the installed chroma-key helper.
- Validate alpha before final folder delivery.

Do not switch to CLI native transparency without explicit user confirmation.
