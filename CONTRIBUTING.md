# Contributing to PAdventures Sprites

Thanks for your interest in improving this collection! This project follows an
**inner-source** model: it is developed in the open with the same rigor we would
apply to any production repository. Anyone is welcome to contribute new sprite
sheets, fixes, metadata, names, or documentation.

By contributing, you agree that your contributions will be licensed under the
project's [CC BY 4.0](./LICENSE) license, and you confirm you have the right to
contribute the material.

## Table of contents

- [Code of conduct](#code-of-conduct)
- [Branching model](#branching-model)
- [Commit conventions](#commit-conventions)
- [Pull request workflow](#pull-request-workflow)
- [Adding or updating sprites](#adding-or-updating-sprites)
- [Keeping the catalog consistent](#keeping-the-catalog-consistent)
- [Reporting issues](#reporting-issues)

## Code of conduct

All participation is governed by our [Code of Conduct](./CODE_OF_CONDUCT.md).
Please read it before engaging.

## Branching model

We use a two-branch, GitHub-Flow-inspired model. **Both branches are protected**
— direct pushes, force-pushes, and deletions are disabled; all changes land via
reviewed pull requests.

| Branch     | Purpose                                  | Protected |
| ---------- | ---------------------------------------- | :-------: |
| `main`     | Stable, released assets.                 | ✅        |
| `develop`  | Integration branch for upcoming changes. | ✅        |

Typical flow:

```text
feature branch  ──PR──▶  develop  ──PR──▶  main
```

1. Branch off `develop` using a descriptive name, e.g.
   `feat/add-water-creatures` or `fix/missile-51390-transparency`.
2. Open a pull request **into `develop`**.
3. Once reviewed and merged, changes are promoted from `develop` to `main` via a
   release pull request.

## Commit conventions

This project uses **[Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)**.
Every commit message must follow:

```text
<type>(<optional scope>): <short, imperative summary>

<optional body>

<optional footer>
```

Common types in this repository:

| Type       | Use it for                                                       |
| ---------- | ---------------------------------------------------------------- |
| `feat`     | Adding new sprite sheets or new catalog data.                    |
| `fix`      | Correcting a broken sheet, wrong metadata, or mislabeled asset.  |
| `docs`     | Documentation-only changes.                                      |
| `chore`    | Repository scaffolding, tooling, and housekeeping.               |
| `refactor` | Reorganizing assets or data without changing their meaning.      |
| `style`    | Formatting/optimization that does not change the artwork's pixels. |

Recommended scopes: `creature`, `item`, `effect`, `missile`, `sprites`, `data`,
`docs`, `repo`.

Examples:

```text
feat(creature): add 12 grass-type evolution sheets
fix(metadata): correct phase count for effect 49731
docs(usage): add Phaser 3 loading example
chore(repo): add issue and pull request templates
```

## Pull request workflow

1. Keep PRs focused — one logical change per PR.
2. Fill out the pull request template completely.
3. Ensure new assets follow the [naming and placement rules](#adding-or-updating-sprites).
4. Update `data/index.json` and `data/metadata.json` when adding or removing assets.
5. At least one approving review is required before merging.
6. Prefer a clean, linear history; squash trivial fixups before requesting review.

## Adding or updating sprites

- **Format:** transparent 32-bit RGBA **PNG**.
- **Location:** `sprites/<category>/<id>.png`, where `<category>` is one of
  `creature`, `item`, `effect`, or `missile`.
- **ID:** every asset has a **globally unique** integer ID across all categories.
  When adding a new asset, choose the next free ID; never reuse an existing one.
- **No Git LFS:** commit PNGs directly. Keep individual files well under GitHub's
  100 MB limit (existing sheets are a few MB at most).

## Keeping the catalog consistent

Whenever you add, remove, or move an asset, update the catalog so it stays in
sync with the files on disk:

- `data/index.json` — add/remove the ID under the correct category key.
- `data/metadata.json` — add/remove the matching `things["<id>"]` entry.
- `data/names.json` — optionally add a human-readable name for the ID.

A change is "consistent" when, for every ID in `index.json`, both the PNG file
and a `metadata.json` entry exist (and vice versa). See
[`docs/data-format.md`](./docs/data-format.md) for the exact schema.

## Reporting issues

Found a broken sheet, wrong metadata, or a missing asset? Open an issue using one
of our [issue templates](https://github.com/DistTopic/padventures-sprites/issues/new/choose).
Please include the asset ID and category whenever possible.
