# Changelog

## v1.2.1 - 2026-06-08

### Added
- **Heatmap toggle** — a new "Show activity heatmap on recurring tasks" setting in the General tab enables or disables the GitHub-style completion heatmap block injected into recurring task files. Disabling the setting removes the heatmap from any existing recurring task file the next time that file is opened.
- **Subtasks** — any task can have subtasks. Convert a checklist item inside a task file into a full task using the existing convert button; the new task automatically gets a `parent_id` frontmatter field linking it back to its parent. Subtasks support arbitrary nesting depth (subtasks of subtasks).
- **Table view hierarchy** — subtasks render indented under their parent with recursive nesting. Each level is indented 20 px deeper. Any row that has children shows a chevron arrow to collapse or expand its subtasks. The arrow indents together with the row so it always reads as belonging to its parent. A `↳ N` count badge always appears after the parent's title showing how many subtasks it has.
- **Table view search finds subtasks** — when any filter or search is active the hierarchy flattens and subtasks appear as standalone rows, making them fully discoverable. The indented hierarchy is restored when all filters are cleared.
- **Table view Tags column** — a Tags column is now shown by default between Group and Start Date. Each tag renders as a pill; clicking a pill (or the empty placeholder) opens a menu to add or remove tags. The column is togglable via the Columns menu and included in all export formats.
- **Table view tag filter** — a Tags multi-select filter appears in the toolbar whenever tasks with tags are present. Selecting one or more tags narrows the list to tasks that have all of the chosen tags. The filter dropdown refreshes automatically when tags are added or removed from any task file.
- **Table view bulk tag action** — the bulk action bar now includes a Tags button. It shows tags present on selected tasks (for removal) and all available tags (for adding), plus a "New tag…" option to create a custom tag and apply it to all selected tasks at once.
- **Table view bulk Columns action** — the bulk action bar includes a Columns button that opens a custom two-level popover: select a custom column, then select a value to stamp on all selected tasks. Values are collected from existing tasks and sorted alphabetically. A "New value…" option accepts freeform input and "Clear" blanks the field. The popover always opens above the bulk bar.
- **Settings info banner** — the General settings tab now shows a plugin info card above the settings with the plugin name, current version, a "View recent updates" button linking to the changelog, and a "Buy me a coffee" button.
- **Table view Saved Filters** — a row of named filter pills appears between the quick-add row and the toolbar. Clicking the bookmark icon in the toolbar saves the current search, filters, sort, and grouping under a custom name (up to 12 saved filters). Clicking a pill restores all those settings instantly; the active pill is highlighted. Hovering a pill reveals an ✕ to delete it.
- **Table view list column popout** — clicking a list-type cell now opens a floating popout instead of editing inline. The popout shows existing values as chips (each with a color swatch and ✕ to remove), a type-to-add input, and a suggestion list of values used on other tasks. Clicking a suggestion adds it immediately.
- **Per-value colors on list columns** — each value in a list-type column can be assigned a custom color via a color swatch in the popout. Chip text automatically switches to black or white for contrast. The color is stored per-value in the column definition and applied consistently across all tasks.
- **All-Day Event Reminders** — a new "All-Day Event Reminders" section in Google Calendar settings lets you configure reminders that apply exclusively to all-day tasks. Each reminder specifies a method (notification or email), a number of days or weeks before the event, and a time of day. Standard timed-event reminders continue to apply only to tasks with a specific time.
- **Kanban drag-and-drop on Android** — cards can now be press-held and dragged between columns on Android. The implementation switches from HTML drag events to the Pointer Events API with a threshold-based capture so taps still register as clicks. A ghost card follows the pointer during the drag. `touch-action: none` prevents the browser scroll from interfering.
- **Kanban card action buttons** — each card now has a pencil (edit) button in the top-right corner and a ⋯ (more) button in the bottom-right. On desktop both are hidden until hover; on mobile they are always visible. The ⋯ menu contains "Open file" and "Remove from board". Right-clicking a card no longer shows a browser context menu on Android.
- **Exclude from sync per task** — a new "Exclude from sync" checkbox sits next to "Enable Date Range" in both the Create and Edit Task modals. When checked, the task is never synced to Google Calendar. If the task already has a linked Google Calendar event, that event is deleted immediately when the setting is saved. The flag is stored as `exclude_from_sync: true` in the task's frontmatter and omitted entirely when false.

