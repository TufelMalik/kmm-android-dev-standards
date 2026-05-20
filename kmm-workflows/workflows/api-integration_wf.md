---
description: Adding API calls
---

# API Integration

> **Context**: Adding API. Never call from UI, always via Repository.

## Flow

```
UI → ViewModel → Repository → API or Helper class
```

## Checklist

1. Define endpoint in API service
2. Create request/response models in `model/`
3. Wrap API call in Repository
4. Call Repository from ViewModel

## Example

```kotlin
// API
interface AuthApi {
    suspend fun login(req: LoginRequest): LoginResponse
}

// Repository
class AuthRepository(private val api: AuthApi) {
    suspend fun login(email: String, pass: String): Result<User> =
        runCatching { api.login(LoginRequest(email, pass)).user }
}

// ViewModel calls repo, NOT api
```

## Rules

> **Must follow:** [api-layer-rules](../rules/api-layer-rules.md) | [solid-principles-rules](../rules/solid-principles-rules.md) | [model-placement-rules](../rules/model-placement-rules.md)
