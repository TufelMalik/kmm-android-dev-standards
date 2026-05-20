---
description: Using strings in KMM project
---

# String Usage

> **Context**: Adding or using strings. Follow naming prefixes.
>
> **Must follow:** [1_string_naming.md](../../rules/1_string_naming.md)

## Checklist

1. Use correct prefix (see table)
2. Add to `composeResources/values/strings.xml`
3. Use `stringResource(Res.string.xxx)` in Compose
4. For multi-lang: same key in `values-{locale}/strings.xml`

## Prefixes

| Prefix | Use |
|--------|-----|
| `action_` | Buttons |
| `title_` | Titles |
| `str_` | Body text |
| `msg_` | Messages |
| `error_` | Errors |
| `hint_` | Hints |
| `label_` | Labels |
| `cd_` | Accessibility |

## Example

```kotlin
// ✅ Correct
Text(stringResource(Res.string.title_home))

// ❌ Wrong
Text("Home")
```
