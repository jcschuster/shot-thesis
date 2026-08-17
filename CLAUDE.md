# Shot: A Concurrent Higher-Order Tableau Prover for the BEAM

Master's thesis of Johannes Schuster, Chair for AI Systems Engineering,
Otto-Friedrich-Universität Bamberg. Supervisor: Prof. Dr. Christoph Benzmüller.

The thesis is written as a set of executable Livebook notebooks. The LaTeX build
(`thesis-tex.zip`) is a thin wrapper that inputs the chapters; the notebooks are
the source of truth.

## Layout

| File | Contents |
| --- | --- |
| `01_introduction.livemd` | Motivation, contributions, scope, outline |
| `02_preliminaries.livemd` | Notation, Church's STT, classical HOL |
| `03_tableaux.livemd` | The calculus: rules, literal processing, search bounds |
| `04_unification.livemd` | Huet pre-unification, decidable fragments |
| `05_term_order.livemd` | NCPO-LNF, the orientation order for demodulation |
| `06_architecture.livemd` | The concurrent actor architecture on the BEAM |
| `07_extensions.livemd` | Peer agents and proof reconstruction |
| `08_evaluation.livemd` | Ablations and parameter sweeps (in progress) |
| `09_conclusion.livemd` | Summary, limitations, future work |
| `90_bibliography.livemd` | Consolidated citations, mirrored to `bibliography.bib` |
| `91_appendix_a.livemd` | Worked examples |

The implementation lives in four Hex packages, not in this repository:
`shot_ds` (terms, types, STT semantics), `shot_un` (unification), `shot_to`
(term order), `shot_tx` (tableau prover, agents, proof objects). Source:
`github:jcschuster/ShotTx`.

## Before claiming anything about the implementation

Read the source. Clone `github:jcschuster/ShotTx` and read the module in
question end to end, not a keyword grep. Module docs occasionally disagree with
the code (for instance `SuggestionAgent`'s moduledoc states a default that
`Data.Parameters` contradicts); the struct and the function bodies win.

Claims in the thesis about defaults, flag names, rule costs and control flow are
checked against the source, so a plausible-sounding paraphrase is worse than no
sentence at all.

## The three registers

Stated in `02_preliminaries.livemd` and binding on every chapter:

- **Definition / Proposition** blocks are the mathematics. Implementation
  independent, the only material a soundness or completeness argument may appeal
  to. They never name a module, a function or a configuration flag.
- **Ordinary prose** motivates and explains. It carries no logical weight.
- **Blockquoted paragraphs** are implementation notes: what the Shot packages
  do, which module does it, which parameter governs it. A note may narrow a
  definition without the definition changing.
- **Executable cells** belong to the third register as well. Where the code is
  the precise statement, it is run rather than paraphrased.

Putting a flag name inside a Definition, or an implementation constraint inside
a Proposition, is a register error even when the content is correct.

## Interactivity

The notebooks should be genuinely executable, and restrained about it. Add a
cell when running it settles something the prose would otherwise assert: a rule
classification, an enumeration of pre-unifiers, an orientation decision, a proof
object rendered as a tree.

- Prefer `Kino.Markdown` for tabulated results, `Kino.Mermaid` for derivations
  and architecture diagrams, `Kino.Tree` for data structures, `Kino.DataTable`
  for row sets.
- Every notebook carries `<!-- livebook:{"persist_outputs":true} -->` so outputs
  survive in the file. Outputs are program-generated: do not hand-edit them.
- Do not add a cell that only restates prose, and do not add interactivity
  (inputs, frames) for its own sake. Roughly one cell per claim that benefits
  from being executed, not one per section.
- Chapters 2 to 7 pin dependencies in a `Mix.install` cell at the top.

## Notation

Fixed in `02_preliminaries.livemd` and used unchanged everywhere else. When a
new symbol is needed, follow the existing shape rather than inventing a style.

- Sequences: `$\bar{a}_n$`, `$()$` empty, `$\bar{n}$` for `1..n`,
  `$\overleftarrow{a}_n$` reversed, `$\bar{a}_{i..j}$` for a slice.
- Types: sorts `$\mathcal{S} = \{\iota, o\}$`, base types `$\mathcal{B}$`
  (`$\varsigma$`), types `$\mathcal{T}$` (`$\tau, \upsilon, \rho$`), arity and
  goal `$\operatorname{ar}$` and `$\operatorname{gl}$`.
- Tableau objects: formula set `$\Phi$`, branch `$\mathfrak{B}$`, tableau
  `$\mathfrak{T}$`.
- Sets attached to a branch use an upright name applied to the branch:
  `$\mathrm{Lit}(\mathfrak{B})$`, `$\mathrm{Eq}(B)$`, `$\mathrm{Fr}(B)$`,
  `$\mathrm{Def}(B)$`, with `$\mathcal{E}(B)$` for the rewrite rules. Do not
  introduce single-letter set names such as `$F(B)$`; `$T$` in particular is
  already the tableau.
- Complement is `${\sim}\varphi$`, never an overbar.
- **`$\theta$` is a substitution. `$\Theta = (\theta, C)$` is a pre-unifier and
  is never called a substitution.** A pre-unifier is not applied; its
  substitution component is. Write "applying `$\Theta$`'s substitution
  `$\theta$`", and "a global pre-unifier `$\Theta$`".
- Agents are `$\mathcal{CA}$`, `$\mathcal{SA}$`, `$\mathcal{MA}$`, introduced
  once alongside their module names in `06_architecture.livemd` and used as
  symbols thereafter, including inside blockquotes. Backticked module names are
  for code and diagrams.
- Rules are set upright in parentheses: `$(\mathrm{inst})$`,
  `$(\mathrm{rename})$`, `$(\mathrm{demod})$`, alongside the Greek rule classes
  `$\alpha$`, `$\beta$`, `$\gamma$`, `$\delta$`.

