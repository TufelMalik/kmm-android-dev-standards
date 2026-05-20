<div align="center">

# 🚀 KMM & Android Master Standards

**The ultimate playbook for scalable, pixel-perfect, and compliant engineering.**

[![Kotlin Multiplatform](https://img.shields.io/badge/Kotlin_Multiplatform-7F52FF?style=for-the-badge&logo=kotlin&logoColor=white)](#)
[![Jetpack Compose](https://img.shields.io/badge/Jetpack_Compose-3DDC84?style=for-the-badge&logo=android&logoColor=white)](#)
[![Clean Architecture](https://img.shields.io/badge/Clean_Architecture-00599C?style=for-the-badge)](#)
[![SOLID Principles](https://img.shields.io/badge/SOLID-Enforced-E34F26?style=for-the-badge)](#)

*Zero guesswork. Strict layering. 100% UI fidelity.*

---

</div>

## 🧠 Core Philosophy

This repository contains the mandatory rules, workflows, and guidelines for our engineering stack. Whether you are a human engineer or an AI coding agent, these rules are **non-negotiable**.

* 🎨 **Pixel-Perfect UI:** 100% adherence to design specifications. No approximations, no "close enough". Padding, typography, and colors must map exactly to the provided tokens.
* 🏗 **Clean Architecture & MVVM:** Strict separation of concerns. UI observes StateFlows; ViewModels handle logic; Repositories manage data. 
* 🧱 **SOLID by Default:** Code must be easily testable, highly cohesive, and loosely coupled.
* 🌐 **Write Once, Run Anywhere:** Optimized for Kotlin Multiplatform Mobile (KMM) to share core logic across platforms seamlessly.

---

## 📂 The Playbook

<details open>
<summary><b>📐 1. Architectural Rules (MANDATORY)</b></summary>
<br>
These rules define how code is structured, named, and separated.

| Rule | Description |
| :--- | :--- |
| 🗂 **[Project Structure](rules/6_project_structure.md)** | Mandatory folder hierarchy for KMM & Android (`ui/`, `components/`, `viewModel/`). |
| 🏷 **[File Naming](rules/3_file_naming.md)** | Strict suffix-based naming (`[Feature]Screen`, `[Feature]ViewModel`). |
| 🧪 **[Testing Protocol](rules/5_testing.md)** | Requirements for Unit, Integration, and UI testing following the Testing Pyramid. |
| 🌳 **[Git Workflow](rules/4_git_workflow.md)** | GitFlow, Conventional Commits, and Semantic Versioning standards. |

</details>

<details open>
<summary><b>🎨 2. UI & Design System</b></summary>
<br>
Rules for creating consistent, accessible, and dynamic interfaces.

| Rule | Description |
| :--- | :--- |
| 🧵 **[String Conventions](rules/1_string_naming.md)** | Prefixing rules (`action_`, `title_`) for `strings.xml`. **No hardcoded text.** |
| 🎨 **[Color Management](rules/2_color_management.md)** | Semantic M3 token mapping. **No hardcoded hex values.** |
| 📏 **[Dimensions & Padding](kmm-workflows/rules/dimensions-rules.md)** | 8dp grid system enforcement. Dynamic padding calculations. |

</details>

---

## ⚡ Development Workflows

Don't reinvent the wheel. Follow these step-by-step workflows to execute common engineering tasks flawlessly.

### 🖼 UI Implementation (From Figma/Images)
1. **[Start Here: UI Implementation Workflow](kmm-workflows/workflows/ui-implementation_wf.md)**
2. Setup Theme: [Colors](kmm-workflows/workflows/theme-colors-setup_wf.md) & [Typography](kmm-workflows/workflows/typography-setup_wf.md)
3. Calculate precision padding: [Padding Workflow](kmm-workflows/workflows/calculate-padding_wf.md)

### ⚙️ Logic & Architecture
* **[ViewModel Implementation](kmm-workflows/workflows/viewmodel-pattern_wf.md)** - Setup DI, Repositories, and State.
* **[State Management](kmm-workflows/workflows/state-management_wf.md)** - Establish single-source-of-truth UI states.
* **[API Integration](kmm-workflows/workflows/api-integration_wf.md)** - Connect robust network clients via Repositories.

### 🛣 Project Setup
* **[Feature Module Creation](kmm-workflows/workflows/feature-module-creation_wf.md)**
* **[Navigation Setup (Type-Safe)](kmm-workflows/workflows/navigation-setup_wf.md)**

---

## 🛑 The "Never Do" List

> **CRITICAL:** Violating these rules will result in immediate PR rejection or prompt failure.

1. **NEVER** hardcode a string in a Compose file. Always use `stringResource(Res.string.prefix_name)`.
2. **NEVER** hardcode a color (`Color(0xFF...)`). Define it in `Color.kt` and access via `MaterialTheme.colorScheme`.
3. **NEVER** use raw `.dp` or `.sp` numeric values in UI. Use the `Dimensions` object (e.g., `Dimensions.Spacing.Md`).
4. **NEVER** call an API or Database directly from a UI file or ViewModel. Route all data through a dedicated Repository interface.
5. **NEVER** skip creating a `@Preview` for a new component or screen. Previews must be added to `PreviewScreen.kt`.

---

<div align="center">
  <i>Maintained for absolute precision and scalable engineering.</i>
</div>
