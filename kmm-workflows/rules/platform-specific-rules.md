# Platform-Specific Rules

> **Why**: Proper isolation via expect/actual.

## ✅ DO

- Use `expect` in commonMain
- Implement `actual` per platform
- Abstract behind interfaces

## ❌ DON'T

- Don't use platform imports in commonMain
- Don't hardcode platform checks in UI

## Example

```kotlin
// commonMain
expect fun getPlatform(): String

// androidMain
actual fun getPlatform() = "Android"

// iosMain
actual fun getPlatform() = "iOS"
```
