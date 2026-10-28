# Changelog / 更新履歴

## 2026-10-29 — XP, XNLP, and time–space obstacles / XP・XNLPと時間・領域に関する障害

- Expanded the bilingual complexity companion with Section 7. Distinguished the unconditional strongly uniform separation FPT ⊊ XP, conditional algorithms from fixed-width SAT encodings with O(h(k) log L) total variables, and existing conjectures. Explained why L^{o(k)} alone does not imply FPT.
- Added a conditional Log-Pathwidth Clique derivation for a supplied path decomposition: O(p+1) variables, O((p+1)²) clauses, fixed width, and FPT time in ceil((p+1)/log₂(n+2)). Included decision, the optimum, a maximum-clique witness, tiny cases, and the separate reported-formalization profile.
- Contrasted XNLP-complete normalized-pathwidth Independent Set and Dominating Set with Clique. Distinguished small workspace from a short complete SAT encoding, and vertex-count bounds from width bounds. Recorded XNLP⊆FPT as the relevant possible inclusion, without assuming its reverse.
- Explained the Slice-wise Polynomial Space Conjecture and why XP time and FPT space must be achieved simultaneously. The comparison table separates current A(n) transfers from hypothetical width-dependent algorithms that would refute SPSC or imply XNLP⊆FPT. No logical-independence or algorithm-impossibility claim is made.
- Added five primary sources and appended Chen–Grohe locators. Recorded an independent, scoped AI audit of both language versions of Section 7 and their source locators, with exact section hashes. The overall note remains draft; no human review or validation of the SAT theorem is asserted.
- Updated navigation, the protocol, and generated snapshot dates. The 35 algorithm entries (16 FPT, 19 exact), 10 candidate records, existing entry reviews, pinned SAT assumptions, and METHODS.md are unchanged.

- 英日の計算量解説に第7節を追加。強一様なFPT ⊊ XPの無条件の分離、全変数数O(h(k) log L)の固定幅SAT符号化から得る条件付きアルゴリズム、既存の予想を区別し、L^{o(k)}だけではFPTにならないことを説明しました。
- 与えられたパス分解を使うLog-Pathwidth Cliqueの条件付き導出を追加。変数O(p+1)・節O((p+1)²)・固定幅を数え、ceil((p+1)/log₂(n+2))についてFPTを得ます。判定・最適値・最大クリークの復元、小さい入力、報告された形式化による別のプロファイルも記載しました。
- 同じ正規化幅でXNLP完全なIndependent Set・Dominating Setと比較。作業領域の小ささと完全なSAT符号化の小ささ、頂点数に関する上界と幅に関する上界を区別し、逆包含を仮定せずXNLP⊆FPTを論点として記録しました。
- Slice-wise Polynomial Space Conjectureと、XP時間・FPT領域を同じアルゴリズムで同時に満たす必要性を説明。現在のA(n)の移送と、SPSCの反証やXNLP⊆FPTに十分な仮想的な幅依存アルゴリズムを表で分けました。論理的独立性やアルゴリズムの不可能性は主張しません。
- 一次資料5件とChen–Groheの参照箇所を追加。英日第7節と対応する出典箇所について、独立したAIによる範囲限定の確認と、対象節のhashを記録しました。解説全体はdraftのままで、人間の確認やSAT定理の検証は意味しません。
- 導線・更新手順・生成物の日付を更新。個別アルゴリズム35項目（FPT 16・厳密19）、候補10項目、既存項目の確認記録、固定されたSAT仮定、METHODS.mdは維持しています。

## 2026-10-28 — Parity counting and additional consequences / 包含排除による計数と追加の帰結

