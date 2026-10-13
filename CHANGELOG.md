# Changelog

## [1.11.0] - 2026-10-13

Batch 5 of the feature checklist: more engines and ways to connect (see *Roadmap and progress* in the README).

### Added
- **CockroachDB, Greenplum, Amazon Redshift and SingleStore** in the engine picker, marked *beta*. They speak the
  PostgreSQL or MySQL protocol, so the existing drivers open them, with each engine's own default port (26257, 5432,
  5439, 3306) and user. They were checked by running the drivers' own tests against PostgreSQL and MySQL servers under
  these engine names, not against the products themselves; features that rely on an engine's own catalog (query
  plans, the Administration views) may not work on them.
- **New connection from a URL or .env** (File menu, ⇧⌘N, or *Paste a URL or .env…* in the engine picker). Paste
  `mysql://user:pass@host:3306/db?ssl-mode=required`, `postgres://…`, `jdbc:mysql://…`, `sqlite:///path/to/file.db`, or the
  `DB_CONNECTION` / `DB_HOST` / `DB_PORT` / `DB_DATABASE` / `DB_USERNAME` / `DB_PASSWORD` (or `DATABASE_URL`) lines of a
  Laravel-style `.env` file; the connection form opens filled in. Passwords are percent-decoded and kept out of the
  saved connection list (they go to the Keychain when you save).
- **SSH jump hosts.** The SSH section of a connection has a *Jump host* field (`user@bastion:22`, several hops separated
  by commas): the tunnel goes through it with `ssh -J`. Only plain host names, user names and ports are accepted.

## [1.10.0] - 2026-10-13

Batch 4 of the feature checklist: editor extras (see *Roadmap and progress* in the README).

### Added
- **Line numbers and code folding in the SQL editor.** A gutter numbers the lines (a wrapped line has one number) and
  shows a triangle where a statement of three or more lines, a bracket, a CASE or a block comment can fold into one
  line with a ⋯ marker. Click the triangle or the ⋯, or use Edit > Folding (⌥⌘← fold, ⌥⌘→ unfold, ⌥⇧⌘← / ⌥⇧⌘→ all).
  The folded text is still there for copying, searching and running; editing inside it opens it. Turn the gutter off in
  Preferences > Grid and editor.
- **Multiple cursors.** ⌘D adds the next occurrence of the selected word, ⇧⌘L selects every occurrence, ⌥⌘↑ / ⌥⌘↓ add a
  cursor above or below, ⌥⌘-click adds one where you click. Typing, Backspace / Delete, Return, paste, copy and cut work
  at every cursor; Esc goes back to one. (Edit > Multiple Cursors.)
- **Split editor** (⌘\\, the Split button, Query menu): a second script beside the first in the same query tab. The pane with
  the keyboard is the one that runs, formats and saves; closing the split keeps what was written there.
- **A notice when a long job finishes** while TableGrip is in the background: the Dock icon bounces and, in the
  installed app, macOS shows a notification. It covers queries and scripts, exports, imports (CSV, JSON, SQL file),
  backup, restore and maintenance. Set the time that counts as long (5 seconds to 5 minutes) or turn it off in
  Preferences > History and data.
- **Connection timeout and retries** (Preferences > History and data): give up on a try after 5 to 120 seconds (or no
  limit), and try again up to 10 times with a pause between. Only a server that cannot be reached is retried; a wrong
  password or an unknown database is not. A message says when another try follows.

## [1.9.0] - 2026-10-13

Batch 3 of the feature checklist: administration (see *Roadmap and progress* in the README). Works on MySQL, MariaDB and
PostgreSQL; checked against MySQL 8.4 and PostgreSQL 18.

### Added
- **Database > Administration…** (⌥⌘A), one window with:
  - **Sessions:** who is connected and what each is running, refreshed every 4 seconds, with *Cancel query* and *End
    session* (both ask first).
  - **Locks:** which session waits for which, how long, and a button to end the blocking session.
  - **Server status** and **Settings**, searchable.
  - **Users & roles:** the accounts, the privileges of the selected one, *New account…* (name, host, password and an
    access level of connect only / read only / read and write / all privileges on the database, with the SQL shown first),
    *Change password…* and *Drop account…*.
  - **Replication:** replica or standby status, or the replicas of a primary.
  - **Maintenance:** analyze, optimize / vacuum, check or rebuild the indexes of one table or all of them.
