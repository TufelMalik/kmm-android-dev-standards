# Feature Isolation Rules

> **Why**: Features must be independent.

## ✅ DO

- Keep feature code within its folder
- Share via interfaces/navigation
- Use DI for cross-feature needs

## ❌ DON'T

- Don't import directly from other features
- Don't create circular dependencies
