# AGENTS.md

## Project Overview

This folder is an individual repository for TRPGLine system rule reference documents. It is stored inside the main TRPGLine room app at `public/doc/TRPGLine-system-rule/`, but should be treated as its own docs/data repo.

The repo currently contains structured rule-reference JSON for:

- `coc7` - Call of Cthulhu 7th Edition
- `dnd5` - Dungeons & Dragons 5th Edition
- `dnd2024` - Dungeons & Dragons 2024

Rule text accuracy is not the primary concern here. The important work is preserving the file structure, JSON shape, locale pairing, and stable identifiers used by the TRPGLine document viewer.

## Repository Structure

```text
TRPGLine-system-rule/
+-- AGENTS.md
+-- README.md
+-- LICENSE
+-- coc7/
|   +-- en/
|   +-- zh-TW/
+-- dnd5/
|   +-- en/
|   +-- zh-TW/
+-- dnd2024/
    +-- en/
    +-- zh-TW/
```

Each system folder contains one folder per supported locale:

- `en` - English content
- `zh-TW` - Traditional Chinese content

The locale folders for a system should mirror each other. If a JSON document exists in `en`, the matching document should usually exist in `zh-TW` with the same filename and compatible `id` values.

## System Folder Contents

Each system has a `categories.json` file plus one JSON file per rule category.

Examples:

- `coc7/en/categories.json`
- `coc7/en/combat.json`
- `dnd5/en/categories.json`
- `dnd5/en/spellcasting.json`
- `dnd2024/en/categories.json`
- `dnd2024/en/classes.json`

`categories.json` defines the category list shown by the app. Each category entry uses this shape:

```json
[
  {
    "id": "combat",
    "name": "Combat"
  }
]
```

The `id` normally matches a sibling JSON filename without the `.json` extension. For example, a category with `"id": "combat"` should correspond to `combat.json`.

## Rule Document Shape

Rule category files are JSON arrays. Each array item represents a section in that category.

Use this shape:

```json
[
  {
    "id": "stableSectionId",
    "name": "Section Name",
    "description": "Optional overview text, usually Markdown",
    "tabs": [
      {
        "name": "Tab Name",
        "content": "Markdown content"
      }
    ]
  }
]
```

Expected fields:

- `id` - Stable identifier used by the viewer. Keep existing IDs unless a structural rename is intentional.
- `name` - Display name for the section.
- `description` - Optional Markdown text shown before tabs.
- `tabs` - Ordered list of tab objects.
- `tabs[].name` - Display name for a tab.
- `tabs[].content` - Markdown body for the tab.

Markdown is allowed inside `description` and `content`, including headings, lists, tables, emphasis, and inline code.

## Editing Rules

- Preserve valid JSON. Do not leave trailing commas or comments in JSON files.
- Preserve the existing top-level array format in every rule document.
- Keep `categories.json` synchronized with sibling category files.
- Keep locale folder structures synchronized when adding, renaming, or removing files.
- Prefer stable ASCII filenames and category IDs such as `combat`, `spells`, or `gameSystem`.
- Do not rewrite large rule content unless the task explicitly asks for content changes.
- Do not normalize existing mojibake or encoding artifacts as part of unrelated structure work.
- Do not add app code, build scripts, package files, or dependencies here unless the repo is intentionally being expanded beyond static JSON docs.

## Validation

After editing JSON files, run a JSON parse check before finishing. From this folder, a simple validation command is:

```bash
find . -name '*.json' -print0 | xargs -0 -n1 node -e "JSON.parse(require('fs').readFileSync(process.argv[1], 'utf8'))"
```

When working from PowerShell, use:

```powershell
Get-ChildItem -Recurse -Filter *.json | ForEach-Object { node -e "JSON.parse(require('fs').readFileSync(process.argv[1], 'utf8'))" $_.FullName }
```

## Relationship to the Main App

The main TRPGLine app serves these files as public static documents. Avoid importing application code from this repo, and avoid assuming this repo has the main app's build tooling available. Treat the JSON files as portable static data.
