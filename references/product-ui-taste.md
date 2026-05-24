# Product UI Taste

Use this reference for admin systems, dashboards, internal tools, operational products, and Chinese product UI copy.

## Default Taste

For product/admin work, aim for a restrained operational workspace:

- Dense but readable information.
- Predictable navigation, filters, tables, dialogs, drawers, and forms.
- Quiet hierarchy over decorative styling.
- Utility copy over marketing copy.
- Existing design system over invented component styles.
- Clear primary action, secondary action, destructive action, and disabled action treatment.

Avoid:

- Marketing hero sections inside tools.
- Decorative gradient blobs, orbs, bokeh, abstract SVG filler, or stock-like decoration.
- Card-on-card page structures.
- Oversized headings inside compact tools.
- New palettes or motion systems for one local fix.
- Rewriting a whole layout when the problem is alignment, spacing, state, or copy.

## When The User Says It Looks Bad

First decide whether the complaint is local polish or direction mismatch:

- Local polish: fix the smallest surface that creates the ugliness, such as alignment, density, button placement, wrapping, icon choice, contrast, or hierarchy.
- Direction mismatch: compare the current screen with nearby accepted screens or reference products. For broad redesign, present 3 to 5 visual directions before coding.
- "Still average": stop polishing the same direction mechanically. Re-evaluate structure, hierarchy, and product purpose.

Do not turn a practical admin workflow into a landing page unless the user explicitly asks for that.

## Layout And Density

- Prefer one clear toolbar/filter area, one main content area, and one predictable pagination/action area for list pages.
- Keep table actions compact and consistent.
- Let forms scan vertically; group related fields, but avoid unnecessary nested cards.
- Keep buttons stable in size and position when loading text appears.
- Use responsive constraints so controls wrap intentionally rather than overlap.
- Long Chinese text should wrap or truncate with a title/tooltip according to table density.
- Empty states should guide the next action without becoming promotional.

## Copy

Chinese product copy should sound like an internal product written by a person:

- Prefer concrete verbs: `保存`, `生成邀请码`, `作废`, `删除`, `重新上传`, `导入`.
- Avoid vague labels like `确定`, `提交`, `处理`, `操作` when the consequence can be named.
- Error messages should say what failed and what the user can do next.
- Confirmation dialogs should name the object and consequence.
- Loading text should be specific only when it helps; otherwise a button spinner plus disabled state is enough.
- Avoid explanatory paragraphs inside dense tools unless the user needs domain guidance to proceed.

## Visual Polish Rules

- Match spacing, border radius, typography, and icon language to the existing app.
- Reserve high-contrast or saturated colors for meaningful state.
- Do not make a one-note palette by overusing one hue family.
- Use icons where the product already uses them, but do not hide unfamiliar actions behind unlabeled icons.
- Text must fit its parent at supported viewport sizes; if it cannot, change the layout rather than shrinking with viewport width.
- Preserve accessible contrast and focus states.

## Loading Taste

- A full-screen or full-page dimmer feels disruptive when the existing content is still useful.
- Fast operations should use local busy state or a stable inline indicator instead of flashing global loading UI.
- Section refreshes should not wipe the section unless stale content would be misleading.
- For tables, local loading inside the table is usually better than blocking the whole page.

## Project Fit

For RuoYi-style admin systems:

- Prefer established table/search/dialog patterns.
- Keep toolbar actions and row actions consistent with neighboring modules.
- Use existing Element UI/Element Plus patterns unless the project has wrapped them.
- Validate with the repo's actual frontend build script, often `npm run build:prod` in older Vue admin projects.

For mini programs or mobile-first flows:

- Avoid global dimming for very fast interactions.
- Keep the primary task visually dominant.
- For repeated capture/input workflows, keep prior entries visible and anchor the primary input control near the bottom instead of placing oversized controls in the content stream.
- Use local locks for answer/submit actions.
- Respect touch target size and narrow-screen wrapping.

## Mobile Operational Layouts

For task-heavy mini-program pages with a top control area, central work area, and bottom primary action:

- Identify the real primary action first. If the bottom action is the main repeated operation, make top actions feel like a compact control strip instead of competing primary buttons.
- Merge low-risk secondary actions such as `切换`, `筛选`, or `更多` into the status row when space allows. Avoid giving a short secondary link its own full-height row.
- Keep one visual rhythm between stacked regions. Top-to-middle and middle-to-bottom gaps should feel intentionally related, not accidental.
- Use fixed middle workspaces when a persistent bottom command would otherwise cover content. Let the middle list scroll internally, but hide native scrollbars unless the product specifically needs a visible scrollbar.
- Avoid the “three unrelated buttons” feeling on mobile. When a page has two top actions and one bottom action, demote or group the top actions so the bottom command remains the obvious next step.
- For dense record lists, keep row typography strong, metadata quiet, and row actions compact. Do not add decorative cards around every row unless each row is a distinct object card.