### Changed
- **Table view hierarchy always visible** — subtasks now render indented under their parent regardless of whether any filter or search is active. Previously, activating any filter collapsed the hierarchy into a flat list. A new **Hierarchy toggle** button in the toolbar (git-branch icon) switches between hierarchy and flat mode; the button is highlighted when hierarchy is on and shows a "Hierarchy: on / off" tooltip. The setting persists across sessions and is saved as part of any Saved Filter.
- **Table view hierarchy respects filters** — when hierarchy is on and a filter is active, only tasks that match the filter are shown. If a child matches but its parent does not, the parent appears as a context row so the child can be displayed nested. Intermediate ancestors at any depth are included automatically, so three-level (and deeper) hierarchies surface correctly. Non-matching siblings are hidden.
- **Table view all columns sortable** — every column header is now clickable for sorting, including Tags and all custom columns. Tags sorts by the first tag alphabetically. Custom columns sort by their type: text (alphabetical), number (numeric, so 10 sorts after 9), date (chronological), checkbox (checked first in ascending), and list (by first item alphabetically). Missing values always sink to the bottom regardless of sort direction. Children within a hierarchy group are also sorted by the active column. Previously only the built-in Title, Type, Group, Start, Due, Priority, and Status columns were sortable.

### Fixed
- **Add Column / Rename Column modal field overflow** — the Property Name and Property Type fields were wider than the modal, causing a horizontal scrollbar. Root cause was `width: 90vw` (viewport-relative) on the inner content element overflowing the modal's `max-width: 420px` cap. Fields now correctly fill the modal width.
- **List view deleted task type sections** — sections for task types that no longer exist in Settings no longer persist as ghost headings in the List view. Tasks whose type has been removed now fall under a generic "Tasks" section.
- **Settings task type default name** — clicking "Add Task Type" no longer pre-fills "type 2" as a default name; the input starts blank and receives focus automatically.
- **Table view list-type column display** — custom columns of type `list` now correctly render values stored in any format: JavaScript array, JSON flow-sequence notation (`["one","two"]`), or comma-separated string. Previously, flow-sequence strings were displayed with raw brackets and quotes. Values are always saved back to frontmatter as a proper YAML list so the format is normalised on first edit.
- **Table view list-type column inline editing** — clicking into a list column cell opens an inline input pre-filled with the existing items. Pressing Enter appends the new item to the list without replacing existing items. Each item renders as an independent chip with an ✕ to remove it individually. Previously, adding one item via bulk action would replace all existing items with the new value alone.
- **Table view checkbox column alignment** — checkboxes in custom checkbox-type columns are now horizontally centered in their cells.
- **Quick-add retains focus after adding a task** — pressing Enter in any quick-add field (Table, Kanban, Calendar list view) now keeps focus in the input so tasks can be added in sequence without re-clicking. The `shouldRefocusQuickAdd` flag in Calendar list view was declared but never set; all three views now consistently restore focus after the post-add refresh.
- **Default tags not applied in Kanban quick-add** — tasks created via the "+ Add Quick Task" input in the Kanban view now receive the default tags configured in Settings. The Calendar and Table view quick-add fields were already applying default tags correctly.
- **Table view custom column display after bulk update** — after applying a bulk Columns action, all selected rows now reliably show the updated values. The root cause was a race condition in `getAllTasks()` where a concurrent `metadataCache.changed` event could call `invalidateCache()` mid-rebuild and partially wipe the extra-props cache before the render ran. The cache is now built into a local map and committed atomically at the end of the loop, so mid-loop invalidations are fully overwritten.
- **Day view drag-and-drop parity with Week / 3-Day** — all-day tasks can now be dragged to the timeline and back to the all-day area in Day view, matching the behaviour in Week and 3-Day views. The all-day row highlights on hover, the timeline scroll position is preserved after a drop, and the drop indicator tracks the cursor correctly.
- **Timeline auto-scroll when dragging from all-day** — dragging an all-day task near the top or bottom edge of the visible timeline now scrolls the timeline in all views (Week, 3-Day, and Day). Previously the timeline would not scroll at all during an all-day → timeline drag, requiring the user to drop first and then re-drag.
- **Week numbers now follow ISO 8601** — previously the week number shown in the calendar (month grid "Wk" column and navigation badge) used a simplified US formula when "First day of week" was set to Sunday, producing numbers that didn't match Google Calendar, Apple Calendar, or Outlook. Week numbers now always use the ISO 8601 standard (the week containing the first Thursday of the year) regardless of the calendar grid's start-day setting.
- **Recurring tasks silently truncated by auto-archive** — marking a recurring task's parent "Complete" or "Skip" from the Calendar status badge, Table view (row pill or bulk action), Kanban card menu, or the Edit Task modal could write that status straight onto the parent file instead of advancing the series. With "Auto-Archive" enabled, the parent was then immediately stamped `archived: true` and moved to the `Archived/` subfolder before its next occurrence was ever created — ending the series with no warning. All of these paths now route through the same recurring-aware completion flow used elsewhere (the one that stamps a dated occurrence file and advances the parent's due date), so the series always continues correctly regardless of where you change the status from.
- **"Skip" on a recurring task now advances the series** — previously, picking "Skip" on a recurring parent had no recurring-aware handling anywhere (including Kanban) and behaved like the bug above. Skipping an occurrence now works the same way completing one does: a dated occurrence file is stamped with the skipped status and the parent advances to its next due date, keeping the series intact and correctly recording the skip in its activity heatmap.

