# TableGrip user guide

## Connecting

1. **New Connection** (⌘N) asks for the database type, then the details. File engines (SQLite) open a file; server
   engines ask for host, port, user, password and **database name**. One connection opens exactly one database.
2. **Test** checks the settings without saving. **Diagnose** walks the path to the database step by step (the name, the port, the SSH sign-in, the login, a first query) with the time of each, and says what a failure most likely means. **Save** keeps the connection in the list; **Connect** saves and opens it.
3. SQLite files can also be opened with **Open Database File** (⌘O) or by dropping them on the window.

### The engines
| Engine | How it connects | What to know |
|---|---|---|
| MySQL, MariaDB, PostgreSQL | Their own protocols | Everything in this guide. |
| SQL Server | The TDS protocol (port 1433), a SQL login such as `sa` | Leave Database empty for `master`. Only the first result set of a batch is shown. |
| ClickHouse | HTTP (port 8123), HTTPS (8443) with SSL mode Required or stricter | Tables without a key open read-only. |
| SQLite, DuckDB | A file (DuckDB also `:memory:`) | DuckDB creates a file that does not exist yet. DuckDB cannot interrupt a running statement. |
| Redis | Port 6379, optional user and password | The *Database* field is the number (0 to 15). Keys are the tables; a key opens as its members (a hash as field / value, a list as index / value, a sorted set as member / score). Run commands in a query tab, one per line. Browsing is read-only. |
| MongoDB | Port 27017, SCRAM sign-in | Databases hold collections; a collection opens as its documents, with a column for every field found. Run `db.items.find({...})`, `db.items.updateOne(...)`, `show collections`, `use shop` or a JSON command such as `{"ping": 1}` in a query tab. Browsing is read-only. |

### Passwords
- *Store securely* keeps the password in the login keychain. It is never written to `connections.json`.
- *Ask every time* asks when you connect and keeps it in memory only.
- *No password* is for servers that do not need one.

### Over SSH
Turn on **Over SSH** in the connection form, then give the SSH server, user and either a password or a private key file.
The database host is the address *as seen from the SSH server*. The first time you connect, TableGrip shows the server's
fingerprint; compare it with the one your administrator gave you before you trust it.

### SSL mode
| Mode | Meaning |
|---|---|
| Disabled | No encryption. |
| Preferred | Encrypt if the server offers it. |
| Required | Always encrypt; the certificate is not checked. |
| Verify CA | Also check that the certificate comes from the CA you give (or a trusted one). |
| Verify identity | Also check that the certificate matches the host name you typed. |

### Tag, colour and safe mode
The **Tag** marks what a connection is for: **None**, **local**, **development**, **testing**, **staging** or
**production** (or type a name of your own, up to 24 characters). The tag colours the connection dot and the connection
pill. **Colour** picks the dot's colour, either from the swatches or as `#RRGGBB`.

Safe mode is per connection: **Off**, **Confirm changes** (every statement that writes or changes the schema asks first)
and **Read-only** (writes are refused). Marking a connection *production* turns Off into Confirm. On a production
connection a destructive statement (`DROP`, `TRUNCATE`, `UPDATE` or `DELETE` without a `WHERE`) asks you to **type the
connection's name** before it runs. Safe mode is a guard against mistakes, not a security boundary: for a hard
guarantee use a database account without write privileges.

## Working with tables

- Click a table to preview it, double-click to keep the tab. Click a column header to sort (on the server).
- **Page**, **Count** and the filter bar all work on the server: sorting, filtering and paging never load the whole
  table. The status line shows the SQL that ran and how long it took.
- The filter bar takes a column, an operator and a value: `=`, `<>`, `<`, `>`, `<=`, `>=`, `IN` / `NOT IN`, `IS NULL` /
  `IS NOT NULL`, `BETWEEN` / `NOT BETWEEN`, `LIKE`, then Contains, Not contains, Has prefix and Has suffix. The quick
  search box on the right matches any text column.
