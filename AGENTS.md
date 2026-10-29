# AGENTS.md

This file provides guidance to AI coding agents (Claude Code, Codex, Cursor, GitHub Copilot) when working with code in this repository.

## Project

SEPA TeX — LaTeX templates for UNIFACS SEPA academic papers, built on the `abntex2` document class. The `article/` variants follow ABNT NBR 6022:2018; the `project/` variant (research project) follows ABNT NBR 15287:2011.

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

Each top-level directory is a paper. Inside, `article/` holds the article variant and `project/` (when present) holds a research-project variant.

Key files per paper:

- `document.tex` — self-contained entry point: declares its own `\documentclass{abntex2}`, inlines every `\usepackage` and config, and pulls the bibliography via `\bibliography{references}`. There is no shared config include.
- `references.bib` — BibTeX bibliography (cited as `references`, without extension)
- `abntex2.tex`, `abntex2cite.tex`, `abntex2cite-alf.tex` — bundled copies of the upstream abnTeX2 manual/example sources (each is its own `ltxdoc` document). They are **not** loaded by `document.tex`; keep them for reference only.

The `project/` variant additionally bundles the abnTeX2 class and style sources locally (`abntex2.cls`, `abntex2cite.sty`, `.bst` files) so it compiles even where those are not installed in the TeX distribution.

## Conventions

- Keep UTF-8 (`\usepackage[utf8]{inputenc}`) and the Times font (`\usepackage{times}`): changing either reflows the whole document.
- Primary language is Portuguese; English is secondary via `babel`.
- Citations: every paper loads `abntex2cite` with `alf` (author-date). Keep one style per paper; mixing `alf` and `num` in one document produces inconsistent references.
- Indentation: match the file you edit. The `article/` documents indent nested environments with four spaces.
- Follow [Git Flow conventions](https://github.com/rios0rios0/guide/wiki/Git-Flow): branch names use `feat/`, `fix/`, `chore/` prefixes; commit subjects are `type(scope): message` in simple past tense.

<!-- chlog:start -->
## Changelog (chlog) — MANDATORY

If the repository you are working in uses chlog (a `.chlog.yaml` or `.chlog.yml`
config file, or a `.changes/` directory, exists at the project root), the
following is binding and ALWAYS applies: whenever you make ANY change, you MUST
create a changelog fragment as part of the same change — automatically, without
being asked, before committing.

- Do NOT edit CHANGELOG.md directly; it is generated from fragments.
- Create the fragment with:
  `chlog new --kind <Kind> --body '<past-tense description>'`
- Write an apostrophe inside the single-quoted body as `'\''`.
- Valid kinds: Added, Changed, Deprecated, Removed, Fixed, Security
- Choose the kind that best matches the change (e.g., new feature → Added,
  bug fix → Fixed, behavior change → Changed, removal → Removed, security fix → Security).
- If the change is backward-INCOMPATIBLE with the public API (a breaking
  change), you MUST add the `--breaking` flag:
  `chlog new --kind <Kind> --breaking --body '<past-tense description>'`.
  This is the ONLY thing that triggers a major version bump — the kind alone
  never does (per SemVer, major = incompatible change). When unsure whether a
  change breaks compatibility, ask the user instead of guessing.
- Fragments are YAML files in `.changes/unreleased/`; stage them with your commit.
- `chlog check` fails the build when a fragment is missing — never skip it.
<!-- chlog:end -->
