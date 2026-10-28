# Research and update protocol / 調査・更新手順

## 日本語

### 調査単位

本資料の題名は **Parameterized and Exact Algorithms from Faster SAT**、選択済みの公開先は `ono-work/sat-algorithm-consequences`、アーカイブ名は `sat-algorithm-consequences` です。継続して参照できる英日カタログとして、条件付きの帰結、根拠、適用範囲を蓄積します。

一回の作業では一つの問題族、または三つまでの具体的な候補を調べます。単位は **問題 × パラメータまたはサイズ尺度 × 入力クラス**。さらに判定・計数・最適化・探索を区別します。発見した件数を目標にして、導出が不完全な項目を主表へ移さないでください。

2026-10-28の対象拡張時点では、FPT 12項目、厳密アルゴリズム11項目、候補9項目を収録し、すべてdraftです。これは新規性を主張する論文の結果一覧ではありません。

### 調査の系統

| 系統 | 探すもの | 典型的な確認点 |
| --- | --- | --- |
| FPT：カーネル | 線形頂点・線形要素・線形制約の縮約 | 実際に選ぶ対象は頂点か、辺か、部分構造か。計数を保存するか。 |
| FPT：構造パラメータ | 小さいmodulator、延長不能の定数サイズ証人 | 境界上の変数だけで表せるか。拡張数も扱えるか。 |
| FPT：目標値による縮小 | 自明な保証、有界次数、有界出現回数 | 非自明な入力で変数と制約の総数がO(k)になるか。 |
| 厳密：局所制約 | 固定領域CSP、彩色、頂点・辺の選択 | NがO(n)、O(m)、O(n+m)のどれになるか。 |
| 厳密：計数・最適化・探索 | 個数制約、目的値の判定、解の復元 | 解の個数・目的関数を保存し、呼出し回数と数値のビット長を制御できるか。 |

### 尺度と問題の版

schema version 2では、`track` は `fpt` または `exact`、`measure_tex` は `k`、`n`、`m`、`n+m` です。`variant` は `decision`、`counting`、`optimization`、`search` から選びます。意味の異なる尺度や版を既存のラベルへ押し込めず、必要ならスキーマと生成器を明示的に拡張します。

元問題のn・mの意味を定義し、元入力の符号長L、CNFの変数数N・節数M・最大幅w、CNFの符号長と区別します。疎な入力でO(n)になる表現が、密な入力ではO(n²)になることがあります。n+mに関する劣指数時間を、密な入力でnに関する改善と読み替えてはいけません。

計数では、各ゲートの一意拡張に加え、前処理から最終出力までの計数保存を確認します。判定カーネルや境界の延長可能性の判定だけでは十分ではありません。密度調整のために固定0変数を追加すれば一意拡張を保てます。自由変数を追加する場合の2のべき倍、孤立成分の除去、複数の証明表現による多重度には、保存の証明か明示的な補正が必要です。

最適化では目的値の保存とSAT呼出しの回数、探索では解の復元を示します。二進重みは値の大きさとビット長を区別し、重みの値に依存する擬多項式時間をLの多項式に含めないでください。重み付き版、above-guarantee版、一意性の問いは、それぞれ追加の議論を要します。

### 一項目の作業

1. 定義・区分・パラメータまたはサイズ尺度・問題の版・固定定数・入力制限を確定する。
2. 一次資料を読み、実際に使う版と定理番号を記録する。
3. 前処理から最終出力までのN、M、w、疎密、乱択、数値のビット長、出力の大きさを追う。
4. 正しさと計算時間の導出を書く。`source-corollary` は定性的な帰結が出典に明記された場合に使う。具体的な計算量への代入はカタログ側の導出を含み得る。`catalog-deduction` とともに、新規性の主張とは区別する。
5. 同じ問題の版・入力クラス・尺度・計算モデルの既存アルゴリズムと比較する。古典的な基準との比較と、最新の最良上界の調査を区別し、未調査ならそのまま記録する。
6. 別の作業担当に、導出と出典を渡して反例・抜けを探してもらう。
7. 英日の説明をそろえ、一覧を再生成して変更を小さな単位でレビュー可能にする。

草稿を状態付きで公開し、Issueで指摘を受け付ける運用で構いません。全項目を先生が公開前に査読する仕組みは必要ありません。特に重要な項目、複雑な導出、修正された項目から人手で確認し、実施した分だけ記録します。

`draft`、`ai-audited`、`human-reviewed` は異なる作業状態です。確認を記録する場合は、実際の確認者の種別・氏名またはAIの説明・日付・対象範囲・対象版を残します。AIの監査を人間の確認へ読み替えず、生成器のチェックを証明検証として扱いません。

`comparison_status` が `not-systematically-surveyed` 以外の場合は、日英の `comparison_note` と根拠となる `comparison_refs` が必要です。例えば `classical-benchmark-only` は、その古典的基準と比較した範囲だけを示し、最新最良の主張を伴いません。

