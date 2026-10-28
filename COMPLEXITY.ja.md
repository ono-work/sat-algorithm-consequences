# 計算量理論への影響

[English](COMPLEXITY.md) · [トップ](README.ja.md) · [FPT](FPT.ja.md) · [厳密アルゴリズム](EXACT.ja.md) · [仮定と出典](ASSUMPTIONS.ja.md)

更新日：2026-10-29。**全体の状態：draft。** 調査・導出・執筆の大部分はCodexによるものです。各節の確認は第9節に記録し、人間による正しさの確認は記録されていません。アルゴリズムの帰結は明記したSATの仮定に依存します。無条件のクラスに関する事実、条件付きの導出、既存の予想は区別して記載します。出典の記載や機械的チェックは元の定理や個々の導出の正しさを認定しません。掲載自体は新規性の主張ではありません。

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
| XP | $\mathrm{FPT}\subsetneq\mathrm{XP}$ は無条件。一方、個々のXP上界には改善の余地がある | 第7節でクラス全体と小さい符号化からの移送を区別 |
| 対数で正規化したpathwidth | 分解が与えられたCliqueは正規化幅についてFPTになる | カタログ側の導出。第7節でXNLP完全なIndependent Set・Dominating Setと比較 |
| XNLPと領域 | 確認した移送から、クラス全体のFPT化やSPSCの反証は得ていない | 第7節に、そのために十分な追加の時間・領域上界を記載 |

これらは、個別アルゴリズム35項目・候補10項目の件数とは別の解説です。以下の対数は底2とし、小さい入力は別処理します。底を別の固定定数にしても、パラメータは定数倍の範囲でしか変わりません。

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

<a id="xp-xnlp"></a>

## 7．XP・XNLPと時間・領域の同時上界

SATの仮定から大きな改善を得られる問題がある一方、ここで確認した移送では解決されないアルゴリズム上の問いも残ります。この節では、無条件のクラス構造、条件付きのアルゴリズム、より広い帰結を得るために十分な追加の進展を分けて整理します。範囲を限定した確認の記録は第9節にあります。

個別アルゴリズムのカタログと同じく、

$$
A(s)=2^{O(s\log\log s/\sqrt{\log s})},\qquad
B(s)=2^{O(s/\sqrt{\log s})}
$$

と書き、小さい値での対数は適宜調整します。これらは、それぞれ原稿の一般密度・固定密度のプロファイルであり、$B$には対応する密度条件またはpaddingの議論が必要です。報告された形式化のプロファイルは、別の仮定として扱います。以下、$L$は与えられた分解なども含む元入力全体の符号長とし、パラメータに依存する関数は計算可能とします。

### 7.1．無条件の境界：FPTはXPの真部分クラス

**強一様なクラスについての、無条件の背景事実です。** $L$ を入力全体の符号長とします。FPTでは、計算可能な関数 $f$ と、$k$ に依存しない定数 $c$ に対して、単一のアルゴリズムが $f(k)L^c$ 時間で動くことを要求します。XPでは、例えば $L^{g(k)}+g(k)$ のように、計算可能な関数によって多項式の次数がパラメータに依存して構いません。この規約では、

$$
\mathrm{FPT}\subsetneq\mathrm{XP}
$$

が無条件で成り立ちます。[Downey, Definition 1・第2節][downey12] と、一様なXPの定義である [Chen–Grohe, Definition 11][cg06] を参照してください。

ここでは時間階層による議論が役立ちます。計算時間を制限した機械の受理問題を考えると、パラメータ $k$ とともに許す多項式の次数を増やし、各スライスで任意の決定性多項式時間機械を模倣する問題を、XPの中に作れます。これに多項式の次数 $c$ を固定したFPTアルゴリズムがあれば、十分大きいパラメータを固定することで、より大きい多項式次数の全言語を次数 $c$ で模倣できてしまいます。これは決定性時間階層定理に反します。乗法因子 $f(k)$ は、その固定したスライスでは定数です。