- **Follow relationships.** A foreign-key cell that holds a value ends in an arrow (**→**): click it to open the table
  it points at, filtered to that row.
- **Select several tables** in the sidebar (⌘-click, ⇧-click, or *Select all tables* in the right-click menu), then
  right-click for **Export**: one file per table (CSV, JSON, Excel) in a folder you choose, or one .sql script.
- **Search** (⌘F) puts the cursor in the search box of the open table (on the server, every column) or of a query result
  (the loaded rows, with a "12 of 250 rows" count). ⌥⌘F filters the sidebar.
- **Right-click a cell** for *Edit value…* (a multi-line editor, with JSON formatting), *Set value* (NULL, empty string),
  *Reload rows*, *Paste* and *Paste to selected cells* (staged, so they can be undone), *New row*, *Duplicate row*,
  *Copy*, *Copy as* (with headers, CSV, JSON, SQL INSERT), *Copy all values of this column*, *Quick filter*, *Follow
  relationship* (the row a key points at, or the rows of other tables that point at this one), *Import CSV*, *Export rows*
  and *Delete row*. The menu acts on the cell under the pointer.
- **Details panel** (right) shows every column of the selected row with its full, selectable value. It follows the cell
  you are on and refreshes when a staged edit changes the row.
- **Structure** shows columns with their types, keys, indexes, the CREATE statement, the size of the table and its
  indexes, **Analyze columns** (share of NULLs, distinct values, smallest and largest) and **Find usages** (the foreign
  keys, views, routines and triggers that mention the table).
- **Console** (bottom) records the SQL behind table pages, query runs, commits and errors, with row counts and timings.

## Editing

**Query results, keys and conflicts.** A plain `SELECT` from one table (columns or `*`, with WHERE, ORDER BY, LIMIT) gives an
editable result: ⌘S commits, ⇧⌘R shows the SQL. A table with no primary key is edited by a unique key, or by all its values; a
change that would touch more than one row is refused. When you commit, a value you replace must still be what you read, or
the commit is refused (Preferences > Editing turns that off).

**Pickers.** ⌥↓ or F4 opens a list for an enum or a CHECK that lists values, TRUE / FALSE / NULL for a true / false column
(Space flips it), the rows of the referenced table for a foreign key, and a calendar for dates and times.

**Details panel.** Its fields are live: leaving a box stages the value, Esc restores it. ⌥⌘V pastes a block copied from a
spreadsheet; ⇧-click a header adds a sort column; double-click a binary cell to look at it.

**Edit structure.** On the Structure side, *Edit structure* turns the columns, indexes, foreign keys and checks into grids.
*Review SQL* (⌘S) shows exactly what will run; *Discard changes* puts everything back.


Nothing is written until you **Commit**. Edits are staged in the tab:

- Double-click a cell (or press Enter) to edit it; Esc cancels. Tab moves on.
- **New row** adds a row; **Duplicate row** copies one, leaving the key columns to the database.
- Delete marks rows; **Undo** (⌘Z) and **Redo** (⇧⌘Z) walk the staged changes back and forth.
- **Preview SQL** shows exactly what Commit will run, with values written out.
- **Commit** runs every statement in one transaction: if one statement fails — or does not affect exactly the row it
  should — nothing is saved and the SQL and the error are shown together.
- **Discard** throws the staged changes away.

A table with no primary key and no unique key cannot be edited: there is no way to identify a row, so it is read-only.

## SQL editor

| Action | Keys |
|---|---|
| Run the statement at the caret, or the selection | ⌘↩ |
| Run every statement | ⇧⌘↩ |
| Stop | Esc |
| Suggest names | ⌃Space (`⌘Esc` in some layouts); also automatic after two letters or a dot |

