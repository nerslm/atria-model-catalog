# atria-model-catalog

Public, capability-only model metadata for [atria-code](https://github.com/nerslm/atria-code).

Clients fetch provider shards from:

```text
api/models/providers/<provider-id>.json
```

Provider shards contain model identifiers, display names, modalities, reasoning metadata, pricing, combined context capacities, and per-request output limits. A separate declarative provider file supplies API protocol IDs, HTTPS endpoints, non-sensitive static headers, and compatibility metadata for automatically discovered providers. API implementations remain local to atria-code; the catalog contains no executable code.

## Refresh policy

- Generated every four hours from npm's current `@earendil-works/pi-ai@latest`, atria-code's live provider sources, and curated overrides.
- The upstream tarball is SHA-512 verified before parsing, and `api/models/upstream.json` records the exact version and integrity digest used for each publication.
- Public model shards contain capability and pricing metadata only. `provider-definitions.json` lets clients automatically register new providers when their API protocol is already implemented locally.
- atria clients cache provider and model metadata for four hours and fall back to the cache or built-in catalog offline.
- `modelCatalogUrl: false` disables remote refresh.

## Layout

- `api/models/catalog.json` — publication metadata
- `api/models/upstream.json` — exact pi-ai package source and integrity metadata
- `api/models/providers.json` — sorted provider IDs
- `api/models/provider-definitions.json` — declarative provider/API transport definitions
- `api/models/providers/*.json` — per-provider capability maps
- `api/models/models.json` — complete catalog

## Updating

This repository's `Update public model catalog` workflow checks out the private atria-code source with the `ATRIA_CODE_TOKEN` Actions secret, then generates, validates, and commits the latest catalog every four hours. Catalog writes use the repository's built-in `GITHUB_TOKEN`.

To reproduce it manually from atria-code:

```bash
npm run generate:model-catalog
npm run check:model-catalog
node scripts/export-public-model-catalog.mjs \
  --input .artifacts/model-catalog \
  --output ../atria-model-catalog/api/models
```