## v1.1.6 — June 3, 2026

### New Features

- **Kanban: group tasks by custom properties** — use the new "Group by:" menu in the Kanban toolbar to organize your board by status, group, or any custom property you've added to your tasks front matter. Columns are created automatically based on the values in the property of tasks on the board. Only supports property type "text".
- **Kanban quick-add now works correctly** — pressing Enter in the "Add Quick Task" bar creates the task and places it in the right column.
- **Kanban cards can show a status badge** — when grouping by something other than status, you can turn on a "Status" option in display settings to show each task's status on its card.
- **Kanban remembers which board you were on** — reopening the Kanban view or restarting Obsidian now returns you to the last board you had open.
- **Right-click to remove tasks from board** — remove tasks from board easily by right clicking and choosing remove from board.
- **Auto-archive completed & skipped tasks** — you can enabled auto-archive in the settings to archive completed tasks. Manually archive old completed tasks.
- **Tasknotes & Notes conversion** — copying tasks created by the Tasknotes plugin into the Task Manager folder will convert them. Notes moved into the configured Task folder will also be converted properly into a task.
- **Kanban: column layouts are saved per grouping** — when you switch how your board is grouped, your previous column arrangement (names, colors, order) is saved and restored when you switch back.
- **Backlog panel: search, filter, and sort** — the Backlog side panel now has the same tools as the Unscheduled panel: a search bar, filters for status, priority, and task type, group filtering, group-by sections, and a sort control.
- **Skipped tasks shown in red on the activity heatmap** — recurring tasks marked as skipped now appear in red on the heatmap so they stand out from completed ones.

### Improvements

- **Kanban: tasks stay on the right board** — tasks are now always assigned to a board by their board ID, so grouping by a custom field no longer accidentally pulls in tasks from other boards.
- **Kanban dropdowns always open downward** — the board selector, sort, and "Group by:" menus now reliably open below the toolbar no matter where the panel is docked in your workspace.
- **Task status options updated** — when editing a task, the status dropdown now shows the full set of options: Open, In Progress, Complete, and Skip.
- **"Group by:" moved to the left of "Sort:" in the Kanban toolbar** — the toolbar order now reads left to right in the order you'd typically use the controls.
- **"View Activity Log" removed from the status bar menu** — cleaned up a menu item that wasn't useful.

### Bug Fixes

- **Archived tasks no longer create duplicate files** — completing a task and archiving it no longer left behind a second copy of the file in the original location.
- **Converting a note to a task now uses the note's filename as the title** — previously, notes without an explicit title field were converted with the name "Untitled."
- **Notes moved into the task folder are fully set up automatically** — when you move a vault note into your task folder, it is now immediately given all the required task fields; previously some fields were only filled in after manually opening and saving the task.
- **Kanban display settings panel no longer closes when you use it** — toggling display options or changing the board background no longer dismisses the settings popup.
- **Kanban quick-add now works correctly** — pressing Enter in the "Add Quick Task" bar creates the task and places it in the right column.
- **Kanban cards can show a status badge** — when grouping by something other than status, you can turn on a "Status" option in display settings to show each task's status on its card.
- **Kanban remembers which board you were on** — reopening the Kanban view or restarting Obsidian now returns you to the last board you had open.
- **Checkboxes no longer appear on unrelated notes** — a task checkbox was incorrectly showing up on any list item in your vault that contained a link; it now only appears on links to actual task files.
- **Calendar week numbers now match Google Calendar and Apple Calendar** — week numbers shown in the month grid and the navigation header now always follow the international standard (ISO 8601), regardless of whether your calendar week starts on Sunday or Monday. Previously, Sunday-start mode used a different counting method that didn't match other calendar apps.
- 
## v1.1.0 - 2026-05-31

