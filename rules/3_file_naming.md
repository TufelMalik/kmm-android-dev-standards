# Rule 3: File Naming & Best Practices

> **Official References:**  
> - [Kotlin Coding Conventions](https://kotlinlang.org/docs/coding-conventions.html)
> - [Android App Architecture Guide](https://developer.android.com/topic/architecture)

---

## Core Philosophy: Suffix-Based Naming

To achieve **fast recognition** and **scalability**, we use strict suffixes. The suffix tells you the *type* of the file, and the prefix tells you the *feature*. This allows you to find any file instantly using `Ctrl+P` (or `Cmd+P`).

---

## 1. File Naming Conventions

Use **PascalCase** for all file names.

| File Type | Naming Pattern | Example | Why? |
|-----------|---------------|---------|------|
| **Screen (Root)** | `[Feature]Screen` | `LoginScreen.kt` | Top-level Composable with Scaffold |
| **Content (Stateless)**| `[Feature]Content` | `LoginContent.kt` | Pure UI, easy to preview |
| **ViewModel** | `[Feature]ViewModel` | `LoginViewModel.kt` | Logic holder |
| **State** | `[Feature]UiState` | `LoginUiState.kt` | Data class for UI state |
| **Navigation** | `[Feature]Navigation` | `LoginNavigation.kt` | NavGraph builders/args |
| **Preview** | `[Feature]Preview` | `LoginPreview.kt` | (Optional) Separate preview file |
| **Repository** | `[Feature]Repository` | `AuthRepository.kt` | Data access |
| **Use Case** | `[Verb][Subject]UseCase` | `LoginUserUseCase.kt` | Single business action |
| **Data Model** | `[Noun]` | `User.kt`, `Product.kt` | Represents a domain entity |
| **Utility** | `[Topic]Utils` | `DateUtils.kt` | Helper functions |
| **Extension** | `[Class]Ext` | `StringExt.kt` | Extension functions |

---

## 2. Compose Specific Naming Patterns

### A. Screen vs. Content (State Hoisting)

Split your screens into two parts to make them testable and previewable.

1.  **`LoginScreen`**: Connected to ViewModel, holds state.
2.  **`LoginContent`**: Stateless, takes data and lambdas.

```kotlin
// LoginScreen.kt
@Composable
fun LoginScreen(
    viewModel: LoginViewModel = koinViewModel()
) {
    val state by viewModel.uiState.collectAsState()
    LoginContent(
        state = state,
        onLogin = viewModel::login
    )
}

// LoginContent.kt (Stateless - Easy to Preview)
@Composable
fun LoginContent(
    state: LoginUiState,
    onLogin: () -> Unit
) {
    // UI Code here...
}
```

### B. Component Naming (Atoms & Molecules)

For reusable components:

*   **Generic:** `AppButton`, `AppTextField` (Prefix with App Name or 'App')
*   **Specific:** `UserAvatar`, `PaymentCard`

**Fast Recognition Tip:** Prefix generic components with your app initials or `App` so they group together in autocomplete.
*   `AppButton`
*   `AppCard`
*   `AppToolbar`

---

## 3. Use Case Naming

Use cases should be named with a **Verb** + **Noun/Subject** + **UseCase**. This makes them read like a sentence.

*   `GetUserUseCase`
*   `LoginUserUseCase`
*   `FormatDateUseCase`
*   `ValidateEmailUseCase`

---

## 4. ❌ Common Naming Mistakes

1.  **Generic Names:**
    *   ❌ `MainScreen.kt` (Too vague)
    *   ✅ `DashboardScreen.kt` (Specific)

2.  **Missing Suffixes:**
    *   ❌ `Login.kt` (Is it a screen? A model? A util?)
    *   ✅ `LoginScreen.kt` or `LoginModel.kt`

3.  **Inconsistent Utils:**
    *   ❌ `Functions.kt`, `Misc.kt`
    *   ✅ `DateUtils.kt`, `StringUtils.kt`

4.  **CamelCase Files:**
    *   ❌ `loginScreen.kt`
    *   ✅ `LoginScreen.kt` (Files should always be PascalCase)

---

## Summary for Developers

*   **Suffix Everything**: Always end files with what they *are* (`Screen`, `Repository`, `UseCase`).
*   **Hoist State**: `Screen` (Stateful) -> `Content` (Stateless).
*   **Prefix Components**: Use `App` or `Brand` prefix for shared UI components (`AppButton`).
*   **Be Specific**: Avoid generic names like `Data.kt` or `Manager.kt`.
