# Frontend Work Workflow

Use this as the operating sequence for frontend changes. It combines hardening, polish, audit, and product-taste work without letting any one concern dominate the task.

## 1. Classify the Change

Identify the smallest useful scope before editing:

- Platform surface: web, mini-program, app, or mixed wrapper. Use [platform-surfaces.md](platform-surfaces.md) when runtime constraints affect interaction or validation.
- Bug fix: reproduce or reason from the failing path, fix the root cause, then sweep nearby similar interactions.
- Interaction change: list the states the user can enter before, during, and after the action.
- Visual polish: inspect the neighboring UI and design system first; keep changes proportional to the complaint.
- Redesign: confirm whether the user wants direction exploration. For visible taste dissatisfaction, generate or propose 3 to 5 concepts before code if the change is broad.
- New UI: establish information hierarchy, primary workflow, validation strategy, and responsive behavior before writing components.

## 2. Read the Local System

Before inventing a solution, inspect:

- Shared UI components, request helpers, form/table abstractions, permissions, route guards, icons, token files, and global styles.
- Nearby screens that solve the same problem.
- Existing validation scripts such as `npm run build`, `npm run build:prod`, `npm run lint`, `npm run typecheck`, or framework-specific checks.
- Project instructions and taste docs: `AGENTS.md`, `PRODUCT.md`, `DESIGN.md`, style guides, README notes.

For component-library admin systems, expect useful patterns around table loading flags, dialog state, form validation helpers, row actions, and repo-specific build scripts. Verify against the actual repo scripts.

## 3. Scope the Fix Conservatively

Default to the existing product shape:

- Narrow UI complaint: first fix alignment, spacing, hierarchy, text overflow, control state, or component choice in the existing area.
- Do not redesign a whole modal/page because one control looks weak.
- Do not introduce a new visual language when shared components already exist.
- Sweep only directly similar cases that share the same failure mode, such as sibling submit buttons or row actions in the same module.

Escalate scope when the current structure blocks the user workflow, creates repeated errors, or the user explicitly asks for a redesign.

## 4. Implement State Completeness

During implementation, handle:

- Initial loading, refresh loading, submit loading, row-action loading, and disabled states.
- Success, recoverable error, validation error, empty state, no search results, permission-denied state, and unauthenticated state.
- Slow network, duplicate click, rapid filter changes, stale responses, and request cancellation where available.
- Long labels, long CJK strings, long numbers, dates, missing values, and responsive wrapping.
- Keyboard focus, focus return after modal close, icon-only button labels, and touch target size.

See [interaction-states.md](interaction-states.md) for concrete async patterns.

## 5. Validate at the Right Layer

Use the lightest validation that can catch the expected regression:

- Static hygiene: `git diff --check`; inspect changed files for accidental churn.
- Code confidence: run available lint, typecheck, unit tests, or framework build scripts.
- Project build: for admin repos, run the actual production build script when available instead of assuming a lightweight syntax check is enough.
- Browser validation: use for modals, forms, responsive layout, routing, filtering, uploads, focus behavior, and visual overlap.
- Mini-program validation: use the target mini-program tool or real device when lifecycle, permissions, camera, scan, voice, payment, location, safe areas, or keyboard behavior matters.
- App validation: use simulator/emulator or real device checks for native navigation, OS permissions, safe areas, keyboard avoidance, deep links, offline behavior, and native modules.
- Current-session validation: use the user's active browser or desktop automation only when the real logged-in session or desktop state matters.

If you start a temporary dev server, stop it before handoff unless the user explicitly wants it left running.

## 6. Handoff

Report only high-signal details:

- What surface changed.
- What predictable frontend pitfalls were covered.
- What validation ran.
- What platform assumptions shaped validation, when relevant.
- Any remaining risk, such as untested browser state or unavailable backend.

Do not dump the whole checklist unless the user asks for it.
