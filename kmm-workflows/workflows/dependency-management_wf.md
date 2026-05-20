---
description: Adding dependencies
---

# Dependency Management

> **Context**: Adding library. Check KMM compatibility first.

## Placement

| Type | Source Set |
|------|------------|
| Multiplatform | `commonMain` |
| Android-only | `androidMain` |
| iOS-only | `iosMain` |

## KMM-Compatible Libraries

- HTTP: Ktor
- JSON: Kotlinx.serialization
- DI: Koin
- DB: SQLDelight
- DateTime: Kotlinx.datetime

## Rules

> **Must follow:** [dependency-compatibility-rules](../rules/dependency-compatibility-rules.md)
