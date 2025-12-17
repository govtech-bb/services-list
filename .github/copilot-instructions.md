# GitHub Copilot instructions for this repo

Purpose
- Help AI coding agents be productive quickly on this small, static website: the "Register of government services" (two pages: `index.html`, `goals.html`).

Quick summary (Big picture)
- This is a static, client-side site built with plain HTML/CSS/JS (no build system, no dependencies).
- `index.html` is the main services register UI (filters, ministries, service list). Several UI handlers referenced inline (e.g. `applyFilters()`, `filterServices()`, `showMinistries()`, `showHome()`) are currently not implemented and the page expects a dataset.
- `goals.html` contains a working example of client-side data handling: a large `goalData` array, a `categories` map, and functions (`init`, `filter`, `render`, `toggle`, `esc`) that demonstrate the project's simple DOM-driven patterns.

What an agent should do first
- Inspect `goals.html` to understand data shape and UI patterns — it is the canonical example for filtering and rendering. Use the exact patterns there when implementing missing behaviour in `index.html`.
- Do not introduce frameworks or build tooling unless explicitly requested by a human maintainer.

Project-specific conventions & patterns
- Data is embedded as plain JS arrays/objects (see `var goalData = [...]` in `goals.html`). Objects commonly include: `name`, `description`, `type` ("Service" or "Form"), `ministry`, `department`, `category`.
- `categories` maps machine keys to human labels (e.g. `'skills_training' -> 'Skills & Training'`). Keep this mapping consistent across pages.
- Rendering and filtering rely on simple DOM manipulation: build HTML strings and set `innerHTML` on container nodes; escaping is handled by creating a DOM node and using `textContent` (see `esc()` in `goals.html`).
- Styles are inline in `<style>` elements; responsive behavior is implemented with a 768px breakpoint.

Concrete tasks an agent can perform (examples)
- Implement `index.html` behaviour by reusing code from `goals.html`: add a `services` array (or fetch `services.json`) and implement `filterServices()`, `applyFilters()`, and `renderServices()` mirroring `filter()` / `render()`.
- If adding a new data file (e.g. `services.json`), keep the same property names (see above) and test by loading it via `fetch(...)` and falling back to an embedded array for offline/local testing.
- Add a minimal `esc` helper to protect against XSS when inserting user-provided strings into HTML.

Developer workflows (no surprises)
- There is no build step — test locally by opening `index.html` / `goals.html` in a browser or run a static server: `python3 -m http.server 8000` from the repo root (macOS / Linux).
- Debug in the browser DevTools. Modify `goalData` in-memory to test filters quickly.

When to change global structure
- Introduce a build system or a framework only after agreement with maintainers. If you must add a new dependency, document it in `README.md` and include simple dev commands (serve, lint, build).

Formatting and tests
- There are no automated tests. If adding JS modules, include small unit tests (Jest or plain test harness) only when requested.

Safety & style notes for agents
- Preserve the government look-and-feel (colors, header/footer). Avoid large stylistic rewrites.
- Prefer small, well-documented incremental changes with clear PR descriptions referencing this file.

Examples (copy-paste friendly)
- Minimal service object:
  { "name": "Apply for X", "description": "...", "type": "Service", "ministry": "Ministry of Y", "department": "Dept", "category": "infrastructure" }

- Simple fetch fallback pattern (pseudo):
  fetch('services.json').then(r=>r.json()).catch(()=> fallbackArray).then(renderServices)

If anything here is unclear or you want deeper coverage (e.g., accessibility rules, test scaffolding, or a migration path to a static site generator), tell me which area to expand and I will update this file.