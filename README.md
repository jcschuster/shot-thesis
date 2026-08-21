# Extensionality and Instance-based Methods in Tableau-based Higher-Order Automated Theorem Proving

Master's thesis of Johannes Schuster, Chair for AI Systems Engineering,
Otto-Friedrich-Universität Bamberg, August 2026.
Supervisor: Prof. Dr. Christoph Benzmüller.

The thesis presents Shot, a higher-order tableau prover for Church's simple type
theory whose branch-level rules are binding-free: a rule records the condition
under which a branch would close instead of choosing a value for a free
variable. Closing the tableau is then a constraint satisfaction problem over the
closure evidence of all open branches, and the branch expansion that produces
that evidence runs on a pool of BEAM workers over shared hash-consed terms.

## What This Repository Contains

The thesis is written as executable [Livebook](https://livebook.dev) notebooks,
and this repository is the record of it. The notebooks are the source of truth.
Each chapter is a `.livemd` file that can be read as a document and evaluated as
a program, and the typeset PDF is produced from the same chapters. Where running
the code settles a question that the prose would otherwise assert, a cell runs it
and its output is stored in the file.

| File | Contents |
| --- | --- |
| `00_index.livemd` | Preface: title page, abstract, acknowledgements, table of contents |
| `01_introduction.livemd` | Motivation, contributions, scope, outline |
| `02_preliminaries.livemd` | Notation, Church's simple type theory, classical HOL |
| `03_tableaux.livemd` | The calculus: rules, literal processing, search bounds |
| `04_unification.livemd` | Huet pre-unification and decidable fragments |
| `05_term_order.livemd` | NCPO-LNF, the orientation order for demodulation |
| `06_architecture.livemd` | The concurrent actor architecture on the BEAM |
| `07_extensions.livemd` | Peer agents and proof reconstruction |
| `08_evaluation.livemd` | Ablations over TPTP and the structured problem set |
| `09_conclusion.livemd` | Summary, limitations, future work |
| `90_bibliography.livemd` | Consolidated references, mirrored to `bibliography.bib` |
| `91_appendix_a.livemd` | Worked examples |
| `99_declaration_of_authorship.livemd` | Declaration of authorship |
| `files/` | The derived tables Chapter 8 charts, and the images the preface uses |
| `bibliography.bib` | The same references in BibTeX form |
| `shot-thesis.pdf` | The printable version |

## The Printable Version

[`shot-thesis.pdf`](shot-thesis.pdf) at the root of this repository is the
typeset thesis, generated from the chapters above. Read it for the linear
document with page numbers and cross-references resolved; read the notebooks to
run the code that the chapters describe.

## Opening the Notebooks

1. Install Livebook, either as the desktop application from
   [livebook.dev](https://livebook.dev), or with
   `mix escript.install hex livebook` followed by `livebook server`, or from the
   `ghcr.io/livebook-dev/livebook` container image.
2. Clone this repository, then open `00_index.livemd` from the Livebook home
   screen under **Open** and **From file**. Keep the directory intact: the
   chapters link to one another by relative path, and Chapter 8 reads its tables
   from the `files/` directory beside it.
3. Start at the preface. Its table of contents links every chapter and every
   section, and each chapter ends with a line linking the previous chapter, the
   contents and the next chapter.

Every notebook persists its outputs, so all results, tables and diagrams are
visible without evaluating anything. To evaluate the cells instead, use Elixir
1.20 or later; Chapters 2 to 8 and Appendix A open with a `Mix.install` cell that
fetches the Shot packages from Hex and GitHub, which requires network access on
first run.

## The Implementation

The prover is not held in this repository. It is four Elixir packages,
installed by the notebooks that use it:

| Package | Contents | Source |
| --- | --- | --- |
| `shot_ds` | Terms, types, hash-consing, semantics of the simple type theory | [jcschuster/ShotDs](https://github.com/jcschuster/ShotDs) |
| `shot_un` | Higher-order pre-unification | [jcschuster/ShotUn](https://github.com/jcschuster/ShotUn) |
| `shot_to` | The term order used for orientation and demodulation | [jcschuster/ShotTo](https://github.com/jcschuster/ShotTo) |
| `shot_tx` | Tableau prover, worker pool, agents, proof objects | [jcschuster/ShotTx](https://github.com/jcschuster/ShotTx) |

The per-problem records behind Chapter 8, of which `files/` holds only the
derived tables, are kept with the implementation.

## Citing This Work

`CITATION.cff` carries the citation metadata, which GitHub renders under **Cite
this repository**. Cite the thesis rather than the repository.

## Licensing

Two licences apply, one to the text and one to the code.

- The prose, figures, tables and the typeset PDF are licensed under the
  Creative Commons Attribution 4.0 International License. See
  [`LICENSE-CC`](LICENSE-CC).
- The source code, including the code cells of the notebooks, is licensed under
  the MIT License. See [`LICENSE-MIT`](LICENSE-MIT).

Copyright (c) 2026 Johannes Schuster. The four Shot packages are licensed in
their own repositories and are not covered by either file here.
