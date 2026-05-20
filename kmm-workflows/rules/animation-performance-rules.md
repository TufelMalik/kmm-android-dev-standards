# Animation Performance Rules

> **Why**: Heavy animations cause jank.

## ✅ DO

- Use lightweight transforms (alpha, scale)
- Keep durations 150-500ms
- Make reusable

## ❌ DON'T

- Don't run heavy work during animation
- Don't block UI thread
