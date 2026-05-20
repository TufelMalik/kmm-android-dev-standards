---
description: Adding typography
---

# Typography Setup

> Use `MaterialTheme.typography` directly. Never hardcode `.sp`.

## Define Typography

```kotlin
// theme/Typography.kt
val AppTypography = Typography()  // Material 3 defaults
```

## Add to Theme

```kotlin
// theme/Theme.kt
MaterialTheme(
    typography = AppTypography,
    content = content
)
```

## Usage

```kotlin
// ✅ Correct
Text(
    text = stringResource(Res.string.title),
    style = MaterialTheme.typography.titleLarge
)

// ❌ Wrong
Text(stringResource(Res.string.title), fontSize = 20.sp)
```

## Rules

> **Must follow:** [typography-usage-rules](../rules/typography-usage-rules.md) | [dimensions-rules](../rules/dimensions-rules.md)
