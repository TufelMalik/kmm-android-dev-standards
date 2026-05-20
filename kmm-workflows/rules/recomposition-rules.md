# Recomposition Rules

> **Why**: Excessive recomposition causes lag.

## ✅ DO

- Use stable keys in lists
- Use `remember` for expensive ops
- Use `derivedStateOf` for computed values

## ❌ DON'T

- Don't do heavy work in Composables
- Don't skip keys in LazyColumn
