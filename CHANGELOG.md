# Changelog / 更新履歴

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
