# Todo App
Version: 1.0
Status: MVP

---

## 1. Purpose

A minimal todo application used for Autonomous Factory end-to-end regression testing.
The factory building this app exercises all 9 phases: spec analysis, planning,
architecture, UX, implementation, review, security, production, and final checks.

---

## 2. Functional Requirements

### FR1 – Single Page Application

- REQ-001: The app must consist of a single `index.html` file with inline CSS and JavaScript
- REQ-002: The page must be openable directly in a browser via `file://` or a static server with zero external dependencies

### FR2 – Todo List

- REQ-003: The page must display a text input and an "Add" button that appends the entered text as a new item to a list below
- REQ-004: Each item in the list must display its text and a "Delete" button that removes it
- REQ-005: The list must be displayed as a vertical list of items using HTML and CSS

---

## 3. Package Manifest

The app must have a valid `package.json` with:
- name: "factory-smoke-test"
- A "test" script that exits 0
- Zero runtime dependencies

---

## 4. Acceptance Criteria

The app is complete when opening `index.html` shows an input, an Add button, items can be added and deleted, and `npm test` exits with code 0.
