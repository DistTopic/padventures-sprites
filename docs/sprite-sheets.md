# Sprite sheets

Each asset is stored as a single transparent PNG that may pack several
individual sprites. This document explains how a sheet is laid out and how to
slice it using the geometry from [`metadata.json`](./data-format.md).

## File anatomy

- **Format:** 32-bit RGBA PNG with an alpha channel.
- **Location:** `sprites/<category>/<id>.png`.
- **Tile unit:** sprites are authored on a tile grid. An object that is `w × h`
  tiles wide and tall occupies a `w × h` block of the base sprite cell.

## What a sheet contains

A single sheet may hold many frames. The total is `nSprites`, distributed across
these dimensions (all found in the asset's geometry block `g`):

- **Size** — `w` × `h` tiles per frame.
- **Layers** — `layers` stacked draw layers per frame (e.g. base + overlay).
- **Patterns** — `px` × `py` × `pz` variants (e.g. facing direction, color
  variant, or state).
- **Phases** — `phases` animation frames in a cycle.

In the general case:

```text
nSprites = w * h * layers * px * py * pz * phases
```

## Slicing strategy

1. Read the asset's geometry from `metadata.json.things["<id>"].g`.
2. Determine the base sprite cell size used by the sheet (commonly a fixed tile
   size such as 32×32 or 64×64 pixels, scaled by `w`/`h`).
3. Walk the sheet in row-major order, extracting `nSprites` cells.
4. Index the extracted cells by `(layer, px, py, pz, phase)` to address a
   specific variant/frame.

Because every count needed to reconstruct the layout lives in the metadata, you
can slice sheets generically without hardcoding per-asset rules.

## Practical tips

- **Animations:** iterate `phase` from `0` to `phases - 1` to play a cycle.
- **Directions:** many creatures encode facing as a pattern axis (`px`/`py`).
- **Static objects:** items with `phases = 1` and all pattern counts `= 1` are a
  single image — no slicing required.
- **Layers:** when `layers > 1`, composite the layers in order for the final
  frame.

See [`usage.md`](./usage.md) for concrete loading examples.
