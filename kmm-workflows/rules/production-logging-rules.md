# Production Logging Rules

> **Why**: Logs can expose sensitive data.

## ✅ DO

- Use centralized logger
- Disable in production
- Mask sensitive data

## ❌ DON'T

- Don't log passwords/tokens
- Don't use `println()` directly
