# Storage Access Rules

> **Why**: Proper abstraction enables testing.

## ✅ DO

- Access storage only via Repository
- Create DataSource classes
- Use expect/actual for platform storage

## ❌ DON'T

- Don't access storage from UI/ViewModel directly
