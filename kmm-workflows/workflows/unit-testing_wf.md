---
description: Writing tests
---

# Unit Testing

> **Context**: Writing tests. Test in commonTest first, focus on Repo & VM.
>
> **Must follow:** [test-organization-rules](../rules/test-organization-rules.md) | [5_testing.md](../../rules/5_testing.md)

## Naming

```kotlin
// Pattern: should_[expected]_when_[condition]
fun should_returnError_when_emailEmpty() { }
```

## Priority

1. Repository tests (data layer)
2. ViewModel tests (logic)
3. UI tests (optional)

## Example

```kotlin
@Test
fun should_showError_when_loginFails() = runTest {
    coEvery { api.login(any()) } throws IOException()
    
    val result = repo.login("email", "pass")
    
    assertTrue(result.isFailure)
}
```

## Location

- `commonMain` → `commonTest`
- `androidMain` → `androidTest`
