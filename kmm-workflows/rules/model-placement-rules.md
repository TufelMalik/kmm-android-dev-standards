# Model Placement Rules

> All data classes, enums, and models must be in `models/` directory, never in UI files.

> [!IMPORTANT]
> **One Class Per File**: Each `data class`, `enum class`, `sealed class`, or `sealed interface` MUST be in its own separate file. Never combine multiple classes in a single file.

## File Structure

```
composeApp/src/commonMain/
└── models/
    ├── navigation/           ← Route sealed classes
    │   └── HomeRoutes.kt
    ├── consts/               ← Constants, enums
    │   └── TaskStatus.kt
    ├── entities/             ← Domain entities
    │   └── User.kt
    ├── request/              ← API request models
    │   └── CreateUserRequest.kt
    ├── response/             ← API response models
    │   └── UserResponse.kt
    └── ui/                   ← UI state models
        └── HomeUiState.kt
```

## Examples

```kotlin
// models/consts/TaskStatus.kt
enum class TaskStatus { PENDING, IN_PROGRESS, COMPLETED }

// models/entities/User.kt
data class User(val id: String, val name: String)

// models/request/CreateUserRequest.kt
@Serializable
data class CreateUserRequest(val name: String, val email: String)

// models/response/UserResponse.kt
@Serializable
data class UserResponse(val id: String, val name: String)

// models/ui/HomeUiState.kt
data class HomeUiState(val isLoading: Boolean, val items: List<Item>)
```

## Subfolder Guide

| Folder | For |
|--------|-----|
| `navigation/` | Route sealed classes |
| `consts/` | Enums, constants |
| `entities/` | Domain/business objects |
| `request/` | API request models |
| `response/` | API response models |
| `ui/` | UI state classes |

## ✅ DO

- Create all models in `models/` with proper subfolder
- **One class per file** (e.g., `User.kt` contains only `User` class)
- Name file exactly as class name (e.g., `TaskStatus.kt` for `enum class TaskStatus`)
- Import models where needed

## ❌ DON'T

- Don't define data class in Screen files
- Don't define enum in UI composables
- Don't scatter models across features
- **Don't combine multiple classes in one file**
