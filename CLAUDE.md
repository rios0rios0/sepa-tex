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

- `document.tex` — self-contained entry point: declares its own `\documentclass{abntex2}`, inlines every `\usepackage` and config, and pulls the bibliography via `\bibliography{references}`. There is no shared config include.
- `references.bib` — BibTeX bibliography (cited as `references`, without extension)
- `abntex2.tex`, `abntex2cite.tex`, `abntex2cite-alf.tex` — bundled copies of the upstream abnTeX2 manual/example sources (each is its own `ltxdoc` document). They are **not** loaded by `document.tex`; keep them for reference only.

The `project/` variant additionally bundles the abnTeX2 class and style sources locally (`abntex2.cls`, `abntex2cite.sty`, `.bst` files) so it compiles even where those are not installed in the TeX distribution.

## Conventions

- All files are UTF-8.
- Primary language is Portuguese; English is secondary via `babel`.
- Font: Times New Roman (`\usepackage{times}`).
- Citation style: ABNT alphabetical or numeric via `abntex2cite`.
- Indentation: two spaces for nested LaTeX environments.
- Commit prefixes follow [Git Flow conventions](https://github.com/rios0rios0/guide/wiki/Life-Cycle/Git-Flow): `feat/`, `fix/`, `chore/`.