したがって、第2節の条件付きの一致 $\mathrm{M}[1]=\mathrm{FPT}$ が成立しても、XPとの外側の境界は残ります。特に、FPT帰着についてXP完全な問題は、無条件でFPTの外側です。これはXP完全性についての事実であり、XNLP完全性に置き換えてはいけません。残る問いは個々の問題や中間のクラスについてのもので、「FPTとXPが異なる」というETH依存の予想ではありません。

### 7.2．FPTへの十分な移送条件と、$L^{o(k)}$ 時間との違い

**既存の移送原理を、指定したSATの仮定へ適用します。** [Chen–Flum, Proposition 43][cf08] は、$2^{o_{\mathrm{eff}}(k\log L)}L^{O(1)}$ 時間ならFPTであることを示しています。ここで指数は、積 $k\log L$ 全体を引数とする、一つの実効的な劣線形関数です。必要な計算可能な収束速度は同論文の第2節で定義されています。これは十分条件であり、全てのFPT問題についての特徴づけをここで主張するものではありません。

固定したSATアルゴリズムの適用には、計算可能なパラメータ変換を許す次の具体形が使えます。入力長 $L$、パラメータ $k$ の問題を固定幅SATへの問い合わせで解くことができ、次を満たすとします。

- 全ての入力とパラメータ値に対し、同じ定数の節幅 $w$ を使う。
- 各問い合わせのBoolean変数数が、定数 $a$ と計算可能な関数 $h$ に対して $N\le a h(k)\log_2(L+2)$ である。**選択、検証、個数制約、Tseitin変換の全ての変数**を含める。
- 問い合わせの構成時間、各CNFの明示的な符号長に1を加えた値、問い合わせ回数、それ以外の計算時間が、必要なら計算可能な関数 $f$ と固定定数 $c$ を大きくして、いずれも $f(k)(L+2)^c$ 以下である。次の問い合わせを前の答えに応じて選んでもよい。

このとき [sat26-general](ASSUMPTIONS.ja.md#sat26-general) または [sat26-reported-formal](ASSUMPTIONS.ja.md#sat26-reported-formal) のいずれかからFPTアルゴリズムを得ます。必要なのは判定だけで、正確なSATの解の個数が正かどうかで各問い合わせに答えられます。

一様な上界を確かめるため、SATの時間を $2^{g(N)}(L_{\mathrm{CNF}}+1)^d$ と書きます。$g(N)=o_{\mathrm{eff}}(N)$ であり、固定した節幅に対して $d$ は定数です。$H(k)=\max\{1,\lceil a h(k)\rceil\}$ と置き、計算可能な閾値 $T(k)\ge1$ を

$$
N\ge T(k)\quad\Longrightarrow\quad g(N)\le N/H(k)
$$

となるように選びます。この範囲の問い合わせでは $2^{g(N)}\le 2^{N/H(k)}\le L+2$ です。$N<T(k)$ の場合、同じ因子は、計算可能でパラメータだけに依存する

$$
F_0(k)=\max_{0\le s<T(k)}2^{g(s)}
$$

以下です。小さい値は適宜定義しておきます。したがって各問い合わせの時間は $(F_0(k)+L+2)f(k)^d(L+2)^{cd}$ 以下となります。FPT回の呼出しと構成時間を含めても、計算可能な $F$ と、$k$ に依存しない定数 $c'$ に対して $F(k)(L+2)^{c'}$ です。仮定一覧の二つの明示的な指数プロファイルはいずれも必要な実効的収束を満たします。$F$ は非常に大きくても構わず、これは実用的なパラメータ依存性ではなく、FPTであるという主張です。

この条件と $L^{o(k)}=2^{o(k)\log L}$ は区別します。例えば $L^{\sqrt{k}}$ の指数は $k$ に関して劣線形ですが、この表示された時間上界を、単一の定数 $c$ による $F(k)L^c$ で抑えることはできません。さらに [Chen–Grohe, Lemma 18の証明の第1段階][cg06] は、任意のXP問題に対し、FPT同値で $L^{O(\sqrt{k})}$ 時間で解ける再パラメータ化を構成しています。したがって、この形の上界だけでは、XP完全問題であってもFPTを導けません。ここでは $2^{o_{\mathrm{eff}}(k\log L)}$ を、上で述べた積全体についての強い意味でのみ使います。

