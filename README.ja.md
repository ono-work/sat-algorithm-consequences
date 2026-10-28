# SAT高速化から導かれるパラメータ化・厳密アルゴリズム

[English](README.md) · [FPT](FPT.ja.md) · [厳密アルゴリズム](EXACT.ja.md) · [計算量理論](COMPLEXITY.ja.md) · [全体一覧](CATALOG.ja.md) · [候補・障害](CANDIDATES.ja.md) · [仮定と出典](ASSUMPTIONS.ja.md)

## このカタログを作るに至った経緯

2026年10月26日、OpenAIは、指数時間仮説（Exponential Time Hypothesis; ETH）の反証を主張する研究結果を[発表](https://openai.com/index/building-scientific-understanding-with-academic-communities/)し、原稿 [*The Exponential Time Hypothesis is False*](https://github.com/openai/math/blob/42907c9e999cc00d6abe7a50bfc1023250edd1b9/preprints/The-Exponential-Time-Hypothesis-is-False-October-25-2026/paper.pdf)と関連するLean形式化を公開しました。これは、固定幅CNFの充足割当てを決定的な劣指数時間・多項式領域で厳密に数えられるという主張です。OpenAIは、Russell Impagliazzo、Ramamohan Paturi、Ryan Williamsの3氏が率いる独立した研究者グループが、その検証と理解を進めると説明しています。すでにShiva Kintaliによる簡易版 [*A simplified proof of ETH is false*](https://shivakintali.github.io/papers/ETH.pdf)や、[John D. Cookによる紹介記事](https://www.johndcook.com/blog/2026/10/27/eth-false/)も公開されており、こうした動きからも、この結果の正しさや意味への関心の広がりがうかがえます。

結果の正しさを確かめることはもちろん重要ですが、**正しいと仮定した場合に、どのような影響があるのかを見極めることも重要**だと考えます。この主張の核心には、固定幅のSATに対して、従来想定されていたより高速なアルゴリズムが存在するかもしれない、という可能性があります。NP完全性の理論から、多くの問題はSATへ帰着できるため、その高速化が他の問題にも及ぶことが期待されます。ただし、NP完全性は多項式時間帰着に基づく概念であり、帰着先の変数数などが、元の問題で測りたいサイズに対して大きく増えることがあります。そのため、SATの劣指数時間アルゴリズムから、すべてのNP完全問題に対する劣指数時間アルゴリズムや既知上界の改善が直ちに導かれるわけではありません。現時点で直接に導ける高速化の範囲には、帰着のサイズや前処理のコストによる制約があります。だからこそ、まず既存の結果や標準的な帰着から素直に導ける帰結を整理し、どの問題に、どの程度の波及効果があるのかを具体的に確かめることが重要です。

私自身もこの波及効果に興味を持ち、具体的に何が導けるのかを調べたいと考えました。同じ疑問を持つ方も多いのではないかと思い、調べた内容を日本語と英語のカタログとしてここに整理し、共有することにしました。最も直接的な対象は **exact exponential-time algorithmsの高速化**ですが、符号化や部分問題への縮約など、技術的に似た構造を持つ **FPTアルゴリズムの設計**にも同じ視点を適用できます。本カタログではこの両方を対象に、判定・計数・厳密最適化を含むアルゴリズム上の帰結を集め、どの入力サイズやパラメータについて、どのような条件のもとで高速化が得られるのかを記録していきます。

## 作成・確認・更新方針

**本カタログの文献調査、帰結の列挙、導出の作成、文章化の大部分はCodexによるものです。私自身が各項目を一つずつ精査し、正しさを確認した資料ではありません。** 多くの項目は、既知の結果と比較的直接的な帰着や線形サイズの符号化を組み合わせたものですが、前提条件の見落とし、符号化や計算量評価の誤り、文献の読み違いなどが残っている可能性があります。議論が単純に見えることや、AIによる点検を経ていることを、人間による独立した正しさの確認と同一視しないようご注意ください。

元のSATアルゴリズムの主張を仮定した場合でも、そこから導く個々の帰結の正しさまで保証するものではありません。各項目には出典、使用する仮定、符号化のサイズ、導出、確認状況を記録していますが、**研究で利用・引用する際には、対応する原典と導出をご確認ください。** 確認が行われた場合は、その対象と範囲を記録します。

本カタログの結論は、各項目で指定するSATアルゴリズムの主張を仮定したものです。元原稿に明記された系、既知の結果を組み合わせた導出、追加調査が必要な候補を区別し、掲載自体によって新規性や網羅性を主張するものではありません。各項目は確認状況を明記した草稿として公開し、GitHub上で文献の追加や原稿の改訂、誤りの指摘に応じて継続的に更新する方針です。誤りや抜けの指摘、関連文献の補足、項目の追加を歓迎します。

## 対象とする版

初版では *The Exponential Time Hypothesis is False* の**2026年10月25日改訂版**を対象にします。これは10月26日という発表日とは別の日付です。原稿と形式化の範囲説明は、GitのコミットとSHA-256で版を固定しています。両者で異なる計算時間は、別の仮定として管理します。具体的な主張と出典は[仮定一覧](ASSUMPTIONS.ja.md)を参照してください。

## 現在の内容

10月29日の更新では、解説に[**XP・XNLPと時間・領域の同時上界**](COMPLEXITY.ja.md#xp-xnlp)を追加しました。無条件の真包含 FPT ⊊ XP、小さい固定幅SAT符号化から得られる条件付きFPT、XNLPやSlice-wise Polynomial Space Conjectureに波及するために必要な追加の進展を分けて説明します。分解が与えられたCliqueでは、その幅を頂点数の対数で割ったパラメータについて肯定的な帰結を得ます。同じ正規化をしたIndependent Set・Dominating Setとの違いも示し、なお大きな課題が残ることを説明します。SATの仮定との論理的独立性を証明したという主張はしていません。解説内の議論として収録し、下記の個別アルゴリズムの項目数は維持しています。

10月28日の更新では、[偶奇による包含排除](METHODS.md)を共通手法に追加しました。目的の個数を固定幅#SATの二つの計数値の差として表し、[Dominating Setの計数](docs/ja/exact-count-dominating-sets.md)と[最小Dominating Set・最適解の復元](docs/ja/exact-minimum-dominating-set.md)を、密なグラフでも元の頂点数nに関する一般密度版のA(n)で扱います。従来のB(n+m)の導出と、密度を固定した場合のB(n)も残しています。この手法は正確な計数を使い、補助的な二つの式のどちらかの充足可能性だけでは目的の条件を判定できません。

新規項目として、[極大独立集合・極小Vertex Coverの計数](docs/ja/exact-count-maximal-independent-sets.md)、[最小独立支配集合・Upper Vertex Cover](docs/ja/exact-minimum-independent-dominating-set.md)、[変数数と節数の和を尺度にする任意幅CNFの計数](docs/ja/exact-count-unrestricted-cnf.md)、元の頂点数を尺度にする[固定有限禁止族への頂点削除](docs/ja/exact-count-finite-obstruction-vertex-deletions.md)と[Tournament Feedback Vertex Set](docs/ja/exact-count-tournament-feedback-vertex-sets.md)の計数・最適化を追加しました。FPT側には、vertex cover numberをパラメータにした[最大Clique](docs/ja/clique-vertex-cover.md)と[指定サイズのクリークの計数](docs/ja/count-cliques-vertex-cover.md)、[rankと最大・最小の正の出現回数の比を固定したHitting Set](docs/ja/bounded-ratio-occurrence-hitting-set.md)を加えました。

2026年10月28日の更新で、[**計算量理論への影響**](COMPLEXITY.ja.md)を追加しました。条件付きの FPT = M[1]、ミニ化された具体的な問題とパラメータ、W階層やETHに基づく下界への影響の範囲、NPとEXPの関係で重要となる二つの劣指数時間の違い、回路下界を扱います。この解説もCodexが作成した草稿で、人間による正しさの確認は記録されていません。個別アルゴリズムの項目数には含めません。

同日の先の更新では、[Vertex Coverの導出](docs/ja/vertex-cover-solution-size.md)と[個数制約の符号化の解説](METHODS.md)に、具体的なゲートの節、計算例、変数O(k)・節O(k²)の区別を追記しました。また、[最大次数を固定したEdge Dominating Set](docs/ja/bounded-degree-edge-dominating-set.md)と、[辺数を尺度とする厳密最適化](docs/ja/exact-edge-dominating-set.md)を追加しました。一般グラフで解サイズだけをパラメータにした場合は、[候補・障害](CANDIDATES.ja.md)に未導出の項目として別に記録しています。

**FPT 16項目、厳密アルゴリズム19項目の計35項目**と、**10項目の候補・障害の記録**を英日で収録しています。10月28日の包含排除の更新で追加・更新した12項目と、先に確認したVC・EDSの3項目を合わせた **15項目** に、対象範囲を限定した **AI監査** の記録があります。一般EDSの候補にもAI監査を記録し、残る20項目と候補9項目は **draft** です。解説の各節に対する確認は、その解説内で別に記録します。人間による確認の記録はありません。調査・導出・記述の大部分はCodexによるもので、AI監査は元のSAT定理の認定や人間による正しさの検証に代わるものではありません。計数と非重み付き最適化は別の版として記録し、明記した項目では最適解の復元も扱います。現在の最良アルゴリズムとの系統的比較は今後の調査項目です。Perfect Matchingの項目では、permanentの古典的な比較用上界だけを、その範囲を明記して収録しています。

| 区分 | 代表的な対象 | 主な尺度 |
| --- | --- | --- |
| [FPT](FPT.ja.md) | 線形カーネル、小さいmodulator、目標値による縮小を使う判定・計数・最適化、vertex cover numberをパラメータにしたClique | 意味を各項目で指定するパラメータ $k$ |
| [厳密アルゴリズム](EXACT.ja.md) | 頂点・辺の選択、支配、固定禁止パターンへの削除、固定領域CSPなどの計数・最適化、任意幅CNFの計数 | 各項目で指定する $n$、$m$、$n+m$ |
| [計算量理論](COMPLEXITY.ja.md) | クラスの一致、M[1]完全問題、XP・XNLPと時間・領域の同時上界、古典的クラスへの帰結の限界、回路下界 | クラス・パラメータ・帰着の種類・計算モデルを明記 |
| [候補・障害](CANDIDATES.ja.md) | 解サイズをパラメータにした一般Edge Dominating Set、行列の次元に関する密なpermanent、Hamiltonian Cycle、Chromatic Number、二進重み問題などの未完成の導出 | 問題名だけで一括せず、目標とする尺度を明記 |

独立集合・固定色数の彩色・Perfect Matchingの計数については、元原稿の系1.3に定性的な帰結が明記されています。これらは `source-corollary` とし、定量的な精密化を別途説明します。それ以外の導出は `catalog-deduction` として記録します。いずれも新規性の主張を意味しません。

## 1項目に記録するもの

**問題 × パラメータまたはサイズ尺度 × 入力クラス × 問題の版**を1項目にします。固定定数、前処理、補助変数を含むBoolean変数数 $N$、節数 $M$、節幅 $w$、使用する仮定、短い導出、適用範囲、定理番号付きの出典、確認状況を記録します。判定・計数・最適化・探索を区別し、判定カーネルが自動的に解の個数も保存するとは扱いません。

3種類の仮定から、次の上界を区別して表示します。

| 仮定 | 指定する尺度 $s$ に関して得られる上界 |
| --- | --- |
| 原稿・$N=O(s)$、節数は多項式 | $A(s)L^{O(1)}=2^{O(s\log\log s/\sqrt{\log s})}L^{O(1)}$ |
| 原稿・$N+M=O(s)$ | $B(s)L^{O(1)}=2^{O(s/\sqrt{\log s})}L^{O(1)}$ |
| 公開された形式化の範囲説明・$N=O(s)$ | $2^{O(s(\log\log s)^2/\sqrt{\log s})}L^{O(1)}$ |

$L$ は元の入力の符号長、$s$ は明記したパラメータまたはサイズ尺度です。いずれも節幅が固定で、符号化の長さが元の入力長の多項式以下であることを要求します。$N+M=O(s)$ の場合は、計数を保存するpaddingで密度を定数にできます。[METHODS.md](METHODS.md)に導出があります。小さい尺度では対数を適宜正則化します。固定した問題定数により、Oの定数や多項式次数は変わります。使っている前提は具体的なアルゴリズムの上界であり、単に「ETHが偽」という命題だけから導くものではありません。

辺数やincidenceの総数を尺度にする上界は、そのまま表示します。例えばPerfect Matchingの計数に対する $B(m)$ は、$m=O(n)$ なら $n$ に関しても劣指数ですが、任意の密なグラフについて同じことは言えません。一般の重み付き版や、より広い代数的表現は、別途サイズと演算コストを調べる必要があります。

## 状態の意味

導出の状態と、誰が確認したかを別々の欄にします。

| 表示 | 意味 |
| --- | --- |
| `derivation-available` | 条件付きの具体的な導出を掲載している。 |
| `candidate` | 導出を引き続き調べる必要がある。 |
| `not-obtained-by-this-route` | 記載した方法では目標の上界を示せていない。高速化の不可能性を意味しない。 |
| `draft` | 確認の記録がまだ付いていない。 |
| `ai-audited` | 記載した範囲をAIが点検した記録がある。 |
| `human-reviewed` | 名前を記録した人が、対象の版と範囲を確認した。 |

草稿も、状態を表示して公開できます。確認状況は個々の項目についてのもので、元のSAT定理の正しさの認定ではありません。機械的チェックはデータと生成物の整合性を調べ、証明や確認者の本人性を検証するものではありません。

## ファイル構成

| ファイル・ディレクトリ | 用途 |
| --- | --- |
| `data/catalog.json` | 個別アルゴリズム・SATの仮定・出典・状態の共通データと、各項目の英日の説明文。 |
| `CATALOG.md`, `CATALOG.ja.md` | 自動生成する全体一覧。 |
| `FPT.md`, `FPT.ja.md` | FPTアルゴリズムの一覧。 |
| `EXACT.md`, `EXACT.ja.md` | 問題の版とサイズ尺度を明記した厳密アルゴリズムの一覧。 |
| `COMPLEXITY.md`, `COMPLEXITY.ja.md` | 計算量理論への影響を扱う手書きの解説。英日の主張と出典の参照箇所をそろえて更新。 |
| `docs/en/`, `docs/ja/` | 自動生成する各項目のページ。 |
| `ASSUMPTIONS.md`, `ASSUMPTIONS.ja.md` | 仮定の定義と出典の版。 |
| `CANDIDATES.md`, `CANDIDATES.ja.md` | 未完成の導出と具体的な障害。 |
| [METHODS.md](METHODS.md) | 共通の初等的導出とサイズ評価。英日併記。 |
| [RESEARCH_PROTOCOL.md](RESEARCH_PROTOCOL.md) | 発見・導出・点検・更新の手順。 |
| [prompts/](prompts/) | Codexに渡す調査・監査・出典更新の指示。 |
| [AGENTS.md](AGENTS.md) | このリポジトリで作業するAIへの指示。 |

## 更新方法

Python 3.10以降を使います。外部パッケージは不要です。

```bash
python3 tools/render_catalog.py
python3 tools/render_catalog.py --check
```

共通JSONと手書きの手順書を編集し、各項目と一覧を再生成します。計算量クラスへの帰結、パラメータ、帰着の種類、出典を変更する場合は、英日の `COMPLEXITY` を一緒に更新します。元データと生成物を一緒にコミットしてください。個別アルゴリズムの数式・出典ID・確認状況は同じデータから両言語へ出力します。説明文の翻訳が数学的に一致しているかは、別途読み合わせます。

追加調査には[調査用タスク](prompts/investigate.md)、導出の点検には[監査用タスク](prompts/audit.md)、元論文の改訂には[出典更新用タスク](prompts/update-sources.md)を使います。候補追加と誤り指摘のIssueテンプレートも含めています。

## 公開先

本カタログは2026年10月28日に [ono-work/sat-algorithm-consequences](https://github.com/ono-work/sat-algorithm-consequences) で公開しました。公開に使ったGitHubアカウントは `agent-maria`、ブランチは `main` です。ライセンスは未選定です。公開記録と認証・更新の手順は [PUBLISH.md](PUBLISH.md) を参照してください。
