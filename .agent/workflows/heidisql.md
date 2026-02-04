---
description: How to work with the HeidiSQL codebase
---

# HeidiSQL Project Overview

HeidiSQL is a graphical database management tool for MariaDB, MySQL, Microsoft SQL Server, PostgreSQL, SQLite, Interbase, and Firebird. It is written in **Delphi/Object Pascal** for Windows.

## Project Structure

```
HeidiSQL/
├── source/              # Main Delphi source files (.pas, .dfm)
│   ├── main.pas         # Main application form
│   ├── dbconnection.pas # Database connection handling
│   ├── apphelpers.pas   # Application helper utilities
│   ├── dbstructures.*.pas # Database-specific structures
│   └── *.pas/*.dfm      # Other forms and units
├── components/          # Third-party Delphi components
│   ├── synedit/         # Syntax highlighting editor
│   └── virtualtreeview/ # Virtual tree view component
├── packages/            # Delphi project files
│   └── Delphi12.3/      # Main project location (heidisql.dpr)
├── res/                 # Resource files (.rc, icons, etc.)
├── out/                 # Build output directory
├── extra/               # Extra tools and utilities
│   └── internationalization/ # Translation tools
└── build.php            # Build script
```

## Key Files

| File | Purpose |
|------|---------|
| `source/main.pas` | Main application form and entry point |
| `source/dbconnection.pas` | Core database connection logic |
| `source/apphelpers.pas` | Utility functions and helpers |
| `source/connections.pas` | Connection manager form |
| `source/table_editor.pas` | Table editing functionality |
| `source/tabletools.pas` | Table tools (maintenance, export, etc.) |
| `packages/Delphi12.3/heidisql.dpr` | Main Delphi project file |

## Development Requirements

> [!IMPORTANT]
> HeidiSQL requires **Delphi 12.1** (or 12.3) for Windows. Older versions will fail; newer versions may work.

### Required Components
1. **SynEdit** - Syntax highlighting editor component
2. **VirtualTreeView** - Tree view component for database objects
3. **madExcept** - Exception handler and debugging tool

## Build Process (Windows Only)

1. **PHP Build Script** - Run `php build.php` which:
   - Detects Git revision
   - Cleans unversioned files (`git clean -dfx`)
   - Downloads translations from Transifex
   - Compiles translation files (.po → .mo)
   - Compiles SynEdit and VirtualTree components
   - Compiles resource files
   - Compiles main project
   - Patches executable with translations
   - Patches with madExcept exception handler

2. **Manual Build** (via Delphi IDE):
   - Install SynEdit packages (from `components/synedit`)
   - Install VirtualTree packages (from `components/virtualtreeview`)
   - Install madExcept
   - Open `packages/Delphi12.3/heidisql.dpr`
   - Build project

## Code Style & Conventions

- **Language**: Object Pascal (Delphi)
- **Form Files**: `.dfm` files contain form layouts
- **Unit Files**: `.pas` files contain Pascal code
- **Naming**: Use meaningful names, PascalCase for types/functions
- **Comments**: Use `//` for single-line, `{ }` or `(* *)` for multi-line

## Working with Forms

Each form typically has two files:
- `formname.dfm` - Visual form definition (do not edit manually)
- `formname.pas` - Form logic and event handlers

## Translation

- Translations managed via [Transifex](https://explore.transifex.com/heidisql/heidisql/)
- Translation files stored in `out/locale/`
- Uses GNU gettext format (.po/.mo files)
- Tool: `extra/internationalization/tx.exe` for pulling translations

## Contributing Guidelines

> [!CAUTION]
> Pull requests are ONLY accepted for **bugfixes**, not new features.

1. Create an issue first if one doesn't exist
2. Reference the issue ID in pull requests
3. Follow existing code style
4. Test changes thoroughly

## Common Tasks

### Understanding a Feature
1. Start with `source/main.pas` - it's the main form
2. Search for related `.pas` files in `source/`
3. Check form events and handlers

### Finding Database-Specific Code
- `source/dbstructures.mysql.pas` - MySQL/MariaDB specifics
- `source/dbstructures.mssql.pas` - Microsoft SQL Server
- `source/dbstructures.postgresql.pas` - PostgreSQL
- `source/dbstructures.sqlite.pas` - SQLite
- `source/dbstructures.interbase.pas` - Interbase/Firebird

### Searching for Functionality
```bash
# Search for specific functions or text
grep -r "searchterm" source/

# Find all form files
find source/ -name "*.pas" -type f
```

## Notes for AI Agents

- This is a **Windows-only** Delphi project
- Cannot compile on macOS/Linux without Delphi
- Build script (`build.php`) requires Windows + Delphi
- Focus on code review, analysis, and documentation
- For code changes, provide Pascal/Delphi syntax
- Large files like `main.pas` (573KB) contain extensive functionality
