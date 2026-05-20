# Expect/Actual Pattern Rules

> Platform-specific code in `bridge/` directory using expect/actual pattern.

## File Structure

```
composeApp/src/
├── commonMain/kotlin/bridge/
│   ├── Toast.kt              ← expect fun showToast()
│   └── PermissionHandler.kt  ← expect class
├── androidMain/kotlin/bridge/
│   ├── Toast.kt              ← actual fun showToast()
│   └── PermissionHandler.kt
├── iosMain/kotlin/bridge/
│   ├── Toast.kt
│   └── PermissionHandler.kt
└── desktopMain/kotlin/bridge/
    ├── Toast.kt
    └── PermissionHandler.kt
```

## Expect (commonMain)

```kotlin
// commonMain/bridge/Toast.kt
expect fun showToast(message: String)

// commonMain/bridge/PermissionHandler.kt
expect class PermissionHandler {
    fun requestPermission(permission: String)
    fun checkPermission(permission: String): Boolean
}
```

## Actual (Per Platform)

```kotlin
// androidMain/bridge/Toast.kt
actual fun showToast(message: String) {
    Toast.makeText(context, message, Toast.LENGTH_SHORT).show()
}

// iosMain/bridge/Toast.kt
actual fun showToast(message: String) {
    // iOS implementation
}
```

## ✅ DO

- Create expect/actual in `bridge/` directory
- Mirror file structure across all platforms
- Keep expect declarations minimal

## ❌ DON'T

- Don't put expect/actual outside `bridge/`
- Don't skip platform implementations
- Don't add platform imports to commonMain
