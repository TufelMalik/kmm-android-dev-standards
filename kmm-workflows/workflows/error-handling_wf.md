---
description: Handling errors
---

# Error Handling

> **Context**: Adding error handling. User-safe messages, no silent failures.

## Error Model

```kotlin
sealed class AppError(val message: String) {
    object Network : AppError("Connection failed")
    object Server : AppError("Something went wrong")
    data class Unknown(val e: Throwable) : AppError("An error occurred")
}
```

## Repository

```kotlin
suspend fun fetchData(): Result<Data> = try {
    Result.success(api.getData())
} catch (e: IOException) {
    Result.failure(AppError.Network)
}
```

## Rules

> **Must follow:** [error-message-rules](../rules/error-message-rules.md)
