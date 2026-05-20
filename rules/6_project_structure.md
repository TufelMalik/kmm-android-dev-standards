# Project Structure

> **Standardized Folder Structure**

This rule defines the mandatory folder structure for both **Compose Multiplatform (KMM)** and **Android Compose** projects to ensure modularity, separation of concerns, and consistent navigation.

---

## 1. Compose Multiplatform (KMM) Structure

**Root Directory:** `composeApp/`

```text
composeApp/
└── src/
    ├── commonMain/          # Shared code across all platforms
    │   ├── kotlin/
    │   │   └── com/bilty/generator/
    │   │       ├── AppNavigation.kt     # Navigation routes
    │   │       ├── KMPApp.kt            # Main app entry point with navigation setup
    │   │       ├── Platform.kt          # Platform-specific definitions
    │   │       ├── bridge/              # Platform bridge implementations
    │   │       ├── di/                  # Dependency Injection (Koin)
    │   │       │   ├── AppModule.kt
    │   │       │   ├── helperModules/
    │   │       │   ├── repositoryModules/
    │   │       │   └── viewModelModules/
    │   │       ├── model/               # Data models and constants
    │   │       │   ├── constants/       # App-wide constants
    │   │       │   ├── data/            # Data classes
    │   │       │   ├── enums/           # Enumerations
    │   │       │   ├── interfaces/      # Interface definitions
    │   │       │   └── reponse/         # API response models
    │   │       ├── modules/             # Organizes features into distinct modules
    │   │       │   ├── auth/            # Authentication feature module
    │   │       │   │   ├── components/  # Reusable UI components for authentication
    │   │       │   │   ├── navigation/  # Navigation routes and logic for authentication
    │   │       │   │   ├── ui/          # UI screens/composables for authentication
    │   │       │   │   └── viewModel/   # ViewModels for authentication screens
    │   │       │   ├── home/            # Home screen feature module
    │   │       │   │   ├── components/  # Reusable UI components for the home screen
    │   │       │   │   ├── navigation/  # Navigation routes and logic for the home screen
    │   │       │   │   ├── ui/          # UI screens/composables for the home screen
    │   │       │   │   └── viewModel/   # ViewModels for home screens
    │   │       │   ├── profile/         # User profile management feature module
    │   │       │   │   ├── components/  # Reusable UI components for profile management
    │   │       │   │   ├── navigation/  # Navigation routes and logic for profile flow
    │   │       │   │   ├── ui/          # UI screens/composables for profile management
    │   │       │   │   └── viewModel/   # ViewModels for profile screens
    │   │       │   └── settings/        # User settings feature module
    │   │       │       ├── components/  # Reusable UI components for settings
    │   │       │       ├── navigation/  # Navigation routes and logic for settings flow
    │   │       │       ├── ui/          # UI screens/composables for settings
    │   │       │       └── viewModel/   # ViewModels for settings screens
    │   │       ├── repository/          # Data repositories
    │   │       ├── theme/               # Theming (Colors, Typography)
    │   │       ├── uiToolKit/           # Reusable UI components
    │   │       └── utils/               # Utility functions
    │   │           ├── extensions/      # Kotlin extensions
    │   │           └── helpers/         # Helper classes
    │   └── composeResources/
    │       ├── drawable/       # Image assets
    │       ├── files/          # Static files (e.g., receipt templates)
    │       ├── font/           # Font files
    │       └── values/
    │           └── strings.xml # String resources
    ├── androidMain/            # Android-specific code
    ├── iosMain/                # iOS-specific code
    ├── jvmMain/                # JVM Desktop-specific code
    └── wasmJsMain/             # Web (WasmJS)-specific code
```

---

## 2. Android Compose Structure

**Root Directory:** `app/`

The structure mirrors the KMM commonMain structure but **excludes the `bridge` directory** and uses standard Android resource directories.

```text
app/
└── src/
    └── main/
        ├── kotlin/
        │   └── com/bilty/generator/
        │       ├── AppNavigation.kt     # Navigation routes
        │       ├── App.kt               # Main Application class / Entry point
        │       ├── di/                  # Dependency Injection (Hilt/Koin)
        │       │   ├── AppModule.kt
        │       │   ├── helperModules/
        │       │   ├── repositoryModules/
        │       │   └── viewModelModules/
        │       ├── model/               # Data models and constants
        │       │   ├── constants/       # App-wide constants
        │       │   ├── data/            # Data classes
        │       │   ├── enums/           # Enumerations
        │       │   ├── interfaces/      # Interface definitions
        │       │   └── reponse/         # API response models
        │       ├── modules/             # Organizes features into distinct modules
        │       │   ├── auth/            # Authentication feature module
        │       │   │   ├── components/  # Reusable components
        │       │   │   ├── navigation/  # Navigation routes
        │       │   │   ├── ui/          # Screens
        │       │   │   └── viewModel/   # ViewModels
        │       │   ├── home/            # Home feature module
        │       │   │   ├── components/
        │       │   │   ├── navigation/
        │       │   │   ├── ui/
        │       │   │   └── viewModel/
        │       │   └── ... (other feature modules)
        │       ├── repository/          # Data repositories
        │       ├── theme/               # Theming (Theme, Color, Type)
        │       ├── uiToolKit/           # Reusable UI components
        │       └── utils/               # Utility functions
        │           ├── extensions/      # Kotlin extensions
        │           └── helpers/         # Helper classes
        └── res/
            ├── drawable/       # Image assets
            ├── font/           # Font files
            ├── values/
            │   └── strings.xml # String resources
            └── ...
```

## Implementation Guidelines

1.  **Feature Modules**: All new features must be added under `modules/` with their own sub-structure (`components`, `navigation`, `ui`, `viewModel`). This applies to both KMM and Android projects.
2.  **Shared Resources**: 
    *   **KMM**: Use `composeResources` (`composeApp/src/commonMain/composeResources`).
    *   **Android**: Use standard `res` directory (`app/src/main/res`).
3.  **Bridge Directory**: The `bridge/` directory is **exclusive to KMM** for platform-specific implementations. Do not include it in pure Android projects.
4.  **Consistency**: Maintain identical package naming and structural hierarchy between `commonMain` (KMM) and `main` (Android) to preserve consistency across project types.
5.  **UI Directory**: The `ui` directory must only contain the Screen composable and its Preview function. All other sub-components must be placed in the module's `components` directory.