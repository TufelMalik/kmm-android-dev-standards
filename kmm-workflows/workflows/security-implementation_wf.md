---
description: Security implementation
---

# Security Implementation

> **Context**: Adding security. Validate inputs, HTTPS only, encrypt sensitive data.

## Input Validation

```kotlin
fun sanitize(input: String) = input.trim().take(MAX_LEN)
fun validEmail(email: String) = email.contains("@")
```

## Secure Storage

```kotlin
expect class SecureStorage {
    fun saveToken(token: String)
    fun getToken(): String?
}
```

## Rules

> **Must follow:** [security-validation-rules](../rules/security-validation-rules.md) | [secrets-management-rules](../rules/secrets-management-rules.md)
