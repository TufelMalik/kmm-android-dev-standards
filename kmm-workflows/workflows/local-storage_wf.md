---
description: Adding local storage
---

# Local Storage

> **Context**: Adding storage. Access only via Repository.

## Architecture

```
UI → ViewModel → Repository → DataSource
```

## Example

```kotlin
// DataSource
class PrefsDataSource(private val prefs: PreferencesHelper) {
    fun getToken() = prefs.getString("token")
    fun saveToken(t: String) = prefs.setString("token", t)
}

// Repository wraps DataSource
class AuthRepository(private val prefs: PrefsDataSource) {
    fun getToken() = prefs.getToken()
}
```

## Rules

> **Must follow:** [storage-access-rules](../rules/storage-access-rules.md)
