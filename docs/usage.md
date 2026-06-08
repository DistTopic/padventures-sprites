# Usage recipes

Practical examples for locating, loading, and slicing assets from this
collection. All paths are relative to the repository root.

## Resolve an asset by ID (Python)

```python
import json
from pathlib import Path

root  = Path(".")
index = json.loads((root / "data" / "index.json").read_text())
meta  = json.loads((root / "data" / "metadata.json").read_text())["things"]

def resolve(asset_id: int):
    category = next(c for c, ids in index.items() if asset_id in ids)
    path = root / "sprites" / category / f"{asset_id}.png"
    geometry = meta[str(asset_id)]["g"]
    return category, path, geometry

print(resolve(40036))
```

## Build a fast ID → category lookup

`index.json` is category-first. Invert it once for O(1) lookups:

```python
id_to_category = {aid: cat for cat, ids in index.items() for aid in ids}
category = id_to_category[40036]
```

## Slice a sheet into frames (Python + Pillow)

```python
from PIL import Image

def slice_sheet(path, geometry, cell=32):
    sheet = Image.open(path).convert("RGBA")
    cols  = sheet.width  // cell
    rows  = sheet.height // cell
    frames = []
    for r in range(rows):
        for c in range(cols):
            box = (c * cell, r * cell, (c + 1) * cell, (r + 1) * cell)
            frames.append(sheet.crop(box))
    return frames[: geometry["nSprites"]]
```

> Adjust `cell` to the base sprite size your sheets use; scale by `w`/`h` for
> multi-tile objects.

## Load in JavaScript (browser / Node)

```js
const index = await fetch("data/index.json").then(r => r.json());
const meta  = await fetch("data/metadata.json").then(r => r.json());

function resolve(assetId) {
  const category = Object.keys(index).find(c => index[c].includes(assetId));
  return {
    category,
    src: `sprites/${category}/${assetId}.png`,
    geometry: meta.things[String(assetId)].g,
  };
}
```

## Load in Phaser 3

```js
// In preload():
this.load.spritesheet("creature-40036", "sprites/creature/40036.png", {
  frameWidth: 32,
  frameHeight: 32,
});

// In create(), build an animation from the phases:
this.anims.create({
  key: "idle-40036",
  frames: this.anims.generateFrameNumbers("creature-40036", { start: 0, end: phases - 1 }),
  frameRate: 8,
  repeat: -1,
});
```

## Attribution reminder

Whatever you build, remember the assets are under **CC BY 4.0** — include a
credit line (see [`../NOTICE`](../NOTICE) for ready-to-paste snippets).
