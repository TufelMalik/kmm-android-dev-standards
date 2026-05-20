# Rule 2: Color Management & Design System

> **Official References:**  
> - [Material Design 3 Color System](https://m3.material.io/styles/color/the-color-system/color-roles)
> - [Material Theme Builder](https://material-foundation.github.io/material-theme-builder/)
> - [Uber Design System (Base/Tokens)](https://baseweb.design/guides/theming/)

---

## Core Philosophy: Primitives vs. Semantics

To make colors **developer-friendly** and **maintainable**, we separate them into two layers. This prevents the "What hex code was that blue again?" problem.

1.  **Primitives (The Palette):** Descriptive names describing the *color itself*. (e.g., `Blue500`, `Neutral10`). These **never change** even if the theme changes.
2.  **Semantics (The Roles):** Descriptive names describing *how the color is used*. (e.g., `BrandPrimary`, `SurfaceBackground`). These **map** to primitives.

---

## 1. Defining Primitives (Color.kt)

**Best Practice:** Do not use abstract names like `PrimaryColor` here. Use literal names. This makes it easy to visualize the color just by reading the name.

```kotlin
// ui/theme/Color.kt
package com.yourpackage.ui.theme

import androidx.compose.ui.graphics.Color

// ═══════════════════════════════════════════════════════════
// 1. PRIMITIVES (Base Palette)
// Naming: {ColorName}{Shade} (0=Lightest, 1000=Darkest)
// ═══════════════════════════════════════════════════════════

// Brand Colors (The "Soul" of the app)
val Violet10 = Color(0xFFF5F0FF)
val Violet40 = Color(0xFFEADDFF) // Light Primary Container
val Violet80 = Color(0xFF6750A4) // Light Primary
val Violet100 = Color(0xFF21005D) // Light OnPrimaryContainer

// Neutral Colors (Grays, Whites, Blacks)
val Neutral0 = Color(0xFFFFFFFF)
val Neutral10 = Color(0xFFFDFDFD)
val Neutral20 = Color(0xFFF0F0F0)
val Neutral90 = Color(0xFF1C1B1F)
val Neutral100 = Color(0xFF000000)

// Semantic Status Colors (Standardized across app)
val Green40 = Color(0xFF4CAF50) // Success
val Red40 = Color(0xFFB3261E)   // Error
val Orange40 = Color(0xFFFFA726) // Warning
val Blue40 = Color(0xFF2196F3)  // Info

// ═══════════════════════════════════════════════════════════
// 2. MATERIAL 3 MAPPING (Generated or Manual)
// Map Primitives to Material Roles
// ═══════════════════════════════════════════════════════════

val md_theme_light_primary = Violet80
val md_theme_light_onPrimary = Neutral0
val md_theme_light_primaryContainer = Violet40
val md_theme_light_onPrimaryContainer = Violet100

val md_theme_light_error = Red40
val md_theme_light_onError = Neutral0

val md_theme_light_background = Neutral10
val md_theme_light_onBackground = Neutral90
// ... map remaining Material roles
```

---

## 2. Developer-Friendly Access (Theme.kt)

While `MaterialTheme.colorScheme` is standard, it doesn't cover everything (like "Success" or "Warning"). We create a **DesignSystem** object or extensions for fast recognition.

### Option A: Standard Material 3 (Recommended for simplicity)

Use standard roles where possible.

```kotlin
// Usage
MaterialTheme.colorScheme.primary
MaterialTheme.colorScheme.error
```

### Option B: Custom Extension for Fast Recognition (Best for Teams)

If you have colors like `Success`, `Warning`, or specific `Brand` gradients that don't fit Material slots, add them as extensions.

```kotlin
// ui/theme/Theme.kt

// 1. Define the extension properties
val ColorScheme.success: Color
    @Composable get() = if (isSystemInDarkTheme()) Green80 else Green40

val ColorScheme.warning: Color
    @Composable get() = if (isSystemInDarkTheme()) Orange80 else Orange40

// 2. Usage in Composables
Text(
    text = "Operation Successful",
    color = MaterialTheme.colorScheme.success // ✅ Fast & Readable
)
```

---

## 3. Naming Rules for Fast Recognition

When creating custom colors, follow this pattern: `[Category][Property][State]`

| Pattern | Example | Why it works |
|---------|---------|--------------|
| `Bg` + `[Type]` | `BgPrimary`, `BgSurface` | Groups all backgrounds together in autocomplete |
| `Text` + `[Importance]` | `TextPrimary`, `TextSecondary` | Groups all text colors |
| `Stroke` + `[Type]` | `StrokeDivider`, `StrokeBorder` | Groups all border colors |
| `Status` + `[Type]` | `StatusSuccess`, `StatusError` | Groups all feedback colors |

**Example in Code:**

```kotlin
// Easy to find with IDE Autocomplete (type "Text...")
object AppColors {
    val TextPrimary = Neutral90
    val TextSecondary = Neutral60
    val TextDisabled = Neutral40
    val TextInverse = Neutral0
}
```

---

## ❌ Common Mistakes

1.  **Naming by Location:**
    *   ❌ `LoginButtonColor` (Bad: What if used on Profile?)
    *   ✅ `BrandPrimary` (Good: Reusable)

2.  **Vague Naming:**
    *   ❌ `Blue` (Bad: Which blue?)
    *   ✅ `Blue500` (Good: Specific from palette)

3.  **Hardcoding Hex:**
    *   ❌ `Color(0xFF0000FF)` in UI code
    *   ✅ `MaterialTheme.colorScheme.primary`

---

## Summary Checklist

1.  **Define Palette first** (`Blue50`, `Blue100`...) in `Color.kt`.
2.  **Map to Roles** (`primary = Blue100`) in `Theme.kt`.
3.  **Use Extensions** for semantic colors missing from Material (`success`, `warning`).
4.  **Never hardcode hex values** in Composables.
