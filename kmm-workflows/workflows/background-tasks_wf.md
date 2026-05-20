---
description: Background tasks
---

# Background Tasks

> **Context**: Adding background work. Use platform workers, no main thread blocking.

## Pattern

```kotlin
// commonMain
expect class BackgroundScheduler {
    fun scheduleSync()
}

// androidMain (WorkManager)
actual class BackgroundScheduler(ctx: Context) {
    actual fun scheduleSync() {
        WorkManager.getInstance(ctx).enqueue(syncRequest)
    }
}

// iosMain (BackgroundFetch)
actual class BackgroundScheduler(ctx: Context) {
    actual fun scheduleSync() {
        WorkManager.getInstance(ctx).enqueue(syncRequest)
    }
}

```


## Rules

> **Must follow:** [main-thread-rules](../rules/main-thread-rules.md) | [expect-actual-pattern-rules](../rules/expect-actual-pattern-rules.md)
