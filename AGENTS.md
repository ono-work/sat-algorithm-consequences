# Instructions for catalog research

This repository, **Parameterized and Exact Algorithms from Faster SAT**, is a living bilingual reference. Its selected publication target is `ono-work/sat-algorithm-consequences`, and its archive slug is `sat-algorithm-consequences`. Record consequences and usable derivations without treating inclusion as a novelty claim. Preserve the unit **problem × parameter-or-size-measure × input class**, with separate records when variants differ. Read README.md, ASSUMPTIONS.md, METHODS.md, and RESEARCH_PROTOCOL.md before changing mathematical content.

本カタログは、SATの高速化から得られるFPT・厳密指数時間アルゴリズムを継続して参照できる英日資料です。問題・パラメータまたはサイズ尺度・入力クラス・問題の版を混同せず、掲載自体を新規性の主張にしないでください。

## Source of truth

- Edit `data/catalog.json`; regenerate generated pages with `python3 tools/render_catalog.py`.
- Run `python3 tools/render_catalog.py --check` before finishing.
- Keep English and Japanese explanations mathematically equivalent. Numerical bounds, source IDs, assumptions, and review labels are shared data.
- Schema version 2 supports `track: fpt | exact`; `measure_tex: k | n | m | n+m`; `variant: decision | counting | optimization | search`; and `origin: catalog-deduction | source-corollary`.
- The general profile substitutes an O(x)-variable fixed-width CNF; the sparse profile requires the corresponding fixed-density condition, typically through N+M=O(x). Here x is the declared parameter or size measure. Other measures, profiles, or representations need an explicit schema/renderer extension.
- `CATALOG*.md` contains both tracks; `FPT*.md` and `EXACT*.md` are their generated indexes. Entry pages remain in `docs/en/` and `docs/ja/`. Never edit generated pages directly.
- `COMPLEXITY.md` and `COMPLEXITY.ja.md` are hand-maintained companion notes, outside the algorithm-entry schema and entry counts. Update both together, preserve exact parameters and reduction types, and record primary-source locators and conditional/review scope. Their source references also appear in the shared source register. Do not turn a limitation of one inference into a theorem of logical independence.

## Research standard

- Read primary sources. Record a version and theorem/lemma/section location for every imported nontrivial fact. Search snippets alone do not establish a kernel's conditions.
- Distinguish an explicit source statement from a catalog deduction. `source-corollary` means that the qualitative consequence is explicitly in a cited source; the displayed quantitative substitution may still be a catalog derivation. Do not attribute a combined FPT or exact algorithm to a paper that supplies only one ingredient.
- Account for all auxiliary variables, clause width, clause count, preprocessing, repeated calls, randomization, bit lengths, and the cost of obtaining a modulator.
- Define original n and m in each exact entry. Keep n, m, or n+m separate from original input bit length L, encoded CNF variable count N, clause count M, width w, and encoded bit length. Derive the complete composition before claiming the final bound.
- A linear vertex kernel does not imply a linear Boolean encoding. A polynomial kernel does not imply subexponential dependence on k. Do not spend a linear-in-k exponent on preprocessing and then claim 2^{o(k)} overall.
- A bound subexponential in n+m need not improve a vertex-based algorithm on dense graphs. State the density regime and compare under the same measure.
- For counting, prove unique extension of gate encodings and preservation of counts throughout preprocessing, or give an explicit multiplicity correction. A decision kernel is not automatically counting-preserving; counting boundary assignments is not counting all extensions. Forced-zero density padding preserves a unique extension; free-variable padding instead needs its power-of-two correction. Account for removed isolated components as well.
- For optimization, preserve the objective and bound repeated decision/counting calls. For search, provide reconstruction within the stated time and space. Separate weighted and above-guarantee variants; binary numerical values are measured by bit length, and a pseudopolynomial factor must not be hidden in L^{O(1)}.
- The supported variants do not include a separate `uniqueness` label. A new variant requires an explicit schema extension and a separate argument.
- Record a precise obstacle when an argument is incomplete. Do not transform an unsuccessful route into an impossibility claim.
- Compare the same variant, input class, measure, and computation model before claiming an improvement. A classical benchmark is not a survey of the latest best algorithm. Use `not-systematically-surveyed` when appropriate; any other comparison status requires bilingual `comparison_note` text and supporting `comparison_refs` with exact locators.

## Reviews and updates

- `draft`, `ai-audited`, and `human-reviewed` record different work. Add a review only when it actually happened, with reviewer type, name or agent description, date, scope, and the version reviewed in the scope text.
- Never invent a human reviewer or promote AI output to human-reviewed. Mechanical checks are not proof verification.
- A draft may be publicly visible with an accurate label; a human-review label is not a prerequisite for publishing a draft.
- Keep assumption versions immutable. For a changed theorem, add a new assumption/source version, identify dependent entries, and recheck them before migrating their dependencies.
- Trace dependencies through `depends_on`, `source_refs`, and `comparison_refs` in both tracks and the candidate records. Extend the profile validation explicitly if a new assumption ID or theorem shape requires it.
- Preserve superseded reasoning in Git history and explain corrections in CHANGELOG.md. Do not silently rewrite the premise while retaining the old conclusion.
- Keep work scoped to the assigned entries. Follow the current user task's authorization for commits, pushes, pull requests, and publication; these instructions do not create additional authorization.

## Useful task sizes

Investigate one family or up to three precise candidates per task. Draft the derivation before requesting a separate audit. Parallelize independent families. Use `prompts/investigate.md`, `prompts/audit.md`, and `prompts/update-sources.md` as starting points.
