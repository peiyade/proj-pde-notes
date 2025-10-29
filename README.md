# PDE Reading Notes Workspace

This repository hosts a modular LaTeX setup for collecting PDE (partial differential equations) reading notes. Each note compiles on its own while also slotting into a master `ctexbook` volume for aggregated exports.

## Layout

```
.
├── main.tex              # Root book that assembles all notes
├── preamble.tex          # Shared packages, theorem styles, and color/link setup
├── note1/
│   ├── Tectonic.toml     # Pinned Tectonic bundle + Synctex toggle
│   ├── note1.bib         # Local bibliography for the note
│   └── src/
│       ├── note1.tex     # Subfiles chapter sharing the preamble
│       └── images/       # Figures dedicated to this note
└── note2/
    ├── Tectonic.toml
    ├── note2.bib
    └── src/
        ├── note2.tex
        └── images/
```

Add more notes by copying one of the existing directories and updating names accordingly. Keep assets (figures, tables, custom macros) inside the note folder to avoid path collisions.

## Building with Tectonic

Tectonic ensures repeatable builds with a locked bundle URL. Install it locally (macOS example) according to the [official docs](https://github.com/tectonic-typesetting/tectonic):

```bash
brew install tectonic
```

Then compile:

- Combined volume:

  ```bash
  tectonic main.tex
  ```

- Stand-alone note:

  ```bash
  cd note1
  tectonic
  ```

  The per-note `Tectonic.toml` targets a stable 2023 bundle and enables Synctex for editor sync.

To upgrade bundles run `tectonic -X new` in the note folder and replace the generated URL in `Tectonic.toml`.

## Citation Workflow

Each note manages its own `.bib` file and loads it via `\addbibresource{}` with `subfix` so the relative path survives both local and aggregated compilation. Bibliographies appear at the end of each chapter courtesy of `biblatex` with `refsection=chapter`.

Remember to rerun `tectonic` whenever bibliography entries change—Tectonic will automatically invoke bibtex if needed.

## Suggestions

- Configure editor tooling (VS Code LaTeX Workshop or Neovim plugins) to run `tectonic` on save.
- Add CI later (e.g., GitHub Actions) to rebuild `main.tex` and spot missing references or figures.
- Extend `preamble.tex` cautiously to keep per-note builds snappy; consider optional packages guarded by `\IfFileExists`.
