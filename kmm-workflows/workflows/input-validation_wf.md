---
description: Adding validation
---

# Input Validation

> **Context**: Adding validation. Keep in commonMain, call from ViewModel.

## Rules

> **Must follow:** [validation-placement-rules](../rules/validation-placement-rules.md)

## Example

```kotlin
// Validator
object EmailValidator {
    fun validate(email: String): String? = when {
        email.isBlank() -> "Email required"
        !email.contains("@") -> "Invalid email"
        else -> null  // Valid
    }
}

// ViewModel
fun onSubmit() {
    EmailValidator.validate(email)?.let { error ->
        _uiState.update { it.copy(emailError = error) }
        return
    }
    // proceed...
}
```