帰着に課した条件には実質的な内容があります。最後のSAT呼出しが速くても、$L^{h(k)}$ 時間の構成や同程度の問い合わせ回数がFPTになるわけではありません。また、証人を $h(k)\log L$ ビットで指定できることは、その証人を固定幅CNFで検証するための変数数の上界にはなりません。SATの多項式領域の保証から、この構成では $F_1(k)L^{O(1)}$ 領域を得ますが、$f(k)\log L$ 領域までは得られません。これらはこの移送原理の適用条件についての注意であり、別の帰着やアルゴリズムに対する不可能性の結果ではありません。

### 7.3．XNLP：非決定性計算の時間と領域を同時に制約するクラス

この小節の定義と困難性の事実は、SAT高速化の仮定によらず成り立ちます。元入力の符号長を$L$、パラメータを$k$とすると、XNLPは、ある計算可能関数$f$と定数$c$について、非決定性計算により

$$
T\le f(k)L^c,\qquad S\le f(k)\log L
$$

という時間・領域の上限を**同時に**満たして解ける判定問題のクラスです。$\mathrm{XNLP}\subseteq\mathrm{XP}$が成り立ちます。計算の状況を表す構成は高々$L^{g(k)}$個なので、その間の到達可能性を決定的に調べればXPアルゴリズムになります。[BGNS24, Sections 2.2–2.4, Lemma 1][bgns24]を参照してください。

完全性で使う帰着は**パラメータ付き対数領域帰着（pl帰着）**です。出力パラメータが入力パラメータの計算可能関数で抑えられ、帰着を決定的領域$g(k)+O(\log L)$で計算できることを要求します。XNLP困難な問題は、固定されたすべての正整数$t$についてW[$t$]困難です[BGNS24, Lemma 2][bgns24]。この事実は、通常のFPT帰着を使うクラス$\mathrm{W}[t]$が、そのままXNLPに包含されるという主張ではありません。

ここで問題になる包含は$\mathrm{XNLP}\subseteq\mathrm{FPT}$です。逆向きの$\mathrm{FPT}\subseteq\mathrm{XNLP}$を仮定すると、多項式時間問題に定数パラメータを与えることで$\mathrm{P}=\mathrm{NL}$が従ってしまいます[BGNS24, Lemma 3][bgns24]。したがって、ここでは$\mathrm{FPT}=\mathrm{XNLP}$とは書きません。XNLPがFPTに含まれるなら、上の困難性から、固定されたすべての$t$について$\mathrm{W}[t]=\mathrm{FPT}$が従います。

作業領域の上限が制約するのは、その時点で保持する情報量です。計算全体で行う非決定的な推測の総量や、その計算をSATの計算表で表現する際の変数数まで抑えるものではありません。したがって、作業領域が$f(k)\log L$であるだけでは、変数数$f(k)\log L$の固定幅SAT符号化は得られません。移送には、各段階を記録する変数や、段階間の整合性を確認する補助変数をすべて数える必要があります。

### 7.4．正の例：Log-Pathwidth Clique

**指定したSATの仮定からのカタログ側の導出（2026年10月29日、JST）。** この適用は、判定、最大クリークの頂点数、最大クリークの復元を扱います。新規性や新しい完全性の分類を主張するものではありません。

入力は、$n$ 個の頂点を明示した単純無向グラフ $G$、明示的に与えられた正しいパス分解 $B_1,\ldots,B_t$、および判定版では整数の目標値 $r$ です。$n\ge1$ について、

$$
p=\max_i|B_i|-1,\qquad s=p+1,\qquad
\kappa=\left\lceil\frac{p+1}{\log_2(n+2)}\right\rceil
$$

