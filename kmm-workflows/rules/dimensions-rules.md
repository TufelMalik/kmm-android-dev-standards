# Dimensions Rules

> All `.dp`/`.sp` values must be in `theme/Dimensions.kt`. Never hardcode in UI.

> [!IMPORTANT]
> **Mandatory**: Create/Use `Dimensions.kt` at path: `composeApp/src/commonMain/kotlin/{package}/theme/Dimensions.kt`
> File name is **`Dimensions.kt`** (NOT `Dimensions.kt`)

## Naming Convention (Material Design 3 - 8dp Grid)

| Group | Prefix | Scale (dp) | Based On |
|-------|--------|------------|----------|
| **Spacing** | `spacing` | `None(0)` `Xxs(4)` `Xs(8)` `Sm(12)` `Md(16)` `Lg(24)` `Xl(32)` `Xxl(48)` | 8dp grid |
| **Radius** | `corner` | `None(0)` `Xs(4)` `Sm(8)` `Md(12)` `Lg(16)` `Xl(28)` `Full(9999)` | M3 Shape |
| **Icon** | `icon` | `Xs(16)` `Sm(20)` `Md(24)` `Lg(40)` `Xl(48)` | M3 Icons |
| **Elevation** | `elevation` | `Level0(0)` `Level1(1)` `Level2(3)` `Level3(6)` `Level4(8)` `Level5(12)` | M3 Elevation |
| **Touch** | `touch` | `Min(48)` | Accessibility |
| **Stroke** | `stroke` | `Thin(1)` `Thick(2)` | M3 Outline |

### Typography (use `MaterialTheme.typography` - not raw sp)

| Token | Font Size (sp) | Line Height (sp) |
|-------|----------------|------------------|
| `displayLarge` | 57 | 64 |
| `displayMedium` | 45 | 52 |
| `displaySmall` | 36 | 44 |
| `headlineLarge` | 32 | 40 |
| `headlineMedium` | 28 | 36 |
| `headlineSmall` | 24 | 32 |
| `titleLarge` | 22 | 28 |
| `titleMedium` | 16 | 24 |
| `titleSmall` | 14 | 20 |
| `bodyLarge` | 16 | 24 |
| `bodyMedium` | 14 | 20 |
| `bodySmall` | 12 | 16 |
| `labelLarge` | 14 | 20 |
| `labelMedium` | 12 | 16 |
| `labelSmall` | 11 | 16 |

## Dynamic Padding Rule

> [!TIP]
> **See**: [padding-calculation-rules](padding-calculation-rules.md) for the correct formula to calculate padding.

**Goal**: Center content within a target size (e.g., 48dp).

**Formula**: `(Target Size - Content Size) / 2`

```kotlin
// Target: 48dp, Icon: 24dp -> Padding: 12dp
Box(Modifier.padding(Dimensions.Spacing.Sm)) { 
    Icon(modifier = Modifier.size(Dimensions.Icon.Md)) 
}
```

## Usage

```kotlin
Modifier.padding(Dimensions.Spacing.Md)
Spacer(Modifier.width(Dimensions.Spacing.Lg))
RoundedCornerShape(Dimensions.Corner.Md)
Icon(modifier = Modifier.size(Dimensions.Icon.Md))
```

## ✅ DO

- Create `Dimensions.kt` in `theme/` folder **before** writing UI
- Use `Dimensions.Spacing.Md` instead of `16.dp`
- Reference `Dimensions` nested objects for all dp/sp values

## ❌ DON'T

- Don't hardcode `.dp` or `.sp` in composables
- Don't name file `Dimens.kt` (use `Dimensions.kt`)
- Don't skip creating `Dimensions.kt` when project doesn't have one
- **Don't use numeric names** like `Size20`, `Size30`, `Padding16` - use semantic scale names (`Xs`, `Sm`, `Md`, `Lg`, `Xl`, `Xxl`)
- **Don't invent custom names** - strictly follow the naming convention in this file

## Dimensions.kt Template

```kotlin
// theme/Dimensions.kt
package {package}.theme

import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

object Dimensions {

    object Spacing {
        val None = 0.dp
        val Xxs = 4.dp
        val Xs = 8.dp
        val Sm = 12.dp
        val Md = 16.dp
        val Lg = 24.dp
        val Xl = 32.dp
        val Xxl = 48.dp
    }

    object Corner {
        val None = 0.dp
        val Xs = 4.dp
        val Sm = 8.dp
        val Md = 12.dp
        val Lg = 16.dp
        val Xl = 28.dp
        val Full = 9999.dp
    }

    object Icon {
        val Xs = 16.dp
        val Sm = 20.dp
        val Md = 24.dp
        val Lg = 40.dp
        val Xl = 48.dp
    }

    object Touch {
        val Min = 48.dp
    }

    object Stroke {
        val Thin = 1.dp
        val Thick = 2.dp
    }
}
```
