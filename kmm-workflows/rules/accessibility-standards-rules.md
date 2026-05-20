# Accessibility Standards Rules

> **Why**: Accessibility ensures all users can use the app.

## ✅ DO

- Add `contentDescription` to meaningful images or icons
- Use minimum 48dp touch targets
- Use sp for text (scales with settings)
- Use `Dimens` for all dp/sp values

## ❌ DON'T

- Don't use `contentDescription = null` for meaningful icons or images
- Don't use small touch targets
- Don't hardcode dp/sp values

## Dimensions

Use/Create `Dimensions.kt` for all dp/sp values. See [dimensions-rules](dimensions-rules.md).
