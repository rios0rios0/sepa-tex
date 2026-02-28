# Contributing

Contributions are welcome. By participating, you agree to maintain a respectful and constructive environment.

For coding standards, testing patterns, architecture guidelines, commit conventions, and all
development practices, refer to the **[Development Guide](https://github.com/rios0rios0/guide/wiki)**.

## Prerequisites

- [TeX Live](https://tug.org/texlive/) 2020+ or [MiKTeX](https://miktex.org/) (with `abntex2`, `bibtex`, and `pdflatex` packages)
- A LaTeX editor such as [TeXstudio](https://www.texstudio.org/), [VS Code](https://code.visualstudio.com/) with [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop), or [Overleaf](https://www.overleaf.com/)

## Development Workflow

1. Fork and clone the repository
2. Create a branch: `git checkout -b feat/my-change`
3. Navigate to the paper directory you want to edit (e.g., one of the subdirectories containing `document.tex`)
4. Compile the document with references:
   ```bash
   pdflatex document.tex
   bibtex document
   pdflatex document.tex
   pdflatex document.tex
   ```
5. Review the generated PDF output for formatting and citation correctness
6. If editing bibliography entries, update the `references.bib` file and repeat step 4
7. Commit following the [commit conventions](https://github.com/rios0rios0/guide/wiki/Life-Cycle/Git-Flow)
8. Open a pull request against `main`
