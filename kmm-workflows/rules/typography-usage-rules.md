# Typography Usage Rules

> Use `MaterialTheme.typography` directly. Never hardcode `.sp` or create custom TextStyles in UI.

## Typography.kt (Use Material Defaults)

```kotlin
// theme/Typography.kt
val AppTypography = Typography()  // Material 3 default typography
```

## Theme Integration

```kotlin
// theme/Theme.kt
@Composable
fun AppTheme(
    darkTheme: Boolean = isSystemInDarkTheme(),
    content: @Composable () -> Unit
) {
    val colorScheme = if (darkTheme) darkColorScheme() else lightColorScheme()
    
    MaterialTheme(
        colorScheme = colorScheme,
        typography = AppTypography,
        content = content
    )
}
```

## Material 3 Typography Tokens

| Token | Use For |
|-------|---------|
| `displayLarge/Medium/Small` | Hero text, large titles |
| `headlineLarge/Medium/Small` | Section headers |
| `titleLarge/Medium/Small` | Card titles, dialogs |
| `bodyLarge/Medium/Small` | Body text, paragraphs |
| `labelLarge/Medium/Small` | Buttons, captions |

## Usage

```kotlin
// ✅ Correct - Use MaterialTheme.typography
Text(
    text = stringResource(Res.string.title),
    style = MaterialTheme.typography.titleLarge
)
Text(
    text = stringResource(Res.string.body),
    style = MaterialTheme.typography.bodyMedium
)

// ❌ Wrong
Text(stringResource(Res.string.title), fontSize = 20.sp)
Text(stringResource(Res.string.title), style = TextStyle(fontSize = 20.sp))
```

## ✅ DO

- Use `MaterialTheme.typography.xxx` for all text
- Stick to Material 3 typography tokens
- Let Material handle font sizes automatically

## ❌ DON'T

- Don't hardcode `.sp` in UI files
- Don't create custom `TextStyle` in composables
- Don't use `fontSize` parameter directly
