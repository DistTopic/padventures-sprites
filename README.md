<div align="center">

# PAdventures Sprites

**An open, attribution-licensed sprite-sheet library for the PAdventures universe — creatures, items, effects, and missiles, ready to drop into 2D games, tools, and prototypes.**

[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-blue.svg)](./LICENSE)
[![Sprite sheets](https://img.shields.io/badge/sprite%20sheets-50%2C854-brightgreen.svg)](#-whats-inside)
[![Categories](https://img.shields.io/badge/categories-4-orange.svg)](#-whats-inside)
[![Format: PNG](https://img.shields.io/badge/format-PNG-lightgrey.svg)](#-using-the-assets)
[![Conventional Commits](https://img.shields.io/badge/Conventional%20Commits-1.0.0-fe5196.svg)](https://www.conventionalcommits.org)
[![PRs welcome](https://img.shields.io/badge/PRs-welcome-success.svg)](./CONTRIBUTING.md)

</div>

---

> [!IMPORTANT]
> **Unofficial, non-commercial fan project.** Pokémon and all related names,
> characters, and imagery are © and ™ of **Nintendo**, **GAME FREAK inc.**,
> **Creatures Inc.**, and **The Pokémon Company**. This project is not affiliated
> with, endorsed, or sponsored by them, and is **not for commercial use**.
> The project license (**CC BY-NC-SA 4.0**) covers only the original
> contributions in this repository — never the underlying Pokémon intellectual
> property. Full details in [`DISCLAIMER.md`](./DISCLAIMER.md).

**PAdventures Sprites** is a curated, openly licensed collection of **50,854** sprite sheets covering the creatures, items, effects, and missiles of the PAdventures universe. Every sheet ships as a transparent PNG and is paired with a machine-readable catalog (`index.json`) and per-asset geometry metadata (`metadata.json`), so you can index, slice, and render the artwork programmatically.

Use it freely in your own **non-commercial** games, mods, tools, tutorials, and prototypes — **all we ask is that you give credit and keep it non-commercial** (see [Attribution](#-how-to-give-credit-attribution)).

## Table of contents

- [Highlights](#-highlights)
- [What's inside](#-whats-inside)
- [Repository layout](#-repository-layout)
- [Quick start](#-quick-start)
- [Data &amp; metadata](#-data--metadata)
- [Using the assets](#-using-the-assets)
- [How to give credit (Attribution)](#-how-to-give-credit-attribution)
- [Branching model &amp; contributing](#-branching-model--contributing)
- [License](#-license)
- [Intellectual property, credits &amp; fan-content notice](#-intellectual-property-credits--fan-content-notice)
- [Acknowledgements](#-acknowledgements)

## ✨ Highlights

- **50,854 ready-to-use sprite sheets** as transparent PNGs.
- **Four clean categories** — `creature`, `item`, `effect`, and `missile`.
- **Machine-readable catalog** — `data/index.json` groups every asset by category; `data/metadata.json` describes each sheet's geometry (tiles, layers, patterns, animation phases, sprite count).
- **Stable, predictable naming** — every asset has a globally unique numeric ID, and its sheet lives at `sprites/<category>/<id>.png`.
- **Openly licensed for non-commercial use** — released under **[CC BY-NC-SA 4.0](./LICENSE)**: free to use and adapt for non-commercial projects, with credit; derivatives stay open under the same license.
- **Inner-source ready** — documented contribution workflow, protected `main`/`develop` branches, Conventional Commits, issue/PR templates, and a code of conduct.

## 📦 What's inside

| Category   | Sheets   | Description                                                        |
| ---------- | -------: | ----------------------------------------------------------------- |
| `item`     | 39,827   | World objects, ground tiles, decorations, equipment, and props.   |
| `creature` |  8,256   | Monsters, NPCs, and other animated beings.                        |
| `effect`   |  2,480   | Spell, impact, and environmental visual effects.                  |
| `missile`  |    291   | Projectiles and thrown/launched objects.                          |
| **Total**  | **50,854** | Every asset cataloged in `data/index.json`.                     |

> Each row above matches exactly the IDs listed under the corresponding key in [`data/index.json`](./data/index.json).

## 🗂 Repository layout

```text
padventures-sprites/
├── sprites/                 # The sprite sheets, one PNG per asset ID
│   ├── creature/            #   8,256 sheets  →  <id>.png
│   ├── effect/              #   2,480 sheets  →  <id>.png
│   ├── item/                #  39,827 sheets  →  <id>.png
│   └── missile/             #     291 sheets  →  <id>.png
├── data/                    # Machine-readable catalog & geometry
│   ├── index.json           #   category → list of asset IDs
│   ├── metadata.json        #   asset ID → sprite geometry
│   └── names.json           #   asset ID → human name (optional, extensible)
├── docs/                    # Deep-dive documentation
│   ├── data-format.md       #   JSON schema reference
│   ├── sprite-sheets.md     #   PNG sheet anatomy & slicing
│   └── usage.md             #   Loading & rendering recipes
├── CONTRIBUTING.md          # Branching model & contribution guide
├── CODE_OF_CONDUCT.md
├── SECURITY.md
├── CHANGELOG.md
├── CITATION.cff             # How to cite this dataset
├── NOTICE                   # Attribution & IP notice
└── LICENSE                  # CC BY 4.0
```

## 🚀 Quick start

Clone the repository (it is a plain Git repository — no Git LFS required):

```bash
git clone https://github.com/DistTopic/padventures-sprites.git
cd padventures-sprites
```

Resolve any asset by ID. For example, in Python:

```python
import json
from pathlib import Path

root = Path("padventures-sprites")
index = json.loads((root / "data" / "index.json").read_text())
meta  = json.loads((root / "data" / "metadata.json").read_text())["things"]

# Find the category an asset belongs to, then build its file path.
asset_id = 40036                      # a creature
category = next(c for c, ids in index.items() if asset_id in ids)
sheet    = root / "sprites" / category / f"{asset_id}.png"
geometry = meta[str(asset_id)]["g"]   # tiles, layers, patterns, phases, nSprites

print(category, sheet, geometry)
```

See [`docs/usage.md`](./docs/usage.md) for slicing animation frames and rendering recipes.

## 🧾 Data &amp; metadata

Three JSON files turn a folder of PNGs into a queryable asset library:

- **`data/index.json`** — maps each category to the list of asset IDs it contains:
  ```json
  { "item": [0, 1, 2, ...], "creature": [40036, ...], "effect": [...], "missile": [...] }
  ```
- **`data/metadata.json`** — maps every asset ID to its sprite-sheet geometry:
  ```json
  { "things": { "0": { "cat": "item", "g": { "w": 1, "h": 1, "layers": 1,
    "px": 4, "py": 4, "pz": 1, "phases": 1, "nSprites": 16 } } } }
  ```
- **`data/names.json`** — an optional `id → name` map for human-readable labels. It ships empty (`{}`) and can be extended by the community.

The full field-by-field schema lives in [`docs/data-format.md`](./docs/data-format.md).

## 🎨 Using the assets

- Every sheet is a **32-bit RGBA PNG** with transparency.
- A sheet may pack multiple frames (`nSprites`) arranged by animation **phases** and **patterns** (`px`, `py`, `pz`). Slice it using the geometry from `metadata.json`.
- The category folder plus the ID is all you need to locate a file: `sprites/<category>/<id>.png`.

Detailed loading, slicing, and rendering walkthroughs are in [`docs/sprite-sheets.md`](./docs/sprite-sheets.md) and [`docs/usage.md`](./docs/usage.md).

## 📝 How to give credit (Attribution)

This collection is licensed under **CC BY-NC-SA 4.0**, which means you can share and adapt it for **non-commercial** purposes **as long as you give appropriate credit and license your derivatives under the same terms**.

A simple, sufficient attribution:

> "PAdventures Sprites" by **DistTopic**, licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/). Source: https://github.com/DistTopic/padventures-sprites

In a game's credits screen, a `CREDITS.txt`, or an "About" dialog is perfect. See [`NOTICE`](./NOTICE) for ready-to-paste snippets.

## 🌱 Branching model &amp; contributing

This repository follows an **inner-source** workflow with two long-lived, protected branches:

- **`main`** — stable, released assets. Protected: no direct pushes, no force-pushes, no deletions.
- **`develop`** — integration branch for upcoming changes. Protected the same way.

All changes flow through pull requests using [Conventional Commits](https://www.conventionalcommits.org). Read [`CONTRIBUTING.md`](./CONTRIBUTING.md) before opening a PR, and please follow our [Code of Conduct](./CODE_OF_CONDUCT.md).

## ⚖️ License

Released under the **[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)](./LICENSE)** license.

You are free to **share** (copy and redistribute) and **adapt** (remix, transform, build upon) the material, provided that you:

- **Attribution** — give appropriate credit, link to the license, and indicate if changes were made.
- **NonCommercial** — do not use the material for commercial purposes.
- **ShareAlike** — distribute your contributions under the same license as the original.

> **Scope:** this license applies **only to the original contributions** in this repository (curation, organization, catalog, metadata, documentation, and any original artwork). It does **not** grant any rights over the underlying Pokémon intellectual property. See [`DISCLAIMER.md`](./DISCLAIMER.md).

## 🛡 Intellectual property, credits &amp; fan-content notice

**PAdventures Sprites is an unofficial, non-commercial, fan-made project.** It is
not affiliated with, endorsed, sponsored, or approved by any of the rights
holders named below.

Pokémon and all related names, characters, designs, sprites, and imagery are the
property of their respective owners:

- **Nintendo Co., Ltd.**
- **GAME FREAK inc.**
- **Creatures Inc.**
- **The Pokémon Company** (株式会社ポケモン)

> Pokémon © 1995–2026 Nintendo / Creatures Inc. / GAME FREAK inc. "Pokémon",
> "Pocket Monsters", and the names, characters, and designs of all Pokémon are
> trademarks and/or registered trademarks of Nintendo, GAME FREAK inc.,
> Creatures Inc., and The Pokémon Company.

Any other third-party brands or trademarks that may be referenced or evoked
remain the property of their respective owners. The maintainers claim **no
ownership** over any third-party intellectual property.

**Rights holders:** if you believe any content here infringes your rights, please
open an [issue](https://github.com/DistTopic/padventures-sprites/issues) or
contact the maintainers and we will **promptly remove** the material in question.

Full details, credits, and the notice &amp; takedown policy are in
[`DISCLAIMER.md`](./DISCLAIMER.md) and [`NOTICE`](./NOTICE).

## 🙏 Acknowledgements

Built and maintained by the **[DistTopic](https://github.com/DistTopic)** organization and the PAdventures community. Thank you to everyone who contributes sheets, fixes, names, and documentation.
