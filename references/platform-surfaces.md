# Platform Surfaces

Use this after classifying the UI intent. Platform is a constraint layer: keep the shared frontend quality rules, then adjust interaction, copy, and validation for the runtime surface.

## Identify The Target

- Web: browser pages, admin systems, dashboards, H5/mobile web, embedded webviews, or SSR/SPA apps.
- Mini-program: WeChat/Alipay/uni-app-style pages where navigation, lifecycle, permissions, and device APIs follow the mini-program runtime.
- App: native, React Native, Flutter, hybrid, or packaged mobile/desktop apps where the OS, native navigation, and device permissions affect behavior.
- Mixed surface: web content inside an app, mini-program pages backed by shared web code, or cross-platform components. Identify the delivery target before choosing validation.

When the target is unclear and the platform affects behavior, inspect project files, route manifests, build scripts, or user screenshots before assuming "browser web".

## Web

Design and review for:

- Browser routing, query params, history behavior, deep links, refresh, and auth redirects.
- Desktop, tablet, and mobile breakpoints when the product supports them.
- Keyboard access, focus order, focus return after dialogs, and accessible names.
- Browser constraints such as overflow, sticky elements, scroll containers, file uploads, and unsupported APIs.
- Admin/product density: tables, filters, dialogs, drawers, pagination, charts, and local loading states.

Validation:

- Use lint, typecheck, unit tests, and framework build scripts when available.
- Use a real browser for route, form, modal, upload, responsive, focus, and visual-overlap checks.
- Use repeatable browser automation for important flows or regressions; use a logged-in Chrome session only when current user state is required.

## Mini-Program

Design and review for:

- Touch-first operation, bottom safe areas, narrow screens, thumb reach, and stable primary actions.
- Page lifecycle such as first load, return from child pages, pull-to-refresh, and navigation-stack limits.
- Authorization, login, profile completion, camera, scan, location, voice, payment, and file APIs.
- Avoiding repeated nuisance prompts; only interrupt when the user must decide now.
- Repeated capture or input workflows: keep prior entries visible, preserve progress, and anchor the next action where the user naturally continues.
- Runtime style limits: avoid assuming DOM APIs, arbitrary CSS support, browser storage behavior, or desktop hover/focus patterns.

Validation:

- Run the project or framework build command when available.
- Use WeChat Developer Tools or the target mini-program tool for page routing, lifecycle, permissions, and device API behavior.
- Use a real device when camera, scan, voice, payment, location, safe-area, keyboard, or permission prompts matter.
- Treat simulator-only layout checks as insufficient for device API or permission-sensitive changes.

## App

Design and review for:

- Native navigation, system back gestures, deep links, tabs, modals, and route restoration.
- Safe areas, keyboard avoidance, orientation, dynamic type/text scaling, and platform-specific controls.
- Permission prompts, offline/weak-network behavior, background/foreground transitions, push entry points, and native module failure paths.
- OS expectations: Android back behavior, iOS swipe-back and safe-area treatment, desktop app window resizing when applicable.
- Cross-platform components: verify that the shared abstraction maps cleanly to each target platform instead of assuming web behavior.

Validation:

- Run the target framework checks or builds when available.
- Use simulator/emulator for broad flow checks, but use a real device for permissions, keyboard, camera, audio, payment, push, Bluetooth, or performance-sensitive paths.
- Do not treat a browser preview, Storybook, or web build as proof that the native app path works.

## Handoff

State the platform assumptions in the final handoff when they shaped the implementation or validation:

- Target platform and any mixed-surface assumptions.
- Validation actually run for that platform.
- Platform-specific risk left unverified, such as no real-device check or unavailable logged-in state.
