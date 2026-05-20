---
description: Workflow for calculating correct padding to achieve target element sizes
---

# Calculate Padding Workflow

> **Context**: Use this when you need an element (like a button or icon container) to be a specific exact size, but you must achieve it using padding around content.

## Steps

1. **Determine Target Size**
    * Check Figma/Design specs or Accessibility standards.
    * *Common*: 48dp (Touch), 40dp (Button), 32dp (Chip).

2. **Determine Content Size**
    * Identify the size of the inner element (Icon, Text height, etc.).
    * *Common*: 24dp (Icon.Md), 16dp (Icon.Xs).

3. **Apply Formula**
    * Calculate: `(Target Size - Content Size) / 2`

4. **Select Dimension Token**
    * Find the closest match in `Dimensions.kt`.
    * If result is 12dp -> `Dimensions.Spacing.Sm`
    * If result is 8dp -> `Dimensions.Spacing.Xs`

5. **Implement**
    * Apply `.padding(Dimensions.Spacing.X)` to the container.

---

## Quick Lookup

| Target | Content | Padding | Token |
|---|---|---|---|
| 48dp | 24dp | 12dp | `Spacing.Sm` |
| 40dp | 24dp | 8dp | `Spacing.Xs` |
| 32dp | 16dp | 8dp | `Spacing.Xs` |
