# PDE Reading Notes Workspace

This repository hosts a modular LaTeX setup for collecting PDE (partial differential equations) reading notes. Each note compiles independently while also contributing to a master `ctexbook` volume.

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

Add new notes by copying one of the directories, renaming the files, and updating the chapter metadata. Keep assets (figures, tables, custom macros) inside each note folder so paths remain local and conflict free.

## Building with Tectonic

Tectonic provides reproducible builds with locked TeX bundles. Install it locally—on macOS, for example:

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

Each note’s `Tectonic.toml` locks the 2023 bundle and enables Synctex for editor integration. To upgrade bundles, run `tectonic -X new` inside the note directory and replace the generated URL.

## Citation Workflow

- Every note owns a `.bib` file that is loaded with `\addbibresource` using `\subfix` so relative paths survive subfile compilation.
- `biblatex` is configured with `refsection=chapter`, letting each chapter print its bibliography via `\printbibliography`.
- Rerun `tectonic` after editing citations—Tectonic automatically triggers BibTeX when needed.

## Suggestions

- Configure editor tooling (VS Code LaTeX Workshop or Neovim plugins) to run `tectonic` on save.
- Add CI later (e.g., GitHub Actions) to rebuild `main.tex` and spot missing references or figures.
- Extend `preamble.tex` cautiously to keep per-note builds snappy; consider optional packages guarded by `\IfFileExists`.

## VS Code Workflow

1. Install the [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop) extension.
2. Create `.vscode/settings.json` containing:

   ```json
   {
     "latex-workshop.latex.tools": [
       {
         "name": "tectonic",
         "command": "tectonic",
         "args": [
           "--synctex",
           "%DOC%"
         ]
       }
     ],
     "latex-workshop.latex.recipes": [
       {
         "name": "tectonic",
         "tools": ["tectonic"]
       }
     ],
     "latex-workshop.latex.autoBuild.run": "onFileChange",
     "latex-workshop.view.pdf.viewer": "tab",
     "latex-workshop.synctex.afterBuild.enabled": true,
     "latex-workshop.latex.rootFile.useSubFile": true
   }
   ```

3. Open `main.tex` for the full book or a note file for focused work. Run *LaTeX Workshop: Build with recipe* and choose `tectonic`. The recipe respects the active file, so subfiles compile locally while `main.tex` yields the aggregated PDF.
4. When working inside a specific note folder, you can also invoke `tectonic` from the VS Code terminal. The pinned bundle in `Tectonic.toml` keeps builds deterministic; regenerate the bundle URL with `tectonic -X new` when you want newer TeX assets.
