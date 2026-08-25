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

<!-- chlog:start -->
## Changelog (chlog) — MANDATORY

If the repository you are working in uses chlog (a `.chlog.yaml` or `.chlog.yml`
config file, or a `.changes/` directory, exists at the project root), the
following is binding and ALWAYS applies: whenever you make ANY change, you MUST
create a changelog fragment as part of the same change — automatically, without
being asked, before committing.

- Do NOT edit CHANGELOG.md directly; it is generated from fragments.
- Create the fragment with:
  `chlog new --kind <Kind> --body "<imperative description>"`
- Valid kinds: Added, Changed, Deprecated, Removed, Fixed, Security
- Choose the kind that best matches the change (e.g., new feature → Added,
  bug fix → Fixed, behavior change → Changed, removal → Removed, security fix → Security).
- If the change is backward-INCOMPATIBLE with the public API (a breaking
  change), you MUST add the `--breaking` flag:
  `chlog new --kind <Kind> --breaking --body "<description>"`.
  This is the ONLY thing that triggers a major version bump — the kind alone
  never does (per SemVer, major = incompatible change). When unsure whether a
  change breaks compatibility, ask the user instead of guessing.
- Fragments are YAML files in `.changes/unreleased/`; stage them with your commit.
- `chlog check` fails the build when a fragment is missing — never skip it.
<!-- chlog:end -->
