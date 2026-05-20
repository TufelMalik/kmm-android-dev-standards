# Padding Calculation Rules

> **Goal**: Ensure touch targets and containers are the exact correct size by calculating padding dynamically.

## The Formula

To center content (like an icon) within a fixed target size (like a touch area), use this formula:

$$
\text{Padding} = \frac{\text{Target Size} - \text{Content Size}}{2}
$$

### Why this works

- **Target Size**: The total size you want the element to be (e.g., 48dp for accessibility).
- **Content Size**: The size of the icon or inner element (e.g., 24dp).
- **Division by 2**: Padding is applied to *both* sides (top/bottom or start/end), so the remaining space must be split in half.

## Common Examples

| Target Size | Icon Size | Calculation | Padding to Apply |
|-------------|-----------|-------------|------------------|
| **48dp** (Min Touch) | 24dp | `(48 - 24) / 2` | **12dp** |
| **48dp** (Min Touch) | 20dp | `(48 - 20) / 2` | **14dp** |
| **40dp** (Standard) | 24dp | `(40 - 24) / 2` | **8dp** |
| **32dp** (Small) | 16dp | `(32 - 16) / 2` | **8dp** |
| **24dp** (Tiny) | 16dp | `(24 - 16) / 2` | **4dp** |

## ✅ DO

- **Calculate First**: Always check the intended target size before applying padding.
- **Use `Dimensions`**: Apply the calculated value using the nearest `Dimensions.Spacing` token.
  - *Example*: Calculated 12dp -> Use `Dimensions.Spacing.Sm` (12dp).
- **Center Content**: Ensure the content is centered within the container (usually default in `Box` or `IconButton`).

## ❌ DON'T

- **Don't Guess**: Do not eyeball padding values.
- **Don't Ignore Target Size**: Adding arbitrary padding to an icon often leads to random, non-standard click areas.
- **Don't use "Minus" Only**: Simply subtracting content from target without dividing by 2 results in double the intended size increase.

## Code Example

```kotlin
// Target: 48dp, Icon: 24dp
// Padding = (48 - 24) / 2 = 12dp -> Dimensions.Spacing.Sm

Box(
    modifier = Modifier
        .size(48.dp) // Explicit size often better, but padding works too if content is fixed
        // OR
        .padding(Dimensions.Spacing.Sm) 
) {
    Icon(
        modifier = Modifier.size(Dimensions.Icon.Md), // 24dp
        ...
    )
}
```
