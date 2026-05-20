---
description: Managing assets
---

# Asset Management

> **Context**: Adding icons/images. Reuse before adding new.

## Location

- KMM: `composeResources/drawable/`
- Android: `res/drawable/`

## Naming

| Type | Pattern |
|------|---------|
| Icons | `ic_name.xml` |
| Images | `img_name.webp` |
| Backgrounds | `bg_name.xml` |

## Usage

```kotlin
Image(
    painter = painterResource(Res.drawable.ic_home),
    contentDescription = stringResource(Res.string.cd_home)
)
```

## Rules

> **Must follow:** [asset-reuse-rules](../rules/asset-reuse-rules.md)
