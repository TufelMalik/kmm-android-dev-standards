# Compose KMM & Android Project Rules

> **Based on Official Guidelines:**  
> - [Android Developer Documentation](https://developer.android.com/)
> - [Kotlin Coding Conventions](https://kotlinlang.org/docs/coding-conventions.html)
> - [Material Design 3](https://m3.material.io/)
> - [Android App Architecture Guide](https://developer.android.com/topic/architecture)

This directory contains AI agent rules for working with Compose Multiplatform (KMM) and Compose Android projects, strictly following official recommendations.

## Rules Overview

1. **[String Naming Conventions](./1_string_naming.md)**  
   **Use when**: Adding new strings to `strings.xml`. Ensures consistent prefixing (`action_`, `title_`, etc.) for easier maintenance.

2. **[Color Management](./2_color_management.md)**  
   **Use when**: Defining new colors or implementing dark mode. Follows Material Design 3 and semantic naming.

3. **[File Naming & Architecture](./3_file_naming.md)**  
   **Use when**: Creating new files, classes, or interfaces. Enforces PascalCase, camelCase, and architectural layering.

4. **[Git Workflow](./4_git_workflow.md)**  
   **Use when**: Creating branches, committing code, or preparing releases. Adheres to GitFlow and Conventional Commits.

5. **[Testing Best Practices](./5_testing.md)**  
   **Use when**: Writing unit, integration, or UI tests. Defines naming standards and testing scope.

6. **[Project Structure](./6_project_structure.md)**  
   **Use when**: Creating new modules, features, or deciding where to place files. Defines the mandatory folder hierarchy for KMM and Android.

7. **[Web Development Best Practices](./7_web_development.md)**  
   **Use when**: Developing web applications in PHP or other web technologies. Covers security (OWASP), architecture, API design, performance optimization, and industry standards.

---

## Quick Reference

### String Prefixes
- `action_` - Buttons/actions
- `str_` - Body text
- `title_` - Titles/headers
- `msg_` - Messages
- `error_` - Errors
- `hint_` - Input hints
- `cd_` - Content descriptions

### File Naming
- Classes: `PascalCase.kt`
- Screens: `LoginScreen.kt`
- ViewModels: `LoginViewModel.kt`
- Repositories: `UserRepository.kt`

### Git Branches
- `feature/123-description` - New features
- `bugfix/234-description` - Bug fixes
- `release/1.2.0` - Release prep
- `hotfix/1.2.1-description` - Urgent fixes

### Commit Types
- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation
- `refactor:` - Code refactoring
- `test:` - Tests
- `chore:` - Maintenance

### Web Development (PHP)
- **Classes**: `PascalCase` (UserController, PaymentService)
- **Methods**: `camelCase` (getUserById, processPayment)
- **Constants**: `UPPER_SNAKE_CASE` (MAX_LOGIN_ATTEMPTS)
- **Tables**: `plural_snake_case` (users, order_items)
- **Columns**: `singular_snake_case` (user_id, created_at)
- **Security**: Always use prepared statements, hash passwords with `password_hash()`, validate & sanitize inputs
- **API**: RESTful design, versioning, proper HTTP methods, rate limiting

---

## Usage

Each rule file contains:
- **Official references** to the source standards
- **Why** the rule exists (rationale)
- **How** to implement it (examples)
- **Common mistakes** to avoid

Refer to individual rule files for detailed implementations and code examples.

---

*Generated for AI Agent use in Compose Multiplatform and Android projects*
