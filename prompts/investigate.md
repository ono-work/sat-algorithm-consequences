# Investigate a candidate / 候補を調べる

対象の「問題・パラメータまたはサイズ尺度・入力クラス・問題の版」を下欄に入れてCodexに渡します。FPTと厳密アルゴリズムを区別し、一次資料、元問題の尺度と符号長、N・M・w、導出、英日説明をそろえます。計数なら一意拡張と前処理の計数保存、最適化・探索なら目的値や解の復元も確認し、不足する場合は候補として残します。

## Candidate

- Problem:
- Track: fpt / exact
- Parameter (FPT) or size measure (exact), including the meaning of n and m:
- Measure symbol supported by schema 2: k / n / m / n+m
- Input restrictions and fixed constants:
- Variant: decision / counting / optimization / search
- Weights or numerical input: absent / unary / binary; specify the exact role
- Proposed provenance: catalog-deduction / source-corollary
- Starting references:

## Task

Read AGENTS.md, the assumption records, METHODS.md, and nearby entries. Investigate this candidate and prepare a reviewable repository patch.

1. Define the parameter or size measure precisely. For a structural parameter, distinguish a supplied structure from its optimum size and account for obtaining it. For an exact algorithm, distinguish vertex/variable count n, edge/constraint count m, n+m, and input bit length L.
2. Read relevant primary kernelization, reduction, structural, or exact-algorithm results, including enough proof to check the conditions used. Record a stable version and theorem/lemma location. Use source-corollary only when the qualitative consequence is explicitly stated in that source; identify any quantitative substitution supplied by the catalog.
3. Track preprocessing time, independent choices, every auxiliary Boolean variable N, clause count M, width w, encoded bit length, output size, numerical bit lengths, and repeated calls. Keep fixed constants distinct from varying parameters. State when a sparse representation becomes larger on dense input.
4. Prove both directions for a decision reduction. For counting, prove unique gate extension and count preservation through every earlier reduction, or an explicit multiplicity correction; account for padding, removed components, and extension counts. For optimization, preserve the objective and bound all decision/counting calls. For search, provide reconstruction. Binary weight values must not introduce a hidden pseudopolynomial cost. A separate uniqueness or other unsupported variant requires an explicit schema extension.
5. Substitute the actual representation size into a named assumption. Justify fixed width and, for the sparse profile, fixed density. Include all preceding costs, the polynomial factor in L, randomization, and space. Do not replace a bound in n+m by a bound in n without a density assumption.
6. Search for established bounds for the same variant, input class, measure, and computation model. Distinguish a classical benchmark from a latest-best survey. Record an unfinished comparison as not-systematically-surveyed; any other comparison status needs bilingual comparison_note text and comparison_refs. Inclusion in this living reference does not claim novelty.
7. Produce matching English/Japanese text and update shared data. If a step fails, write a candidate/obstacle record giving the missing ingredient.

Run the renderer and its --check mode. Check the overall index, the relevant FPT/EXACT index, and both entry languages. Summarize what was established, what sources were read, and what remains unresolved. A new item starts as draft; add an audit record only for an audit actually performed. Mechanical checks do not establish mathematical correctness. Repository mutations follow the user's task authorization.
