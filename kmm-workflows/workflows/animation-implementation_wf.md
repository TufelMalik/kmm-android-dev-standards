---
description: Adding animations
---

# Animation Implementation

> **Context**: Adding animations. Keep lightweight, reusable, non-blocking.

## Example

```kotlin
// Fade
val alpha by animateFloatAsState(if (visible) 1f else 0f)

// Reusable modifier
fun Modifier.fadeIn(visible: Boolean) = composed {
    val alpha by animateFloatAsState(if (visible) 1f else 0f)
    this.alpha(alpha)
}
```

## Rules

> **Must follow:** [animation-performance-rules](../rules/animation-performance-rules.md)
