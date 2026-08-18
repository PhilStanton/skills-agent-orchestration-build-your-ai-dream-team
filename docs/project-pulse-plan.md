# Mona's Project Pulse — Implementation Plan

Saved file: docs/project-pulse-plan.md

---

1) Summary

Mona's Project Pulse is a small, static dashboard that displays the current status of engineering projects. The app is a single-page static site (no backend). It reads its data from app/project-data.json and renders a responsive grid of project cards (name, owner, status, recentActivity, priority). Deliverables are a polished frontend (app/index.html + app/styles.css + optional app/main.js), a sample data file (app/project-data.json), and a VS Code launch configuration (.vscode/launch.json) so reviewers can run the app inside the Codespace easily.

Scope (explicit)
- Static, client-side only (vanilla HTML/CSS/JS).
- Data comes from app/project-data.json (top-level "projects" key).
- Visual design to be provided by Designer; Coder implements the design.
- Provide a small runnable support via .vscode/launch.json (preLaunchTask to serve the app and open index.html).
- Accessibility and responsive layout included.

---

2) Ordered implementation steps

Each step lists owner, purpose, files to create/modify, and acceptance criteria.

Step 1 — Save plan and kickoff (Planner → Orchestrator)
- Owner: Planner / Orchestrator
- Purpose: Persist this plan and assign tasks to Designer and Coder.
- Files to create: docs/project-pulse-plan.md (this file)
- Acceptance criteria: Plan saved in docs/project-pulse-plan.md; Orchestrator posts assignments to Designer and Coder and confirms start dates.

Step 2 — Create project data schema & sample dataset (Coder)
- Owner: Coder
- Purpose: Provide deterministic sample data that the UI will consume.
- Files to create: app/project-data.json (CREATE)
- Requirements / acceptance:
  - Top-level key "projects" with an array of project objects.
  - Each project object contains keys: name, owner, status, recentActivity, priority.
  - At least 5 sample projects with varied status and priority values.
  - Example (for guidance only; Coder should store in file):
    {
      "projects": [
        { "name": "Acorn CRM", "owner": "M. Patel", "status": "On Track", "recentActivity": "2026-08-17T14:20:00Z", "priority": "High" },
        ...
      ]
    }
- Acceptance criteria: app/project-data.json exists, parses as JSON with the required keys, and contains representative items.

Step 3 — Designer: visual spec & assets (Designer)
- Owner: Designer
- Purpose: Provide UI/UX directions, component specs and assets for implementation.
- Files to create: docs/design-spec.md (CREATE), docs/designs/pulse-wireframe.png (or .svg / .png) and any exported icons in docs/designs/
- Deliverables:
  - High-level layout (desktop/tablet/mobile), spacing, and component specs for a "project-card" and ".dashboard" grid.
  - Color tokens and mapping (status → color), typography choices (system font or Google font choice), border-radius and box-shadow specs.
  - Accessibility notes (contrast ratios, focus states).
  - Small set of SVG icons (optional) and exact CSS variables to implement (e.g., --pp-primary, --pp-bg, --pp-status-ontrack).
- Acceptance criteria: Designer provides a short spec and at least one high-fidelity mockup (desktop + mobile).

Step 4 — Coder: HTML scaffold & placeholder content (Coder)
- Owner: Coder
- Purpose: Create a semantic HTML skeleton so Designer can validate layout and the rest of the code can be integrated.
- Files to create/modify: app/index.html (CREATE), app/main.js (CREATE as lightweight stub)
- Requirements:
  - index.html must include a <main> with a container element that has class .dashboard (explicit requirement).
  - Include placeholder .project-card elements (static) so Designer can verify spacing and hierarchy without data binding.
  - Link to app/styles.css and include <script src="main.js" defer></script>.
- Acceptance criteria: index.html opens in a browser and shows the static placeholder layout; .dashboard exists in DOM and can be styled; placeholder cards use class .project-card.

