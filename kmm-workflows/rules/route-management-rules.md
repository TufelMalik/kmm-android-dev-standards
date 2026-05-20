# Route Management Rules

> Type-safe routes using `@Serializable sealed class` per module.

## File Structure

```
modules/
└── home/navigation/
    ├── HomeRoutes.kt         ← Sealed class routes
    └── HomeGraph.kt          ← NavGraph extension
└── profile/navigation/
    ├── ProfileRoutes.kt
    └── ProfileGraph.kt
```

## Routes File (Per Module)

```kotlin
// modules/home/navigation/HomeRoutes.kt
@Serializable
sealed class HomeRoutes {
    @Serializable
    data object HomeScreen : HomeRoutes()

    @Serializable
    data object Settings : HomeRoutes()
}

// modules/profile/navigation/ProfileRoutes.kt
@Serializable
sealed class ProfileRoutes {
    @Serializable
    data class Detail(val userId: String) : ProfileRoutes()

    @Serializable
    data class Edit(val userId: String) : ProfileRoutes()
}
```

## Graph File (Per Module)

```kotlin
// modules/home/navigation/HomeGraph.kt
fun NavGraphBuilder.homeGraph(navController: NavController) {
    composable<HomeRoutes.HomeScreen> {
        HomeScreen(navController)
    }
    composable<HomeRoutes.Settings> {
        SettingsScreen(navController)
    }
}

// modules/profile/navigation/ProfileGraph.kt
fun NavGraphBuilder.profileGraph(navController: NavController) {
    composable<ProfileRoutes.Detail> { backStackEntry ->
        val route = backStackEntry.toRoute<ProfileRoutes.Detail>()
        ProfileDetailScreen(navController, route.userId)
    }
    composable<ProfileRoutes.Edit> { backStackEntry ->
        val route = backStackEntry.toRoute<ProfileRoutes.Edit>()
        ProfileEditScreen(navController, route.userId)
    }
}
```

## Navigation Usage

```kotlin
// No args
navController.navigate(HomeRoutes.HomeScreen)

// With args
navController.navigate(ProfileRoutes.Detail(userId = "123"))

// Back
navController.popBackStack()
```

## ✅ DO

- Create `XxxRoutes.kt` sealed class per module
- Create `XxxGraph.kt` as `NavGraphBuilder` extension
- Use `data object` for no-arg routes
- Use `data class` for routes with args
- Use `backStackEntry.toRoute<T>()` to get args

## ❌ DON'T

- Don't use string routes
- Don't put all routes in one file
- Don't hardcode route paths
