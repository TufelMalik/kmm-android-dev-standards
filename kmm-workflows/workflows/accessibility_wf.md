---
description: Accessibility
---

# Accessibility

> **Context**: Adding accessibility. Content descriptions, large fonts.

## Content Description

```kotlin
// ✅ Required for meaningful icons
Icon(icon, contentDescription = stringResource(Res.string.cd_close))

// ✅ Decorative only
Icon(icon, contentDescription = null)
```

## Touch Targets

```kotlin
IconButton(modifier = Modifier.size(48.dp)) { Icon(...) }
```

## Rules

> **Must follow:** [accessibility-standards-rules](../rules/accessibility-standards-rules.md)
