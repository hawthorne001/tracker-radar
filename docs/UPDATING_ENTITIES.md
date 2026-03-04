# Updating Entities

Guide for adding or modifying domain-to-entity mappings in Tracker Radar.

## Prerequisites

Clone the [tracker-radar-detector](https://github.com/duckduckgo/tracker-radar-detector) repository alongside this repo and install its dependencies:

```bash
git clone https://github.com/duckduckgo/tracker-radar-detector.git
cd tracker-radar-detector
npm install
```

Update the tracker-radar-detector `config.json` so that `trackerRadarRepoPath` points to your local clone of this repository.

## Adding a Domain to an Existing Entity

### 1. Edit `entity_map.json`

In [`build-data/generated/entity_map.json`](/build-data/generated/entity_map.json), find the target entity and add the new domain to its `properties` array in alphabetical order:

```json
{
    "Google LLC": {
        "aliases": ["Alooma", "Alphabet Inc.", "Google Inc.", "Google LLC"],
        "properties": [
            "google.com",
            "googlenestcommunity.com",
            "youtube.com"
        ],
        "displayName": "Google"
    }
}
```

### 2. Apply the Changes

From the tracker-radar-detector directory, run:

```bash
npm run apply-entity-changes
```

This generates updates to the following files in this repo:

| File | What changes |
|---|---|
| `build-data/generated/entity_map.json` | Source of truth you edited |
| `build-data/generated/domain_map.json` | Reverse mapping (domain → entity with aliases and displayName) |
| `entities/<Entity Name>.json` | Entity file gets the new domain in its `properties` array |

### 3. Verify

Check that all three files were updated correctly:

- **`entity_map.json`** — new domain appears in the entity's `properties`
- **`domain_map.json`** — new entry keyed by domain with correct `entityName`, `aliases`, and `displayName`
- **`entities/<Entity Name>.json`** — new domain in `properties` array, alphabetically sorted

## File Formats

### `entity_map.json` (entity → domains)

```json
{
    "Entity Name": {
        "aliases": ["Alias 1", "Alias 2"],
        "properties": ["domain1.com", "domain2.com"],
        "displayName": "Short Name"
    }
}
```

### `domain_map.json` (domain → entity)

```json
{
    "domain1.com": {
        "entityName": "Entity Name",
        "aliases": ["Alias 1", "Alias 2"],
        "displayName": "Short Name"
    }
}
```

### `entities/<Entity Name>.json`

```json
{
    "name": "Entity Name",
    "displayName": "Short Name",
    "properties": ["domain1.com", "domain2.com"],
    "prevalence": { "tracking": 0.05, "nonTracking": 0.01, "total": 0.06 }
}
```
