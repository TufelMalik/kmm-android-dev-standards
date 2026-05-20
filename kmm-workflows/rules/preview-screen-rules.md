# Preview Rules

> Every component and screen must have a preview with demo data.

> [!CAUTION]
> **MANDATORY**: `PreviewScreen.kt` MUST exist at root and contain ALL screen previews. When creating/modifying ANY screen, you MUST update `PreviewScreen.kt` to include it. Never skip this step.

## File Structure

```
composeApp/src/commonMain/
├── PreviewScreen.kt              ← All screen previews
└── modules/
    └── home/
        ├── HomeScreen.kt         ← Screen + @Preview
        └── components/
            └── HomeCard.kt       ← Component + @Preview
```

## Component Preview

```kotlin
// modules/home/components/HomeCard.kt
@Composable
fun HomeCard(title: String, onClick: () -> Unit) {
    // Component implementation
}

@Preview
@Composable
private fun HomeCardPreview() {
    AppTheme {
        HomeCard(
            title = "Demo Title",
            onClick = { }
        )
    }
}
```

## Screen Preview

```kotlin
// modules/home/HomeScreen.kt
@Composable
fun HomeScreen(navController: NavController) {
    // Screen implementation
}

@Preview
@Composable
private fun HomeScreenPreview() {
    AppTheme {
        HomeScreen(navController = rememberNavController())
    }
}
```

## PreviewScreen.kt (Root)

```kotlin
// PreviewScreen.kt
@Composable
fun PreviewScreen() {
    Column(modifier = Modifier.verticalScroll(rememberScrollState())) {
        HomeScreenPreview()
        ProfileScreenPreview()
        SettingsScreenPreview()
    }
}

@Preview
@Composable
private fun AllPreviewsPreview() {
    AppTheme {
        PreviewScreen()
    }
}
```

## ✅ DO

- Add `@Preview` to every component
- Add `@Preview` to every screen
- Use demo/mock data in previews
- **Always add screen preview to `PreviewScreen.kt`** after creating any screen
- **Create `PreviewScreen.kt`** at root if it doesn't exist
- Wrap preview in `AppTheme`

## ❌ DON'T

- Don't create component without preview
- Don't create screen without preview
- **Don't skip adding screen to `PreviewScreen.kt`** - this is mandatory
- Don't forget to update `PreviewScreen.kt` when adding new screens
- Don't use real API data in previews
