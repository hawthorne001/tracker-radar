---
name: update-entities
description: How to add or remove domains from an entity (company/organization) in Tracker Radar. Use when modifying domain-to-entity mappings.
---

# How to Update Entities

## Prerequisites

1. Clone the [tracker-radar-detector](https://github.com/duckduckgo/tracker-radar-detector) repo alongside this one and run `npm install`.
2. Update the tracker-radar-detector `config.json` so `trackerRadarRepoPath` points to your local clone of this repo (tracker-radar).

## Steps

### 1. Edit `build-data/generated/entity_map.json`

Find the target entity and add the new domain to its `properties` array in **alphabetical order**.

### 2. Run `npm run apply-entity-changes` in the tracker-radar-detector repo

This propagates changes to all dependent files:

| File | What changes |
|---|---|
| `build-data/generated/entity_map.json` | The source of truth you edited |
| `build-data/generated/domain_map.json` | Reverse mapping — one entry per domain with `entityName`, `aliases`, `displayName` |
| `entities/<Entity Name>.json` | The entity file gets the domain added to its `properties` array |

### 3. Verify

- **`entity_map.json`** — domain in the entity's `properties`, alphabetically sorted
- **`domain_map.json`** — new key for the domain with correct `entityName`, `aliases`, `displayName`
- **`entities/<Entity Name>.json`** — domain in `properties`, alphabetically sorted
- All three files are valid JSON
- Run `npm test` to validate schemas
