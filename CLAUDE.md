# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

SEPA TeX — LaTeX templates for UNIFACS SEPA academic papers. All documents conform to ABNT NBR 6022:2018 and use the `abntex2` document class.

## Build

No automated build system. Compile from inside a paper's subdirectory:

```bash
pdflatex document.tex
bibtex document
pdflatex document.tex
pdflatex document.tex
```

Three `pdflatex` passes are required: first generates `.aux`, second resolves citations after `bibtex`, third resolves cross-references.

## Structure

Each top-level directory is a paper. Inside, `article/` holds the article variant and `project/` (when present) holds a project-report variant.

Key files per paper:

- `document.tex` — entry point, contains all content
- `abntex2.tex` — package imports and abntex2 config, loaded via `\input{abntex2}`
- `references.bib` — BibTeX bibliography

The `project/` variant may include local `abntex2.cls` and `.sty` overrides.

## Conventions

- All files are UTF-8.
- Primary language is Portuguese; English is secondary via `babel`.
- Font: Times New Roman (`\usepackage{times}`).
- Citation style: ABNT alphabetical or numeric via `abntex2cite`.
- Indentation: two spaces for nested LaTeX environments.
- Commit prefixes follow [Git Flow conventions](https://github.com/rios0rios0/guide/wiki/Life-Cycle/Git-Flow): `feat/`, `fix/`, `chore/`.
