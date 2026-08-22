# atria-model-catalog

Public, capability-only model metadata for [atria-code](https://github.com/nerslm/atria-code).

Clients fetch provider shards from:

```text
api/models/providers/<provider-id>.json
```

The catalog contains model identifiers, display names, modalities, reasoning metadata, pricing, combined context capacities, and per-request output limits. It does not control provider endpoints, headers, credentials, compatibility behavior, or API implementations; those remain local to atria-code.

## Refresh policy

- Generated every four hours from npm's current `@earendil-works/pi-ai@latest`, atria-code's live provider sources, and curated overrides.
- The upstream tarball is SHA-512 verified before parsing, and `api/models/upstream.json` records the exact version and integrity digest used for each publication.
- Public shards contain capability and pricing metadata only; endpoints, headers, credentials, compatibility behavior, and API implementations remain local to atria-code.
- atria clients cache provider shards for four hours and fall back to the cache or built-in catalog offline.
- `modelCatalogUrl: false` disables remote refresh.

## Layout

- `api/models/catalog.json` — publication metadata
- `api/models/upstream.json` — exact pi-ai package source and integrity metadata
- `api/models/providers.json` — sorted provider IDs
- `api/models/providers/*.json` — per-provider model maps
- `api/models/models.json` — complete catalog

## Updating

The `Publish latest public model catalog` workflow in atria-code generates, validates, and pushes this repository automatically every four hours. Its cross-repository token is stored as the `MODEL_CATALOG_TOKEN` Actions secret.

To reproduce it manually from atria-code:

```bash
npm run generate:model-catalog
npm run check:model-catalog
node scripts/export-public-model-catalog.mjs \
  --input .artifacts/model-catalog \
  --output ../atria-model-catalog/api/models
```
