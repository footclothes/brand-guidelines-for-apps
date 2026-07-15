# brand_guidelines_for_apps

Shared FootClothes / Dashboard World UI tokens and developer notes for internal
desktop and web apps. **Not** a runtime package — apps copy tokens into their own
`globals.css` / `tailwind.config` and follow the rules here.

## Contents

| File | Purpose |
| --- | --- |
| [`BRAND_GUIDELINES.md`](BRAND_GUIDELINES.md) | Colors, typography, components, layout rules |
| [`DEV_NOTES.md`](DEV_NOTES.md) | Windows UTF-8 encoding gotcha (critical for agents) |

Source of truth for token *values*: `dashboardworld` repo (`globals.css`, `tailwind.config.ts`).

## Apps that use this repo

Clone this repo as a **sibling** next to each app under `~/GitHub/`:

| App repo | How it links |
| --- | --- |
| [`Image_Naming_2`](../Image_Naming_2/) | Inlined tokens in `src/globals.css`; docs point here |
| [`header_card_project`](../header_card_project/) | PyQt stylesheet follows same palette; see its `app/ui/styles.py` |
| [`illustrator-import-to-template`](../illustrator-import-to-template/) | PyQt app; mirrors tokens in `app/ui/styles.py`. Reference implementation of the §4.1 disabled-button, §4.2 image cell, and §4.3 settings gear/drawer patterns |
| [`dashboardworld`](../dashboardworld/) | Canonical token source |

See [`REPO_LAYOUT.md`](REPO_LAYOUT.md) for the full `~/GitHub/` folder map.

## Keeping guidelines updated

When Dashboard World tokens change:

1. Update `dashboardworld/src/app/globals.css` and `tailwind.config.ts` first.
2. Update `BRAND_GUIDELINES.md` in this repo.
3. Refresh inlined tokens in consuming apps (`Image_Naming_2`, `header_card_project`, etc.).