と置きます。$p$ は**入力として与えられた分解の幅**であり、各バッグの頂点数は高々 $p+1$ です。分解を含む入力全体の符号長を $L$ とします。頂点とバッグの記録が明示されているので、$n,t\le L$ です。分解の正しさは多項式時間で確認できます。最小幅の分解を計算するわけではありません。目標値 $r$ は通常の入力であり、パラメータではありません。

[BGNS24, 第4.2節][bgns24] は $n\ge2$ に対して $\alpha=\lceil p/\log_2 n\rceil$ を使っています。ここでの小さい値の調整は同値なパラメータ化で、$\alpha\le2\kappa$、$\kappa\le2\alpha+1$ です。$n=0$ と $n=1$ の入力は直接処理し、最大クリークの頂点数はそれぞれ0と1です。$p=0$、$n\ge1$ なら、分解が正しいことから辺のないグラフなので、最大クリークの頂点数は1です。$0\le r\le p+1$ の範囲外の目標値も直接判定できます。空クリークを許します。

**各バッグだけを調べればよい理由。** 一つの頂点が現れるバッグの添字は区間になります。クリークの頂点対はどこかのバッグに同時に現れるため、対応する区間は二つずつ交わります。有限個の区間が二つずつ交われば、左端の最大値は右端の最小値以下であり、全てに共通する添字があります。したがって全てのクリークは一つのバッグに含まれます。この観察は、[BGNS24, Proposition 19の直前、印刷p.20][bgns24] にも記されています。よって、

$$
\omega(G)=\max_{1\le i\le t}\omega(G[B_i])
$$

です。

**一つのバッグの固定幅符号化。** 頂点数 $b\le p+1$ のバッグ $B$ に対し、各頂点の選択変数 $x_v$ を置きます。$G[B]$ の各非辺 $uv$ に $\neg x_u\lor\neg x_v$ を加えます。さらに、[METHODS第2節](METHODS.md) の平衡二進加算器と比較器で $\sum_{v\in B}x_v\ge r$ を課します。必要なゲートは合計 $O(b)$ 個であり、各ゲートを同値で符号化すると節幅は高々3です。空バッグや不可能な閾値は直接処理します。全ての補助変数を含め、非自明な問い合わせは

$$
N=O(p+1),\qquad M=O((p+1)^2),\qquad w\le3
$$

を満たします。バッグ内で変数名を付け直せば符号長は $O((p+1)^2\log(p+2))$ です。全問い合わせの構成は $L$ の多項式時間です。二次になり得る節数は一般密度の仮定で扱えます。変数数が線形であることから、密度まで一定だと主張しているわけではありません。

一つの閾値の判定には高々 $t\le L$ 回のSAT呼出しを使います。したがって、原稿の一般密度プロファイルから

$$
A(p+1)L^{O(1)}
=2^{O\!\left((p+1)\log\log(p+1)/\sqrt{\log(p+1)}\right)}L^{O(1)}
$$

を得ます。小さい値での対数は適宜調整します。これとは独立に、報告された形式化のプロファイルからは

$$
2^{O\!\left((p+1)(\log\log(p+1))^2/\sqrt{\log(p+1)}\right)}L^{O(1)}
$$

を得ます。これらはそれぞれの仮定に基づく別々の条件付き保証であり、いずれも多項式領域です。定量的な上界を同一視しません。

**最適化と解の復元。** $0,\ldots,p+1$ を二分探索すれば、$O(\log(p+2))$ 回の閾値判定で $\omega(G)$ を求められます。最適値を達成できるバッグを一つ探して保持し、その元の選択ビットを順に固定します。最適値の閾値の下で延長できる値を、SATへの問い合わせで残します。追加の高々 $p+1$ 回で、そのバッグ内の実際の最大クリークを復元できます。呼出しは合計 $O(t\log(p+2)+p+1)$ 回で、ビット固定の単位節を加えても上の符号化の上界を保ちます。各回を逐次実行するので、同じ時間の形と多項式領域の保証が成り立ちます。

