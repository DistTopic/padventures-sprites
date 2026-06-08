# Data format reference

This collection is more than a folder of images: three JSON files in `data/`
turn it into a queryable asset library. This document describes each one.

## Overview

| File             | Shape                         | Purpose                                   |
| ---------------- | ----------------------------- | ----------------------------------------- |
| `index.json`     | `category → [id, id, ...]`    | Groups every asset ID by category.        |
| `metadata.json`  | `things → { id → entry }`     | Per-asset sprite-sheet geometry.          |
| `names.json`     | `id → name`                   | Optional human-readable names (empty by default). |

Across all files, an **asset ID** is a globally unique non-negative integer.
The four categories are `creature`, `item`, `effect`, and `missile`.

## `index.json`

Maps each category to the sorted list of asset IDs it contains.

```json
{
  "item":     [0, 1, 2, 3, ...],
  "creature": [40036, 40037, ...],
  "effect":   [48905, 48906, ...],
  "missile":  [51387, 51388, ...]
}
```

For any ID listed here, the sprite sheet is at `sprites/<category>/<id>.png`.

## `metadata.json`

A single top-level `things` object maps each asset ID (as a string key) to an
entry describing its category and sprite-sheet geometry.

```json
{
  "things": {
    "0": {
      "cat": "item",
      "g": {
        "type": 0,
        "w": 1,
        "h": 1,
        "layers": 1,
        "px": 4,
        "py": 4,
        "pz": 1,
        "phases": 1,
        "realSize": null,
        "animator": null,
        "nSprites": 16
      }
    }
  }
}
```

### Entry fields

| Field | Type             | Meaning                                                        |
| ----- | ---------------- | -------------------------------------------------------------- |
| `cat` | string           | Category: `creature`, `item`, `effect`, or `missile`.          |
| `g`   | object           | Geometry block (below).                                        |

### Geometry (`g`) fields

| Field      | Type            | Meaning                                                              |
| ---------- | --------------- | ------------------------------------------------------------------- |
| `type`     | integer         | Internal asset type discriminator.                                  |
| `w`        | integer         | Width of the object in tiles.                                       |
| `h`        | integer         | Height of the object in tiles.                                      |
| `layers`   | integer         | Number of draw layers per frame.                                    |
| `px`       | integer         | Pattern count along the X axis.                                     |
| `py`       | integer         | Pattern count along the Y axis.                                     |
| `pz`       | integer         | Pattern count along the Z axis.                                     |
| `phases`   | integer         | Number of animation phases (frames in an animation cycle).          |
| `realSize` | integer or null | Real pixel size hint when present.                                  |
| `animator` | object or null  | Animation timing descriptor when present, otherwise `null`.         |
| `nSprites` | integer         | Total number of individual sprites packed into the sheet.           |

The number of sprites satisfies, in general:

```text
nSprites = w * h * layers * px * py * pz * phases
```

Use these values to slice the PNG into individual frames — see
[`sprite-sheets.md`](./sprite-sheets.md).

## `names.json`

An optional `id → name` map for human-friendly labels. It ships empty:

```json
{}
```

Contributors may extend it, for example:

```json
{ "40036": "Bulbasaur", "51387": "Ember" }
```

Names are advisory metadata only; the canonical identifier is always the integer
ID.

## Consistency contract

The collection is considered **consistent** when, for every ID `n`:

1. `n` appears under exactly one category in `index.json`.
2. `sprites/<category>/n.png` exists.
3. `metadata.json.things["n"]` exists and its `cat` matches the category from
   step 1.

`names.json` keys are optional and need not cover every ID.
