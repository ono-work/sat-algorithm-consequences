# Parameterized and Exact Algorithms from Faster SAT

[日本語](README.ja.md) · [FPT](FPT.md) · [Exact algorithms](EXACT.md) · [All entries](CATALOG.md) · [Candidates](CANDIDATES.md) · [Assumptions and sources](ASSUMPTIONS.md)

## How this catalog began

On October 26, 2026, OpenAI [announced](https://openai.com/index/building-scientific-understanding-with-academic-communities/) a claimed refutation of the Exponential Time Hypothesis (ETH), releasing the manuscript [*The Exponential Time Hypothesis is False*](https://github.com/openai/math/blob/42907c9e999cc00d6abe7a50bfc1023250edd1b9/preprints/The-Exponential-Time-Hypothesis-is-False-October-25-2026/paper.pdf) and an associated Lean formalization. The claim is a deterministic algorithm for exactly counting the satisfying assignments of fixed-width CNF formulas in subexponential time and polynomial space. OpenAI states that an independent group led by Russell Impagliazzo, Ramamohan Paturi, and Ryan Williams will examine and explain the result. Shiva Kintali's simplified exposition, [*A simplified proof of ETH is false*](https://shivakintali.github.io/papers/ETH.pdf), and [an introductory article by John D. Cook](https://www.johndcook.com/blog/2026/10/27/eth-false/) have also appeared. These developments suggest growing interest in the correctness and implications of the claimed result.

Checking correctness is, of course, essential, but **understanding the impact if the claim is correct is also important**. At its core, the claim raises the possibility that fixed-width SAT admits faster algorithms than previously expected. The theory of NP-completeness provides reductions from many problems to SAT, suggesting that such a speedup may extend to other problems. However, NP-completeness is based on polynomial-time reductions, which may substantially increase the number of variables or another quantity relative to the size measure of the original problem. A subexponential-time SAT algorithm therefore does not automatically yield subexponential algorithms, or improvements over known bounds, for every NP-complete problem. The speedups that can be derived directly are constrained by the size increase and preprocessing costs of the reductions. This makes it important to first identify the consequences that follow straightforwardly from existing results and standard reductions, and determine which problems benefit and by how much.

I became interested in these consequences myself and wanted to investigate what can actually be derived. Expecting that many others may share this question, I decided to organize and share the findings here as a catalog in Japanese and English. The most direct application is to **faster exact exponential-time algorithms**, but related structures in encodings and reductions to smaller subproblems also make this perspective relevant to **FPT algorithm design**. The catalog covers both directions, including consequences for decision, counting, and exact optimization. For each entry, it records the input-size measure or parameter governing the speedup and the conditions under which it follows.

## Preparation, verification, and updates

**Most of the literature research, identification of consequences, derivations, and writing in this catalog were carried out by Codex. I have not carefully examined and verified the correctness of every entry myself.** Many entries combine known results with relatively direct reductions or linear-size encodings, but they may still contain overlooked assumptions, encoding or running-time errors, or misreadings of the literature. An apparently simple argument, or one checked by AI, should not be treated as independently verified by a human.

Even assuming the underlying SAT-algorithm claims are correct, the correctness of each consequence derived from them is not guaranteed. Each entry records its sources, assumptions, encoding sizes, derivation, and review status, but **please check the original sources and the derivation before relying on or citing an entry in research.** When a review takes place, its subject and scope will be recorded.

Every conclusion is conditional on the SAT-algorithm assumptions named in its entry. We distinguish corollaries explicitly stated in the source manuscript, deductions combining known results, and candidates requiring further investigation. Inclusion is not a claim of novelty or completeness. Entries are intended to be shared as drafts with their review status visible and updated on GitHub as references are added, manuscripts are revised, and errors are reported. Reports of errors or omissions, additional references, and new entries are welcome.

## Source version

The initial focus is the **October 25, 2026 revision** of *The Exponential Time Hypothesis is False*. This manuscript revision date is distinct from the October 26 announcement date. The manuscript and the accompanying formalization-scope description are pinned by Git commit and SHA-256. Their different running-time bounds are recorded as separate assumptions. See [the assumption register](ASSUMPTIONS.md) for the precise statements and sources.

## Current snapshot

The catalog contains **23 entries: 12 FPT and 11 exact-algorithm entries**, plus **9 candidate/obstacle notes**, in English and Japanese. All are labeled **draft**. Most of the research, derivation, and writing were carried out by Codex; this is not a collection whose correctness has been carefully verified by a human. The exact entries cover counting and unweighted optimization, including optimum witnesses. A systematic comparison against the latest best algorithms remains open; the perfect-matching entry records one classical permanent benchmark explicitly as such.

| Track | Representative scope | Main scale |
| --- | --- | --- |
| [FPT](FPT.md) | Decision algorithms using linear kernels, small modulators, or target-based size reductions | Parameter $k$ |
| [Exact algorithms](EXACT.md) | Counting independent sets, fixed-color colorings, CSP solutions and constrained edge sets; optimizing Independent Set, Dominating Set, Max-Cut and fixed-domain Max-CSP | $n$, $m$, or $n+m$, as stated per entry |
| [Candidates and obstacles](CANDIDATES.md) | Incomplete routes including dense permanent by matrix dimension, Hamiltonian Cycle, Chromatic Number and binary-weight problems | The specific target scale, not a generic problem-name claim |

Three qualitative exact-counting consequences—independent sets, fixed-color colorings and perfect matchings—already appear in the source manuscript's Corollary 1.3. Their entries are labeled `source-corollary`, with quantitative refinements identified. Other entries record `catalog-deduction`. These provenance labels do not assert novelty.

## What an entry records

An entry is one **problem × parameter or size measure × input class × variant**. It records fixed constants, preprocessing, the Boolean variable count $N$ including auxiliary variables, the clause count $M$ and width $w$, the chosen assumptions, a short derivation, limitations, references with theorem locations, and review status. Decision, counting, optimization and search must be distinguished; a decision kernel is not automatically counting-preserving.

The three initial assumption profiles yield distinct bounds:

| Profile | Consequence at the declared scale $s$ |
| --- | --- |
| Manuscript, $N=O(s)$ and polynomially many clauses | $A(s)L^{O(1)}=2^{O(s\log\log s/\sqrt{\log s})}L^{O(1)}$ |
| Manuscript, $N+M=O(s)$ | $B(s)L^{O(1)}=2^{O(s/\sqrt{\log s})}L^{O(1)}$ |
| Published formalization scope, $N=O(s)$ | $2^{O(s(\log\log s)^2/\sqrt{\log s})}L^{O(1)}$ |

Here $L$ is the original encoded input length, and $s$ is the declared parameter or size measure. Fixed clause width and polynomial encoding length are required throughout. For $N+M=O(s)$, count-preserving padding makes the density bounded; see [METHODS.md](METHODS.md). Logarithms are regularized for small measures. Fixed problem constants may affect the O constants and polynomial degree. These are assumptions about explicit algorithms, not deductions from the bare statement that ETH is false.

An edge-size or incidence-size bound must retain that measure. For example, $B(m)$ for perfect-matching counting gives subexponential dependence on $n$ when $m=O(n)$; it does not give the same statement on arbitrary dense graphs. General weighted variants and broader algebraic representations need separate accounting.

## Review labels

Mathematical completion and review are separate fields.

| Field | Meaning |
| --- | --- |
| `derivation-available` | A concrete conditional derivation is written down. |
| `candidate` | The derivation still needs investigation. |
| `not-obtained-by-this-route` | A specified approach does not establish the desired bound; this is not an impossibility result. |
| `draft` | The entry has no recorded review. |
| `ai-audited` | A recorded AI audit checked the stated scope. |
| `human-reviewed` | A named human reviewed the stated scope and version. |

Drafts can be published with their labels. A review label concerns this entry, not validation of the underlying SAT theorem. Mechanical checks validate data and generated files, not proofs or reviewer identities.

## Repository map

| File or directory | Purpose |
| --- | --- |
| `data/catalog.json` | Shared mathematical fields, sources, statuses, and bilingual text. |
| `CATALOG.md`, `CATALOG.ja.md` | Generated overall indexes. |
| `FPT.md`, `FPT.ja.md` | Generated parameterized-algorithm indexes. |
| `EXACT.md`, `EXACT.ja.md` | Generated exact-algorithm indexes, with variants and size measures. |
| `docs/en/`, `docs/ja/` | Generated entry pages. |
| `ASSUMPTIONS.md`, `ASSUMPTIONS.ja.md` | Generated assumption definitions and source versions. |
| `CANDIDATES.md`, `CANDIDATES.ja.md` | Unfinished routes and concrete obstacles. |
| [METHODS.md](METHODS.md) | Shared elementary derivations and parameter accounting, in both languages. |
| [RESEARCH_PROTOCOL.md](RESEARCH_PROTOCOL.md) | Discovery, derivation, audit, and update procedure. |
| [prompts/](prompts/) | Reusable investigation, audit, and source-update tasks for Codex. |
| [AGENTS.md](AGENTS.md) | Instructions for assistants working in this repository. |

## Updating the catalog

Use Python 3.10 or newer; no third-party packages are needed.

```bash
python3 tools/render_catalog.py
python3 tools/render_catalog.py --check
```

Edit the shared JSON and hand-written methods/protocol, then regenerate the entry pages and indexes. Commit the source data and regenerated files together. Shared formulas, source IDs, and status fields are rendered into both languages from the same record. Translation equivalence still requires reading both explanations.

To add a result, use [the investigation task](prompts/investigate.md). To challenge a derivation, use [the audit task](prompts/audit.md). To handle an upstream revision, use [the source-update task](prompts/update-sources.md). Candidate and correction issue templates are included.

## Publication target

The selected destination for public publication is [ono-work/sat-algorithm-consequences](https://github.com/ono-work/sat-algorithm-consequences), using the GitHub account `agent-maria` and branch `main`. These are prepared settings, not a record of a completed upload. No license has been selected. See [PUBLISH.md](PUBLISH.md) for authentication, repository preparation, and publication steps.
