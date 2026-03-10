# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A single-file personal finance spending pace tracker (`fun-spend-tracker_3.html`). No build process, no dependencies, no package manager — open the HTML file directly in a browser.

## Development

Open the file directly in a browser:
```
open fun-spend-tracker_3.html
```

There are no build, lint, or test commands. All code (HTML, CSS, JS) lives in the single file.

## Architecture

The entire app is one self-contained HTML file with three sections:
- `<style>` — CSS with custom properties for theming
- `<body>` — Tab-based UI (Status, Update Balance, History) + Settings modal
- `<script>` — All app logic in vanilla JS

**State** is stored in `localStorage` under two keys:
- `funmoney_settings` — `{ monthlyLimit, billingDay }`
- `funmoney_history` — array of `{ balance, note, timestamp }` entries

**Core data flow:**
1. `getCycleInfo()` — Determines current billing cycle start/end and days elapsed
2. `getStatus()` — Computes expected vs. actual spend, calculates pacing delta
3. `render()` — Updates the DOM; called after every state change
4. `save()` — Persists `settings` and `history` to localStorage

**Chart** is rendered as inline SVG by `renderChart()`, drawing a plan line (expected linear pace) and stepped actual line from history entries.

The billing cycle wraps across months — if `billingDay` is 15, the cycle runs from the 15th of one month to the 14th of the next. `getCycleInfo()` handles month-boundary edge cases.
