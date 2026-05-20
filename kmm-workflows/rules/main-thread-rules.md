# Main Thread Rules

> **Why**: Long operations freeze UI.

## ✅ DO

- Run IO on `Dispatchers.IO`
- Use coroutines for async work

## ❌ DON'T

- Don't block main thread
- Don't make sync network calls
