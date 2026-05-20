# Dependency Compatibility Rules

> **Why**: Only KMM-compatible libs work across platforms.

## ✅ DO

- Check KMM compatibility first
- Add multiplatform libs to commonMain
- Remove unused dependencies

## ❌ DON'T

- Don't add Android-only libs to commonMain
- Don't add duplicates

## KMM Libraries

Ktor, Kotlinx.serialization, Koin, SQLDelight, Kotlinx.datetime