- Database menu entries for Locks, Server Status, Users & Roles, Replication and Maintenance open the window on that view.
- Changes honour the connection's safe mode; on a read-only connection the buttons that change anything are off.

### Fixed
- **The loading fill on the connection pill now runs to the end of the pill** before it fades (the last step was dropped).

## [1.8.0] - 2026-10-13

Batch 2 of the feature checklist: data and schema tools (see *Roadmap and progress* in the README).

### Added
- **Table designer.** Database > Create Table… opens a grid of columns (name, type with suggestions, default, NULL, key,
  auto-increment) with the SQL shown underneath; *Create* runs it, *Open in query tab* hands it over.
- **Alter table** (sidebar, Database menu): rename, retype, add and drop columns; only the changes are written, in the
  syntax of the engine (MySQL `MODIFY COLUMN`, PostgreSQL `ALTER COLUMN … TYPE / SET NOT NULL`, SQL Server `sp_rename`).
  SQLite changes it cannot make in place are written as a note about rebuilding the table.
- **Drop table and Empty table** from the sidebar and the Database menu. The connection's safe mode asks as it does for
  any statement; a connection with safe mode off gets a confirmation of its own.
- **Import JSON.** *Import CSV or JSON…* reads an array of objects, `{"rows": [...]}` or one object per line, and uses
  the same column matching, validation and single transaction as CSV (nested values become JSON text, `null` becomes NULL).
- **Import SQL File…** runs a dump or migration statement by statement with progress, *Stop at the first error*, a Stop
  button, and safe mode on every statement.
- **Columns button** on a table: show or hide columns of the grid (they stay in exports and the row details).
- **Schema compare** (Tools): two open connections; lists tables only on one side and differing columns, indexes and
  foreign keys, and writes the script that makes the target match the source. Drops are comments unless you include them.

## [1.7.0] - 2026-10-13

Batch 1 of the feature checklist (see *Roadmap and progress* in the README).

### Added
- **Connection menu:** Connect, Disconnect, Reconnect, Edit, Duplicate and Test Connection (it reports how long the
  server took), SSH Tunnel, SSL/TLS, Connection Properties and Health.
- **Database menu:** Create Database / Schema / Table / View open a query tab with a starting statement for the engine;
  Users & Roles, Permissions and Maintenance (analyze, optimize or vacuum, check, rebuild indexes) do the same; Refresh
  and Browse Tables. Nothing runs by itself.
- **Format SQL** (⌥⌘L, the Format button, Edit and Query menus): keywords in capitals, one clause per line, the select
  list and AND / OR one per line, subqueries indented, one column per line in CREATE TABLE. Strings, comments and quoted
  names are never touched; stored routines and `DELIMITER` scripts are left as written.
- **Find and Replace in the SQL editor** (⇧⌘F, ⇧⌘H).
- **Tools > Generate SQL** writes CREATE, SELECT, INSERT, UPDATE or DELETE for the selected table in a query tab.
- **View:** Object Browser, Data Grid, SQL Editor, and Zoom In / Out / Actual Size (⌘+ ⌘− ⌘0) for the grid and editor.
- **File:** New SQL Query, Open SQL File, Save Query As, Import and Export. **Query:** Run Selection, Cancel Query, Query
  History. **Window:** Move Tab Left / Right, Reset Layout.
- **Help:** Documentation, Keyboard Shortcuts (a window listing them), Troubleshooting and Report a Bug.

### Fixed
- The loading fill on the connection pill now runs on to the end of the pill before it fades.
- The README tracks the checklist, batch by batch.

## [1.6.3] - 2026-10-13

