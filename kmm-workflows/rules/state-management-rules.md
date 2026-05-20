# State Management Rules

> **Why**: Single source of truth prevents bugs.

## ✅ DO

- One state class per screen
- Single StateFlow in ViewModel
- UI observes via `collectAsState()`

## ❌ DON'T

- Don't duplicate state in UI
- Don't mutate state from UI
