# Changelog

## v1.1.6 — June 3, 2026

### New Features

- **Kanban: group tasks by custom property** — use the new "Group by:" menu in the Kanban toolbar to organize your board by status, group, or any custom field you've added to your tasks. Columns are created automatically based on the values in your tasks.
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

