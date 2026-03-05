# Acceptance Criteria (Factory Smoke Test — Production Line)

The app is complete when:

1. Opening `index.html` in a browser shows **5 stations** animating sequentially left → right.
2. All stations reach the green **"Complete"** state.
3. After the final station completes, **"ALL STATIONS COMPLETE"** appears below the line.
4. `npm test` exits with **code 0**.

## How to verify

### Browser (file://)
- Open `artifacts/factory/index.html` directly in a browser.

### npm test
```bash
cd artifacts/factory
npm test
```

### Optional: static server
```bash
cd artifacts/factory
npm start
```