- Added METHODS.md Section 9 in both languages: fixed-width bad events, optional markers, full-equivalence XOR gates, the difference of two exact #SAT counts, the cancellation identity, integer bit lengths, and positive-difference tests for optimization and witness recovery. The scope requires exact counting, not just SAT decision.
- Strengthened the two Dominating Set entries to A(n) on general graphs, measured by the original number of vertices. Preserved the direct B(n+m) construction and the B(n) bound for fixed-density graphs.
- Added seven exact-algorithm entries: maximal independent set/minimal vertex cover counting; minimum independent domination/Upper Vertex Cover; arbitrary-width CNF counting at scale n+m; and separate counting/optimization entries for fixed finite-obstruction vertex deletion and Tournament Feedback Vertex Set at scale n. The latter two families do not establish a solution-size-k bound.
- Added three FPT entries: maximum Clique and specified-size clique counting parameterized by vertex cover number, and fixed-rank Hitting Set with a fixed maximum/minimum positive occurrence ratio. The ratio condition allows unbounded absolute occurrence; incidence counting gives the required O(k) universe. Added a variant column to the FPT indexes.
- Added the 2008 van Rooij–Nederlof–van Dijk report as historical context for inclusion-exclusion in dominating-set counting, without attributing the two-query SAT transformation to it. No novelty or systematic latest-best claim is made.
- The snapshot now has 35 entries (16 FPT and 19 exact) and 10 candidates. Separate AI reviewers audited the twelve added/revised entry records, including their English/Japanese text, encoding sizes, boundary cases, arithmetic, and variant/parameter scope; their reviewed payload hashes and scopes are recorded. There are 15 AI-audited entries and one AI-audited candidate in total; the remaining records are draft. This is not human review or verification of the underlying SAT theorem. The pinned SAT assumptions, other existing entries, and candidate records are unchanged.

- METHODS.md第9節を英日で追加。固定幅の禁止事象、任意に選べる印、完全同値のXORゲート、正確な#SATの2回の計数値の差、相殺の恒等式、整数のビット長、差の正値性による最適化・復元を説明。SATの判定だけでなく、正確な計数を使う範囲を明示しました。
- Dominating Setの2項目を、一般グラフでも元の頂点数nに関するA(n)へ強化。直接構成によるB(n+m)と、密度を固定したグラフのB(n)を維持しました。
- 厳密アルゴリズム7項目を追加。極大独立集合・極小vertex coverの計数、最小独立支配集合・Upper Vertex Cover、n+mを尺度とする任意幅CNFの計数、元のnを尺度とする固定有限禁止族への頂点削除とTournament Feedback Vertex Setの計数・最適化を収録。後二者から解サイズkに関する上界は主張しません。
- FPT 3項目を追加。vertex cover numberに関する最大Cliqueと指定サイズのクリークの計数、rankと最大・最小の正の出現回数の比を固定したHitting Setです。出現回数の絶対値は非有界でもよく、所属関係の数え上げで台集合をO(k)に抑えます。FPT索引にも問題の版の列を追加しました。
- 支配集合の計数における包含排除の背景としてvan Rooij・Nederlof・van Dijkの2008年報告を追加。SATを2回数える変換の出典とは扱わず、新規性や最新最良の系統的比較は主張しません。
- 計35項目（FPT 16・厳密19）と候補10項目になりました。追加・更新した12項目は別担当のAIが英日、符号化サイズ、境界例、数値演算、版・パラメータの範囲を監査し、対象データのhashと確認範囲を記録。全体で15項目と候補1項目がAI監査済み、残りはdraftです。人間の確認や元のSAT定理の証明検証を意味しません。固定されたSATの仮定、他の既存項目、候補のデータは維持しています。

## 2026-10-28 — Vertex Cover encoding and Edge Dominating Set / VCの符号化とEdge Dominating Set