**正規化した幅に関してFPTになる理由。** どちらの指数因子も、$g(s)=o_{\mathrm{eff}}(s)$ による $2^{g(s)}$ と書けます。$s\le\kappa\log_2(n+2)$ なので、$s\ge T(\kappa)$ なら $g(s)\le s/\kappa$ となる計算可能な閾値 $T(\kappa)$ を選びます。この範囲では、

$$
2^{g(s)}\le2^{s/\kappa}\le n+2\le L+2
$$

です。$s<T(\kappa)$ なら、指数因子は $\kappa$ だけに依存する計算可能な関数で抑えられます。多項式の構成・問い合わせコストを掛けても、判定・最適化・復元のいずれも $F(\kappa)L^{O(1)}$ となります。与えられた幅に関する通常の単一指数の依存を、ここでは実効的な劣指数の依存へ置き換えられるため、この小さい正規化パラメータでFPTになります。

このバッグ内への分解には、明確な適用範囲があります。一つのクリークが複数のバッグに現れ得るので、バッグごとの個数を加えると重複して数えます。そのような計数の集約は主張しません。一方、[BGNS24, Theorems 17・18、印刷pp.17–18][bgns24] は、Log-Pathwidth Dominating SetとLog-Pathwidth Independent Setが、パラメータ化対数領域帰着についてXNLP完全であると示しています。これらのどちらかに、与えられた幅 $p$ に関する決定性 $2^{o_{\mathrm{eff}}(p)}L^{O(1)}$ アルゴリズムがあれば、同じ閾値の議論と完全性から $\mathrm{XNLP}\subseteq\mathrm{FPT}$ が従います。カタログの現在の $A(n)L^{O(1)}$ アルゴリズムは頂点総数を尺度にしており、$A(p)L^{O(1)}$ を示してはいません。与えられた分解は幅が小さくても多数のバッグを持ち得るため、$n$ を $p$ に置き換えるには新たな議論が必要です。ここにその議論がないことから、分離や論理的独立性は導きません。

### 7.5．XP時間と小さい領域を同時に満たす必要がある

**Slice-wise Polynomial Space Conjecture（SPSC）**は、XNLP完全問題について、決定的アルゴリズムが

$$
T\le L^{f(k)},\qquad S\le g(k)L^c
$$

を同時に満たすことはないという予想です。$f,g$は計算可能関数で、定数$c$は$k$にも$L$にも依存しません。これは[PW16, Conjecture 2, Corollary 13][pw16]のXP時間・FPT領域という定式化であり、[BGNS24, Conjectures 30–31][bgns24]にも記載されています。領域の上界に現れる多項式の次数を固定することが重要です。XP時間のアルゴリズムとFPT領域のアルゴリズムが別々にあっても、この同時保証にはなりません。

現在のSAT仮定に対して、次の二つの一般的な代入だけでは、このようなシミュレーションは得られません。

1. **XPサイズの式を構成する場合。** 明示的な帰着後の式の符号長が$L'=L^{h(k)}$なら、その符号長に関する多項式領域の保証から得られるのは$(L')^{O(1)}=L^{O(h(k))}$です。この表示上界はXP領域であり、固定された$c$に対する$g(k)L^c$を保証しません。式の構成時間とSATの実行時間も別途含める必要があります。
2. **FPTサイズの計算表を構成する場合。** FPT時間で動くXNLPの計算を展開すると、固定された$d$について$N+M\le F(k)L^d$となる固定幅の式を構成できます。この場合、多項式領域はFPT領域になります。しかし、直接の代入で得られる時間保証は$A(F(k)L^d)F(k)L^{O(1)}$という形であり、$k$を固定したときに$L$の多項式であることを保証しません。したがって、この経路からXP時間は得られません。疎な場合の$B(F(k)L^d)$を使っても、この問題は解消しません。

これは表示された保証を使う議論の限界であり、あらゆる符号化やアルゴリズムに対する下界ではありません。

具体的に、幅$p$のパス分解が与えられた、明示的に表現された$n$頂点グラフ上のIndependent SetまたはDominating Setを考えます。分解も入力長$L$に含め、第7.4節と同じ、微小な入力に配慮したパラメータ

