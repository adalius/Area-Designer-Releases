# Changelog

All notable changes to Area Designer are listed here, newest first.
## 

## Version 1.0.5

### Improvements

- The **WC List** and **AC List** fields in Items/Objects and the **Increase
  WCS** and **Increase ACS** fields in Monsters have been replaced with ten
  individual labelled entry boxes — one per damage type (edged, blunt, fire,
  ice, acid, electric, mind, energy, poison, radiation). Each box accepts an
  integer from `-9999` to `9999` and turns red on focus-out if the value
  is invalid. The underlying database storage and file export format are
  unchanged.
- The Windows MSI installer now creates a **Start Menu** shortcut under
  *Programs* automatically during installation and removes it on uninstall.
  No user prompt is shown.
## 

## Version 1.0.4

### New Features

- Added automatic update checking. The application silently checks for a new
  release on startup (after a 3-second delay) and prompts the user if one is
  found. On Windows and macOS the installer is downloaded automatically and
  launched; on Linux the archive is saved to `~/Downloads` and the file
  manager is opened at that location.
- Added a **Check for Updates** button to the Select Map window for
  manually triggering an update check at any time.
## 

## Version 1.0.3

### New Features

- Added zoom controls to the Edit Map toolbar. Five discrete levels are
  available (50 %, 75 %, 100 %, 150 %, 200 %); the current zoom percentage is
  shown between the `−` and `+` buttons, and the view re-centres on the
  same map position after each zoom step.
- Added a hover tooltip on the map grid: holding the cursor over a room for
  half a second pops up a small label showing the room's filename and any
  items or monsters placed in that room.

### Improvements

- The *Weapon Inherit Files* and *Armour Inherit Files* fields in Configure
  Area now default to `"obj/weapon"` and `"obj/armour"` respectively when
  no value has been saved, matching the most common usage and saving a step for
  new areas.
- Renamed the **Can Get** and **Can Drop** checkboxes in the Items/Objects form
  to **Include Get Hook** and **Include Drop Hook** to make their purpose
  clearer (they control whether the generated `.c` file includes the
  corresponding hook function, not a simple permission flag).
## 

## Version 1.0.2

### Bug Fixes

- Fixed a crash when opening Room Details for rooms that have items assigned
  to them (`NameError: name 'list_canvas' is not defined`).
- Fixed entrance rooms not turning gold immediately after being marked as the
  entrance — the colour now updates as soon as the Room Details window is
  closed without needing to reload the map.
- Fixed monsters appearing greyed-out and unaddable in the Room Details
  Monsters tab.
- Fixed inherit file fields (e.g. *Global Inherit File*, *Room Inherit Files*)
  mangling `MACRO+"filename"` entries — for example, `ROOMS+"myroom"` was
  incorrectly corrected to `"ROOMSmyroom"`.
- Fixed the .c exporter double-escaping escape sequences already present in
  user-entered text — for example, a user-typed `\"` was being exported as
  `\\"` instead of being left as `\"`.
- Fixed wrapped exported lines consistently exceeding the 80-column hard limit
  by 1–3 characters.

### Improvements

- The application version number is now shown in the title bar of every window,
  making it easy to confirm which version is running at a glance.
- Added a versioned schema migration system to the database layer. The
  application now tracks the internal database schema version and applies
  incremental migrations automatically on startup. For future schema changes
  that cannot be made via `ALTER TABLE` (column renames, constraint changes,
  etc.), the system performs a safe full-database rebuild: it backs up the
  existing database, builds a fresh copy with the new schema, migrates all
  data across, and atomically replaces the old file. No user data is lost and
  no manual action is required.
## 

## Version 1.0.1

*Bug fix release.*

- Fixed exported long descriptions and other wrapped text losing spaces between
  words at line-break points.
- Fixed array output (e.g. spell lists) wrapping to misaligned continuation
  lines.
- Fixed weapons being exported as generic objects rather than the correct
  weapon type.
- Fixed SW and SE exit arrows displaying in blue instead of their correct
  colours (red and green respectively).
- Fixed the application failing to start after installation due to the
  `logging` module being incorrectly excluded from the packaged build.
- Fixed `set_long()` output not splitting at paragraph boundaries (`\n`
  markers) when wrapping long descriptions.
- Fixed description strings in `add_item()` and similar calls being skipped
  during line wrapping, leaving them unwrapped regardless of length.
## 

## Version 1.0.0

*Initial release.*

### Grid & Map Editor

