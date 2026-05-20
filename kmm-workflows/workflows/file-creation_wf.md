---
description: Creating files in KMM project
---

# File Creation

> **Context**: Creating a new file. Check placement and naming.

## Checklist

1. Determine context (why needed)
2. Check [folder-structure-rules](../rules/folder-structure-rules.md)
3. Name file properly (PascalCase)
4. If Screen → add to navigation
5. If Screen → **add `@Preview` + update `PreviewScreen.kt`** (see [preview-screen-rules](../rules/preview-screen-rules.md))
6. If Component → check if exists in `uiToolKit/`
7. If Component → **add `@Preview`** (see [preview-screen-rules](../rules/preview-screen-rules.md))
8. If Platform-specific → use expect/actual

## Placement

| Type | Location |
|------|----------|
| Screen | `modules/{feature}/ui/` |
| Component (feature) | `modules/{feature}/components/` |
| Component (shared) | `uiToolKit/` |
| ViewModel | `modules/{feature}/viewModel/` |
| Repository | `repository/` |
| Model | `model/data/` |
| Utility | `utils/helpers/` |

## Rules

> **Must follow:** [folder-structure-rules](../rules/folder-structure-rules.md) | [model-placement-rules](../rules/model-placement-rules.md) | [preview-screen-rules](../rules/preview-screen-rules.md)
