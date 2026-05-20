---
description: Handling permissions
---

# Permissions Handling

> **Context**: Adding permissions. Use expect/actual, no platform checks in UI.

## Pattern

```kotlin
// commonMain
expect class PermissionHandler {
    fun checkCamera(): Boolean
    suspend fun requestCamera(): Boolean
}

// androidMain
actual class PermissionHandler(private val ctx: Context) {
    actual fun checkCamera() = ContextCompat.checkSelfPermission(
        ctx, Manifest.permission.CAMERA
    ) == PackageManager.PERMISSION_GRANTED
}

// iosMain
actual class PermissionHandler { ... }
```

## Rules

> **Must follow:** [expect-actual-pattern-rules](../rules/expect-actual-pattern-rules.md) | [platform-specific-rules](../rules/platform-specific-rules.md)