- Expanded the bilingual Vertex Cover derivation: edge clauses use existing vertex variables, the cardinality circuit adds only O(k) variables, and the final O(k)-variable/O(k²)-clause formula uses the general-density SAT assumption. The bound itself is unchanged.
- Expanded METHODS.md Section 2 with exact AND/OR/XOR clauses, the balanced-adder size proof, a comparator for an arbitrary budget, and the four-input/at-most-two example. Added Sinz's parallel-counter reference and distinguished its optimized encoding from the explicit width-three, unique-extension implementation used here.
- Added Edge Dominating Set on fixed-maximum-degree graphs as a B(k) FPT decision consequence, and general-graph exact optimization with an optimum witness as a B(m) consequence measured by the original edge count.
- Added a separate general-k EDS obstacle note. The checked quadratic-vertex/cubic-edge kernel does not yield O(k) Boolean variables; the vertex-cover necessary condition and a P4 example do not justify reusing VC kernel guarantees. This is not an impossibility or latest-best claim.
- The snapshot now has 13 FPT entries, 12 exact entries, and 10 candidate/obstacle notes. Scoped AI audits are recorded for the three updated/new entries and the general-EDS note; no human review or verification of the assumed SAT theorem is asserted. Existing SAT assumptions remain pinned and unchanged.

- VCの英日導出を拡充。辺の節は既存の頂点変数だけで書け、個数制約の追加変数はO(k)、全体は変数O(k)・節O(k²)となるため、SATの一般密度版を使うことを明示。上界自体は変更していません。
- METHODS.md第2節にAND・OR・XORの具体的な節、平衡加算木のサイズ評価、任意の予算との比較器、4入力の和を2以下にする例を追加。Sinzの文献を加え、その最適化された符号化と、ここで用いる幅3・一意拡張の明示的実装を区別しました。
- 最大次数を固定したEDSのB(k)のFPT判定と、元の辺数mを尺度にする一般グラフのB(m)の厳密最適化・最適解復元を追加。
- 一般kのEDSは別の候補・障害として記録。確認した二乗頂点・三乗辺カーネルではO(k)変数が得られず、頂点被覆という必要条件やP4の例からもVCの保証をそのまま使えないことを説明。不可能性や最新最良の主張ではありません。
- 現在はFPT 13項目、厳密12項目、候補10項目。更新・追加した3項目と一般EDSの記録には範囲限定のAI監査を記録し、人間の確認や元のSAT定理の検証とは扱いません。SATの仮定と固定版は維持しています。

## 2026-10-28 — Complexity-theoretic consequences / 計算量理論への影響

- Added matched English and Japanese companion notes, [COMPLEXITY.md](COMPLEXITY.md) and [COMPLEXITY.ja.md](COMPLEXITY.ja.md), with conditional scope, draft status, and exact primary-source locations.
- Explained FPT = M[1] under the assumed effective subexponential SAT algorithms; included the threshold argument and known complete miniaturizations with explicit measures, unary padding, parameters, and FPT Turing completeness. Recorded logarithmically normalized Vertex Cover separately, preserving the source's reduction-type qualification.
- Distinguished the continued validity of W-hardness reductions from the loss of ETH as a quantitative lower-bound premise. Added the fixed-width obstacle in the direct binary Clique encoding.
- Explained why the current SAT upper bounds do not establish NP versus EXP, and why the stronger hypothetical 2^{N^{o(1)}} bound would suffice. No logical-independence claim is made.
- Recorded Williams's qualitative E^NP circuit lower bound and the manuscript's stronger, infinitely-often quantitative corollary with distinct provenance and assumption scope.
- Linked the notes from both READMEs and generated indexes; added eight primary references to the shared source register and updated maintenance guidance. The algorithm-entry schema, 23 entries, 9 candidates, pinned SAT assumptions, and their review labels are unchanged.

