---
schema_version: "1.0.0"
document_id: "b48f0af14006a10fec61c1353cc95b847d422f4874cd9813c76c1b5a26ec5195"
company_key: "yc-zelos-cloud"
company: "Zelos Cloud"
source_id: "yc-zelos-cloud-news-import-c65d5f69242c"
canonical_url: "https://docs.zeloscloud.io/26.0.4/reference/release-notes/app/"
published_at: null
first_seen_at: "2026-07-27T06:12:43.410034+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:466a9cbaeef0b3be54ca702a5d207505a3d5011b3de7cb92954a2f677848bf23"
---

# Next: App

# App


26.0.4


## Summary¶


This release introduces **App Extensions** , a new extension surface for building rich, custom workflows directly inside the Zelos desktop app. Extensions can now provide full APP tabs for dashboards and purpose-built tools, alongside major improvements to workspace history, panel display controls, layout editing, search, and large-workspace performance.


### Added¶


- **App Extensions:** Installable extensions can now add full workflow UIs to Zelos, not just signals and actions. App extensions open in dedicated APP tabs and can be launched from the app rail, making room for workflows like custom dashboards and extension-specific tools.[Learn more →](https://docs.zeloscloud.io/app/extensions/#app-extensions)
- Developers can create their own app extensions with the new app-extension templates and SDK.[Build an app extension →](https://docs.zeloscloud.io/sdk/how-to/develop-app-extensions/)
- **Workspace History:** Undo and redo workspace changes with` Cmd+Z` /` Ctrl+Z` and` Cmd+Shift+Z` /` Ctrl+Shift+Z` . The command palette shows the next undo/redo action when history is available.[Learn more →](https://docs.zeloscloud.io/app/workspace/#workspace-history)
- **Display Format Menu:** Right-click panel chips, table cells, or value cells to copy values, copy signal paths, or show values as default, hex, binary, octal, or scientific notation.[Learn more →](https://docs.zeloscloud.io/app/panels/#display-format-and-copy-actions)
- **Multi-Signal Value Panels:** Value panels can now show multiple signals in a compact grid, with configurable column count.[Learn more →](https://docs.zeloscloud.io/app/panels/value-panel/#grid-behavior)
- **Plot Legend Placement:** Plot legends can be placed at the bottom, left, or right of the chart, with a resize handle for tuning the space between legend and plot.[Learn more →](https://docs.zeloscloud.io/app/panels/plot-panel/#legend-layout)
- **Signal Units:** Signal units now appear alongside values across panels, tooltips, chips, and measurement results.
- **Action Panel Controls:** Action panels now support Enter-to-execute, inline execute buttons, configurable request timeouts, and clearer execution states.[Learn more →](https://docs.zeloscloud.io/app/panels/action-panel/#execution-results)
- **Analytics Opt-Out:** Settings now include a toggle for sharing anonymous usage analytics.[Learn more →](https://docs.zeloscloud.io/app/settings/)


### Changed¶


- **Workspace Naming:** Default workspaces are now named "Workspace" instead of "Untitled", and empty workspaces no longer clutter Recents.
- **Live Cursor Behavior:** When pausing, scrubbing, or zooming live data, the cursor stays anchored to the right edge of the visible window so paused live inspection matches live-follow behavior.
- **Layout Drag and Drop:** Panel creation and rearranging now have clearer merge-vs-insert previews, ghost placeholders, edge auto-scroll, and cross-tab panel moves.[Learn more →](https://docs.zeloscloud.io/app/layouts/#editing-a-loaded-layout)
- **Plot Line Style:** New plot panels default to linear lines instead of stepped lines. Existing panel overrides are preserved.
- **Table Signal Names:** Table panels now default to short signal names, with the full-name toggle still available per panel.
- **Sidebar Search:** Signal tree sources expand by default, search jumps to the first match, clearing search restores prior expansion state, and manually closed branches stay closed.
- **Natural Sorting:** Numbered names now sort naturally across lists, so` signal_2` appears before` signal_10` .


### Fixed¶


- Layouts no longer accidentally lock dragged signals to a specific trace or agent, so reusable layouts work more reliably across similar traces and live connections.
- Plot colors now stay stable when signals update, table panels re-render, or segment-scoped signals are displayed.
- Large numeric values render correctly on plot Y-axis tick labels.
- Recorded live data is now flushed more reliably on shutdown.
- Missing action status is no longer treated as a passing result.
- Linux auto-update is restored with clearer installation errors and timeouts.
- Login and loading screens have improved light-mode icon contrast, a clearer "Login with code" button, and fewer title-bar glitches.
- Large workspaces with tens of thousands of signals are faster and more stable, especially in the signal tree and multi-signal drag-and-drop flows.
- Table column width, order, and pinning now persist across sessions.


26.0.2


## Summary¶


This release adds a **Command Palette** for instant access to any action in the app, along with trace re-export, multi-file open, and automatic segment expansion. It also includes a redesigned plot tooltip, improved measurement tool, and various panel sizing and UI refinements.


### Added¶


- **Command Palette:** Open with` Cmd+K` /` Ctrl+K` to quickly search and run any action across the app — playback controls, workspace management, layouts, settings, extensions, and more. Includes keyboard shortcut hints and fuzzy search.[Learn more →](https://docs.zeloscloud.io/app/command-palette/)
- **Trace Re-Export:** Export opened trace files to a new` .trz` file. Supports time-range selection and merging multiple open traces into a single output.
- **Segment Auto-Expansion:** Overlapping data segments for the same signal now automatically expand into distinct series in plots, tables, and value panels — similar to scoped signals. Non-overlapping segments remain merged.
- **Multiple File Selection:** The open file dialog now supports selecting and opening multiple` .trz` files at once.


### Changed¶


- **Plot Tooltip Redesign:** Compact, data-dense layout with monospace numeric alignment and better readability.
- **Measurement Tool:** Updated design to match the new tooltip style. Cursors can now snap to any data point and move freely with automatic swap behavior.
- **Plot Panel at Small Sizes:** Adaptive Y-axis tick labels that scale to available panel height. X-axis ticks adjust based on plot width.
- **Signal Chips Area:** Cleaner expand/collapse with fixed row heights and a compact chevron toggle.
- **Panel Minimum Size:** Increased minimum panel width and height to prevent unusable sizing.
- **Extensions UI:** Clearer status messaging and disabled buttons when not connected to an agent.


### Fixed¶


- Cursor not following navigation (pan, zoom, scroll) while paused
- Incorrect file name display on Windows due to backslash path separators


26.0.1


## Summary¶


This release introduces a brand-new timeline and playback experience for live and trace workflows. You can now navigate with a shared cursor across any panel, compare traces more easily with` ABS/REL` modes, and inspect data faster in any workflow.


### Added¶


- **All-new Timeline UI:** A redesigned timeline with transport controls, zoom controls, fit-all, view presets, and more - all in one place.[Learn more →](https://docs.zeloscloud.io/app/timeline/)


- Supports collapsed and expanded views
- Expanded mode includes source segment lanes with rich hover details


- **Shared Cursor Across Panels:** Timeline, plot, log, table, and value panels now work from one shared cursor for synchronized inspection.
- **Plot Cursor Controls:**


- Right-click in plot to jump the global timeline cursor
- Drag the plot cursor handle in paused mode to scrub precisely


- **Log Cursor Controls:**


- Right-click any log row to jump timeline cursor to that log timestamp
- Use **Jump to cursor** to re-center log view while following cursor context


- **Live and Playback Modes:**


- Clear **Live** state indicator
- One-click return to live follow (plus` Esc` shortcut)
- Timeline playback/step controls for trace and paused live analysis


- **Multi-Trace Time Alignment (` ABS/REL` ):**


- ` REL` mode aligns traces at` t=0` for side-by-side run comparison
- ` ABS` mode preserves wall-clock timestamps for real-world event correlation


- **Expanded Timeline Shortcuts:** Keyboard support for play/pause, stepping, zoom, fit-all, and jump-to-live.
- **Tab Bar Menu:** New tab bar menu that mimics your browser. Keyboard shortcuts included.


### Changed¶


- **Tooltips:** Displays left closest shared x timestamp and closest value to the hover position
- **Contrast:** Improved visibility between dark/light modes
- **Export data:** Export larger data sets from panels into CSV and JSON with proper feedback


### Fixed¶


- Workspace migration issues in recents list
- Action config reset when changing the chip
- Lots of a little bugs and UI/UX consistency improvements throughout!


26.0.0


## Summary¶


This release introduces **multi-trace and multi-agent support** . You can now open multiple trace files simultaneously, connect to multiple live agents, and compare data across them in the same panels.


### Added¶


- **Multi-Trace Support:** Open and compare multiple trace files in a single workspace.[Learn more →](https://docs.zeloscloud.io/app/workspace/#multi-trace-mode)


- View signals from different traces side-by-side
- Signal paths show trace and agent context (e.g.,` flight_test_001:agent::motor/status.rpm` )


- **Multi-Agent Support:** Connect to multiple live agents and view their data together.


- Monitor signals from different agents in the same panels
- Signal tree organized by Agent → Source → Message → Signal


- **Right Sidebar:**


- Pin frequently used layouts, connections, and traces for one-click access
- Sections for pinned and recently used items
- Status indicators show items states


- **Color Palette:** New expanded color palette.


- Colors are visually distinct even for colorblind users
- Click any signal chip to customize its color


- **Panel Descriptions:** Add notes to panels.


- Descriptions are saved with layouts


- **Panel Data Builder:** New workflow for configuring panel signals/actions.[Learn more →](https://docs.zeloscloud.io/app/panels/#query-builder)


- Autocomplete signal paths as you type
- Scope signals to specific traces or agents
- Copy and paste signal configurations between panels


- **Cross-Tab Panel Movement:** Drag panels between tabs to reorganize your workspace.[Learn more →](https://docs.zeloscloud.io/app/panels/#cross-tab-panel-movement)


### Changed¶


- **Table Panel:** Faster performance with large datasets, filter/search state persists across sessions
- **Plot Panel:** Scrollable signal list (shows 2 rows by default, expandable)
- **Panel Chips:** Better drag and drop support for all signals/action with visual dropzone indications


### Fixed¶


- Inconsistent app zoom handling across all platforms
- App lockup when configuring an extension
- Improved cache strategy
- Lots of a little bugs and UI/UX consistency improvements throughout!


25.0.22


## Summary¶


This release introduces a major redesign of the application with a new Extension Marketplace, improved navigation with redesigned sidebars, and enhanced plot panel capabilities. Key highlights include the ability to browse and install extensions directly from the app, multi-select signals for easier plot creation, and improved user feedback throughout. The layout has been completely reimagined with dedicated sections for connections and layouts, making it easier to manage your workspace.


### Added¶


- **Extension Marketplace:** Browse, install, and manage extensions with one click.[Read more](https://docs.zeloscloud.io/sdk/how-to/develop-extensions/) .


- Install community-built protocols, integrations, and tools directly from the app
- Start, stop, restart, and uninstall extensions from the extensions section
- Detailed extension pages showing readme, changelog, and version history
- Quickly and easily configure extensions directly in the app
- Works with both local and remote agents


- **Redesigned Application Layout:**


- New left sidebar for signals, actions, and installed extensions
- New right sidebar for managing connections and layouts
- Sidebars can be collapsed for unobtrusive visualization dashboards
- App rail for quick navigation between home, workspaces, marketplace, and settings
- Custom title bar that works across Windows, macOS, and Linux
- Three distinct tab types with icons: Layout tabs, Extension tabs, and Settings tabs


- **Connections & Layouts Sections:**


- Dedicated connections section in right sidebar for managing agent connections
- Dedicated layouts section for creating, saving, and managing layouts
- Share personal layouts with your team
- Rename layouts directly from the layouts section
- Improved layout save/save-as workflow with dialogs


- **Plot Panel Improvements:**


- Choose line rendering style: stepped (default), linear, or smooth spline
- Fill area under plot lines with configurable transparency
- Add custom Y-axis labels to your plots
- Navigate zoom history with backward button or double-click
- Configure plot options globally in settings or per-panel
- All plot settings save with your layouts


- **Signal & Action Tree Enhancements:**


- Multi-select signals and drag them all at once to your plots
- Multi-select works seamlessly with search
- Trees automatically expand for easier navigation
- Hover over signals to see their data type
- Expand/collapse state persists when switching tabs


- **Improved User Feedback:**


- Better error messages throughout the app
- More informative feedback when operations succeed or fail
- Loading states and progress indicators throughout


- **Settings Improvements:**


- Reorganized settings pages with better grouping
- Memory limit controls now show units (MB, GB) with dropdown selector
- Update notifications show available version information


### Changed¶


- Table expand/collapse state now persists when changing tabs
- More consistent styling and feedback throughout all user interactions
- Improved memory management for better stability
- Finer control of panel sizing and placement


### Fixed¶


- Fixed scroll behavior in sidebars and resizable panels
- Fixed font sizing in log and value panels
- Fixed play/pause button position when zooming in plots
- Fixed plot highlighting in light mode (now more visible)
- Fixed various Windows-specific issues with file paths and dialogs
- Many additional bug fixes, stability improvements, and UI polish throughout the app


25.0.21


### Added¶


- Memory limit controls for limiting the memory footprint of the Zelos App to a particular value


- Navigate to` Settings -> General -> Memory Limit` to enforce a particular limit
- Default limit = 25% of total system memory
- Clear memory limit setting to set value to None/Unlimited


- Plot panel configurable Y axis settings


### Changed¶


- Memory management to facilitate limit-based pruning by removing oldest data


### Fixed¶


- Hourly heap memory trimming to release heap space back to OS


25.0.20


### Added¶


- Added "Release Notes" to the app so you can stay up to date on all the latest changes!


### Changed¶


- Renamed zelos-server process to zelos-app-agent


### Fixed¶


- Improved agent shutdown behavior when closing the app across all platforms
- Improved client and agent reconnection handling
- Fixed flickering tooltip


25.0.19


### Added¶


- **Connect Shortcut:** Double-click a device in the connection list to quickly establish a connection.
- **Remote Actions:** Execute Actions on remote agents.


### Changed¶


- **Actions UI:** The Actions sidebar is now automatically hidden when viewing a trace file.
- **Performance:** Improved performance for actions by reusing a single client instance.


### Fixed¶


- Resolved an issue where Action panels had a generic title if no action was selected.
- Fixed a bug that prevented using "double-click to go live" when viewing a trace.
- Fixed an issue with the workspace timeline when switching between trace and live modes.


25.0.18


### Changed¶


- **Actions UI:** Actions are now organized in a nested tree structure, similar to signals, for easier navigation.
- **Value Panels:** Value panels will now automatically use the signal name as the title by default.


### Fixed¶


- Removed redundant labels from toggle widgets in the Actions panel.


25.0.17


### Added¶


- **Actions:** Introducing Actions, a powerful new way to script and automate interactions with your devices and services directly from the Zelos app.


- A dedicated sidebar pane for browsing and managing your Actions.
- Support for a variety of input widgets to create rich, interactive Action panels.


25.0.16


### Fixed¶


- **X Axis Labels:** Resolved issue where X axis labels were displaying incorrect values.


25.0.15


### Added¶


- **Plot Measurement Tool:** Measure Δx and Δt between two points in any plot with a new overlay. See more here.


25.0.14


### Added¶


- **Log Panels:** Visualize live log streams in dedicated panel. See more here.
- **Log TXT Export:** Download raw log data directly from any Log panel.
- **Font Size Picker:** Quickly adjust log font size for better readability.
- **Persistent Filters & Search:** Filters and search terms are now saved with your layout, so they persist across sessions.
- **Signal Chips:** Interactive chips beneath each plot for per-signal series control. See more here.
- One-click show/hide toggle
- Inline color picker with preset & custom colors
- Drag chips between panels to reorganize signals


### Fixed¶


- **Improved Query Performance:** Queries are now 10‑1000× faster.
- **Value Truncation:** Axis and tooltip values are no longer truncated.
- **Timeline:** Resolved timeline zoom quirks.


25.0.13


### Added¶


- **Log in with Code:** Added support for signing into the app via code.


25.0.12


### Fixed¶


- **Query Performance:** Resolved issues with query performance in live mode.


25.0.11


### Fixed¶


- **Duplicate Time Values in Export:** Merged duplicate time values in the exported csv.


25.0.9


### Added¶


- **Plot Search:** All new signal tree with significant performance improvements


- Added glob pattern searching with highlighting for matches
- Added new controls to plot pane for expanding/collapsing all data
- Added ctrl+f to quickly search across sources, events, fields


### Fixed¶


- **Panel Exports:** Fixed a bug where you couldn't export panel data from trace
- **Color Picker:** Fixed a bug where the color picker would not work properly


25.0.8


### Fixed¶


- Fixed a bug with displaying value tables in the UI


25.0.7


### Added¶


- **Remote Connections:**


- Connect and capture data from either a local or remote app
- Data is stored in your local app in addition to the remote app


- **Panel Exports:** Exports all signal data from a panel within the timeline range into CSV/JSON format.[Read more](https://docs.zeloscloud.io/app/panels/#panel-export-feature) .
- **Panel Titles:** Titles appear at the top of a panel and saved as apart of the overall layout. Set and edit these directly in the panel editor.


### Changed¶


- **Connect/Open:** Open file and connect live buttons have been moved from the top to the plot sidebar
- **Layouts:**


- Better searching across name, user, dates, etc.
- Continue loading layouts as you scroll the dropdown


25.0.6


### Fixed¶


- **Export With No Fields:** Fixed a bug that prevented exporting trace events when no fields were present.
- **Export Error Handling:** The export process will continue with remaining trace events, rather than stop at the first error.


25.0.5


### Changed¶


- **Double-Clicking:** On Plot panels, the first double-click will return to your initial pause point, and the second will resume live mode
- **Timeline Reset Button:** When zoomed or panned on a plot, a reset icon will appear to return back to your initial view


25.0.4


### Fixed¶


- **Crash Reporting On Native Errors:** Improved crash reporting to capture native process crashes.
- **Plotting Non-Numeric Signals:** Fixed issue where plotting non-numeric signals resulted in no data displayed.
- **Signal Pane Flickering:** The signal pane no longer flickers when not attached to a live connection or trace.
- **Log Spam With Expired Refresh Tokens:** We no longer attempt to retry logins once the refresh token has expired.


25.0.2


### Fixed¶


- **Log spam resulting in full disk:** Improved log filtering to decrease log disk utilization.


25.0.1


### Fixed¶


- **Exporting Data with NaN values:** Resolved an issue where NaN values were not processed correctly when exporting data.


25.0.0


### Changed¶


- **Improved Table Panel:** All new redesign and implementation for the table panel. Read more.
- Easily filter, sort, and view multiple events/fields in a tabular view
- Built in column searching that works across events, field names and values
- Quickly copy data to your clipboard as JSON
- **Default Data Retention:** 8 hours is now the default data retention period.


### Fixed¶


- **Memory Leak in Live Mode:** The app should no longer cause an out-of-memory condition when running in live mode.
- **Auto-restart UI on Crash:** The UI will automatically restart if it crashes (white screen).


24.1.11


### Added¶


- **Timeline:** An interactive timeline bar with finely tuned controls. Read more.
- Drag left and right markers to adjust the start and end times of your time window
- Click and drag the highlighted area to move the entire time window along the timeline range
- New pulsating live data indicator shows when live data is being streamed
- Play/Pause live data with a single click, and use Left/Right arrow buttons to navigate the time window
- **Timeline Shortcuts:** Added shortcuts for controlling the timeline
- Left/Right Arrow Keys: Move the time window left or right
- Spacebar: Play or pause live data in live mode


### Changed¶


- **Tooltips:** Improved tooltip behavior for better usability and responsiveness
- **Station Configs:** Renamed "Trace Config" to "Station Config" for improved clarity and consistency


### Fixed¶


- **Tabs:**
- Enhanced light/dark mode support for tabs
- Active tab is always visible in the tab bar
- Fixed maximum character limit for tab names to prevent truncation


24.1.10


### Added¶


- **Tab Reordering:** Tabs can now be dragged to reorder them
- **Tab Shortcuts:** Added shortcuts (with associated macOS command variants)


- ` ctrl-t` new tab
- ` ctrl-w` close tab
- ` ctrl-\[1-9\]` switch to tab
- ` ctrl-shift-\[` previous tab
- ` ctrl-shift-\]` next tab
- ` alt-left-arrow` previous tab
- ` alt-right-arrow` next tab


24.1.9


### Fixed¶


- **Time Axis Labels:** Axis labels always indicate the full timestamp
