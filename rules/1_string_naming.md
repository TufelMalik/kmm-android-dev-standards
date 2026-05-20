# Rule 1: String Naming Conventions in strings.xml

> **Official Reference:** [Android String Resources](https://developer.android.com/guide/topics/resources/string-resource)

---

## Rule: Use Proper Prefixes for String Resources

**Why:** According to Android's official resource naming guidelines, consistent naming conventions improve code maintainability, enable IDE auto-completion, and make string resources easily searchable. Prefixes immediately communicate the string's purpose and prevent naming collisions in large projects.

**Official Best Practice:** Android recommends descriptive resource names that indicate their purpose rather than their location in the UI.

---

## Naming Conventions

| Prefix | Usage | Example |
|--------|-------|---------|
| `action_` | Buttons, clickable actions | `action_submit`, `action_cancel` |
| `str_` | General screen/body text | `str_welcome_message`, `str_description` |
| `title_` | Screen titles, section headers | `title_home`, `title_settings` |
| `msg_` | User-facing messages | `msg_success_saved`, `msg_loading` |
| `error_` | Error messages | `error_network_failed`, `error_invalid_input` |
| `hint_` | Input field hints | `hint_enter_email`, `hint_password` |
| `label_` | Labels for UI elements | `label_username`, `label_age` |
| `cd_` | Content descriptions for accessibility | `cd_menu_icon`, `cd_close_button` |

---

## Additional Rules

- **Avoid duplicate strings** - If the same text appears in multiple places, use the same string resource
- Use **snake_case** for string names
- Always provide **meaningful names** that describe the content, not the location

---

## Example: strings.xml

```xml
<!-- strings.xml -->
<resources>
    <!-- Actions -->
    <string name="action_login">Login</string>
    <string name="action_sign_up">Sign Up</string>
    <string name="action_forgot_password">Forgot Password?</string>
    <string name="action_save">Save</string>
    <string name="action_cancel">Cancel</string>
    <string name="action_delete">Delete</string>
    <string name="action_retry">Retry</string>
    
    <!-- Titles -->
    <string name="title_onboarding">Welcome to Know U</string>
    <string name="title_profile">Your Profile</string>
    <string name="title_settings">Settings</string>
    <string name="title_home">Home</string>
    
    <!-- Messages -->
    <string name="msg_login_success">Successfully logged in!</string>
    <string name="msg_processing">Processing your request…</string>
    <string name="msg_saved">Changes saved successfully</string>
    <string name="msg_loading">Loading…</string>
    
    <!-- Errors -->
    <string name="error_empty_email">Email cannot be empty</string>
    <string name="error_invalid_credentials">Invalid email or password</string>
    <string name="error_network">Network error. Please try again.</string>
    <string name="error_unknown">Something went wrong</string>
    
    <!-- Hints -->
    <string name="hint_enter_email">Enter your email</string>
    <string name="hint_enter_password">Enter your password</string>
    <string name="hint_search">Search…</string>
    
    <!-- Content Descriptions (Accessibility) -->
    <string name="cd_app_logo">Application logo</string>
    <string name="cd_back_button">Navigate back</string>
    <string name="cd_menu_icon">Open menu</string>
    <string name="cd_close_button">Close</string>
    <string name="cd_profile_image">User profile image</string>
    
    <!-- Labels & General Text -->
    <string name="label_email">Email Address</string>
    <string name="label_password">Password</string>
    <string name="str_terms_conditions">By continuing, you agree to our Terms and Conditions</string>
    <string name="str_no_items">No items found</string>
</resources>
```

---

## Usage in Jetpack Compose

```kotlin
// In Composable
Text(
    text = stringResource(R.string.title_onboarding),
    style = MaterialTheme.typography.headlineMedium
)

Button(onClick = { /* ... */ }) {
    Text(stringResource(R.string.action_login))
}

Icon(
    imageVector = Icons.Default.Menu,
    contentDescription = stringResource(R.string.cd_menu_icon)
)

OutlinedTextField(
    value = email,
    onValueChange = { email = it },
    label = { Text(stringResource(R.string.label_email)) },
    placeholder = { Text(stringResource(R.string.hint_enter_email)) }
)
```

---

## ❌ Common Mistakes to Avoid

```xml
<!-- ❌ WRONG: No prefix, unclear purpose -->
<string name="login">Login</string>

<!-- ✅ CORRECT: Clear prefix indicates it's an action/button -->
<string name="action_login">Login</string>

<!-- ❌ WRONG: Location-based naming (fragile) -->
<string name="home_screen_title">Home</string>

<!-- ✅ CORRECT: Purpose-based naming -->
<string name="title_home">Home</string>

<!-- ❌ WRONG: Duplicate strings -->
<string name="action_ok_dialog">OK</string>
<string name="action_ok_button">OK</string>

<!-- ✅ CORRECT: Single reusable string -->
<string name="action_ok">OK</string>

<!-- ❌ WRONG: Missing content description -->
<Icon imageVector = Icons.Default.Close, contentDescription = null />

<!-- ✅ CORRECT: Accessibility-friendly -->
<Icon imageVector = Icons.Default.Close, contentDescription = stringResource(R.string.cd_close_button) />
```

---

## String Formatting with Arguments

```xml
<!-- strings.xml -->
<string name="msg_welcome_user">Welcome, %1$s!</string>
<string name="msg_items_count">You have %1$d items</string>
<string name="msg_price">Price: %1$.2f</string>
```

```kotlin
// Usage in Compose
Text(stringResource(R.string.msg_welcome_user, userName))
Text(stringResource(R.string.msg_items_count, itemCount))
Text(stringResource(R.string.msg_price, price))
```
 ---

## Multi-language Support

*   **Rule**: For multi-language support, use the same string `name` (key) across different `strings.xml` files (e.g., `values/strings.xml`, `values-es/strings.xml`, `values-fr/strings.xml`). The value of the string will vary based on the locale.

```xml
<!-- values/strings.xml (English) -->
<string name="title_onboarding">Welcome to Know U</string>
```

```xml
<!-- values-es/strings.xml (Spanish) -->
<string name="title_onboarding">Bienvenido a Know U</string>
```

```kotlin
// Usage in Compose (system handles locale automatically)
Text(stringResource(R.string.title_onboarding))
```
---

## Plurals (Quantity Strings)

```xml
<!-- strings.xml -->
<plurals name="msg_tasks_remaining">
    <item quantity="zero">No tasks remaining</item>
    <item quantity="one">%d task remaining</item>
    <item quantity="other">%d tasks remaining</item>
</plurals>
```

```kotlin
// Usage in Compose
Text(pluralStringResource(R.plurals.msg_tasks_remaining, count, count))
```

---

*Following these conventions ensures consistent, maintainable, and accessible string resources across your project.*
