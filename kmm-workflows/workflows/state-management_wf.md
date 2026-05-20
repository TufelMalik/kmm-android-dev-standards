---
description: Managing UI state
---

# State Management

> **Context**: Adding state. Single source of truth - UI observes, VM controls.

## Rules

> **Must follow:** [state-management-rules](../rules/state-management-rules.md)

## Example

```kotlin
// State class
data class LoginUiState(
    val email: String = "",
    val isLoading: Boolean = false,
    val error: String? = null
)

// ViewModel
private val _uiState = MutableStateFlow(LoginUiState())
val uiState: StateFlow<LoginUiState> = _uiState.asStateFlow()

fun onEmailChange(email: String) {
    _uiState.update { it.copy(email = email) }
}

// UI - observe only
@Composable fun Screen(vm: LoginViewModel) {
    val state by vm.uiState.collectAsState()
    TextField(value = state.email, ...)
}
```