**Parameters.** Write `:name` in a query (`WHERE customer_id = :id`) and TableGrip asks for the value each time it runs;
the values are sent as parameters, never pasted into the text. Leave a box empty or type NULL for NULL; numbers and
true/false are sent as such, anything else as text. Colons inside strings and comments, and PostgreSQL's `::` casts, are
left alone. History keeps the query with its `:name` placeholders, so running it again asks again.

**Snippets.** Type a trigger word and press Tab to expand it, or pick it from the suggestions. In its text `${1:name}`
is a place to type with a default, and `$0` is where the caret finishes. Eight starters ship with the app (`sel`, `selc`,
`ins`, `upd`, `del`, `join`, `grp`, `dup`); **Snippets…** in the Tools menu adds your own.

**Several statements.** Run every statement (⇧⌘↩), or select several and run the selection: each statement that returns
rows gets a tab (Result 1, Result 2, …) and the **Messages** tab lists what every statement did, or the error that
stopped the script. The tabs are replaced by the next run.

**SQL files.** **Open SQL File…** (⇧⌘O) reads a `.sql` file into a query tab and remembers it: ⌘S saves the file
(asking for a name the first time), ⇧⌘S saves it under another name, and **Open Recent SQL File** lists the last ten. A
dot after the tab's name means the text differs from the file, and closing the tab asks first. In a table tab ⌘S
commits instead.

**History, favorites and snippets.** Every run is added to **History** in the sidebar; type in the search box to find
one. Right-click > **Pin to top** keeps a query at the top, and pinned queries survive *Clear history*. **Favorite**
keeps the query under **Queries**; right-click a saved query > **Move to folder…** groups the list, and **Make a
snippet with a keyword…** lets you type the keyword and press Tab to insert it. The three lists each keep their own
search text.

## Plans, diagrams and comparing data

- **Explain** (⌘L) shows how the engine would run the statement at the caret or the selection, step by step: the
  operation, what it works on, estimated rows and cost, and each step's share of the work. Likely hot spots and remarks
  are marked. **Analyze** (PostgreSQL) runs a *read* and reports real row counts and times. *Copy plan as text* puts the
  plan in the clipboard.
- **ER diagram** draws the tables with their columns and the foreign keys between them, related tables side by side and
  tables without relations below, in crow's-foot notation. Focus on one table and *within N relations* to see only its
  neighbourhood, choose how many columns show, click a table to highlight its relations, double-click to open it, and
  **Export PNG** or **Export SVG**.
- **Compare data** compares two tables by key, each from any open connection. It lists the rows that exist only in the
  source, only in the target, and the rows whose values differ, then writes the script that would make the target match
  the source. **Nothing is changed**: review the script, copy it, or open it in a query tab and run it there, where safe
  mode and the change log apply. The target is read into memory, so it is limited to 500,000 rows.

## Backup and restore

- **Backup** writes one SQL script: the structure of each table and view, then the rows as multi-row INSERT statements.
  Tables come in foreign-key order and views last, and you can ask for `DROP TABLE IF EXISTS` before each table so a
  restore replaces what is there.
- **Restore** runs a SQL script on the chosen connection **as one transaction**, so a failure leaves the database as it
  was. MySQL and MariaDB commit every CREATE and DROP on their own, so a failed restore there can be partly applied and
  the message says so. A read-only connection refuses to restore.
- The script is held in memory while it runs, so a file above 256 MB is refused. For a backup that large use the
  database's own tools (`pg_dump`, `mysqldump`, `sqlite3 .dump`).

## Change log and connection health

Both are in **Tools** and in the command palette (⌘K).

- **Change log** lists what was committed on this connection: table edits, statements run from a query tab that change
  data or structure, and CSV imports, each with the time and the SQL as it ran, values included. Search it or copy the
  SQL of an entry. The file (`audit.json` in your data folder) stays on this computer, and passwords typed into a
  statement are masked.
