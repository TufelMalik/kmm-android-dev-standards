---
description: Implementing UI from image/Figma to Compose code
---

# UI Implementation from Image

> **Context**: Converting UI design image to pixel-perfect Compose code.

> [!CAUTION]
> **MANDATORY**: Follow EVERY step. UI MUST match the image EXACTLY. No approximations allowed.

## Pre-Implementation Checklist

- [ ] Analyze image: spacing, colors, typography, icons, radius
- [ ] Check/Create `Dimensions.kt` exists
- [ ] Check/Create `Color.kt` with exact colors from image
- [ ] Check/Create `strings.xml` with all text content
- [ ] Identify all components needed

## Implementation Steps

1. **Extract Design Tokens** (MANDATORY)
   - Measure all spacing/padding from image
   - Extract exact colors (use color picker)
   - Identify typography styles
   - Note corner radius values
   - Note icon sizes

2. **Setup Theme Files FIRST**
   - Add colors to `Color.kt` (exact hex values)
   - Add dimensions to `Dimensions.kt` (use `Dimensions.Spacing.X`)
   - Add strings to `strings.xml`

3. **Build Components**
   - Create reusable components in `uiToolKit/`
   - Add `@Preview` to each component
   - Use theme values, NEVER hardcode

4. **Assemble Screen**
   - Use exact layout structure from image
   - Apply precise padding/margin from `Dimensions`
   - Match alignment exactly (start, center, end)
   - Update `PreviewScreen.kt`

## Alignment Rules

| Image Shows | Use |
|-------------|-----|
| Left aligned | `Arrangement.Start`, `Alignment.Start` |
| Center | `Arrangement.Center`, `Alignment.CenterHorizontally` |
| Right aligned | `Arrangement.End`, `Alignment.End` |
| Space between items | `Arrangement.SpaceBetween` |
| Equal spacing | `Arrangement.SpaceEvenly` |

## Must Follow Rules

> **Must follow:** [dimensions-rules](../rules/dimensions-rules.md) | [preview-screen-rules](../rules/preview-screen-rules.md) | [ui-implementation-rules](../rules/ui-implementation-rules.md)
