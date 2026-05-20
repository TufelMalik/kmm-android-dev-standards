# UI Implementation Rules

> Convert UI images to EXACT pixel-perfect Compose code. No approximations.

> [!CAUTION]
> **NON-NEGOTIABLE**: UI output MUST match the input image exactly. Every spacing, color, size, radius, alignment must be precise.

## Mandatory Extraction (Before Coding)

| Extract | Action |
|---------|--------|
| **Colors** | Use color picker → add to `Color.kt` with exact hex |
| **Spacing** | Measure pixels → map to `Dimensions.Spacing.X` |
| **Radius** | Measure corners → map to `Dimensions.Corner.X` |
| **Icons** | Identify size → use `Dimensions.Icon.X` |
| **Typography** | Match to `MaterialTheme.typography.X` |
| **Text** | Add ALL text to `strings.xml` first |

## Hardcode Rules

| ❌ NEVER | ✅ ALWAYS |
|----------|-----------|
| `16.dp` | `Dimensions.Spacing.Md` |
| `Color(0xFF...)` | `MaterialTheme.colorScheme.X` or `ThemeColors.X` |
| `"Login"` | `stringResource(R.string.action_login)` |
| `24.dp` icon | `Dimensions.Icon.Md` |
| `12.dp` radius | `Dimensions.Corner.Md` |

## Layout Precision

```kotlin
// ❌ WRONG - Guessing values
Column(modifier = Modifier.padding(20.dp)) { ... }

// ✅ CORRECT - Using design system
Column(modifier = Modifier.padding(Dimensions.Spacing.Lg)) { ... }
```

## Spacing Reference

| Visual Gap | Dimension |
|------------|-----------|
| 4dp (tiny) | `Dimensions.Spacing.Xxs` |
| 8dp (small) | `Dimensions.Spacing.Xs` |
| 12dp | `Dimensions.Spacing.Sm` |
| 16dp (default) | `Dimensions.Spacing.Md` |
| 24dp (large) | `Dimensions.Spacing.Lg` |
| 32dp | `Dimensions.Spacing.Xl` |
| 48dp (extra) | `Dimensions.Spacing.Xxl` |

## Icon Size Reference

| Visual Size | Dimension |
|-------------|-----------|
| 16dp | `Dimensions.Icon.Xs` |
| 20dp | `Dimensions.Icon.Sm` |
| 24dp | `Dimensions.Icon.Md` |
| 40dp | `Dimensions.Icon.Lg` |
| 48dp | `Dimensions.Icon.Xl` |

## Corner Radius Reference

| Visual Curve | Dimension |
|--------------|-----------|
| 4dp | `Dimensions.Corner.Xs` |
| 8dp | `Dimensions.Corner.Sm` |
| 12dp | `Dimensions.Corner.Md` |
| 16dp | `Dimensions.Corner.Lg` |
| 28dp | `Dimensions.Corner.Xl` |
| Pill/Circle | `Dimensions.Corner.Full` |

## ✅ DO

- Measure EVERY spacing from image
- Use EXACT colors from image (color picker)
- Match typography style precisely
- Verify alignment matches image
- Create reusable components
- Add `@Preview` to every component/screen
- Update `PreviewScreen.kt` with new screens

## ❌ DON'T

- Don't guess spacing values
- Don't approximate colors
- Don't hardcode any `.dp`, color, or string
- Don't skip preview creation
- Don't use generic layouts - match image structure
- Don't invent names like `Size20` - use `Xs/Sm/Md/Lg/Xl`

## Final Verification

Before completing, verify:

- [ ] All colors match image exactly
- [ ] All spacing matches image exactly
- [ ] All text from `strings.xml`
- [ ] All dimensions from `Dimensions` object
- [ ] Alignment matches image
- [ ] `@Preview` added
- [ ] `PreviewScreen.kt` updated
