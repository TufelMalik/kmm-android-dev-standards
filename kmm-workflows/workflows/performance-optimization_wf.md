---
description: Performance optimization
---

# Performance Optimization

> **Context**: Optimizing performance. Avoid recomposition loops.

## Fixes

```kotlin
// ✅ Stable key
LazyColumn {
    items(list, key = { it.id }) { Item(it) }
}

// ✅ Remember expensive work
val result = remember(input) { calculate(input) }

// ✅ Derived state
val enabled by remember { derivedStateOf { email.isNotBlank() } }
```

## Rules

> **Must follow:** [recomposition-rules](../rules/recomposition-rules.md)