- **Connection health** times a few do-nothing statements and shows the typical round trip with a rating, plus the
  server version, TLS and uptime. A slow round trip means the network, a VPN or a tunnel is slow; if it is fast, a slow
  query is slow inside the server.

## Export and import

- **Export** (⇧⌘E): CSV, JSON, SQL INSERTs or Excel. A table can be exported whole (current sort and filter) or just the
  loaded rows; a query result is exported as shown.
- **Import CSV** (⇧⌘I): the delimiter is detected, columns are matched by name and can be remapped, and the file is
  validated *before* anything is written. An error leaves the table untouched.

## Tabs come back

When you open a connection again, its tabs are as you left them: each table with its sort, filter, page and Data or
Structure side, each query with its text and cursor, and the sidebar filter. They are written down every few seconds and
when you quit. Preferences > Tabs turns this off. Passwords are never part of it.

## Switch database

**Database > Switch Database…** (⇧⌘D) lists the databases of the server that your login can open and opens the one you
choose, with the same login and tunnel and without saving another connection. The old connection closes once the new
one is open; if it has staged edits and you keep them, both stay open. A SQLite or DuckDB file is one database, so open
another file instead.

## Several connections at once

The **new connection button** in the top bar opens the *Open connection* list; pick one and it opens **in the same
window**, while the connection you are in stays open. With two or more open, a **rail of connection tabs** appears on
the far left, one entry per connection, coloured by its environment tag. Each connection keeps its own sidebar, tabs,
selection and unsaved changes. Closing the last one returns to the start screen.

## Keyboard

| Keys | Action |
|---|---|
| ⌘K | Command palette |
| ⌘P | Search tables and saved queries |
| ⌘N / ⌘O | New connection / open file |
| ⌘T / ⌘W | New query tab / close tab |
| ⌘↩ / ⇧⌘↩ | Run the statement, run every statement |
| Esc | Stop the running statement |
| ⌘R | Refresh |
| ⌘F | Filter the sidebar |
| ⌘, | Preferences |
| ⌘Z / ⇧⌘Z | Undo / redo staged edits |
| ⌘S / ⇧⌘⌫ | Commit staged edits (save the file in a query tab) / discard staged edits |
| ⇧⌘R | Review the SQL of the staged edits |
| ⇧⌘O / ⇧⌘S | Open a SQL file / save the SQL file as |
| ⇧⌘D | Switch database |
| ⌥⌘1, ⌥⌘2, ⌥⌘3 | Sidebar, details panel, console |
| ⌘K / ⌘P | Command palette: commands, tables and saved queries (type `>` for commands only) |
| ⇧⌘[ / ⇧⌘] | Previous / next tab |
| ⌘F / ⌥⌘F | Search the table or result / filter the sidebar |
| ⇧⌘W | Close the connection |
| ⌘L | Explain the statement at the caret |
| ⇧⌘E / ⇧⌘I | Export / import CSV |

## Updates

TableGrip updates itself. It looks for a new version when it starts (and every few hours while it stays open); you can
turn that off in Preferences > History and data.

- **When a new version exists** a window opens: *A new version of TableGrip is available!* with the release notes.
  **Install Update** downloads it with a progress bar and checks it (right app, right version, intact signature); then
  **Install and Restart** replaces the app and opens the new one. Your connections, settings and history are kept, and
  it asks first if a tab has edits that were not committed.
- **Remind Me Later** closes the window; the start screen keeps an orange notice (*What's new*, *Update*) and the
  status bar shows an *Update* chip. **Skip This Version** hides that version until a newer one appears.
- **Check for Updates…** (the TableGrip menu, or Preferences > *Check now*) looks once, whenever you like, and offers a
  skipped version again.
- It works for a copy in `/Applications` (or any folder you can write to). A copy run from the Downloads folder or
  straight from `swift run` can only point you at the release page.
- Nothing but the release page on GitHub is contacted, and nothing about you or your data is sent.
