# Copilot Instructions

## Project Overview

**SEPA TeX** is a collection of LaTeX document templates for academic papers submitted to SEPA (Seminário Estudantil de Produção Acadêmica) at UNIFACS university. All templates conform to **ABNT NBR 6022:2018** standards and are built on top of the [`abntex2`](https://github.com/abntex/abntex2) LaTeX class.

The repository contains multiple completed academic papers as reference implementations, each demonstrating the correct use of the template.

## Repository Structure

```
sepa-tex/
├── .github/
│   └── copilot-instructions.md        # This file
├── CONTRIBUTING.md                    # Contribution guidelines
├── README.md                          # Project overview
├── LICENSE                            # License information
├── .gitignore                         # Git ignore rules
│
├── MAIS - Multi Agent Intelligent System/
│   └── article/
│       ├── document.tex               # Self-contained content + config entry point
│       ├── abntex2.tex                # Bundled upstream abnTeX2 manual (not compiled)
│       ├── abntex2cite.tex            # Citation examples (numeric)
│       ├── abntex2cite-alf.tex        # Citation examples (alphabetical)
│       └── references.bib             # BibTeX bibliography database
│
├── Inteligência Artificial Distribuída e Automação Industrial/
│   └── article/
│       ├── document.tex
│       ├── abntex2.tex
│       ├── abntex2cite.tex
│       ├── abntex2cite-alf.tex
│       └── references.bib
│
└── Execução Especulativa - Limites da Exploração de Informações Sensíveis/
    ├── article/
    │   ├── document.tex
    │   ├── abntex2.tex
    │   ├── abntex2cite.tex
    │   ├── abntex2cite-alf.tex
    │   └── references.bib
    │   ├── images/                    # Figures embedded by document.tex
    │   └── listings/                  # C source listings + a local style.sty
    └── project/
        ├── document.tex
        ├── references.bib
        ├── abntex2.cls                # Local copy of the abnTeX2 class
        ├── abntex2cite.sty            # Local citation style sources
        ├── abntex2-{alf,num}.bst      # Local BibTeX styles
        └── abntex2cite-alf.tex
```

Each paper directory follows the same layout:
- **`document.tex`** — Self-contained entry point: declares its own `\documentclass{abntex2}`, inlines every `\usepackage` and configuration, and pulls the bibliography via `\bibliography{references}`. No shared config include.
- **`references.bib`** — BibTeX bibliography for the paper (cited as `references`, no extension).
- **`abntex2.tex`** / **`abntex2cite.tex`** / **`abntex2cite-alf.tex`** — Bundled copies of the upstream abnTeX2 manual/example sources (each is its own `ltxdoc` document). **Not** loaded by `document.tex` — kept for reference only.

The `Execução Especulativa…/article/` paper also embeds figures from `images/` and C listings from `listings/`. Its `project/` variant bundles the abnTeX2 class and style sources locally so it compiles where those are not installed.

## Technology Stack

- **LaTeX engine**: `pdflatex`
- **Document class**: `abntex2` (ABNT standards for academic papers)
- **Bibliography**: `bibtex`
- **Key LaTeX packages**: `times`, `fontenc`, `inputenc` (UTF-8), `babel` (Portuguese + English), `graphicx`, `abntex2cite`, `fancyhdr`, `nomencl`, `backref`, `lipsum`, `microtype`
- **Language**: Portuguese (primary), English (secondary via `babel`)

## Build Commands

There is no automated build system. Compile any paper manually from within its directory:

```bash
cd "<paper-directory>/article"   # or /project for project reports
pdflatex document.tex            # First pass (generates .aux)
bibtex document                  # Resolve bibliography references
pdflatex document.tex            # Second pass (resolves citations)
pdflatex document.tex            # Third pass (resolves cross-references)
```

After compilation the generated PDF will be `document.pdf` in the same directory.

> **Tip**: If you use VS Code with the [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop) extension, a recipe matching the four-step sequence above will compile the document automatically on save.

## Architecture and Design Patterns

- **Self-contained documents**: Each `document.tex` carries its own `\documentclass{abntex2}`, all `\usepackage` directives, and configuration inline — there is no shared config include. The sibling `abntex2*.tex` files are bundled upstream manuals, not part of the build.
- **Local class bundling**: The `project/` variant ships local copies of the abnTeX2 class and style sources (`abntex2.cls`, `abntex2cite.sty`, `.bst`) so it compiles standalone, even without those installed in the TeX distribution.
- **Modular bibliography**: Each paper keeps its own `references.bib` to avoid cross-contamination between papers.

## Document Class Options

All `document.tex` files use a configuration block similar to:

```latex
\documentclass[
    article,        % Academic article format
    12pt,           % Body font size
    oneside,        % Single-sided printing
    a4paper,        % A4 paper size
    chapter=TITLE,  % Chapter titles in uppercase
    section=TITLE,  % Section titles in uppercase
    english, brazil % Portuguese as main language, English secondary
]{abntex2}
```

## CI/CD Pipeline

A `release.yaml` workflow runs on push to `main` and delegates to a shared pipeline (`rios0rios0/pipelines`) to create GitHub releases. There is no build or test CI — all validation is performed locally before submitting a pull request.

## Development Workflow

1. Fork the repository and clone your fork locally.
2. Create a feature branch: `git checkout -b feat/my-change`
3. Navigate to the relevant paper directory and edit `document.tex` (or other files as needed).
4. Compile the document using the four-step build sequence above.
5. Review the generated `document.pdf` for correct formatting and citation rendering.
6. If bibliography entries were modified, update `references.bib` and recompile.
7. Commit following the [Git Flow conventions](https://github.com/rios0rios0/guide/wiki/Life-Cycle/Git-Flow).
8. Open a pull request targeting `main`.

## Coding Conventions

- **Encoding**: All `.tex` and `.bib` files use UTF-8 encoding (`\usepackage[utf8]{inputenc}`).
- **Font**: Times New Roman family (`\usepackage{times}`).
- **Citation style**: ABNT alphabetical or numeric via `abntex2cite`; choose one per paper and stay consistent.
- **Comments**: Use `%` inline comments sparingly; prefer self-documenting section/command names.
- **Indentation**: Two spaces for nested LaTeX environments is the convention used throughout existing files.
- **Branch naming**: Follow the external [Development Guide](https://github.com/rios0rios0/guide/wiki) (e.g., `feat/`, `fix/`, `chore/` prefixes).

## Common Tasks

### Adding a new paper

1. Create a new top-level directory named after the paper topic.
2. Copy the `article/` folder from an existing paper as a starting template.
3. Update `document.tex` with the new paper's metadata (author, title, abstract, etc.).
4. Replace the content sections and update `references.bib`.
5. Compile and verify the PDF output.

### Updating bibliography

Edit `references.bib` and add a new `@article`, `@book`, or other BibTeX entry:

```bibtex
@article{key2024,
  author  = {Surname, Firstname},
  title   = {Title of the Article},
  journal = {Journal Name},
  year    = {2024},
  volume  = {1},
  pages   = {1--10}
}
```

Then cite it in the document with `\cite{key2024}` and recompile.

### Switching citation style

- Numeric citations: `\usepackage[num]{abntex2cite}`
- Alphabetical citations: `\usepackage{abntex2cite}` (default)

## Troubleshooting

| Problem | Solution |
|---|---|
| Missing `abntex2` class error | Install `abntex2` via your TeX distribution package manager (`tlmgr install abntex2` for TeX Live) |
| Bibliography not appearing | Make sure `bibtex document` ran without errors and that at least one `\cite{}` command is used |
| Garbled characters in PDF | Verify the file is saved as UTF-8 and `\usepackage[utf8]{inputenc}` is present |
| Cross-references show `??` | Run `pdflatex` a second and third time to resolve forward references |
| Compilation fails on Windows | Avoid non-ASCII characters in directory names; use a path without accented letters |
