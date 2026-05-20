# Secrets Management Rules

> **Why**: Leaked secrets are vulnerabilities.

## ✅ DO

- Store in `local.properties` (gitignored)
- Access via BuildConfig

## ❌ DON'T

- Don't commit secrets to git
- Don't hardcode API keys
