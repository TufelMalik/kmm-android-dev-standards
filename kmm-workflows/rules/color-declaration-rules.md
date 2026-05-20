# Color Declaration Rules

> ALL colors MUST be declared in `theme/Color.kt`. Hardcoded color values in UI = INVALID.

## File Location

```
composeApp/src/commonMain/
└── theme/
    └── Color.kt              ← All colors defined here
```

## Naming (Semantic Only)

| ✅ Correct (Semantic) | ❌ Invalid (Visual) |
|----------------------|---------------------|
| `primary` | `red500` |
| `onPrimary` | `blueDark` |
| `background` | `lightGray` |
| `surface` | `buttonBlue` |
| `error` | `redError` |

## Color.kt Structure

```kotlin
// theme/Color.kt
object ThemeColors {
    // Primary
    val primary = Color(0xFF6750A4)
    val onPrimary = Color(0xFFFFFFFF)

    // Background
    val background = Color(0xFFFFFFFF)
    val onBackground = Color(0xFF000000)

    // Surface
    val surface = Color(0xFFF5F5F5)
    val onSurface = Color(0xFF1C1C1C)

    // Error
    val error = Color(0xFFB00020)
    val onError = Color(0xFFFFFFFF)

    // Outline
    val outline = Color(0xFFDDDDDD)
}
```

## Theme Integration

```kotlin
// theme/Theme.kt
private val LightColorScheme = lightColorScheme(
    primary = ThemeColors.primary,
    onPrimary = ThemeColors.onPrimary,
    background = ThemeColors.background,
    onBackground = ThemeColors.onBackground,
    surface = ThemeColors.surface,
    onSurface = ThemeColors.onSurface,
    error = ThemeColors.error
)

@Composable
fun AppTheme(content: @Composable () -> Unit) {
    MaterialTheme(
        colorScheme = LightColorScheme,
        content = content
    )
}
```

## Usage in UI

```kotlin
// ✅ Correct - Use MaterialTheme
Text(color = MaterialTheme.colorScheme.primary)
Box(modifier = Modifier.background(MaterialTheme.colorScheme.surface))

// ❌ Invalid - Hardcoded or direct ThemeColors in UI
Text(color = Color(0xFF6750A4))
Text(color = ThemeColors.primary)  // Use MaterialTheme instead
```

## ✅ DO

- Define all colors in `theme/Color.kt`
- Use SEMANTIC names only
- Access via `MaterialTheme.colorScheme.xxx`
- One source of truth

## ❌ DON'T

- Don't hardcode `Color(0xXXXXXX)` in UI
- Don't use visual names like `red500`
- Don't duplicate color definitions