Step 5 — Coder: CSS implementation (Coder)
- Owner: Coder
- Purpose: Turn Designer tokens and wireframes into working CSS.
- Files to create/modify: app/styles.css (CREATE or MODIFY)
- Requirements:
  - Provide CSS variables for color tokens and spacing.
  - Include a .dashboard selector (required).
  - Implement .project-card with border-radius and box-shadow, status badge styling, and responsive grid (mobile-first).
  - Ensure readable typography and spacing, and visible keyboard focus styles.
- Acceptance criteria: When index.html is loaded, the page visually matches the design direction; .dashboard and .project-card are styled per spec; color contrast meets WCAG AA for body text.

Step 6 — Coder: Data binding & client-side rendering (Coder)
- Owner: Coder
- Purpose: Load app/project-data.json and render project cards dynamically.
- Files to create/modify: app/main.js (MODIFY), app/index.html (may add a template or container if missing)
- Requirements:
  - Fetch app/project-data.json (using fetch) and render DOM elements into .dashboard using the project-card template.
  - Display name, owner, status, recentActivity (human-friendly) and priority.
  - Graceful error handling: show a user-friendly message if data fails to load or is invalid.
  - Fallback: display a short note suggesting "Run the local server (python3 -m http.server) if loading via file:// fails".
- Acceptance criteria: With a local static server running (see Step 8), the page renders all sample projects from app/project-data.json. Missing fields show fallbacks (e.g., "Unknown owner").

Step 7 — Accessibility, responsiveness, micro-UX polish (Designer + Coder)
- Owner: Designer + Coder
- Purpose: Final visual and accessibility pass.
- Files to modify: app/index.html, app/styles.css
- Tasks:
  - Add ARIA roles where appropriate (cards as list items with role="article" if necessary).
  - Ensure interactive elements are keyboard focusable; visible focus ring; readable contrast.
  - Test across 320px → 1280px widths; fix wrapping/overflow; apply ellipses where needed.
- Acceptance criteria: Keyboard navigation works; a11y checklist passed; cards remain readable and not clipped at narrow widths.

Step 8 — Add VS Code launch + serve configuration (Coder)
- Owner: Coder
- Purpose: Provide runnable-app support inside the Codespace.
- Files to create/modify: .vscode/launch.json (CREATE), .vscode/tasks.json (MODIFY — add a "Serve Project Pulse" task)
- Requirements (strict):
  - .vscode/launch.json must be strict JSON (no comments).
  - Provide a launch configuration named "Run Project Pulse Dashboard" that uses a browser debug type or URL to open http://localhost:8000/index.html and has cwd set to "${workspaceFolder}/app".
  - Use a preLaunchTask "Serve Project Pulse" that runs a lightweight static server in ${workspaceFolder}/app (e.g., python3 -m http.server 8000).
- Acceptance criteria: Running the launch configuration starts the server (task) and opens the dashboard in the browser at the expected URL.

Step 9 — Smoke test & validation pass (Coder)
- Owner: Coder
- Purpose: Validate behavior and produce verification artifacts for the PR.
- Files to create/modify: docs/final-handoff.md (CREATE — optional notes & screenshots)
- Tests:
  - Start the preLaunchTask or run: cd app && python3 -m http.server 8000 and open http://localhost:8000/index.html.
  - Verify the UI loads, project cards render from JSON, and status/priority display is correct.
  - Test error states (rename project-data.json to simulate missing file).
  - Check keyboard navigation and mobile width screenshots (e.g., 360px and 1024px).
- Acceptance criteria: Smoke test checklist passes and screenshots included in PR.

Step 10 — PR & handoff (Orchestrator)
- Owner: Orchestrator
- Purpose: Merge and close the work with clear handoff notes.
- Files to create/modify: docs/final-handoff.md (MODIFY), PR description (on GitHub)
- PR description must include run instructions, validation checklist, screenshots, known issues, and design link(s).
- Acceptance criteria: PR includes the required files, passes repository checks, and includes the verification checklist and screenshots.

