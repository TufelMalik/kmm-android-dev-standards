# API Layer Rules

> **Why**: Proper layering for testability.

## ✅ DO

- Call APIs only from Repository
- Return `Result<T>` from Repository
- Handle errors in Repository

## ❌ DON'T

- Don't call API from ViewModel
- Don't call API from UI

## Flow

```
UI → ViewModel → Repository → API
```
