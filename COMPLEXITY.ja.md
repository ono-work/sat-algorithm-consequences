# 計算量理論への影響

[English](COMPLEXITY.md) · [トップ](README.ja.md) · [FPT](FPT.ja.md) · [厳密アルゴリズム](EXACT.ja.md) · [仮定と出典](ASSUMPTIONS.ja.md)

更新日：2026-10-28。**状態：draft。** 調査・導出・執筆の大部分はCodexによるものです。人間による正しさの確認は記録されていません。以下の主張は明記したSATの仮定に依存し、出典の記載や機械的チェックは元の定理や個々の導出の正しさを認定しません。掲載自体は新規性の主張ではありません。

## 1．対象と主な帰結

[sat26-general](ASSUMPTIONS.ja.md#sat26-general) または [sat26-reported-formal](ASSUMPTIONS.ja.md#sat26-reported-formal) のいずれかを仮定すると、固定幅SATに対して、明示的な決定性 $2^{o_{\mathrm{eff}}(N)}L_{\mathrm{CNF}}^{O(1)}$ 時間アルゴリズムを得ます。実効的なlittle-ohとは、収束の速さを計算可能な形で抑えられることです。厳密計数値が正かどうかで充足可能性を判定できます。定性的な帰結には、二つの仮定はそれぞれ単独で十分です。第6.2節の定量的な回路下界は、特に [sat26-sparse](ASSUMPTIONS.ja.md#sat26-sparse) を使います。

| 論点 | 仮定した結果から記録する内容 | 範囲 |
| --- | --- | --- |
| 指数時間仮説 | ETH・SETH・Gap-ETH・NSETHが成立しない。厳密計数は#ETHも反証する | 出典に明記された帰結：[SAT26, 第1.1.1–1.1.2節][sat26] |
| パラメータ化計算量クラス | $\mathrm{FPT}=\mathrm{M}[1]$ | 既知のM階層の対応を適用。第2節 |
| 具体的な完全問題 | 第3節のミニ化された判定問題がFPTになる | パラメータとpaddingを問題の定義に含める |
| W階層 | ここでW[1]やW[2]との等号を得るわけではない | 困難性の帰着は維持される。ETHに基づく下界はその前提を失う |
| 古典的計算量クラス | 掲載したSATの上界は、P対NPやNP対EXPを決着させない | 第5節で多項式のサイズ増大を説明 |
| 回路計算量 | $\mathrm{E}^{\mathrm{NP}}\not\subseteq\mathrm{SIZE}(O(n))$ | Williamsの既存定理。元原稿のより強い主張は第6節で別に記載 |

これらは、個別アルゴリズム23項目・候補9項目の件数とは別の解説です。以下の対数は底2とし、小さい入力は別処理します。底を別の固定定数にしても、パラメータは定数倍の範囲でしか変わりません。

## 2．条件付きの等号 FPT = M[1]

[Flum–Grohe, Definition 4.2][fg04] のM階層を使います。第1層は、各固定節幅 $d$ についての **$d$-CNFに対するp-log-SAT** のFPT多対一帰着による下方閉包を、$d$ 全体で合併したものです。$N$ 変数、組合せ的な式サイズ $S$ の式に対し、この問題のパラメータは $\lceil N/\log S\rceil$ です。式の表現は節の重複を許し、paddingできます。$S$ は変数名を記述するビット長とは区別します。

既知の対応と包含関係に、上記のSATの仮定のいずれかを適用すると、

$$
\boxed{\mathrm{FPT}=\mathrm{M}[1]}
\subseteq\mathrm{W}[1]\subseteq\mathrm{M}[2]\subseteq\mathrm{W}[2]
$$

を得ます。[FG04, Corollary 4.3・Theorem 4.4・第7節][fg04] を参照してください。次の議論では、必要なアルゴリズムの向きを説明します。結論が適用されるのは、完全問題を含む **M[1]に属する問題** です。M[1]-hardであることだけから上界は得られません。ここで扱うのは判定クラスであり、パラメータ化計数クラスの崩壊は主張していません。

### 多項式の次数をパラメータに依存させずに済む理由

判定問題 $Q$ と、明示した尺度 $\nu(x)$ に対し、[Chen–Grohe, Definition 13][cg06] のミニ化の規約を使います。

$$
\text{入力 }(x,1^b),\qquad b\ge\max\{2,|x|_{\mathrm{bits}}\},\qquad
\kappa=\left\lceil\frac{\nu(x)}{\log b}\right\rceil.
$$

問は元と同じく $x\in Q$ か、です。整数 $b$ は **単項表現** で与え、paddingを含めた入力長は $\Theta(|x|_{\mathrm{bits}}+b)$ です。尺度 $\nu$ はビット長とは別に定義します。

固定 $d$ に対する変数数によるmini-$d$-SATでは、$\nu=N$、$L=|x|_{\mathrm{bits}}$ です。仮定した時間を $2^{Na(N)}L^c$ と書き、$a(N)\to0$ の収束が実効的であることを使います。各 $\kappa\ge1$ に対し、$N\ge H(\kappa)$ なら $a(N)\le1/\kappa$ となる計算可能な閾値 $H(\kappa)$ を選べます。すると、

$$
N\ge H(\kappa)\quad\Longrightarrow\quad
2^{Na(N)}\le 2^{N/\kappa}\le b
$$

です。$N<H(\kappa)$ なら、全探索は高々 $2^{H(\kappa)}L^{O(1)}$ 時間です。両者を組み合わせれば、多項式の次数が $\kappa$ に依存しない $f(\kappa)(L+b)^{O(1)}$ 時間を得ます。変数が0個の場合は自明です。p-log-SATでも $b=S$ とすれば同じ議論が使えます。その場合、$S$ は符号長の多項式以下で、別のpadding文字列は不要です。これは [CG06, Theorem 14][cg06] の必要な向きを直接説明したもので、実用的な $f$ の上界を主張するものではありません。

より一般には、FPT帰着によって、変数数が高々 $h(\kappa)\log L$、符号長が $g(\kappa)L^c$ の固定幅SATが得られる場合にも同じ閾値の議論を使えます。$h,g$ は計算可能な関数、$c$ は定数とし、幅は入力にもパラメータにも依存しない固定定数である必要があります。証人のビットだけを数え、補助変数を除いてしまっては、この条件を満たしたことになりません。

## 3．既知の完全問題と正確なパラメータ

以下の各行の入力には、上で定義した単項padding $1^b$ を含めます。$N$ は式の変数数、$n$ はグラフの頂点数です。求める解の大きさ $r$ は通常の入力であり、**パラメータではありません**。

表の完全性は、すべて **FPT Turing帰着の下での完全性** です。この条件と、クラスの定義に用いた多対一閉包を混同しません。根拠は [FG04, Theorem 5.3、Corollaries 5.6・5.8、Proposition 5.10とその直後の議論][fg04] です。

| ミニ化された判定問題 | パラメータ $\kappa$ |
| --- | --- |
| 変数数によるmini-3SAT：3-CNFは充足可能か | $\lceil N/\log b\rceil$ |
| 頂点数によるmini-$q$-Colorability（$q\ge3$ は固定）：$q$ 彩色可能か | $\lceil n/\log b\rceil$ |
| 頂点数によるmini-Independent Set：大きさ $r$ 以上の独立集合があるか | $\lceil n/\log b\rceil$ |
| 頂点数によるmini-Clique：大きさ $r$ 以上のクリークがあるか | $\lceil n/\log b\rceil$ |
| 頂点数によるmini-Vertex Cover：大きさ $r$ 以下の頂点被覆があるか | $\lceil n/\log b\rceil$ |

頂点数によるmini-Cliqueとmini-Independent Setは、頂点数 $n$ を保つ補グラフの操作で対応します。新しい明示的な符号化の長さを満たすため、必要なら単項paddingを $b$ の多項式まで増やします。尺度は重要で、**分子にグラフ全体のサイズ $n+m$ を使うmini-Cliqueは、以前からFPTです**。[FG04, Corollary 5.9][fg04] を参照してください。そのため、表では尺度を付けずに「Mini-Clique」とは記載していません。

単項paddingの長さは計算時間の入力長に含めます。これらの帰結から、通常のCliqueを $r$ でパラメータ化した場合のFPTアルゴリズムが得られるわけではありません。

p-log-3SATでも、式をpaddingできる構文を保つ必要があります。入力を重複節のない標準形の固定幅の式に限定すると $S=N^{O(1)}$ となり、$\lceil N/\log S\rceil=\kappa$ から $N=O(\kappa\log(\kappa+2))$ を得るため、全探索だけでFPTになります。これは、問題の定義を変更した場合についてのカタログ側の観察であり、引用した完全性の主張ではありません。

### Vertex Coverの解の大きさを対数で正規化した例

[Müller, Example 3.2.17、印刷pp.125–126][mueller08] は、次の問題をM[1]-completeと記載しています。

$$
\text{入力 }(G,r),\quad 0\le r\le n,\qquad
\kappa=\left\lceil\frac{r}{\log\max\{2,\|G\|\}}\right\rceil.
$$

問は大きさ $r$ の頂点被覆が存在するか、です。この入力範囲では「大きさ $r$ 以下」と同値です。出典で用いる構造サイズ $\|G\|$ は、グラフでは $\Theta(n+m)$ です（Appendix A.2、p.195）。頂点数だけでも符号長でもありません。別の単項paddingは入力に持ちません。ごく小さいグラフについてだけ対数を正則化しています。

引用箇所は完全性とETHの不成立との同値を述べていますが、その箇所で完全性の帰着の種類を明示していないため、ここでも追加しません。今回のSATの仮定の下で、この再パラメータ化はFPTになります。通常のVertex Coverは $r$ に関して以前からFPTであり、帰結の対象は、より小さい $r/\log\|G\|$ をパラメータとする場合です。

## 4．W困難性と定量的な下界

通常の解の大きさをパラメータとすると、CliqueはW[1]-complete、Dominating SetはW[2]-completeです。[Downey–Fellows, Introduction・Corollary 3.2][df95] を参照してください。これらの帰着の主張はETHに依存せず、有効なままです。ただし困難性だけでクラスの分離は示せず、本解説から $\mathrm{FPT}=\mathrm{W}[1]$ や $\mathrm{FPT}=\mathrm{W}[2]$ も結論しません。

一見使えそうな直接符号化を見ると、障害が分かります。グラフの $r$ 頂点の選択は $r\lceil\log n\rceil$ ビットで表せます。しかし、選んだ二つの位置が非辺になる組合せを禁止する節の幅は、最大 $2\lceil\log n\rceil$ です。固定幅SATの仮定をそのまま適用できず、3-CNFへ変換するなら追加される補助変数も数える必要があります。これはその符号化の問題点であり、あらゆる符号化に対する下界を主張するものではありません。

別の論点として、[Marx, Theorem 1.5][marx10] は、Cliqueに $f(r)n^{o(r)}$ 時間アルゴリズムがあればETHが成立しないことを示しています。ETHが偽であっても、この含意自体は正しい定理ですが、ETHを根拠にそのアルゴリズムを排除することはできなくなります。それによってアルゴリズムが得られるわけでもありません。他の仮説に基づく下界は、実際に用いている前提に即して判断する必要があります。

また、$2^{O(\kappa)}L^{O(1)}$ から $2^{o(\kappa)}L^{O(1)}$ への高速化は、もともとFPTであったアルゴリズムの改善です。M[1]完全な再パラメータ化がFPTになることとは異なる帰結です。

## 5．NPとEXPの分離が示されたとは言えない理由

**本カタログは、これらのSAT上界から確立した帰結として $\mathrm{NP}\ne\mathrm{EXP}$ を記録しません。** ここでは、

$$
\mathrm{EXP}=\bigcup_{d\ge1}\mathrm{DTIME}(2^{n^d}),\qquad
\mathrm{SUBEXP}=\bigcap_{\varepsilon>0}\mathrm{DTIME}(2^{n^\varepsilon})
$$

と定義します。このSUBEXPは、単に $2^{o(n)}$ 時間を許すことより強い条件です。クラスの定義と関連する時間階層による分離は [Vadhan, Lecture 5、第2節][vadhan07] に記載されています。

入力長 $n$ のNP言語からの多項式時間帰着が、$N=\Theta(n^d)$ 変数、符号長は多項式の3-CNFを生成する場合があります。原稿の一般密度の上界へ代入すると、

$$
2^{O\!\left(n^d\log\log n/\sqrt{\log n}\right)}n^{O(1)}
$$

という評価です。$d>1$ なら、この評価は $2^{o(n)}$ ではありません。次数は言語や帰着によって変わるため、この議論からNP全体を十分小さい決定性時間クラスに含められません。原稿もp.2の系1.3の直前で、帰着によるサイズ増大について注意しています。[IPZ01, 第1.1.4節][ipz01] も参照してください。

これに対して、仮に次の大幅に強い上界があれば、分離を導けます。

$$
\text{3SATが }2^{N^{o(1)}}L_{\mathrm{CNF}}^{O(1)}\text{ 時間}
\quad\Longrightarrow\quad
\mathrm{NP}\subseteq\mathrm{SUBEXP}\subsetneq\mathrm{EXP}.
$$

実際、$(n^d)^{o(1)}=n^{o(1)}$ なので、この形は任意の固定次数の多項式帰着を通して保たれます。最後の真包含は、決定性時間階層定理によります。[Vadhan, 第2節][vadhan07] を参照してください。上で挙げた単一アルゴリズムの上界は十分条件です。一方、共通部分で定義したSUBEXPでは、$\varepsilon$ ごとに異なるアルゴリズムを用いて構いません。

今回の指数 $N\log\log N/\sqrt{\log N}=N^{1-o(1)}$ は、$N^{o(1)}$ よりはるかに大きい形です。これは示された上界の保証を比べているのであって、将来の全てのSATアルゴリズムに対する下界ではありません。ここで指摘するのはこの推論の不足であり、**ETHとNP対EXPの論理的独立性を証明したという主張ではありません。**

高速化からこの種の分離が得られること自体はあります。例えばP = NPなら、既知の真包含P $\subsetneq$ EXPから直ちにNP $\subsetneq$ EXPが従います。今回の上界は、クラス全体についてそのような十分な改善を与えてはいません。

## 6．回路下界

### 6.1．Williamsの定理から得られる定性的な帰結

**由来：既存定理を仮定したSATアルゴリズムへ適用。状態：draft。**

[Williams, Theorem 6.1][williams10] は、

$$
\neg\mathrm{ETH}
\quad\Longrightarrow\quad
\mathrm{E}^{\mathrm{NP}}\not\subseteq\mathrm{SIZE}(O(n))
$$

を示しています。$\mathrm{E}^{\mathrm{NP}}$ はNPオラクルを使う決定性 $2^{O(n)}$ 時間、$\mathrm{SIZE}(O(n))$ はfan-inを定数に制限したAND・OR・NOTゲートによる非一様な線形サイズ回路のクラスです。固定した原稿にも、この適用が系C.1として記載されています。

必要なのは判定の保証だけです。$n$ 入力・$s$ ゲートの回路は、$O(n+s)$ 変数・節の3-CNFに符号化できます。そのため、線形サイズ回路に対してWilliamsの議論で使う劣指数時間のSATアルゴリズムを得ます。定性的な帰結には、一般密度の仮定と報告された形式化の仮定のいずれも単独で十分です。これはNP対EXPの分離でも、NPに対する超多項式回路下界でもありません。

### 6.2．元原稿に記載された定量的な系

**由来：source-corollary。状態：draft。依存：sat26-sparseと付録C.1の議論。**

[SAT26, Corollary 1.4、p.3][sat26] は、ある言語 $A\in\mathrm{E}^{\mathrm{NP}}$ と定数 $c>0$ が存在し、

$$
C_A(n)>c\,n\sqrt{\log n}\qquad\text{が無限に多くの }n\text{ で成り立つ}
$$

と述べています。$C_A(n)$ は、fan-inが高々2の任意のBooleanゲートと制限のないfan-outを許した場合の、**入力以外のゲートの最小数**です。証明は付録C.1、pp.51–52にあります。これは出典に明記された条件付きの系として記録するものであり、カタログがその証明や局所帰着の各要素を独立に検証したものではありません。より弱い報告された形式化の上界に、同じ定量的な回路下界を帰属させません。

## 7．一次資料と参照箇所

書誌情報は[共通の出典一覧](ASSUMPTIONS.ja.md#flum-grohe2004)にも登録しています。以下は本解説で使用した箇所です。

| 出典 | 版と参照箇所 |
| --- | --- |
| [SAT26: OpenAI, The Exponential Time Hypothesis is False][sat26] | 2026年10月25日改訂、Gitコミット42907c9e999cc00d6abe7a50bfc1023250edd1b9。定理1.1と帰着に関する注意（p.2）、系1.4（p.3）、第1.1.1–1.1.2節、付録C.1（pp.51–52）。 |
| [FG04: Flum–Grohe, Parameterized Complexity and Subexponential Time][fg04] | Bulletin of the EATCS 84 (2004), 71–100。Definition 4.2、Corollary 4.3、Theorem 4.4、Theorem 5.3、Corollaries 5.6・5.8・5.9、Proposition 5.10と直後、第7節。 |
| [CG06: Chen–Grohe, An Isomorphism between Subexponential and Parameterized Complexity Theory][cg06] | ECCC TR06-011 (2006)。第2.1節、Definition 13（印刷p.7）、Theorem 14（p.8）。番号はこの報告の版による。 |
| [M08: Müller, Parameterized Randomization][mueller08] | 2008年11月の博士論文。Example 3.2.17（印刷pp.125–126）、Appendix A.2（p.195）。 |
| [DF95: Downey–Fellows, Fixed-Parameter Tractability and Completeness II][df95] | 著者公開版。Introduction（Dominating Set）、Corollary 3.2（Clique）。 |
| [Marx10: Can You Beat Treewidth?][marx10] | Theory of Computing 6 (2010), 85–112。Theorem 1.5、p.89。 |
| [IPZ01: Which Problems Have Strongly Exponential Complexity?][ipz01] | JCSS 63(4) (2001), 512–530。第1.1.1–1.1.4節。 |
| [Vadhan07: Basic Derandomization Techniques][vadhan07] | CS225 Lecture 5、2007年2月15日。第2節、p.2、Definition 1とその直後の階層定理の記述。 |
| [Williams10: Improving Exhaustive Search Implies Superpolynomial Lower Bounds][williams10] | 2010年5月4日付の著者full version。Theorem 6.1、印刷pp.20–21（PDF pp.21–22）。雑誌版はSICOMP 42(3) (2013), 1218–1244。 |

[sat26]: https://github.com/openai/math/blob/42907c9e999cc00d6abe7a50bfc1023250edd1b9/preprints/The-Exponential-Time-Hypothesis-is-False-October-25-2026/paper.pdf
[fg04]: https://www.uni-ulm.de/fileadmin/website_uni_ulm/iui.inst.190/Mitarbeiter/toran/beatcs/column84.pdf
[cg06]: https://eccc.weizmann.ac.il/report/2006/011/download
[mueller08]: https://d-nb.info/993356915/34
[df95]: https://homepages.ecs.vuw.ac.nz/~downey/publications/2.pdf
[marx10]: https://toc.cs.uchicago.edu/articles/v006a005/v006a005.pdf
[ipz01]: https://cseweb.ucsd.edu/~russell/ipz.pdf
[vadhan07]: https://people.seas.harvard.edu/~salil/cs225/spring07/lecnotes/lec5.pdf
[williams10]: https://people.csail.mit.edu/rrw/improved-algs-lbs2.pdf
