---
description: Build configuration
---

# Build Configuration

> **Context**: Configuring builds. No secrets in code.

## Secrets

```properties
# local.properties (gitignored)
API_KEY=secret_key
```

```kotlin
// build.gradle.kts
buildConfigField("String", "API_KEY", "\"${localProps["API_KEY"]}\"")
```

## Usage

```kotlin
// ✅ Correct
val key = BuildConfig.API_KEY

// ❌ Wrong
val key = "sk-12345..."
```

## Rules

> **Must follow:** [secrets-management-rules](../rules/secrets-management-rules.md)
