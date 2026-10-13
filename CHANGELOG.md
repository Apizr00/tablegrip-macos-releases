# Changelog

## [1.5.2] - 2026-10-13

### Fixed
- **Updating a copy that was opened from Downloads.** macOS runs a freshly downloaded app from a read-only, randomised
  location, so it could not replace itself and the update failed. The update is now installed in the Applications folder
  (or ~/Applications) and opened from there. A copy in a folder that needs an administrator (an Applications folder owned
  by another account) asks for the password instead of failing, and the update window says where it will install.
- An update can be installed where no copy exists yet.

## [1.5.1] - 2026-10-13

### Changed
- **One download for every Mac.** The app is now a universal build that runs natively on Apple silicon (M series) and
  Intel chips. 1.5.0 was Apple silicon only.

## [1.5.0] - 2026-10-13

### Changed
- **A TablePlus-style window.** The toolbar sits in the title bar beside the traffic lights; the item sidebar has its
  Items / Queries / History switch along the bottom and Tables and Views sections with counts; tabs are joined to the
  content, the single-click preview tab is shown in italics; the grid is a native table with stripes, a sort arrow in the
  header, right-aligned numbers and proportional text; the status bar carries + Row, Data | Structure, paging and
  Filters. Light and Dark are neutral greys with one blue accent (System follows macOS); Classic Light and Classic Dark
  keep the Windows palettes.
- The red button closes the open connection first (as on Windows), then the window.
- The Structure side is a table (column_name, data_type, nullable, default), and the editor and results are split by a
  handle you can drag.

### Added
- **The app updates itself.** It looks for a new version at start-up and every six hours, shows the release notes,
  downloads with a progress bar, verifies the download and replaces itself on *Install and Restart*. *Remind Me Later*,
  *Skip This Version*, a notice on the start screen and an *Update* chip in the status bar, as on Windows.
- **Export several tables at once** from the sidebar (⌘-click / ⇧-click, then Export): one file per table or one SQL script.
- **A fuller grid menu:** Edit value… (multi-line, JSON), Set value (NULL / empty string), Paste and Paste to selected
  cells, Duplicate row, Copy as (CSV, JSON, SQL INSERT), Copy all values of a column, Quick filter, Follow relationship
  (including rows that point at this one), Import / Export, Delete row, and the matching keys.
- **Search** the loaded rows of a query result (⌘F), with a match count.
- **Command palette** with fuzzy matching (`nq` finds *New query*), saved queries, `>` for commands and arrow-key
  navigation; ⌘P opens it too.
- **Snippets with places to fill in:** type a trigger and press Tab; Tab moves between fields and repeated fields mirror.
  Suggestions open by themselves while typing and know the columns and aliases of the statement.
- **Colour picker, custom tags** (with delete) in the connection editor; **editor font and size**, auto-complete switch.
- Each open connection keeps its own tabs, staged edits and sidebar while another is in front.
- Drop a database file on the window, or open it from Finder.
- MariaDB connections open (they use the MySQL driver).

### Fixed
- ⌘S did nothing while the menu item looked disabled.
- Settings written by an older version are read instead of being replaced by defaults when a key is new.

### Known
- SQL Server is not supported on macOS yet.