$$
\kappa=\left\lceil\frac{p+1}{\log_2(n+2)}\right\rceil
$$

を用います。両方の判定問題は、このパラメータについてXNLP完全です。これは、元の出典の対数パス幅パラメータと、定数倍および微小な場合の調整を除いて同値です[BGNS24, Section 4.2, Theorems 17–18][bgns24]; [Linear-XNLP25, Theorem 6][linear-xnlp25]。**どちらか一方**の問題についても、次の上界は異なる強さを持ちます。

| 分解が与えられた問題に対する時間上界 | 領域上界 | パラメータ$\kappa$での帰結 |
| --- | --- | --- |
| 現在の一般入力への移送：$A(n)L^{O(1)}$ | $L^{O(1)}$ | この代入では、$\kappa$についてXP時間やFPT時間を保証しない。$\kappa$が定数でも、グラフは大きくなり得る。 |
| 仮想的な$2^{O(p)}L^{O(1)}$時間 | $p^{O(1)}L^{O(1)}$ | $\kappa$についてXP時間かつFPT領域となり、SPSCを反証する。 |
| 仮想的な$2^{o_{\mathrm{eff}}(p)}L^{O(1)}$時間 | この含意に追加の領域仮定は不要 | $\kappa$についてFPTとなり、完全性から$\mathrm{XNLP}\subseteq\mathrm{FPT}$が従う。 |

第2行では、$p\le\kappa\log_2(n+2)$を代入すると、時間は$(n+2)^{O(\kappa)}L^{O(1)}$となります。一方、表示された領域上界の多項式の次数は$\kappa$に依存しません。第3行では、$p\le\kappa\log_2(n+2)$に対して第7.2節の計算可能な閾値を使う議論を適用します。有界な$p$の場合は$\kappa$の計算可能関数に吸収できます。得られたアルゴリズムは、完全性を与えるpl帰着を通じて移送できます。正規化したパス幅と時間・領域の同時上界の関係は、[PW16, Theorems 14–15][pw16]でも扱われています。

第2・第3行は、各帰結に十分な**仮想的な進展**を示したもので、現在のSAT仮定から得たアルゴリズムではありません。これに対して、第7.4節のCliqueでは、個々のbagに問題を制限することにより、$p+1$に関する計算可能な収束率を持つ劣指数時間が実際に得られます。

### 7.6．残る障害をどう解釈するか

ここでは、三種類の主張を区別する必要があります。

- **無条件のクラス分離。** 第7.1節の$\mathrm{FPT}\subsetneq\mathrm{XP}$はクラス全体に関する事実です。特定のW困難問題やXNLP完全問題がFPTの外にあることまで特定するものではありません。
- **困難性の定理と仮説に基づく障害。** 出典にある帰着による困難性の定理は、そのまま有効です。アルゴリズムが存在しないと結論するには、対応するクラスが一致しないという仮定や、SPSCのような時間・領域に関する予想が必要です。それぞれの仮定を明示する必要があります。
- **現在の移送方法の限界。** 全補助変数の計数、節幅の制御、時間と領域の同時上界の確保は、引き続き必要です。これらを満たせないときに分かるのは、その経路では不十分だということであり、問題に対する不可能性の定理ではありません。

固定したSAT仮定の下では、ETHに依存する下界は前提を失います。含意そのものは数学の定理として残りますが、その前提によってアルゴリズムを排除することはできなくなります。アルゴリズムは、それでも別途構成しなければなりません。この区別は、XPの内部で指数を細かく比較する場合にも当てはまります。

明示的なSAT式に関する多項式領域も、元入力長に関する対数領域のシミュレーションを与えるものではありません。XLとXNLは、それぞれ決定的・非決定的領域$f(k)\log L$を使うクラスで、XNLPのような追加のFPT時間制約を課しません[BGNS24, Section 2.2][bgns24]; [PW16, Section 3][pw16]。ここでは、$\mathrm{XNLP}\subseteq\mathrm{XL}$も、$\mathrm{XL}=\mathrm{XNL}$も導いていません。

