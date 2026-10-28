# Audit a derivation / 導出を点検する

対象のentry IDを指定し、導出を作った担当とは別の作業として実行します。FPT・厳密アルゴリズムの双方を対象に、条件の不一致、尺度や変数数の抜け、計数の多重度、目的関数・解の復元の問題を探します。結論を支持することや新規性を示すことを目標にしません。

## Entry IDs

-

## Task

Read the entry, its pinned assumptions, and the cited primary theorem statements and relevant proofs. Check the actual argument independently; do not treat another assistant's summary as source evidence.

Inspect:

- Whether the kernel really outputs the stated unweighted/unannotated problem.
- Whether vertices, edges, tuples, or labels are the independent choices.
- Whether the track, variant, parameter or size measure, and input class match the claim exactly.
- Whether original n and m, input bit length L, encoded variable count N, clause count M, width w, and encoded bit length are distinguished consistently.
- Whether fixed-width conversion or arithmetic adds more variables than claimed, and whether every sparse-profile use has a fixed-density justification.
- Whether a bound in m or n+m has been presented as a bound in n without its required density restriction.
- Whether exponential branching, color-coding, amplification, or preprocessing dominates the final bound.
- Whether the clause width and every hidden constant are fixed.
- Whether counting uses full gate equivalences with unique extension, preserves counts in every earlier reduction, and handles unused-variable padding, removed components, boundary extensions, and multiplicities correctly.
- Whether a decision kernel has incorrectly been treated as counting-preserving, or distinct certificates have been counted as distinct solutions.
- Whether optimization preserves the objective and bounds repeated calls, and search supplies reconstruction within the claimed time and space.
- Whether binary weights or large numerical values introduce a value-dependent rather than bit-length-dependent cost. Inspect uniqueness and above-guarantee claims separately.
- Whether the cited theorem actually supports the imported claim and version.
- Whether source-corollary is backed by an explicit qualitative source statement, and any quantitative catalog deduction is identified.
- Whether a classical benchmark is distinguished from the latest best bound, with comparison notes and primary references supporting exactly the stated comparison.
- Whether English and Japanese say the same mathematical thing.

Return a precise correction, a specific unresolved gap, or a scoped audit note. An unsuccessful route is not an impossibility result. If recording an AI audit, use reviewer_type='ai', a meaningful reviewer description, the date, and a scope identifying the entry/source version. Use ai-audited only for work actually performed; never convert it into human-reviewed or claim to have validated the foundational SAT theorem. Mechanical schema/render checks are not a substitute for this audit. Regenerate affected bilingual pages only after making supported changes to the shared data.
