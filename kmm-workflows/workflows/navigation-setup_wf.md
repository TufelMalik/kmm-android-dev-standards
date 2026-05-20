---
description: Setting up navigation
---

# Navigation Setup

> Type-safe navigation with `@Serializable sealed class`.

## Step 1: Create Routes

```kotlin
// modules/home/navigation/HomeRoutes.kt
@Serializable
sealed class HomeRoutes {
    @Serializable
    data object HomeScreen : HomeRoutes()
}
```

## Step 2: Create Graph

```kotlin
// modules/home/navigation/HomeGraph.kt
fun NavGraphBuilder.homeGraph(navController: NavController) {
    composable<HomeRoutes.HomeScreen> {
        HomeScreen(navController)
    }
}
```

## Step 3: Add to AppNavigation

```kotlin
// navigation/AppNavigation.kt
NavHost(startDestination = HomeRoutes.HomeScreen) {
    homeGraph(navController)
}
```

## Navigate

```kotlin
navController.navigate(HomeRoutes.HomeScreen)
navController.popBackStack()
```

## Rules

> **Must follow:** [route-management-rules](../rules/route-management-rules.md) | [screen-navigation-rules](../rules/screen-navigation-rules.md)
