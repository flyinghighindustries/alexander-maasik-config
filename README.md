# alexander-maasik-config

Yext **configuration-as-code** for the Alexander Maasik personal site. Pattern matches the established `sendoplex-config` repo — Yext pulls this repo from GitHub, no CLI install required.

Companion repo: [`alexander-maasik-site`](https://github.com/flyinghighindustries/alexander-maasik-site) — the React/Tailwind app that reads the Knowledge Graph entity defined by these fields.

## Layout

```
alexander-maasik-config/
└── platform-config/
    └── c/
        └── km/
            ├── field/                          # 15 custom field definitions (JSON)
            │   ├── c_tagline.json
            │   ├── c_heroSubheadline.json
            │   ├── c_heroPortrait.json
            │   ├── c_aboutPhoto.json
            │   ├── c_aboutBody.json
            │   ├── c_serviceTitles.json
            │   ├── c_serviceBodies.json
            │   ├── c_serviceEmailSubjects.json
            │   ├── c_approachTitles.json
            │   ├── c_approachBodies.json
            │   ├── c_caseClients.json
            │   ├── c_caseResults.json
            │   ├── c_caseBodies.json
            │   ├── c_ctaHeadline.json
            │   └── c_ctaBody.json
            └── field-eligibility-group/
                └── location.default.json       # All 15 fields attached to Location
```

No `package.json`, no build files, **no other JSON anywhere** — this repo is Yext config only. Yext scans every JSON in the repo as a resource, so anything else (entity content, seed data, scripts) would fail with a `$schema` error. Initial entity content lives inline in [`RUNBOOK.md`](RUNBOOK.md) as `curl` heredocs instead.

## Field set (15 custom fields, all on `location` entity)

| Field                       | Type                  | Localized |
| --------------------------- | --------------------- | --------- |
| `c_tagline`                 | string · SIMPLE       | ✓         |
| `c_heroSubheadline`         | string · RICH_TEXT    | ✓         |
| `c_heroPortrait`            | image                 | static    |
| `c_aboutPhoto`              | image                 | static    |
| `c_aboutBody`               | string · RICH_TEXT    | ✓         |
| `c_serviceTitles`           | list<string · SIMPLE> | ✓         |
| `c_serviceBodies`           | list<string · RICH_TEXT> | ✓      |
| `c_serviceEmailSubjects`    | list<string · SIMPLE> | ✓         |
| `c_approachTitles`          | list<string · SIMPLE> | ✓         |
| `c_approachBodies`          | list<string · RICH_TEXT> | ✓      |
| `c_caseClients`             | list<string · SIMPLE> | ✓         |
| `c_caseResults`             | list<string · SIMPLE> | ✓         |
| `c_caseBodies`              | list<string · RICH_TEXT> | ✓      |
| `c_ctaHeadline`             | string · SIMPLE       | ✓         |
| `c_ctaBody`                 | string · RICH_TEXT    | ✓         |

Lists are used in place of structs (per Sendoplex pattern — structs aren't available in all Yext accounts). The site component zips parallel arrays at render time.

UI strings (nav labels, button text, section headings) are **not** here — they live in [`alexander-maasik-site/src/i18n/index.ts`](https://github.com/flyinghighindustries/alexander-maasik-site/blob/main/src/i18n/index.ts), matching the Sendoplex pattern.

## How to apply

See [`RUNBOOK.md`](RUNBOOK.md) for the exact step-by-step. Short version:

1. Push this repo to GitHub.
2. Yext → **Settings → Resources → Connected GitHub repo** → connect `flyinghighindustries/alexander-maasik-config` → **Repull → Apply**.
3. Create the location entity via Management API curl (uses `seed-content/create-entity.en.json`).
4. Populate Estonian profile via second curl (uses `seed-content/profile.et.json`).
