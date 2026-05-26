# Review Checklist

Use this before final handoff and when asked to review frontend code. Keep the checklist mostly internal; report only meaningful findings and validation.

## Findings Severity

- P0: blocks the main workflow, loses data, creates duplicate writes, exposes unauthorized action, or breaks production build.
- P1: common user path fails, slow network creates bad state, major responsive overlap, inaccessible critical action, or likely regression.
- P2: polish, edge case, copy, minor accessibility, or maintainability issue that can reasonably wait.

When the user asks for a review, lead with findings in severity order and include file/line references.

## Scope And Context

Check:

- The implementation matches the user's requested scope.
- The target platform is identified: web, mini-program, app, or mixed wrapper.
- Existing project components and patterns are reused.
- The change does not introduce a second visual language.
- Similar nearby actions with the same failure mode were considered.
- Unrelated files and formatting churn are absent.

## Interaction Completeness

Check:

- Async writes have visual loading/disabled state and handler-level re-entry guard.
- Every lock releases on success, failure, validation cancel, and user cancel.
- Row actions, batch actions, toggles, uploads, imports, and keyboard submit paths are protected.
- Refreshes use local loading where appropriate and avoid disruptive page-wide overlays.
- Stale responses cannot overwrite newer filter/tab/page state.
- Success and error paths leave the UI in a clear, recoverable state.
- Command inputs distinguish selected mode, suggestion text, and real user input. Slash commands should not duplicate the mode label in the chip and the submitted content.
- Composer auto-grow behavior has been checked for empty, one-line, multi-line, max-height, and clear-after-submit states.

## Data And Edge Cases

Check:

- Empty list, no search result, permission denied, readonly, and missing-value states.
- Long CJK text, long numbers, long names, and narrow viewport wrapping.
- Pagination reset after filter changes.
- Date/time formatting and timezone assumptions when visible to users.
- API error messages are specific enough without exposing internals.

## Accessibility

Check:

- Keyboard reachability and activation.
- Focus-visible styles and modal focus behavior.
- Accessible names for icon-only controls.
- Color-independent state communication.
- Sufficient contrast for text, disabled states, errors, and links.
- Touch target size, safe-area spacing, and keyboard avoidance for mobile, mini-program, or app surfaces.

## Visual And Taste Fit

Check:

- The screen feels like the same product as neighboring pages.
- Product/admin UI remains restrained, dense, and useful.
- Cards are not nested unnecessarily.
- Top controls, central work area, and persistent bottom commands have deliberate spacing and hierarchy.
- Persistent bottom composers or command bars leave enough bottom padding for empty states, the last message/row, and mobile keyboards; verify narrow viewports after content exists, not only on the empty screen.
- Starter prompts or example cards fill the composer as suggestions when the user is expected to customize them; they should clear predictably on focus or click.
- Popovers, dropdowns, selects, command palettes, and tooltips match the current theme and density, including panels rendered outside the page root.
- Secondary actions do not occupy standalone vertical space when they can live in a status row or compact toolbar.
- Loading, empty, and error states do not feel theatrical.
- Button text, dialog titles, and destructive confirmations name the actual action.
- No decorative gradients/orbs/stock filler were added to operational tools.

## Performance And Stability

Check:

- Heavy renders, table updates, search, and filtering avoid unnecessary full-page refresh.
- Loading states do not cause layout jumps.
- Debounce/throttle is used only when it fits the interaction.
- Timers, listeners, subscriptions, and pending requests are cleaned up where relevant.

## Validation

Choose validation based on risk:

- `git diff --check` for whitespace/conflict hygiene.
- Lint/typecheck/unit tests when available and relevant.
- Frontend build script for component/template regressions.
- Browser verification for real interaction, modal, form, route, upload, layout, or responsive behavior.
- Browser verification for theme switching should inspect component-library portal surfaces such as select dropdowns, context menus, tooltips, and poppers, not only the main page background.
- For slash commands or command palettes, verify keyboard navigation, active option movement, command acceptance, command cancellation, and the final normalized input text.
- Mini-program tool or real-device verification for lifecycle, navigation stack, permissions, camera/scan/voice/payment/location, safe area, or keyboard paths.
- Real-device or target-tool verification for press-and-hold gestures, cancellation thresholds, and thumb-reach issues.
- App simulator/emulator or real-device verification for native navigation, OS permissions, deep links, offline/foreground transitions, native modules, safe area, or keyboard behavior.

If validation cannot run, state the blocker and what risk remains.

## Handoff Template

Keep final responses short:

- Changed: the surfaces or files touched.
- Covered: the main frontend pitfalls addressed.
- Verified: commands or browser checks run.
- Risk: anything not verified or dependent on unavailable services.
