# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an Anki add-on that imports/exports CSS and card templates for all note types. The add-on allows users to manage their Anki templates as files on disk, making version control and bulk editing easier.

## Architecture

### Entry Point and Initialization
- `__init__.py`: Entry point that calls `gui.init()`
- `gui.py`: Initializes the add-on by adding menu items to Anki's Tools menu and setting up the error dialog

### Core Components

**templates.py**: Core import/export logic
- `export_tmpls()`: Exports all note types to a directory structure where each note type becomes a folder containing:
  - A CSS file (default: `style.css`)
  - Template files for each card (front and back separated by delimiter)
- `import_tmpls()`: Imports templates from directory structure back into Anki
  - Matches folders to note types by name
  - Supports optional global CSS file at root level
  - Only updates existing note types; doesn't create new ones

**gui.py**: User interface and Anki integration
- Creates menu under Tools → "Import / Export templates"
- `get_dir()`: Directory picker with last used directory persistence
- `show_error()`: Displays error messages using Anki's error dialog
- `notify()`: Shows tooltip notifications

**utils.py**: Configuration management
- Maps simplified config keys to full config file keys
- `reload_config()`: Loads configuration from Anki's add-on manager
- `cfg(key)`: Retrieves config values with error handling

### Configuration System

Configuration is stored in `config.json` with these settings:
- `delimiter between front and back template`: Separates front/back templates in files (default: `` ``` ``)
- `CSS file name`: Name for CSS files (default: `style.css`)
- `insert global CSS before individual ones of all note types`: Whether to prepend global CSS (default: `false`)
- `filename extensions for card template files`: Extension for template files (default: empty string)
- `last_dir`: Persisted last used directory for file picker

### Anki API Keys

Key field names used to interact with Anki's data model:
- `css`: Note type CSS
- `tmpls`: Templates array
- `name`: Note type or template name
- `qfmt`: Question (front) template format
- `afmt`: Answer (back) template format

## Development Commands

### Building Add-on Package
```bash
python zzz_makeAnkiAddonFile.py
```
Creates an `.ankiaddon` file (timestamped ZIP) excluding `__pycache__`, `.vscode`, `meta.json`, and `.ankiaddon` files.

### Testing
No automated tests present. Testing requires:
1. Installing add-on in Anki
2. Manually testing export/import with various note types
3. Verifying CSS handling (individual, global, merged)

## Key Implementation Details

### API Compatibility
The codebase includes a fix for Anki 23+ compatibility:
- Changed from deprecated `col.models.byName()` to `col.models.by_name()` (line templates.py:49)

### Error Handling
- Invalid note type names with leading/trailing spaces trigger warnings
- Invalid filesystem characters in note type/card names are caught and reported with detailed error messages
- Missing configuration keys show specific error messages

### File Structure for Import/Export
```
export_directory/
├── style.css              # Optional global CSS
├── Note Type 1/
│   ├── style.css         # Note type CSS
│   ├── Card 1            # Front/back separated by delimiter
│   └── Card 2
└── Note Type 2/
    ├── style.css
    └── Card 1
```
