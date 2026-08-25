# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

This file is not edited by hand. Every change writes its own fragment under
`.changes/unreleased/` with [chlog](https://github.com/luizjhonata/chlog), and a release compiles
the pending fragments into a version section here — so two branches each adding an entry no
longer touch the same lines, and a rebase that used to conflict on this file now conflicts on
nothing.

When a new release is proposed:

1. Create a new branch `bump/x.x.x` (this isn't a long-lived branch!!!);
2. The fragments pending under `.changes/unreleased/` are compiled into a version section by `chlog batch auto && chlog merge` (AutoBump does this for you — it reads the fragments directly);
3. Open a Pull Request with the bump version changes targeting the `main` branch;
4. When the Pull Request is merged, a new Git tag must be created using <LINK TO THE PLATFORM TO OPEN THE PULL REQUEST>.

Releases to productive environments should run from a tagged version.
Exceptions are acceptable depending on the circumstances (critical bug fixes that can be cherry-picked, etc.).

## [Unreleased]

## [1.1.3] - 2026-08-04

### Changed

- corrected `.github/copilot-instructions.md` citation-style note: alphabetical citations require the explicit `[alf]` option (as every paper uses), and numeric is the `abntex2cite` default

## [1.1.2] - 2026-06-03

### Changed

- corrected `CLAUDE.md` and `.github/copilot-instructions.md` to drop the inaccurate `\input{abntex2}` composition claim; each `document.tex` is self-contained and the bundled `abntex2*.tex` files are upstream manuals, not part of the build

## [1.1.1] - 2026-05-19

### Changed

- refreshed `.github/copilot-instructions.md` to document the new `release.yaml` CI workflow

## [1.1.0] - 2026-04-28

### Added

- added `CLAUDE.md` with build commands, project structure, and repo conventions for Claude Code sessions

## [1.0.0] - 2018-11-17

The changes weren't tracked until this version.
