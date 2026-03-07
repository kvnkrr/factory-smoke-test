# UI/UX Design: Todo App MVP

**Project:** factory-smoke-test | **Version:** 1.0 | **Status:** COMPLETE

---

## Wireframes

### Desktop (≥ 768px)
```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│                      💡 Todo App                            │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐    │
│  │   ┌────────────────────────┐  ┌───────────────┐    │    │
│  │   │ What needs doing?      │  │     Add       │    │    │
│  │   └────────────────────────┘  └───────────────┘    │    │
│  └─────────────────────────────────────────────────────┘    │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  • Buy groceries                              [×]   │    │
│  ├─────────────────────────────────────────────────────┤    │
│  │  • Call dentist at 3pm                        [×]   │    │
│  └─────────────────────────────────────────────────────┘    │
│                    2 items                                  │
└─────────────────────────────────────────────────────────────┘
                      480px max-width
```

### Mobile (< 768px)
```
┌───────────────────────────┐
│        💡 Todo App        │
│  ┌──────────────────────┐ │
│  │ What needs doing?    │ │
│  └──────────────────────┘ │
│  ┌──────────────────────┐ │
│  │         Add          │ │
│  └──────────────────────┘ │
│  • Buy groceries     [×]  │
│  • Call dentist      [×]  │
│         2 items           │
└───────────────────────────┘
```

---

## Design System

| Element | Specification |
|---------|---------------|
| **Colors** | Primary `#3b82f6`, Text `#1f2937`, Border `#e5e7eb`, Delete `#ef4444` |
| **Typography** | System fonts, 28px heading, 16px body |
| **Spacing** | 4px base unit, 16px standard padding |
| **Icons** | Unicode only: 💡 × • |

---

## Component Specs

### Header
- Text: "💡 Todo App"
- Font: 28px, weight 600, color #1f2937, center aligned

### Input Form
- **Desktop:** Side-by-side input + button (flex row)
- **Mobile:** Stacked (flex column)
- Input: placeholder "What needs doing?", blue focus ring
- Button: bg #3b82f6, white text, rounded corners

### Todo Item
- Bullet prefix (•) + text + delete button
- Padding: 16px, bottom border #e5e7eb
- Delete: "×" character, gray default, red on hover

### Empty State
- Message: "No todos yet! Add one above to get started"
- Centered, muted color (#9ca3af), 48px padding

### Footer
- Item count centered, 14px, muted color

---

## Responsive Breakpoint
```css
@media (max-width: 767px) {
  .form { flex-direction: column; }
  .button { width: 100%; margin-top: 8px; }
}
```

---

## Accessibility
- ✅ Semantic HTML (`<form>`, `<ul>`, `<li>`)
- ✅ Keyboard navigation (Enter to submit)
- ✅ Focus indicators (blue ring)
- ✅ WCAG AA contrast
- ✅ 44px touch targets

---

## Implementation Notes
- All CSS inline in `<style>` tag
- Use `textContent` (not `innerHTML`) for XSS prevention
- Trim input whitespace
- No animations

---

**Status:** COMPLETE → Ready for Implementation
