---
description: ViewModel with repo and DI
---

# ViewModel Pattern

> **Context**: Creating ViewModel. Always use Repository + DI.

## Checklist

1. Create in `modules/{feature}/viewModel/`
2. Inject Repository (never access data directly)
3. Register in `di/viewModelModules/`
4. Expose state as `StateFlow`

## Structure

```kotlin
class LoginViewModel(
    private val repo: AuthRepository  // Injected
) : ViewModel() {
    
    private val _uiState = MutableStateFlow(LoginUiState())
    val uiState: StateFlow<LoginUiState> = _uiState.asStateFlow()
    
    fun onLogin(email: String, pass: String) {
        viewModelScope.launch {
            repo.login(email, pass)
                .onSuccess { /* update state */ }
                .onFailure { /* update state */ }
        }
    }
}
```

## DI

```kotlin
val authVmModule = module {
    viewModelOf(::LoginViewModel)
}
```

## Rules

> **Must follow:** [state-management-rules](../rules/state-management-rules.md) | [api-layer-rules](../rules/api-layer-rules.md) | [solid-principles-rules](../rules/solid-principles-rules.md) | [model-placement-rules](../rules/model-placement-rules.md)