---

3) File assignments (explicit per step)

- Step 1 (Planner)
  - docs/project-pulse-plan.md (CREATE) — this plan file.

- Step 2 (Coder)
  - app/project-data.json (CREATE)

- Step 3 (Designer)
  - docs/design-spec.md (CREATE)
  - docs/designs/pulse-wireframe-desktop.png (or .svg) (CREATE)
  - docs/designs/pulse-wireframe-mobile.png (CREATE)
  - docs/designs/icons/*.svg (OPTIONAL)

- Step 4 (Coder)
  - app/index.html (CREATE)
  - app/main.js (CREATE: initial stub)

- Step 5 (Coder)
  - app/styles.css (CREATE)

- Step 6 (Coder)
  - app/main.js (MODIFY to implement data binding)

- Step 7 (Designer + Coder)
  - app/index.html (MODIFY for ARIA or micro-UX changes)
  - app/styles.css (MODIFY for accessibility tweaks)

- Step 8 (Coder)
  - .vscode/launch.json (CREATE)
  - .vscode/tasks.json (MODIFY: add serve task)

- Step 9 (Coder)
  - docs/final-handoff.md (CREATE — test results and screenshots)

- Step 10 (Orchestrator)
  - docs/final-handoff.md (MODIFY — final PR summary if needed)
  - GitHub PR (created by Orchestrator / developer)

Consolidated owners (no overlaps during parallel work)
- Designer owns: docs/design-* and docs/designs/*
- Coder owns: app/* and .vscode/*
- Orchestrator/Planner owns: docs/project-pulse-plan.md and docs/final-handoff.md (coordination + PR)

---

4) Designer responsibilities

Deliverables
- A compact design spec (docs/design-spec.md) including:
  - Desktop and mobile mockups (one each) in docs/designs/.
  - Color palette with hex values and a status → color mapping (On Track / At Risk / Blocked / Complete).
  - Typography decisions (font-family and sizes for headings/body) and spacing scale.
  - Component spec for .project-card and status badge (border-radius and box-shadow numbers).
  - Focus and active states for interactive elements.
  - Accessibility notes: minimum contrast ratios and expected ARIA roles.
- Provide exported assets:
  - SVG icons (if used) sized for inline use.
  - Any avatar or placeholder images to be used in the cards.
- Validation:
  - Approve a first-implementation screenshot (desktop & mobile) produced by the Coder before final polish.
  - Confirm status color mapping and that status badges are visually distinct and accessible.

Designer acceptance checklist
- Provide docs/design-spec.md and at least one desktop + one mobile export.
- Supply token values (CSS variables) to be used in app/styles.css.
- Sign off on implemented screenshots before final PR is submitted.

---

5) Coder responsibilities

Implementations
- Implement and maintain the app folder:
  - app/index.html — semantic markup, .dashboard container, accessible markup.
  - app/styles.css — CSS variables, .dashboard selector, .project-card styling with border-radius and box-shadow.
  - app/project-data.json — sample project data (top-level "projects" key).
  - app/main.js — fetch data and render DOM; handle errors/fallbacks; keep code framework-free (vanilla JS).
- Provide runnable support:
  - .vscode/launch.json — strict JSON launch config to open the dashboard.
  - .vscode/tasks.json — add a "Serve Project Pulse" task to run a simple static server from workspaceFolder/app.
- Accessibility & testing:
  - Implement visible focus states and ARIA roles where applicable.
  - Ensure responsive CSS and handle text overflow with ellipses and wrapping rules.
- Documentation:
  - docs/final-handoff.md — run instructions and validation results + screenshots.

Validation steps Coder must run locally
- Start the server (preLaunchTask or manually): cd app && python3 -m http.server 8000
- Open http://localhost:8000/index.html (or use VSCode launch, which should run the server and open the page).
- Verify:
  - The page loads and renders the sample projects from app/project-data.json.
  - The .dashboard element exists and contains .project-card elements equal to the number of items in JSON.
  - Status badges display proper color mapping.
  - At least one screenshot: desktop (1024px) and mobile (375px).
  - Error state: temporarily rename app/project-data.json and confirm the UI shows a useful error message.
  - Keyboard navigation: tab to each card and interactive elements; focus is visible.
- Include these checks in the PR description and in docs/final-handoff.md.

---

6) Dependencies

Internal file ordering
- app/project-data.json (Step 2) should be created early so Coder can integrate data quickly.
- app/index.html (Step 4) must exist before the final data-driven rendering (Step 6).
- app/styles.css (Step 5) should be implemented or have a first pass before final design sign-off, but an initial CSS scaffold can be done in parallel to design.
- .vscode/launch.json (Step 8) depends on the application being present (app/index.html) and on an available serve task in .vscode/tasks.json.

External requirements
- Codespace or local machine with:
  - Python 3 (for python3 -m http.server) OR any static server (node http-server or live-server).
  - Recent browser (Chrome, Edge, Firefox).
  - VS Code with the Copilot CLI environment (per repository expectations).
  - No npm/build step is required; keep everything static.

Notes about file:// vs http://
- Fetching a JSON file via fetch from file:// is blocked in many browsers. Use the provided VS Code launch preLaunchTask (or run python3 -m http.server) when testing locally. The code should detect fetch errors and show helpful instructions if run via file://.

---

7) Parallel work decisions

Work that can run in parallel (no file conflicts)
- Designer: create docs/design-spec.md and assets in docs/designs/ (no changes to app/ files).
- Coder (parallel):
  - Create app/index.html skeleton with placeholder cards (Step 4).
  - Create app/project-data.json (Step 2).
  - Start a basic app/styles.css with CSS variables (a minimal baseline, Step 5 initial pass).
- These three can run simultaneously because designers work only under docs/ and coders under app/ and .vscode/.

Work that must run sequentially
- Final CSS polishing (Step 5 final polish) should wait for Designer sign-off (Step 3 deliverables).
- Data-driven rendering (Step 6) should be implemented once app/index.html exists (Step 4) and app/project-data.json exists (Step 2).
- Launch config that runs a server and opens the page (.vscode/launch.json) should be created after app/index.html is in place (Step 8).
- Final accessibility sign-off (Step 7) should follow implementation of data binding and CSS.

File-ownership rules to prevent conflicts
- Designer: writes only in docs/design-* and docs/designs/*
- Coder: writes only in app/* and .vscode/*
- Planner/Orchestrator: writes coordination docs (docs/project-pulse-plan.md, docs/final-handoff.md)
- This prevents two people editing the same files when work is parallelized.

---

8) Validation expectations

Manual checks (must run before PR)
- Start the app:
  - Preferred: use VS Code launch task (Run Project Pulse Dashboard) which runs the serve task and opens the browser.
  - Alternative: cd app && python3 -m http.server 8000; open http://localhost:8000/index.html
- Verify:
  - Page loads and renders projects from app/project-data.json (minimum 5 sample entries).
  - .dashboard is present and contains .project-card elements with name, owner, status, recentActivity, and priority visible.
  - Status badges use the Designer-specified colors and meet contrast requirements.
  - Layout adapts to mobile widths (360–420px) and desktop widths (≥1024px).
  - Keyboard accessibility: tab order covers interactive items; focus is visible.
  - Error handling: rename or corrupt project-data.json and confirm the UI shows a clear message telling the reviewer how to start the server.
  - Confirm that app/styles.css contains a .dashboard selector (explicit requirement).
  - Confirm .vscode/launch.json exists and references workspaceFolder/app as cwd.

Minimal smoke tests (automatable or manual)
- curl http://localhost:8000/index.html => HTTP 200
- Browser loads → check console for fetch errors (none)
- Data count matches JSON length
- Status color mapping: check a known project status has the correct color

What to include in the PR description (required)
- Short summary of implemented features (1–2 paragraphs).
- Files changed (list).
- How to run locally (explicit steps; e.g., "Open VS Code, run 'Run Project Pulse Dashboard' configuration" and fallback "cd app && python3 -m http.server 8000").
- Validation checklist with results (tick boxes):
  - [ ] index.html loads
  - [ ] sample projects render
  - [ ] responsive screenshots attached
  - [ ] accessibility checks performed (list)
  - [ ] launch config verified
- Attachments: 2 screenshots (desktop and mobile) and one short GIF (optional) showing data rendering.
- Known issues and limitations (e.g., no search/sort/filter implemented, fallback needed for file://).
- Links to design files (docs/design-spec.md and docs/designs/).

---

Edge cases to handle

- project-data.json missing or 404: show friendly message with steps to run the local server.
- Invalid JSON: show "Data invalid" message with console error for debugging.
- Empty projects array: show "No projects to display" UI state.
- Missing fields in a project object: render safe fallbacks (e.g., "Unknown owner", status: "Unknown").
- Long text (project name / owner): use word-wrap or ellipsis; ensure cards do not overflow container.
- Unrecognized status/priority values: map to a neutral style rather than breaking.
- Large number of projects: for this simple app accept normal rendering but ensure the grid wraps gracefully; avoid expensive DOM operations (render in documentFragment).
- file:// fetch restrictions: include a clear fallback message that instructs reviewers to run a simple server.
- Time parsing: recentActivity might be an ISO timestamp or free text; prefer to accept ISO 8601 and render relative time (or human date), but fall back to raw string if parsing fails.

---

Open questions (needs quick Orchestrator / stakeholder decisions)

- Status taxonomy: Confirm canonical status values (example: "On Track", "At Risk", "Blocked", "Complete"). Are others needed?
- Priority values: Confirm allowed priorities (High, Medium, Low) and whether they must map to visual emphasis.
- RecentActivity format: Should the UI parse ISO 8601 and show "2 days ago" or just show a short date string? (Recommended: ISO 8601 → human-friendly.)
- Interactivity: Should we include basic filters (by status or priority) or sorting? (Recommendation: keep minimal; add filter UI as an enhancement if time permits.)
- Avatars/avatars source: Should owner pictures be shown (require external images) or just use initials? (Recommendation: initials.)
- Fonts: Use system fonts or include a web font (Google Font)? (System fonts are smallest & simplest.)
- Browser support baseline (modern browsers only? IE is not required.)
- Are we permitted to modify .vscode/tasks.json? (Plan assumes yes so launch works.)

---

Appendix — Quick example of required JSON structure (for Designer & Coder reference)
- Note: do not paste this into repo from the plan; Coder will create the file in Step 2.

{
  "projects": [
    {
      "name": "Acorn CRM",
      "owner": "M. Patel",
      "status": "On Track",
      "recentActivity": "2026-08-17T14:20:00Z",
      "priority": "High"
    },
    {
      "name": "Breeze Billing",
      "owner": "L. Gomez",
      "status": "At Risk",
      "recentActivity": "2026-08-16T10:05:00Z",
      "priority": "Medium"
    }
  ]
}

---

Notes for the Orchestrator (task delegation guidance)
- Assign Step 3 to Designer and set a 24–48 hour window for a first mockup.
- Assign Steps 2, 4, and initial Step 5 (baseline CSS variables) to Coder to run in parallel with Step 3.
- Reserve final CSS polish (Step 5 final) and Step 7 (a11y sign-off) until Designer approves screenshots from Step 4/5.
- Require Coder to open a draft PR once Steps 2–6 are implemented; Designer should attach final mockups to that PR and sign off before final merge.

This plan is intentionally concrete: file ownership is split to avoid collisions, required files (app/index.html, app/styles.css, app/project-data.json, .vscode/launch.json) are assigned, and validation steps are explicit so the Orchestrator can hand-off the work to Designer and Coder agents without ambiguity.