この意味で、残る障害は依然として大きいと言えます。アルゴリズムそのものの構成が必要な場合もあれば、帰着を通じて所定の資源上界を保つことが課題になる場合もあります。この解説は、SAT仮定と未解決のクラス・領域計算量の問題との論理的独立性を証明するものではありません。

## 8．一次資料と参照箇所

書誌情報は[共通の出典一覧](ASSUMPTIONS.ja.md#出典一覧)にも登録しています。以下は本解説で使用した箇所です。

| 出典 | 版と参照箇所 |
| --- | --- |
| [SAT26: OpenAI, The Exponential Time Hypothesis is False][sat26] | 2026年10月25日改訂、Gitコミット42907c9e999cc00d6abe7a50bfc1023250edd1b9。定理1.1と帰着に関する注意（p.2）、系1.4（p.3）、第1.1.1–1.1.2節、付録C.1（pp.51–52）。 |
| [FG04: Flum–Grohe, Parameterized Complexity and Subexponential Time][fg04] | Bulletin of the EATCS 84 (2004), 71–100。Definition 4.2、Corollary 4.3、Theorem 4.4、Theorem 5.3、Corollaries 5.6・5.8・5.9、Proposition 5.10と直後、第7節。 |
| [CG06: Chen–Grohe, An Isomorphism between Subexponential and Parameterized Complexity Theory][cg06] | ECCC TR06-011 (2006)。第2.1節、Definitions 11・13（印刷p.7）、Theorem 14（p.8）、Lemma 18の証明の第1段階（p.9）。番号はこの報告の版による。 |
| [M08: Müller, Parameterized Randomization][mueller08] | 2008年11月の博士論文。Example 3.2.17（印刷pp.125–126）、Appendix A.2（p.195）。 |
| [DF95: Downey–Fellows, Fixed-Parameter Tractability and Completeness II][df95] | 著者公開版。Introduction（Dominating Set）、Corollary 3.2（Clique）。 |
| [Marx10: Can You Beat Treewidth?][marx10] | Theory of Computing 6 (2010), 85–112。Theorem 1.5、p.89。 |
| [IPZ01: Which Problems Have Strongly Exponential Complexity?][ipz01] | JCSS 63(4) (2001), 512–530。第1.1.1–1.1.4節。 |
| [Vadhan07: Basic Derandomization Techniques][vadhan07] | CS225 Lecture 5、2007年2月15日。第2節、p.2、Definition 1とその直後の階層定理の記述。 |
| [Williams10: Improving Exhaustive Search Implies Superpolynomial Lower Bounds][williams10] | 2010年5月4日付の著者full version。Theorem 6.1、印刷pp.20–21（PDF pp.21–22）。雑誌版はSICOMP 42(3) (2013), 1218–1244。 |
| [Downey12: A Basic Parameterized Complexity Primer][downey12] | 2012年の章の著者公開38ページ版。Definition 1（PDF p.5）、第2節（PDF p.8）。強一様なFPTと無条件の真包含FPT ⊊ XP。 |
| [CF08: Chen–Flum, Subexponential Time and Fixed-Parameter Tractability: Exploiting the Miniaturization Mapping][cf08] | 2008年8月20日付の著者プレプリント。第2節（印刷p.4）、Proposition 43（pp.26–27）。実効的なlittle-oと、k log Lに関する実効的な劣指数時間からFPTへの移送。 |
| [BGNS24: Bodlaender–Groenland–Nederlof–Swennenhuis, Parameterized problems complete for nondeterministic FPT time and logarithmic space][bgns24] | Information and Computation 300 (2024), 105195。第2.2–2.3節（印刷p.4）、第2.4節のLemmas 1–3（p.5）、第4.2節のTheorems 17–18（pp.17–18）、Proposition 19直前のCliqueの議論（p.20）。第5.1節のConjectures 30–31（p.31）。雑誌版の番号を用い、リポジトリのPDFには表紙が1ページ付く。 |
| [Linear-XNLP25: Bodlaender–Groenland–Jacob–Jaffke–Lima, XNLP-Completeness for Parameterized Problems on Graphs with a Linear Structure][linear-xnlp25] | Algorithmica 87 (2025), 465–506。オンライン公開は2024年11月4日。IntroductionのConjecture 1・Footnote 2、Theorem 6、第3.1–3.2節。 |
| [PW16: Pilipczuk–Wrochna, On Space Efficiency of Algorithms Working on Structural Decompositions of Graphs][pw16] | STACS 2016、LIPIcs 47、Article 57。Conjecture 2（印刷p.57:3）、第3節のTheorem 12・Corollary 13・Theorems 14–15（pp.57:8–57:9）。番号はSTACS版による。 |

## 9．範囲を限定した確認記録

- **確認日：**2026年10月29日（JST）。
- **状態と担当：**第7節のみ `ai-audited`。確認者の種別はAI、担当は独立したCodexエージェント `transfer_audit`。
- **対象：**英日第7節のクラスの定義・帰着規約、実効的な閾値による移送、全変数を含めた符号化サイズと与えられた幅を使うCliqueアルゴリズム、最適化・解の復元の呼出し回数、正規化したパラメータの同値性、時間・領域の同時上界の含意、および第8節・共通出典一覧に記載した対応する一次資料の参照箇所。この確認では、実質的な数学的誤りや日英の不一致は見つかりませんでした。
- **範囲外：**人間による正しさの確認、仮定したSAT定理の検証、既存の第2–6節の再監査は含みません。解説全体の状態は `draft` のままです。生成・既存データの維持に関する機械的チェックは、この数学的な確認とは別です。

確認した各ファイルの第7節は、`<a id="xp-xnlp"></a>` から、その次の `## 8` 見出しの直前までのUTF-8文字列です。末尾の空行も含めた指紋は次のとおりです。

| ファイルと範囲 | バイト数 | SHA-256 |
| --- | ---: | --- |
| `COMPLEXITY.md` 第7節 | 19,225 | `4cd67cb0739e917ecd7468590813ca7a6090a69383d5cda2dc068041c5697551` |
| `COMPLEXITY.ja.md` 第7節 | 23,308 | `9b99786939cb217b50c3c5fb700f19ccc6cfa3fa80fa52c07dd004da8003ce63` |

[sat26]: https://github.com/openai/math/blob/42907c9e999cc00d6abe7a50bfc1023250edd1b9/preprints/The-Exponential-Time-Hypothesis-is-False-October-25-2026/paper.pdf
[fg04]: https://www.uni-ulm.de/fileadmin/website_uni_ulm/iui.inst.190/Mitarbeiter/toran/beatcs/column84.pdf
[cg06]: https://eccc.weizmann.ac.il/report/2006/011/download
[mueller08]: https://d-nb.info/993356915/34
[df95]: https://homepages.ecs.vuw.ac.nz/~downey/publications/2.pdf
[marx10]: https://toc.cs.uchicago.edu/articles/v006a005/v006a005.pdf
[ipz01]: https://cseweb.ucsd.edu/~russell/ipz.pdf
[vadhan07]: https://people.seas.harvard.edu/~salil/cs225/spring07/lecnotes/lec5.pdf
[williams10]: https://people.csail.mit.edu/rrw/improved-algs-lbs2.pdf
[downey12]: https://homepages.ecs.vuw.ac.nz/~downey/publications/basic.pdf
[cf08]: https://basics.sjtu.edu.cn/~chen/papers/subeptjlc.pdf
[bgns24]: https://research-portal.uu.nl/ws/portalfiles/portal/236951425/1-s2.0-S0890540124000609-main.pdf
[linear-xnlp25]: https://link.springer.com/article/10.1007/s00453-024-01274-9
[pw16]: https://drops.dagstuhl.de/storage/00lipics/lipics-vol047-stacs2016/LIPIcs.STACS.2016.57/LIPIcs.STACS.2016.57.pdf
