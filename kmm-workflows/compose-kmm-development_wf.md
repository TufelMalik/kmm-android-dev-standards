---
description: Master workflow for Compose KMM development
---

# Compose KMM Development

> **Context**: You are an experienced Compose KMM developer. Start here for any task.

> [!CAUTION]
> **MANDATORY**: Follow ALL rules below. No exceptions. No hardcoded strings/colors.

---

## 🎨 UI Implementation (When Images/Figma Provided)

When user provides **UI images**, **Figma designs**, or **UI descriptions**:

1. **Analyze** the design thoroughly before coding
2. **Follow workflows step-by-step** in this order:
   - [ui-implementation_wf](workflows/ui-implementation_wf.md) → **START HERE for UI images**
   - [feature-module-creation_wf](workflows/feature-module-creation_wf.md) → Create feature structure
   - [theme-colors-setup_wf](workflows/theme-colors-setup_wf.md) → Add any new colors
   - [typography-setup_wf](workflows/typography-setup_wf.md) → Set up text styles
   - [navigation-setup_wf](workflows/navigation-setup_wf.md) → Add routes and graphs
   - [string-usage_wf](workflows/string-usage_wf.md) → Add all strings first
   - [viewmodel-pattern_wf](workflows/viewmodel-pattern_wf.md) → Create ViewModel if needed
3. **Create screens** in `modules/{feature}/ui/`
4. **Create components** in `modules/{feature}/components/`
5. **Add previews** for all screens and components
6. **Follow ALL rules** from the Rules Reference table

---

## ⚠️ CRITICAL Rules (MUST Follow)

| Rule | Description |
|------|-------------|
| **NO Hardcoded Strings** | All text MUST come from `strings.xml` |
| **NO Hardcoded Colors** | Use `MaterialTheme.colorScheme` only |
| **File Placement** | Files MUST go in correct location (see table below) |
| **Architecture** | UI → ViewModel → Repository → DataSource |
| **Platform Code** | Use `expect/actual`, no platform checks in UI |

---

## 📁 File Placement (MANDATORY)

> [!IMPORTANT]
> Place files in the EXACT location specified. No exceptions.

| File Type | Location | Example |
|-----------|----------|---------|
| **Screen** | `modules/{feature}/ui/` | `modules/auth/ui/LoginScreen.kt` |
| **Component** | `modules/{feature}/components/` | `modules/auth/components/LoginForm.kt` |
| **Shared UI** | `uiToolKit/` | `uiToolKit/PrimaryButton.kt` |
| **ViewModel** | `modules/{feature}/viewModel/` | `modules/auth/viewModel/LoginViewModel.kt` |
| **Model/Data Class** | `model/{feature}/` | `model/auth/LoginRequest.kt` |
| **UI State** | `model/{feature}/` | `model/task/TaskUiState.kt` |
| **Repository** | `repository/` | `repository/AuthRepository.kt` |
| **DataSource** | `data/` | `data/AuthDataSource.kt` |
| **Navigation** | `navigation/` | `navigation/AppNavGraph.kt` |
| **Extensions** | `utils/extensions/` | `utils/extensions/StringExt.kt` |
| **Helpers** | `utils/helpers/` | `utils/helpers/DateHelper.kt` |

---

## 🔄 Task Routing

### 1. Project Setup

| Task | Workflow |
|------|----------|
| New feature module | [feature-module-creation_wf](workflows/feature-module-creation_wf.md) |
| Creating file | [file-creation_wf](workflows/file-creation_wf.md) |
| Adding dependency | [dependency-management_wf](workflows/dependency-management_wf.md) |
| Build config | [build-configuration_wf](workflows/build-configuration_wf.md) |
| Network setup | [network-configuration_wf](workflows/network-configuration_wf.md) |

### 2. UI Development

| Task | Workflow |
|------|----------|
| **UI from Image** | [ui-implementation_wf](workflows/ui-implementation_wf.md) |
| Theme/Colors | [theme-colors-setup_wf](workflows/theme-colors-setup_wf.md) |
| Typography | [typography-setup_wf](workflows/typography-setup_wf.md) |
| Assets | [asset-management_wf](workflows/asset-management_wf.md) |
| Strings | [string-usage_wf](workflows/string-usage_wf.md) |
| Navigation | [navigation-setup_wf](workflows/navigation-setup_wf.md) |
| Animations | [animation-implementation_wf](workflows/animation-implementation_wf.md) |
| Accessibility | [accessibility_wf](workflows/accessibility_wf.md) |
| **Padding Calc** | [calculate-padding_wf](workflows/calculate-padding_wf.md) |

### 3. Data & Logic

| Task | Workflow |
|------|----------|
| ViewModel | [viewmodel-pattern_wf](workflows/viewmodel-pattern_wf.md) |
| State | [state-management_wf](workflows/state-management_wf.md) |
| API integration | [api-integration_wf](workflows/api-integration_wf.md) |
| Local storage | [local-storage_wf](workflows/local-storage_wf.md) |
| Validation | [input-validation_wf](workflows/input-validation_wf.md) |
| Permissions | [permissions-handling_wf](workflows/permissions-handling_wf.md) |
| Background tasks | [background-tasks_wf](workflows/background-tasks_wf.md) |

### 4. Quality & Production

| Task | Workflow |
|------|----------|
| Error handling | [error-handling_wf](workflows/error-handling_wf.md) |
| Logging | [logging-implementation_wf](workflows/logging-implementation_wf.md) |
| Security | [security-implementation_wf](workflows/security-implementation_wf.md) |
| Testing | [unit-testing_wf](workflows/unit-testing_wf.md) |
| Performance | [performance-optimization_wf](workflows/performance-optimization_wf.md) |

---

## 📋 Pre-Implementation Checklist

Before writing ANY code, verify:

- [ ] **Strings**: Added to `strings.xml` with proper prefix (`action_`, `title_`, `msg_`, etc.)
- [ ] **Colors**: Using theme colors only (`MaterialTheme.colorScheme.primary`)
- [ ] **File Location**: Confirmed correct folder per File Placement table
- [ ] **Model Classes**: Placed in `model/{feature}/` folder

---

## Global Rules Reference

| Rule | Link |
|------|------|
| **UI Implementation** | [ui-implementation-rules](rules/ui-implementation-rules.md) |
| **Dimensions** | [dimensions-rules](rules/dimensions-rules.md) |
| **Padding Rules** | [padding-calculation-rules](rules/padding-calculation-rules.md) |
| **Preview Screen** | [preview-screen-rules](rules/preview-screen-rules.md) |
| **Model Placement** | [model-placement-rules](rules/model-placement-rules.md) |
| String Naming | [string-naming-rules](rules/string-naming-rules.md) |
| Colors | [color-declaration-rules](rules/color-declaration-rules.md) |
| SOLID Principles | [solid-principles-rules](rules/solid-principles-rules.md) |
| Folder Structure | [folder-structure-rules](rules/folder-structure-rules.md) |
| Routes | [route-management-rules](rules/route-management-rules.md) |
