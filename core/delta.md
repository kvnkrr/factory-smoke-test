# Delta Brief (initial build)

New: 46
Modified: 0
Removed: 0
Unchanged: 0

## New Requirements

Initial build — all requirements are new.

### Factory Smoke Test
- Version: 1.0
- Status: MVP
- ---

### 1. Purpose
- A single-page web app that displays a visual factory production line.
- The page shows 5 stations in a horizontal pipeline. Each station
- transitions from grey to amber to green with a short animation,
- simulating items moving through a manufacturing line.
- Used for Autonomous Factory end-to-end regression testing.
- The factory building this app exercises: workspace setup, delta extraction,
- agent invocation, install, test, and release stages.
- ---

### FR1 – Single Page Application
- The app must:
- Consist of a single index.html file (no build step, no dependencies)
- Be openable directly in a browser via file:// or a static server
- Use inline CSS and JavaScript only (zero external dependencies)

### FR2 – Production Line Visualisation
- The page must display:
- A title: "Factory Smoke Test — Production Line"
- A horizontal row of 5 stations labelled: Intake, Build, Test, Inspect, Release
- Each station is a box/card showing the station name and a status indicator
- On page load, stations animate sequentially (left to right):
- Start: all stations are grey ("Pending")
- Each station turns amber ("Running") for 1 second, then green ("Complete")
- The next station starts after the previous one turns green
- After all 5 stations are green, display "ALL STATIONS COMPLETE" below the line
- Use a green colour (#22c55e) for complete and amber (#f59e0b) for running

### FR3 – Package Manifest
- The app must:
- Have a valid package.json with name "factory-smoke-test"
- Define a "start" script: npx serve . (or similar static server)
- Define a "test" script that exits 0 (e.g., echo "OK")
- Have zero runtime dependencies
- ---

### 3. Regression Reset
- The factory stores core/spec.md in the product repo after each build.
- On subsequent runs with the same spec, the delta engine finds all items
- "unchanged" and generates zero tasks — making the run a no-op.
- For repeatable regression runs, the product repo must be reset before
- each run. The factory provides scripts/reset-regression-repo.sh which:
- Force-pushes the repo to an initial state (single commit with just README.md)
- Removes all factory-generated code, specs, and evidence
- Ensures the delta engine sees all requirements as "new" on next run
- Usage:
- ---

### 4. Acceptance Criteria
- The app is complete when:
- Opening index.html in a browser shows 5 stations animating through the pipeline
- All stations reach green "Complete" state
- "ALL STATIONS COMPLETE" text appears after the animation
- npm test exits with code 0