- 英日の解説を追加し、条件付きの範囲、draft表示、一次資料の参照箇所を明記。
- 実効的な劣指数時間SATの仮定から FPT = M[1] を得る閾値の議論を説明。完全なミニ化問題の尺度・単項padding・パラメータ・FPT Turing完全性を明示し、対数で正規化したVertex Coverは、出典の帰着種別の留保を保って別に記載。
- W困難性の帰着が有効であることと、ETHを定量的な下界の前提として使えなくなることを区別。Cliqueの直接的な二進符号化で節幅が増える問題を説明。
- 今回の上界からNP対EXPを決着できない理由と、より強い仮定なら時間階層定理で分離を導けることを記載。論理的独立性は主張しない。
- Williamsの定性的な回路下界と、元原稿の「無限に多くの入力長」に対する定量的な系を、由来・仮定を分けて記録。
- READMEと生成索引からのリンク、8件の一次資料、更新手順を追加。個別アルゴリズムのスキーマ、23項目・候補9項目、固定されたSATの仮定、既存の確認状況は維持。

## 2026-10-28 — Initial public publication / 初回公開

- Published the bilingual catalog at [ono-work/sat-algorithm-consequences](https://github.com/ono-work/sat-algorithm-consequences) using `agent-maria`, with 12 FPT entries, 11 exact-algorithm entries, and 9 candidate/obstacle notes.
- Confirmed public visibility, branch `main`, and the GitHub author/committer association with `agent-maria` for initial commit [ca01598](https://github.com/ono-work/sat-algorithm-consequences/commit/ca0159823fdef57bf970de2dae9cb3bccb2702d8).
- Retained the Codex provenance and verification notice, all draft labels, and the unselected license. Publication does not change the mathematical review status.

- `agent-maria` から上記の公開リポジトリへ英日カタログを公開。FPT 12項目、厳密アルゴリズム11項目、候補・障害の記録9項目を収録。
- 公開範囲がpublic、ブランチが `main`、初回コミットの作成者・コミッターがGitHub上で `agent-maria` に対応することを確認。
- Codexによる調査と未確認であることの注意書き、すべてのdraft表示、ライセンス未選定の状態を維持。公開によって数学的な確認状況が変わるものではない。

## 2026-10-28 — Publication target prepared / 公開先の準備

- Recorded the selected public destination `ono-work/sat-algorithm-consequences`, publishing account `agent-maria`, and branch `main` in the maintenance documents.
- Added GitHub CLI web authentication and account verification before repository creation/upload; distinguished authentication from Git commit identity and preserved the workflow for existing repositories.
- Left the license unselected and all mathematical data and review labels unchanged. This records preparation, not successful publication.

- 公開先 `ono-work/sat-algorithm-consequences`、公開に使うアカウント `agent-maria`、ブランチ `main` を手順書に反映。
- リポジトリ作成・アップロード前のGitHub CLIによるWeb認証とアカウント確認を記載。認証とコミットの作成者情報を区別し、既存リポジトリを保つ手順を明示。
- ライセンスは未選定のまま、数学的データと確認状況は変更しない。公開完了ではなく準備の記録。

## 2026-10-28 — Codex provenance and verification notice / Codexによる調査と未確認であることの明示

- Made explicit in both READMEs that Codex carried out most research, derivation, and writing, and that the maintainer has not carefully verified every entry.
- Distinguished relatively direct reductions and linear-size encodings from verified correctness; asked readers to check sources and derivations before research use or citation.
- Kept the three-paragraph motivation and all mathematical entries and review statuses unchanged.

- 調査・導出・記述の大部分をCodexが行い、管理者が各項目の正しさを詳細に確認した資料ではないことを英日で明示。
- 比較的直接的な帰着・線形サイズの符号化に基づくことと正しさの確認を区別し、研究での利用・引用時に原典と導出の確認を依頼。
- 冒頭の経緯三段落、数学的な各項目、確認状況は変更しない。

## 2026-10-28 — Three-paragraph motivation / 三段落による動機の整理

- Reorganized the opening around the announcement and independent review group, quantitative limits of transferring SAT speedups, and the maintainer's motivation to share exact/FPT consequences.
- Linked Kintali's simplified exposition and Cook's introductory article as background reading; retained the existing pinned mathematical assumptions.
- Moved AI-assisted preparation and continuing correction policies to their own section after the three-paragraph introduction.

- 発表と独立検証グループ、SATの高速化を帰着で移す際の定量的な制約、exact/FPTの波及効果を共有する動機の三段落に再構成。
- Kintaliの簡易版とCookの紹介記事へのリンクを追加。計算量の根拠とする既存の仮定は変更しない。
- AIによる作成と継続的な訂正の方針を、三段落の後の独立した節へ移動。

## 2026-10-28 — Historical introduction / 冒頭の経緯説明

- Added a bilingual account of the October 26 OpenAI announcement, the motivation to investigate consequences beyond SAT, and the expansion from FPT to exact algorithms.
- Distinguished the public announcement date from the pinned October 25 manuscript revision. Linked the primary announcement and manuscript.
- Brought AI-assisted investigation, conditional scope, and the intended continuing correction process into the opening; moved version-management details into a separate section.

- 10月26日のOpenAIの発表から、SAT以外への帰結に関心を持ち、FPTから厳密アルゴリズムへ対象を広げた経緯を英日で追加。
- 発表日と対象原稿の10月25日改訂日を区別し、一次資料へのリンクを追加。
- AIによる調査、条件付きの対象範囲、継続的に訂正する方針を冒頭に移し、版管理の説明を独立した節に整理。

## 2026-10-28 — FPT and exact-algorithm scope extension / FPT・厳密アルゴリズムへの対象拡張

- Expanded the living bilingual reference to **12 FPT entries and 11 exact-algorithm entries**, with **9 candidate/obstacle notes**. All entries and candidates remain draft.
- Adopted the proposed title **Parameterized and Exact Algorithms from Faster SAT** and proposed repository/archive slug `sat-algorithm-consequences`.
- Added schema version 2: FPT/exact tracks; explicit k, n, m, or n+m measures; decision, counting, optimization, and search variants; and separate catalog-deduction/source-corollary provenance.
- Added generated FPT and EXACT indexes in both languages and retained the overall catalog. Bounds now substitute each entry's declared measure.
- Extended maintenance guidance to count preservation and unique extension, objective preservation and reconstruction, encoded input lengths, sparse/dense distinctions, and binary weights.
- Kept classical-benchmark comparisons separate from latest-best surveys; preserved the original pinned assumptions and review/source rules.

- 英日の継続参照資料を **FPT 12項目・厳密アルゴリズム11項目・候補9項目** に拡張。項目と候補はすべてdraft。
- 題名とリポジトリ・アーカイブ名の候補を上記のとおり拡張。
- schema version 2で区分、k/n/m/n+mの尺度、判定・計数・最適化・探索の版、原稿の系とカタログ側の導出の由来を明示。
- 英日それぞれのFPT・EXACT索引を追加し、全体一覧も維持。各項目の尺度に対応した上界を生成。
- 計数保存・一意拡張、目的関数の保存・解の復元、入力符号長、疎密、二進重みを扱う更新手順を補完。
- 古典的基準との比較と最新最良上界の調査を区別し、既存の固定された仮定・確認状態・出典の規則を維持。

## 2026-10-28 — Initial draft / 初版草稿

- Added 12 bilingual derivation drafts and 4 candidate/obstacle notes.
- Pinned the October 25 SAT manuscript and formalization-scope description to Git commit `42907c9e999cc00d6abe7a50bfc1023250edd1b9` and their SHA-256 hashes.
- Separated general-density, sparse, and reported-formalization assumptions.
- Added a shared-data renderer, investigation/audit/update prompts, and issue templates.
- All entries remain draft; no human-review claims or systematic latest-best comparisons were added.

- 英日12項目の導出草稿と、4項目の候補・障害を追加。
- SAT原稿と形式化の範囲説明の版を、上記コミットとSHA-256で固定。
- 一般密度・疎な場合・形式化の説明にある上界を別々の仮定として記録。
- 共通データからの生成、調査・監査・更新タスク、Issueテンプレートを追加。
- 全項目はdraft。人間の確認済みという表示や、最新の最良上界を調べ尽くしたという主張は付けていない。
