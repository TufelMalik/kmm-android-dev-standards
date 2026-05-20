---
description: Adding theme colors
---

# Theme Colors Setup

> All colors in `theme/Color.kt`. Use `MaterialTheme.colorScheme` in UI.

## Step 1: Add Color to Color.kt

```kotlin
// theme/Color.kt
object ThemeColors {
    val primary = Color(0xFF6750A4)
    val onPrimary = Color(0xFFFFFFFF)
    // Add new semantic colors here
}
```

## Step 2: Update Theme.kt

```kotlin
// theme/Theme.kt
private val LightColorScheme = lightColorScheme(
    primary = ThemeColors.primary,
    onPrimary = ThemeColors.onPrimary
)
```

## Step 3: Use in UI

```kotlin
Text(color = MaterialTheme.colorScheme.primary)
```

## Rules

> **Must follow:** [color-declaration-rules](../rules/color-declaration-rules.md) | [dimensions-rules](../rules/dimensions-rules.md)
