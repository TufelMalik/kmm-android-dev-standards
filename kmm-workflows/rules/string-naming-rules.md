# String Naming Rules

> [!CAUTION]
> **NEVER write text directly in Compose code.**  
> **ALWAYS use `stringResource(R.string.xxx)`**

## MANDATORY 2-Step Process

### Step 1: Add to `strings.xml`

```xml
<string name="action_login">Login</string>
```

### Step 2: Use in Compose

```kotlin
Text(stringResource(R.string.action_login))
```

---

## Prefixes (MUST use one)

| Prefix | For | Example |
|--------|-----|---------|
| `action_` | Buttons | `action_submit` |
| `title_` | Titles | `title_home` |
| `msg_` | Messages | `msg_success` |
| `error_` | Errors | `error_network` |
| `hint_` | Hints | `hint_email` |
| `label_` | Labels | `label_name` |
| `str_` | Other | `str_welcome` |

## Format: `prefix_purpose` in snake_case

---

## ❌ WRONG (hardcoded in Compose)

```kotlin
Text("Login")
Button(onClick = {}) { Text("Submit") }
```

## ✅ RIGHT (stringResource)

```kotlin
Text(stringResource(R.string.action_login))
Button(onClick = {}) { Text(stringResource(R.string.action_submit)) }
```

---

## Plurals

```xml
<plurals name="msg_items">
    <item quantity="one">%d item</item>
    <item quantity="other">%d items</item>
</plurals>
```

```kotlin
pluralStringResource(R.plurals.msg_items, count, count)
```
