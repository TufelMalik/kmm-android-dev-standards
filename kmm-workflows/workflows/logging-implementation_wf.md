---
description: Adding logging
---

# Logging Implementation

> **Context**: Adding logs. No logs in production, mask sensitive data.

## Logger

```kotlin
object AppLogger {
    private val enabled = BuildConfig.DEBUG
    
    fun d(tag: String, msg: String) {
        if (enabled) println("D/$tag: $msg")
    }
}
```

## Usage

```kotlin
// ✅ Masked
AppLogger.d("Auth", "Login: ${email.take(3)}***")

// ❌ Wrong
println("Token: $token")
```

## Rules

> **Must follow:** [production-logging-rules](../rules/production-logging-rules.md)