### Changed
- **The connection pill is a solid bar in the connection's colour**, like TablePlus: `DEVELOPMENT | MariaDB 10.6.22 :
  connection : database` in white on blue, green for local, red for production, purple for staging; a connection
  without a tag uses its own colour. The loading fill still plays over it.

### Fixed
- **Pressing Refresh repeatedly** restarts the pill's fill from the left and the newest refresh takes over, as on
  Windows. Before, the fill could vanish when an earlier refresh finished.
- **A blank toolbar.** A window that opened while TableGrip was not the front app (launched from another app, or with a
  database file) showed an empty strip where the toolbar belongs: the opaque title bar was covering it. It is now kept
  transparent.

## [1.6.2] - 2026-10-13

### Changed
- **The start screen opens compact** (760 × 520, centred), like TablePlus's, instead of at the size it was last left.
- **The connection pill fills for table loads and query runs too**, not only for a refresh: the mosaic creeps along while
  rows load or a statement runs, completes, then fades.

### Fixed
- **Refresh can be pressed again and again.** A second ⌘R while one was running was ignored; each press now starts a
  refresh and the pill's fill stays until the last one is done.
- The environment chip (LOCAL, PRODUCTION, ...) is back inside the connection pill, in its colour.

## [1.6.1] - 2026-10-13

### Added
- **⌘F opens the filter bar** on a table and puts the cursor in the first value field (Esc closes it). On a query tab it
  still searches the results.
- **SQL opens a query tab.** On a table, *SQL* opens a new query tab with the statement the grid runs, filter values
  filled in, ready to edit with suggestions as you type. The toolbar's *SQL* button opens an empty query tab.
- **The connection pill fills like on Windows** while a refresh runs: a mosaic of small cells grows from the left, with
  coloured pixels flickering at the leading edge, then fades.

### Changed
- **The toolbar is arranged like TablePlus.** Buttons sit in rounded groups; Discard, Preview and Commit are icons that
  stay in view (greyed out until there is something to commit); the connection pill fills the middle in a monospaced
  font and ends with the open table's name.

### Fixed
- **The search box on the start screen** was hidden under the title bar.
- **The last filter condition could not be removed.** Removing it now clears the filter and closes the bar; ⌘F or the
  Filter button brings it back.

## [1.6.0] - 2026-10-13

### Added
- **Resizable panels.** Drag the divider of the sidebar, the details bar or the console to change its size; the sizes are
  remembered.
- **Draggable ER diagram.** Move the tables around; the relationship lines follow, leaving from the side that faces the
  other table, with the crow's foot and the optional circle turning with them.
- **Case-sensitive text filters:** *Contains*, *Not contains*, *Has prefix* and *Has suffix*, each with a
  "- Case sensitive" variant (MySQL `LIKE BINARY`, SQLite `GLOB`, SQL Server case-sensitive collation, PostgreSQL `LIKE`).
- An *Acknowledgements* button in About.

### Changed
- **Filter bar like TablePlus and the Windows app.** One line per condition with an on/off box, column, operator and
  value; nothing runs until you press Apply, Apply All or Return, so typing no longer reloads the table. *Match any
  condition* and *Clear* sit underneath.
- Environment tags on the start screen are small tinted badges.

### Fixed
- **The foreign key arrow in the grid** now shows on MySQL, PostgreSQL and SQLite tables (the lightweight table lookup
  did not read foreign keys). A NULL value shows a faint arrow that does nothing. The ER diagram draws its lines from
  the same data.

## [1.5.5] - 2026-10-13

### Changed
- **The update notice and About follow the Windows app.** On the start screen the notice sits under the version: the
  orange "Version X is available" line with *What's new* and *Update* beneath it. About shows the mark, name and version,
  who maintains TableGrip, and GitHub / User guide / Changelog buttons.

## [1.5.4] - 2026-10-13

### Fixed
- **The red close button.** With a connection open it closes the connection and returns to the start screen; pressed again
  on the start screen it quits. The second press did nothing before. Closing a connection also no longer waits for the
  server to hang up, so a slow or busy server cannot leave you stuck on the workspace.

## [1.5.3] - 2026-10-13

### Fixed
- **Check for Updates always answers.** Choosing it from the menu opens a window: the new version with its notes,
  "TableGrip is up to date", or why the check could not be made (offline, release page unreachable) with *Check Again*.
  Before, nothing appeared unless a newer version existed.

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
