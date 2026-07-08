# Repository layout — brand guidelines

This repo is **shared documentation**, not bundled into other apps at build time.
Each app keeps its own copied tokens and links here for the rules.

## Recommended `~/GitHub/` layout

```
C:\Users\alaba\GitHub\
├── brand_guidelines_for_apps\   ← this repo
├── Image_Naming_2\              ← FootClothes Image Namer (Vite + React)
├── header_card_project\         ← Header Card Generator (Python + PyQt)
├── fc-illustrator-scripts\      ← Illustrator JSX (header card batch runner)
├── dashboardworld\              ← token source of truth (Next.js BFF)
└── …
```

## Linking from an app

| Method | When |
| --- | --- |
| **Sibling folder** (default) | Both repos under `~/GitHub/` — open docs side by side |
| **Relative path in docs** | `../brand_guidelines_for_apps/BRAND_GUIDELINES.md` |
| **Git submodule** (optional) | Pin guidelines inside an app repo at a specific commit |

There is no npm package yet. If you add one later, document it here.

## New app checklist

1. Clone `brand_guidelines_for_apps` next to your app repo.
2. Copy semantic CSS variables from `BRAND_GUIDELINES.md` into the app's stylesheet.
3. Add your app to the table in [`README.md`](README.md).
4. Read [`DEV_NOTES.md`](DEV_NOTES.md) before agent-assisted edits on Windows.
