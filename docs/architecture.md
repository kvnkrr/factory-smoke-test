# Architecture: Todo App MVP

## Overview

This document defines the system architecture for the Todo App MVP—a minimal single-page application designed for Autonomous Factory end-to-end regression testing. The architecture prioritizes simplicity, zero dependencies, and file-system compatibility.

---

## Architectural Principles

| Principle | Rationale |
|-----------|-----------|
| **Single File** | All code (HTML, CSS, JS) in one `index.html` file per REQ-001 |
| **Zero Dependencies** | No external libraries, frameworks, or CDN resources per REQ-002 |
| **File-System Compatible** | Works when opened via `file://` protocol without a server |
| **Security First** | XSS prevention through safe DOM APIs |
| **Minimal Complexity** | Just enough to validate all 9 factory phases |

---

## File Structure

```
factory-smoke-test/
├── index.html          # Single-page application (all code inline)
├── package.json        # Package manifest with test script
└── docs/
    └── architecture.md # This document
```

---

## Component Architecture

### HTML Structure

```
index.html
├── <head>
│   ├── <meta charset>     # UTF-8 encoding
│   ├── <meta viewport>    # Mobile-responsive viewport
│   ├── <title>            # Page title
│   └── <style>            # All CSS inline
└── <body>
    ├── <h1>               # App header
    ├── <form>             # Input form
    │   ├── <input>        # Text input for todo
│   │   └── <button>       # Add button (submit)
│   └── <ul>               # Todo list container
        └── <li> × N       # Dynamic todo items
            ├── <span>     # Todo text
            └── <button>   # Delete button
```

### CSS Organization

| Section | Purpose |
|---------|---------|
| Reset | Basic margin/padding normalization |
| Layout | Body container and spacing |
| Form | Input and Add button styling |
| List | Vertical list layout and item styling |
| Item | Individual todo item with delete button |
| States | Hover effects and empty state |

### JavaScript Architecture

#### State Management

```javascript
// In-memory state (volatile, resets on refresh)
const todos = [];      // Array of todo objects: { id, text }
let nextId = 1;        // Auto-incrementing ID counter
```

#### Core Functions

| Function | Purpose | Triggers |
|----------|---------|----------|
| `addTodo(text)` | Add new item to array | Form submission |
| `removeTodo(id)` | Remove item by ID | Delete button click |
| `render()` | Rebuild DOM from state | After any state change |

#### Event Handling

| Event | Target | Handler | Action |
|-------|--------|---------|--------|
| `submit` | `<form>` | `handleAdd()` | Prevent default, trim input, call `addTodo()`, clear input |
| `click` | Delete `<button>` | `handleDelete(id)` | Call `removeTodo(id)` |

---

## Data Model

### Todo Item Schema

```typescript
interface Todo {
  id: number;    // Unique identifier (auto-incrementing)
  text: string;  // User-entered todo content
}
```

### State Characteristics

- **Storage**: In-memory only (JavaScript array)
- **Persistence**: None (localStorage intentionally avoided)
- **Lifetime**: Page session only
- **Reset**: On browser refresh

---

## Rendering Strategy

### Approach: Full Re-render

Given the minimal scope, a simple full re-render on every state change is sufficient:

1. Clear existing list (`list.innerHTML = ''`)
2. Check for empty state
3. Iterate todos array
4. Create DOM elements for each todo
5. Append to list container

### DOM Creation Pattern

```javascript
// Safe DOM creation (XSS prevention)
const li = document.createElement('li');
const span = document.createElement('span');
span.textContent = todo.text;  // Safe: escapes HTML
li.appendChild(span);
```

**Critical**: Use `textContent`, never `innerHTML` for user input.

---

## Security Considerations

### XSS Prevention

| Risk | Mitigation |
|------|------------|
| User input in DOM | Use `textContent` instead of `innerHTML` |
| Script injection | No dynamic HTML generation from user input |
| Event handlers | Inline `onclick` with bound ID (no eval) |

### CSP Compatibility

- All resources inline (no external fetches)
- No inline event handlers in HTML (attached via JS)
- Compatible with strict Content Security Policies

### File:// Protocol Safety

- No AJAX/fetch calls (blocked by browser)
- No WebSocket connections
- No cookies or storage mechanisms needed

---

## Browser Compatibility

| Feature | Support |
|---------|---------|
| Modern browsers | Chrome, Firefox, Safari, Edge (last 2 versions) |
| ES6 features | Arrow functions, const/let, template literals |
| DOM APIs | Standard Level 2 DOM (universally supported) |

---

## Testing Strategy

### Unit Testing (via package.json)

```json
{
  "scripts": {
    "test": "node -e \"console.log('OK'); process.exit(0)\""
  }
}
```

The test script satisfies the requirement to exit with code 0.

### Manual Testing

1. Open `index.html` in browser
2. Verify input field visible
3. Type text, click Add → item appears
4. Click Delete → item removed
5. Run `npm test` → exits 0

---

## Build & Deployment

### No Build Process

The application requires no compilation, bundling, or transpilation.

### Deployment Options

| Method | Command/Action |
|--------|----------------|
| File protocol | Double-click `index.html` |
| Static server | `python -m http.server` or `npx serve` |
| CDN | Upload single file to any static host |

---

## Requirements Mapping

| Requirement | Architecture Decision |
|-------------|----------------------|
| REQ-001: Single `index.html` | All code inline; no external files |
| REQ-002: Zero dependencies | No `<script src>` or CDN links |
| REQ-002: File:// compatible | No external resources, no modules |
| REQ-003: Input + Add button | Form with submit handler |
| REQ-004: Text + Delete button | Each item renders span + button |
| REQ-005: Vertical list | `<ul>` with `<li>` children, CSS styling |
| Package: name | `factory-smoke-test` |
| Package: test script | Node one-liner exiting 0 |
| Package: zero dependencies | No `dependencies` in package.json |

---

## Open Questions

None. Scope is intentionally minimal for factory validation.

---

## Decision Log

| Date | Decision | Rationale |
|------|----------|-----------|
| 2025-03-06 | Single-file architecture | Satisfies REQ-001 and simplifies deployment |
| 2025-03-06 | No persistence | Reduces complexity; volatile state acceptable for MVP |
| 2025-03-06 | Full re-render on change | Simpler than diffing for small N (todos) |
| 2025-03-06 | `textContent` over `innerHTML` | XSS prevention |

---

## References

- [core/spec.md](../core/spec.md) — Product specification
- [docs/spec-analysis.md](./spec-analysis.md) — Spec analysis
- [docs/plan.md](./plan.md) — Project plan
