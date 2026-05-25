# Interaction States And Async Safety

Use this reference whenever a change touches user actions, forms, data loading, or component states.

## Universal State Matrix

For every meaningful interactive area, consider whether these states exist and behave coherently:

- Default, hover, focus-visible, active, disabled.
- Loading or pending.
- Success confirmation.
- Validation error and API error.
- Empty and no-result states.
- Permission denied, unauthenticated, hidden-by-role, or readonly.
- Long content, missing content, and malformed content.
- Desktop, tablet, and narrow mobile if the product supports them.

Do not implement all states as visible UI if the product does not need them, but do deliberately decide how each path behaves.

## Async Write Actions

Every async write action must have both a UI lock and a handler lock.

Minimum pattern:

```js
async submit() {
  if (this.submitting) return
  this.submitting = true
  try {
    await save(this.form)
    this.$message.success('保存成功')
    this.closeDialog()
    this.refreshList()
  } catch (error) {
    this.$message.error(getErrorMessage(error, '保存失败，请稍后重试'))
  } finally {
    this.submitting = false
  }
}
```

For callback-style form validation, release the lock when validation fails:

```js
submitForm() {
  if (this.submitting) return
  this.submitting = true

  this.$refs.form.validate(valid => {
    if (!valid) {
      this.submitting = false
      return
    }

    save(this.form)
      .then(() => {
        this.$message.success('保存成功')
        this.open = false
        this.getList()
      })
      .catch(() => {
        this.$message.error('保存失败，请稍后重试')
      })
      .finally(() => {
        this.submitting = false
      })
  })
}
```

Rules:

- Set the lock before any async boundary that can race.
- Bind the lock to the submit button's loading/disabled state.
- Do not close the dialog before the API returns unless the product has a deliberate background-submit model.
- Preserve form input on error so the user can fix and retry.
- Avoid a single global lock if independent row actions can run safely; use row-level keys such as `pendingRowId` or a `Set`.
- Release locks in `finally`, and release immediately on validation cancel or confirm cancel.

## Duplicate Clicks And Re-entry

Protect:

- Submit buttons in dialogs, drawers, pages, and inline forms.
- Batch actions, import/export, upload, delete, invalidate, publish/unpublish, enable/disable, and status toggles.
- Toolbar buttons that trigger API writes or expensive reads.
- Keyboard submit paths such as Enter key handlers.
- Icon buttons and dropdown menu items that call the same handler as a visible button.

Do not rely on network latency being low. Slow API calls are part of normal behavior.

## Press-And-Hold Capture Gestures

Use this for voice, camera, scan, or other press-and-hold capture controls:

- Provide an explicit cancel gesture when capture can be started by mistake. Release-to-complete and slide-to-cancel should be different states, not the same release path with later cleanup.
- Separate service startup from capture. If runtime dependencies or permission checks are still warming up, label the state as connecting or preparing and tell the user when to start; do not show copy that implies capture is already active.
- The cancel target must be visible for both left- and right-thumb operation. Do not place the only hint in the bottom-left or bottom-right zone where the pressing thumb can cover it.
- Use a weak hint before the user crosses the cancel threshold, then a clear active state after the threshold. Active cancel state should change copy, icon, and color or emphasis.
- Releasing in the active cancel state must call the real cancellation path and must not complete or persist partial content.
- Clear timers, countdowns, previews, pending capture state, and temporary UI flags on cancel. Auto-clear weak cancellation feedback so it does not become a nuisance prompt.
- Prewarm cold-start dependencies early when the runtime allows it, but do not trigger surprise permission prompts on page load. Silently check existing authorization and fetch/cache non-sensitive startup data before the first press.
- Gesture thresholds should be forgiving and platform-sized, not raw pixel guesses. Convert design units to runtime pixels where needed and cache device metrics when the handler runs often.
- Validate with the target device/runtime when microphone, camera, scan, or other device APIs are involved. Syntax checks are not enough for gesture feel or permission behavior.

## Async Reads And Refreshes

Separate first load from refresh:

- First load can use skeletons or an obvious section loading state.
- Refresh should preserve existing content when it remains usable.
- Avoid root `v-loading` or page-level dimming for a small table/chart/detail refresh.
- Prefer local flags such as `listLoading`, `detailLoading`, or `chartLoading`.

Guard against stale responses:

```js
async getList() {
  const requestId = ++this.listRequestId
  this.listLoading = true
  try {
    const res = await listApi(this.queryParams)
    if (requestId !== this.listRequestId) return
    this.rows = res.rows || []
    this.total = res.total || 0
  } finally {
    if (requestId === this.listRequestId) {
      this.listLoading = false
    }
  }
}
```

Use stale-response guards for quick filters, tabs, pagination, route params, search-as-you-type, and dependent dropdowns.

## Streaming And Event-Driven Feedback

For chat, long-running generation, live logs, or event-driven progress surfaces:

- Classify incoming events by user-facing purpose before shaping the UI: primary content, progress, completion, diagnostics, errors, and structured results should not all become the same visible message type.
- Control or completion signals should update existing activity state instead of creating extra content entries.
- Transport and diagnostic signals should stay out of the primary transcript unless the product explicitly exposes a technical trace view.
- Keep streamed generated text as one continuous answer per logical turn, appending chunks into the same message so Markdown, collapsible detail blocks, and tables render coherently.
- Structured results should use the current page theme and density rules; do not let third-party table/card defaults break dark mode or visual consistency.

## Tables And Row Actions

For data tables:

- Row delete/invalidate/status actions should lock the target row or action until the API returns.
- Batch actions should lock the batch command and preserve selected rows intentionally.
- After success, either patch the row locally or refresh the list. Avoid a stale table that still shows the deleted item as actionable.
- Confirm destructive actions with consequence-specific copy, not vague "Are you sure?"
- Disable actions the user cannot perform instead of letting them fail late when permissions are known.

For pagination and filters:

- Reset page number when filters materially change.
- Show no-result state when a search returns zero rows.
- Avoid layout jumps when total count or page size controls update.

## Forms

Form expectations:

- Required state is visible before submission.
- Validation messages are close to the field and written in user language.
- Submit buttons show loading and are protected against re-entry.
- Cancel/close while submitting is either disabled or clearly safe.
- Enter key behavior matches user expectation and uses the same guarded submit path.
- Errors do not erase user input.
- Date/time pickers specify format, timezone implications, and clearable behavior when relevant.

## Uploads And Imports

Handle:

- No file selected.
- File type and size validation.
- Upload in progress, success, failure, retry.
- Duplicate submit while upload is in progress.
- Partial failure for batch imports when the backend can return row-level errors.
- Download-template actions and import-submit actions independently.

## Optimistic UI

Use optimistic updates only when rollback is simple and trustworthy:

- Record previous state before changing the UI.
- Disable the same control while the request is pending.
- Roll back on API failure or confirmation cancel.
- Communicate failure with a specific message.

When the consequence is destructive or business-critical, prefer pessimistic updates unless product requirements say otherwise.

## Accessibility Basics

- Keyboard users can reach and activate controls.
- Focus-visible styles are preserved.
- Modal focus is trapped and returns to the opener when practical.
- Icon-only controls have accessible labels or visible tooltips.
- Disabled controls do not hide essential information.
- Color is not the only signal for error, success, or required state.