### 更新の契機

新しい項目の追加、元論文の改訂、関連するアルゴリズム・カーネルの改善、指摘・反例を契機に更新します。必要なときに `prompts/` のタスクをCodexへ渡して始められます。

元論文が変わった場合は、既存の仮定IDを黙って書き換えません。新しい版の仮定を追加し、両区分の `depends_on`、`source_refs`、`comparison_refs` と候補の参照をたどります。影響する導出・比較を再点検してから依存先を移します。新しい仮定IDや上界の形が既存のプロファイルと異なる場合は、生成器の検証規則も明示的に更新します。

### 次に調べるもの

- FPT 12項目と厳密11項目について、同じ尺度での既存上界の調査と追加出典の補完。
- finite-obstruction deletionの他の線形カーネル。
- 固定q彩色のmodulator先の拡張と、計数版の拡張数。
- 候補9項目の未完了部分。FPTのカーネル・符号化の障害と、厳密アルゴリズムの密な入力・連結性・重みの障害を区別する。
- 重み付き版、above-guarantee版、探索版をそれぞれ別項目として検討。

## English

**Parameterized and Exact Algorithms from Faster SAT** is a living bilingual reference; its selected publication target is `ono-work/sat-algorithm-consequences`, and its archive slug is `sat-algorithm-consequences`. At the 2026-10-28 scope extension it contains 12 FPT entries, 11 exact-algorithm entries, and 9 candidates, all draft. Inclusion is not a novelty claim.

Investigate one family or at most three precise candidates per task. Keep the unit **problem × parameter-or-size-measure × input class**, with distinct decision, counting, optimization, or search variants. Do not turn a discovery quota into incomplete positive entries.

Discovery routes include linear kernels, small structural modulators with bounded extension witnesses, and target-value/degree/occurrence reductions for FPT; and local finite-domain encodings, count-preserving cardinality constraints, optimization calls, or solution reconstruction for exact algorithms.

Schema version 2 uses `track: fpt | exact`, `measure_tex: k | n | m | n+m`, and `variant: decision | counting | optimization | search`. Define original n and m, and distinguish them from input bit length L, encoded variable count N, clause count M, width w, and encoded bit length. Track density: a representation linear in n for sparse graphs may be quadratic in n for dense graphs. Subexponential dependence on n+m does not by itself improve a vertex-based bound on dense inputs.

For counting, verify unique gate extensions and preservation of counts throughout preprocessing, or supply an explicit multiplicity correction. A decision kernel or an extension-existence test does not establish a counting result. Forced-zero density padding preserves unique extension; free-variable padding requires its power-of-two correction. Account for removed isolated components and multiple certificates for the same solution. For optimization, preserve the objective and bound repeated calls; for search, supply reconstruction. Binary weights must be accounted for by bit length: do not hide a pseudopolynomial dependence on their values in L^{O(1)}. Weighted, above-guarantee, and uniqueness questions need separate arguments and, when needed, an explicit schema extension.

For each entry: establish its definition, track, measure, variant, fixed constants, and restrictions; read primary sources and record versions/theorem locations; account for every stage, randomness, bit lengths, and output size; write correctness and runtime arguments; compare under the same variant/class/measure/model; obtain a separate critical audit; synchronize both languages and regenerate the indexes. `source-corollary` identifies a qualitative consequence explicitly stated in the source; its quantitative specialization may still be a catalog derivation. Neither this label nor `catalog-deduction` claims novelty.

Separate a classical benchmark from a survey of the latest best algorithm. A comparison status other than `not-systematically-surveyed` requires bilingual `comparison_note` text and supporting `comparison_refs`. For example, `classical-benchmark-only` records only the stated comparison.

Labeled drafts can be publicly visible and corrected through Issues. Human review can focus on important, intricate, or corrected entries. Record only reviews that occurred, with reviewer type, name or AI description, date, scope, and reviewed version. `draft`, `ai-audited`, and `human-reviewed` remain distinct. Mechanical checks are not proof verification; the workflow does not require the maintainer to pre-review every draft.

Updates can follow new candidates, upstream revisions, algorithm/kernel improvements, or corrections. Invoke the tasks in `prompts/` when work is wanted.

Treat assumption versions as immutable. Trace `depends_on`, `source_refs`, and `comparison_refs` across both tracks and the candidates; recheck affected derivations and comparisons before migrating dependencies. New assumption IDs or theorem shapes may require an explicit update to profile validation. Preserve corrections in CHANGELOG.md.

The next batch should compare all 23 entries under their actual measures, broaden finite-obstruction deletion and coloring-modulator families, study extension counts, and investigate the nine candidates. Distinguish FPT kernel/encoding obstacles from dense-input, connectivity, and weight obstacles for exact algorithms; keep variant-specific claims separate.
