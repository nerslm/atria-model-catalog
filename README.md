# atria-model-catalog

Public, capability-only model metadata for [atria-code](https://github.com/nerslm/atria-code).

Clients fetch provider shards from:

```text
api/models/providers/<provider-id>.json
```

The catalog contains model identifiers, display names, modalities, reasoning metadata, pricing, combined context capacities, and per-request output limits. It does not control provider endpoints, headers, credentials, compatibility behavior, or API implementations; those remain local to atria-code.

## Refresh policy

- Generated from atria-code's model generator and curated overrides.
- atria clients cache provider shards for four hours and fall back to the cache or built-in catalog offline.
- `modelCatalogUrl: false` disables remote refresh.

## Layout

- `api/models/catalog.json` — publication metadata
- `api/models/providers.json` — sorted provider IDs
- `api/models/providers/*.json` — per-provider model maps
- `api/models/models.json` — complete catalog

## Updating

Generate from the atria-code repository:

```bash
npm run generate:model-catalog
npm run check:model-catalog
```

Then copy `.artifacts/model-catalog` into `api/models`, review the diff, and publish.