### Added
- **List view status badge** — each task row now shows a clickable status pill (Open / In Progress / Complete / Skip) anchored to the far right
- **List view time** — list view time display now shows the start time, fallback is due time if no start time is set, otherwise the end time is ignored
- **Global status options** — fixed status list (Open, In Progress, Complete, Skip) used by all dropdowns across List and Table views; not user-configurable
- **Skip status** — new `skip` status (numeric 3) with grey pill style; skipped tasks are excluded from Overdue, active counts, ghost generation, and the unscheduled panel
- **MCP: Kanban board management** — `list_kanban_boards`, `create_kanban_board`, `delete_kanban_board` tools; boards created via MCP appear instantly in open Kanban views without restart
- **MCP: `create_kanban_board` always includes Done** — custom column lists automatically get a "Done" column appended if none is present
- **Kanban board selector dropdown** — board navigation replaced with a `<select>` dropdown listing all boards; pencil button beside it triggers inline rename
- **Kanban Done column rename** — the Done column can now be renamed inline; its `matchValue` stays `"done"` regardless of the display name so task completion routing is unaffected
- **Table view shift-click multi-select** — click one row, shift-click another to select all rows in between
- **Tooltips on icon-only buttons** — hover tooltips on all unlabeled buttons across Table, Calendar, and Kanban views (using Obsidian's `setTooltip` API)
- **Custom Status collapsible section** — the three frontmatter status value settings (Open, In Progress, Completed) are now grouped under a collapsible "Custom Status" section in General settings, styled to match the Google Calendar section
- **First day of week setting** — new dropdown in General settings to choose Sunday or Monday as the week start; affects month grid headers/layout and week/3-day view column order
- **Month view item cap** — day cells cap at 8 items; overflow shows a `+N more` link that navigates to the day view for that date

### Changed
- **Overdue filter** — tasks with status ≥ 2 (done or skipped) no longer appear as overdue in any view
- **Status writes respect settings** — Table view status dropdowns now route through `taskManager.updateTask` so the user-configured frontmatter labels are always used when writing to files
- **Settings title** renamed from "Tasks + Google Calendar" to "Obsidian Task Manager"
- **Modal auto-focus** — title field no longer auto-focuses on mobile (prevented viewport scroll-to-bottom when keyboard opened); desktop behavior unchanged
- **Split-pane button tooltip** — all three views now consistently say "Open tasks split right"
- **Mobile Table view scroll** — the whole page scrolls naturally (title, filters, and rows); `flex: none` on the table wrapper prevents it from being height-constrained by the flex container
- **Mobile Table view horizontal scroll** — table rows scroll horizontally within the wrapper while vertical page scroll is handled by the outer container
- **KanbanBoardManager observer pattern** — views register a change listener directly on `KanbanBoardManager` so any board mutation (UI or MCP) triggers an immediate re-render

### Fixed
- **24-hour time format** — week/day view timeline hour labels, agenda event times, event popover times, and the calendar event time chip now all respect the 12h/24h setting; previously these ignored the setting and fell back to the OS locale
- Overdue date label now appears before the time range in list rows
- Overdue time range uses the same red color as the overdue date label
- `status < 2` check applied consistently across CalendarView, TableView, KanbanView, and RRuleParser so Skip tasks behave the same as Complete tasks
- Split-pane button tooltip no longer changes to "Open tasks in active pane" based on pane state
- **Mobile list view scroll jump** — marking a task complete no longer causes the list to jump to the top; scroll position is preserved by tracking the actual scroll container via scroll events and restoring position synchronously before the browser paints
- **Mobile list view flash** — re-renders on task completion no longer flash; card content is now swapped atomically via `replaceChildren` instead of empty-then-rebuild

