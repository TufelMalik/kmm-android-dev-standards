# Screen Navigation Rules

> Keep `App.kt` minimal. NavHost in `AppNavigation.kt`.

## File Structure

```
composeApp/src/commonMain/
├── App.kt                        ← Minimal, calls AppNavigation
└── navigation/
    └── AppNavigation.kt          ← NavHost + all module graphs
```

## App.kt (Minimal)

```kotlin
@Composable
fun App() {
    AppTheme {
        val navController = rememberNavController()
        Scaffold { innerPadding ->
            AppNavigation(navController, innerPadding)
        }
    }
}
```

## AppNavigation.kt

```kotlin
// navigation/AppNavigation.kt
@Composable
fun AppNavigation(
    navController: NavHostController,
    innerPadding: PaddingValues
) {
    NavHost(
        navController = navController,
        startDestination = HomeRoutes.HomeScreen,
        modifier = Modifier.padding(innerPadding)
    ) {
        // Call all module graphs
        homeGraph(navController)
        profileGraph(navController)
        settingsGraph(navController)
    }
}
```

## Adding New Screen

1. Create `XxxRoutes.kt` in module
2. Create `XxxGraph.kt` in module
3. Call `xxxGraph(navController)` in `AppNavigation.kt`

## ✅ DO

- Pass `innerPadding` to `AppNavigation`
- Apply `innerPadding` via `Modifier.padding()`
- Call `xxxGraph()` extensions in NavHost
- Keep `App.kt` under 10 lines

## ❌ DON'T

- Don't put NavHost in `App.kt`
- Don't put composable() calls in `AppNavigation.kt`
- Don't skip `innerPadding` handling