## Typography and conventions

- **British spelling throughout.** `-ise`/`-isation`, never `-ize`/`-ization`:
  parallelisation, serialised, normalised, optimised, minimising,
  Skolemisation, memoised, realising. Also `-yse` (analyse), `-our`
  (behaviour, colour), `artefact`, `judgement`, `modelling`. Backticked code
  identifiers, module names, ETS options and verbatim quotations keep their
  original spelling, as do titles in the reference lists.
- **No dashes as punctuation.** No `$-$`, no em dash, no en dash, no
  space-hyphen-space. Use commas, colons, semicolons or a new sentence.
- Paired terms are hyphenated: `flex-flex`, `rigid-rigid`, `flex-rigid`,
  `reflexive-transitive`. Number ranges are written out ("Chapters 3 to 5").
- Configuration flags appear in backticks with the default in parentheses:
  ``` `unification_depth` ($8$) ```.
- Citations are placeholders in the form `[TODO: cite Author Year]`, collected
  per chapter under a final `## Footnotes` section and consolidated in
  `90_bibliography.livemd`.
- Blocks are separated by `<!-- livebook:{"break_markdown":true} -->`.
- **Code cells are ASCII only.** No `λ`, `→`, `ι`, `⊤`, `≻` in Elixir source,
  comments or inline code spans; spell them out. Unicode inside persisted
  *output* blocks is produced by the ShotDs formatters and stays as it is.

### Cross-references

Chapters link as `<a href="06_architecture.livemd">Chapter 6</a>`; sections as
`<a href="#literal-processing">§ Literal Processing</a>`, or
`<a href="03_tableaux.livemd#literal-processing">§ Literal Processing</a>` across
files. Anchors are the heading lowercased with non-alphanumerics stripped and
spaces turned to hyphens.

> **Never begin a line with `<a href=`.** Livebook parses a line-initial HTML
> tag as an HTML block and, on its next save, silently deletes the rest of that
> paragraph. This has already destroyed text in four chapters. Rewrite the
> sentence so the link sits mid-clause: "Branch closure was reduced in
> <a href="03_tableaux.livemd">Chapter 3</a> to a question about terms", not
> "<a href="03_tableaux.livemd">Chapter 3</a> reduced branch closure".

## Prose style

**The target register is a CADE/IJCAR conference paper.** Every paragraph of
every chapter should read as if it were going to a referee at an automated
reasoning conference: plain, precise, technical, unornamented. Before delivering
any prose written for this thesis, reread it against that standard and confirm
in the reply that this was done; if a sentence would look out of place in the
LNCS proceedings of CADE, rewrite it before handing it over.

**No picturesque, metaphorical or anthropomorphic language.** This is the rule
broken most often. Do not write "paid in three separate currencies", "rides on
the standard pipeline", "pays for itself many times over", "the semantic engine
underneath the tableau", "buys soundness", "closes a loop with", "the pieces
assemble into a single choreography", "the load path of the whole prover", "the
cheap path to the same place". Describe the mechanism instead. Processes,
branches and rules do not want, try, watch, guess, listen, wake, get born, die,
win or lose; state what they compute, record, send or terminate on.
Metaphor-derived jargon is also out wherever a standard term exists: write
"matrix", not "recipe"; "introduced" or "created", not "minted"; "originating
branch", not "birth branch"; "parameter", not "knob". Established technical
terms that happen to be metaphors in origin are fine: branch, tree, leaf, node,
head, spine, hole, redex, scratchpad, stream, queue, tombstone, blackboard,
supervision tree, dead end, starvation.

Write plainly and stay in the register. The following patterns have been removed
from the notebooks and should not come back.

**Do not announce significance.** No "it is worth noting/naming/stating", "worth
reading verbatim", "earns its keep", "load-bearing", "the point is", "the whole
point", "importantly", "notably", "interestingly".

**Do not use the "not X but Y" cadence** to simulate insight: "this is not
fastidiousness", "does not merely tolerate, it makes", "not chosen but forced".
State the thing. The variants count as the same fault and are easy to miss when
rereading: "X is not Y. It is Z", "X is not Y; it is Z", "not merely/only X but
Y", "not a X but a Y", "the design is not C for its own sake". Where a contrast
really is needed, put the accepted option first and the rejected one after it
("a work-stealing pool over shared structures, rather than a process per unit of
work"). A bare qualification such as "syntactic identity, not matching modulo a
substitution" is fine; it states a boundary rather than staging a reveal.

**Prefer neutral terms for process roles.** Write "one process directs the
others" or "coordinator and workers", not "master-slave".

**Avoid the flagged vocabulary:** delve, tapestry, testament, landscape, realm,
leverage, underscore, showcase, pivotal, crucial, robust, seamless, intricate,
nuanced, multifaceted, myriad, plethora, elegant, precisely, exactly right,
deliberate, moving parts, at its core.

**Avoid the tics:** sentences opening with "And" or "But"; three-part lists used
for rhythm rather than because there are three things; parenthetical asides that
hedge instead of committing.

Prefer the concrete claim over the evocative one. "A branch that has not changed
its commitments will not answer differently" beats "is not worth probing again".

## Working notes

- Livebook rewrites the whole file when it saves. If a notebook is open in
  Livebook, edits made outside it can be silently overwritten; close or reload
  the notebook before and after editing.
- Only `02_preliminaries.livemd` and `03_tableaux.livemd` are tracked in git.
  Committing the rest would give lost prose somewhere to be recovered from.
- After any structural edit, verify that every `<a href>` resolves and that no
  line starts with an HTML tag.
- `erl_crash.dump` is a stray artefact and is not part of the thesis.
