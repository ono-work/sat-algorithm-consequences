# Update source dependencies / 出典の改訂を反映する

調べる出典IDや改訂URLを指定します。仮定の式を一括で置き換えず、FPT・厳密アルゴリズム・候補・既存上界との比較のうち、影響する項目を列挙してから点検します。原稿の明記された系、カタログ側の導出、計数・最適化・探索の要件をそれぞれ確認します。

## Sources or new versions

-

## Task

1. Compare the old pinned source with the new primary source. Record the exact new version, theorem location, URL, and commit/hash where available.
2. State what changed: correctness, scope, supported variants, parameter/size measure, density restrictions, numerical assumptions, time, space, uniformity, randomization, or only presentation. Distinguish n, m, n+m, original input bit length, and the encoded SAT measures.
3. Keep the old assumption record. Add a new one when the statement changes.
4. Find dependent FPT and exact entries through depends_on, source_refs, and comparison_refs, and inspect references in candidate records. For each, determine whether the derivation or comparison remains valid, needs recalculation, or is unresolved. Recheck count-preserving/unique-extension arguments, objective preservation, reconstruction, and bit costs when affected.
5. Update checked entries and both languages together. Preserve old reasoning in Git history and describe corrections in CHANGELOG.md. Preserve source-corollary only when the explicit source statement still supports that provenance. Keep classical-benchmark comparisons distinct from latest-best surveys. Do not carry an AI or human review label across a changed statement without a corresponding review of that scope/version.
6. Regenerate and check the overall catalog, FPT and EXACT indexes, individual pages, assumptions, and candidates in both languages. Summarize source changes, affected entries, remaining work, and precisely what the mechanical check does and does not establish.

Schema version 2 records tracks fpt/exact, measures k/n/m/n+m, and variants decision/counting/optimization/search. The current general/sparse profiles substitute a representation linear in the declared measure. If a new source needs another measure, bound shape, representation, or assumption ID, extend the shared schema/profile validation and renderer explicitly. Retain the old assumption record; do not force a changed theorem into an existing label merely to pass validation.