- Scrollable canvas grid for placing and navigating rooms across a space of
  101 × 101 × 101 cells (X and Y from −50 to +50, Z from −50 to +50).
- Z-level depth shadows give a visual indication of rooms above and below the
  currently displayed level.
- Entrance room highlighted in gold; directional exit arrows colour-coded by
  axis (cardinal exits in blue, NE/NW diagonals in red/green, SE/SW diagonals
  in red/green).
- Multi-room selection via click-and-drag and drag-to-move, preserving exit
  connections and filenames.
- Edit Map view automatically centres on the entrance room when one exists,
  falling back to the centroid of all rooms, or the origin if the map is empty.
- Left-click to create or remove rooms; right-click to open Room Details.

### Room Details

- Full set of room fields: Short description, Long description, Light level,
  Filename, Extra Header, Extra Inherit (with Override checkbox), and per-room
  code hooks (Extra Create, Extra Reset, Extra Init, Extra Code).
- Add Items and Search Items tabs for defining interactive objects within the
  room.
- Monsters and Items quantity tabs listing all entities defined for the map,
  with per-room quantity spinboxes and `create()` / `reset()` spawn-type
  selectors.
- Entrance checkbox automatically sets the room filename.
- Override Search Message field to replace the map-level default.

### Configure Area

- Area-wide settings: Creator Name, Realm, Area Path, Save Path (with browse
  button), and subpath overrides for rooms, objects, and monsters.
- Inherit file fields for Global, Room, Monster, and Object scopes.
- Per-scope code hooks: Create, Init, Reset, and Additional Code for each of
  global, room, monster, and object.
- Default Search Message applied to all rooms that do not override it.

### Monsters

- All monster fields: Name, Aliases, Short and Long descriptions, Race, Gender,
  Alignment, Level (minimum 1), Unique flag, and Legend text.
- Weapon class and armour class adjustment lists.
- Chat and aggressive-chat: percentage chance and message lists.
- Dead object reference.
- Spell system: spell chance, damage array, types, handlers, probabilities,
  target messages, and room messages — with cross-field count validation
  ensuring all spell arrays have matching element counts.
- Extra Inherit with Override, plus Extra Create, Init, Reset, and Additional
  Code.
- Carry Objects tab for assigning items carried by the monster.

### Items / Objects

- All item fields across a tabbed interface: Basic, Type-Specific, and
  Bind / Extra tabs.
- Type-specific field sets for Weapon, Armour, Container, Food, Drink, Scroll,
  Key, Light, and other item types.
- Bind settings: `will_bind`, bind type, bind name, and bind message.
- Extra Inherit with Override, plus Extra Create, Init, Reset, and Additional
  Code.

### .c File Export

- Single-button export generating all room, monster, and object `.c` files
  for the area.
- Automatic line wrapping at 80 columns for `set_long()` and other
  multi-line calls, including correct handling of paragraph breaks.
- LPC-correct output for all field types: quoted strings, integer literals,
  arrays (`({ ... })`), and raw code blocks.
- `.c` extension automatically stripped from filenames in `add_clone()`
  and `add_exit()` calls.
- Correct non-integer bind type quoting in `set_will_bind()`.

### Validation

- Comprehensive inline field validation across all forms, with fields turning
  red to indicate an error.
- Auto-correction of common formatting issues: missing quotes, comma-list
  normalisation, file extension removal, and inherit file path cleaning.
- Spell field count cross-validation: the number of elements in the Spell
  Damage array drives the required count for all other spell arrays; mismatches
  are flagged immediately on focus-out.
- Spinbox values clamped to their allowed range when entered by typing and
  focus leaves the field.
- Invalid field values are preserved so the user can correct them in-place
  rather than having to retype.

### Installers & Distribution

- Windows MSI installer built with cx_Freeze; requires UAC elevation and
  offers to remove user data on uninstall.
- Linux tarball with an `install.sh` script and a `.desktop` file for
  desktop integration.
- macOS DMG with an Applications symlink for the standard drag-to-install
  experience.
- Automated builds for all three platforms via GitHub Actions, triggered by
  pushing a version tag.

### Database & Storage

- SQLite database stored in the platform-appropriate user data directory:
  `%APPDATA%\AreaDesigner` on Windows,
  `~/Library/Application Support/AreaDesigner` on macOS, and
  `~/.local/share/AreaDesigner` on Linux.
- Database errors logged to `error.log` in the same directory.

### Help System

- Built-in Help button opening this browser-based Sphinx documentation.
- Tutorial covering installation, first map creation, configuring an area, and
  exporting files.
