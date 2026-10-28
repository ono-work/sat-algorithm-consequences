# Shared methods / 共通の導出

These are elementary arguments used by both the FPT and exact-algorithm entries. They are catalog deductions and remain subject to review. The SAT algorithms are assumptions recorded in [ASSUMPTIONS.md](ASSUMPTIONS.md). Exact encodings are not implementations or independent verification of those assumed algorithms.

以下はFPTと厳密アルゴリズムの両方で共通に使う初等的な導出です。カタログ側の議論として記録し、確認の対象にします。SATアルゴリズムの仮定は[仮定一覧](ASSUMPTIONS.ja.md)にあります。符号化の導出は、仮定したアルゴリズムの実装や独立検証を意味しません。

## 1. Quantitative transfer / 計算時間の移送

A polynomial-time reduction to a fixed-width CNF with b(k) variables gives

$$
2^{O(b(k)\log\log b(k)/\sqrt{\log b(k)})}L^{O(1)}
$$

under the general manuscript profile. The width and all problem constants must be fixed. The output encoding length must be polynomial in the original input length L. Space is polynomial when both stages use polynomial space.

固定幅CNFへ多項式時間で帰着し、変数数がb(k)以下なら、原稿の一般密度の仮定から上の時間を得ます。幅や問題の定数は固定し、出力の符号長は元の入力長Lの多項式以下である必要があります。全段階が多項式領域なら合成も多項式領域です。

For b(k)=k(log k)^a, the exponent is O(k(log k)^{a−1/2} log log k). In particular, O(k log k) variables do not establish an improvement over single-exponential dependence on k through this substitution. The description “almost linear” alone is insufficient.

b(k)=k(log k)^aなら、指数はO(k(log k)^{a−1/2} log log k)です。特にO(k log k)変数という評価を代入しても、kに関する単一指数時間の改善にはなりません。「ほぼ線形」という表現だけでは判断できません。

For exact algorithms, replace k by the explicitly declared size measure s (for example n, m or n+m) and use b(s). This mathematical substitution does not allow changing the meaning of s. An O(m)-size encoding gives an edge-measure bound; it gives a vertex-measure subexponential bound only when m=O(n) or another sufficient density relation is established. The full bit length L remains in the polynomial factor, including numerical data and the description of isolated objects.

厳密アルゴリズムではkを明記したサイズ尺度s（例えばn、m、n+m）に替え、b(s)を代入します。その際にsの意味を変えることはできません。O(m)サイズの符号化は辺数を尺度にした上界を与え、頂点数に関する劣指数上界を得るにはm=O(n)などの十分な密度条件が必要です。数値データや孤立した対象の記述も含め、入力全体のビット長Lを多項式因子に残します。

## 2. Cardinality in linear size / 個数制約の線形符号化

The binary-counter/comparator approach is an established technique; see Carsten Sinz, [*Towards an Optimal CNF Encoding of Boolean Cardinality Constraints*](https://www.carstensinz.de/techreports/CardConstraints.pdf), CP 2005, author technical report §4, especially §§4.1–4.2 and Theorem 2. Below we give a full-equivalence implementation so that its variable count, width and counting behavior can be checked directly. These properties are derived from the gates below, rather than attributed to every optimized encoding in that paper.

二進カウンタと比較器を使う方法は既知の手法です。Carsten Sinzの[上記論文](https://www.carstensinz.de/techreports/CardConstraints.pdf)（CP 2005、著者の技術報告版§4、特に§§4.1–4.2とTheorem 2）を参照してください。以下では全ゲートを同値で符号化する実装を示し、変数数・節幅・計数の保存を直接確認できるようにします。これらの性質は以下のゲートから導くもので、同論文の最適化された符号化がすべて同じ性質を持つという意味ではありません。

### 2.1 Exact gate clauses / ゲートを表す節

Use a fresh output variable z for each binary gate. The following CNFs express the indicated equivalences exactly; each uses at most four clauses of width at most three.

二入力ゲートごとに新しい出力変数zを置きます。次のCNFは各同値を正確に表し、一ゲート当たり高々4節、各節の幅は3以下です。

$$
\begin{aligned}
z\leftrightarrow(a\land b)\quad&:\quad
(\neg z\lor a)\land(\neg z\lor b)\land(z\lor\neg a\lor\neg b),\\
z\leftrightarrow(a\lor b)\quad&:\quad
(z\lor\neg a)\land(z\lor\neg b)\land(\neg z\lor a\lor b),\\
z\leftrightarrow(a\oplus b)\quad&:\quad
(a\lor b\lor\neg z)\land(a\lor\neg b\lor z)\\
&\phantom{{}:\quad}\land(\neg a\lor b\lor z)\land(\neg a\lor\neg b\lor\neg z).
\end{aligned}
$$

Here ⊕ is exclusive OR. Literals such as ¬s can be used as gate inputs, and fixed constants can be substituted and simplified. In an acyclic circuit, the input bits determine every gate output uniquely. Adding a unit clause requiring the final output to be true therefore preserves the number of accepted input assignments. Merely preserving satisfiability with one-way implications would not by itself justify this counting claim.

⊕は排他的論理和です。ゲートの入力には¬sのようなリテラルも使え、定数は代入して簡約できます。閉路のない回路では、入力ビットから全ゲートの出力が一意に決まります。最終出力を真にする単位節を加えると、受理される入力割当てと充足割当てが一対一に対応します。片向きの含意で充足可能性だけを保存しても、この計数の主張はそれだけでは正当化されません。

### 2.2 A balanced addition tree / 平衡加算木

To encode x₁+⋯+xᵣ≤K, first handle trivial bounds (including r=0, K<0 and K≥r). Sum the r input bits in a balanced binary tree, padding to the next power of two with zero constants if necessary. Each node adds the binary sums of its two children. A ripple-carry adder needs a constant number of the above gates per bit, so adding O(j)-bit numbers uses O(j) gates.

x₁+⋯+xᵣ≤Kを符号化するには、まずr=0、K<0、K≥rなどの自明な場合を処理します。r個の入力ビットを平衡二分木状に加算し、必要なら次の2の冪まで定数0で埋めます。各節点で二つの子の二進和を足します。桁上がりを順に伝える加算器は一桁当たり定数個の上記ゲートで作れるため、O(j)ビットの数の加算はO(j)ゲートで済みます。

At level j there are O(r/2^j) such additions. The upper levels add longer numbers but contain fewer adders; all intermediate sum and carry gates are included in

第j段にはこの加算がO(r/2^j)個あります。上段ほど足す数の桁数は増えますが、加算器の個数は減ります。途中の和と桁上がりを含む総ゲート数は

$$
O\!\left(\sum_{j\ge1}\frac{rj}{2^j}\right)=O(r)
\qquad\left(\sum_{j\ge1}\frac{j}{2^j}=2\right).
$$

The sum has b=O(log(r+1)) bits, enough to represent every value from 0 to r. Thus the counter uses O(r) auxiliary variables and O(r) clauses, not O(r log r).

和は0からrまでを表せるb=O(log(r+1))ビットです。したがって、カウンタの補助変数と節はO(r)個であり、O(r log r)個にはなりません。

### 2.3 Comparing the binary sum with K / 二進和とKの比較

Write the sum as s_{b−1}⋯s₀ and the fixed threshold as K_{b−1}⋯K₀, with leading zeros as needed. Set q₀=1 and process bits from least significant to most significant. Define

和をs_{b−1}⋯s₀、固定閾値をK_{b−1}⋯K₀と二進表記し、必要なら先頭を0で埋めます。q₀=1とし、下位ビットから上位ビットへ次のように定義します。

$$
q_{j+1}\leftrightarrow
\begin{cases}
\neg s_j\land q_j,&K_j=0,\\
\neg s_j\lor q_j,&K_j=1,
\end{cases}
\qquad 0\le j<b.
$$

The invariant is that q_j is true exactly when the lowest j bits of the sum represent a number at most the lowest j bits of K. The next bit is more significant than all the bits already processed, which gives the two cases above. Encode each equivalence using §2.1 and add the unit clause q_b. The threshold bits are constants; q₀ can also be a constant or a variable fixed by a unit clause. The comparator therefore adds only O(log(r+1)) gates and clauses of width at most three.

q_jは「和の下位jビットが表す数が、Kの下位jビットが表す数以下である」ときに限り真、という不変条件を持ちます。次のビットはそれまでの全ビットより上位なので、上の二通りで比較を更新できます。各同値を§2.1の節に展開し、最後に単位節q_bを加えます。閾値の各ビットは定数です。q₀も定数として扱うか、単位節で固定します。比較器が加えるのはO(log(r+1))ゲートと、幅3以下の節だけです。

Together, the counter and comparator give O(r) auxiliary variables and O(r) clauses of width at most three, with a unique extension precisely for the inputs satisfying the cardinality bound. The same sum circuit also supports ≥ (compare K−1 and require a false comparison output) and = (fix the sum bits to K), after handling trivial bounds.

カウンタと比較器を合わせても、補助変数O(r)、節O(r)、節幅3以下です。個数制約を満たす入力に限り、充足割当てへの拡張が一意に存在します。自明な場合を処理した上で、同じ和の回路から≥（K−1との比較結果を偽に固定）や=（和のビットをKに固定）も扱えます。

### 2.4 Four inputs with a bound of two / 4入力の和を2以下にする例

For x₁+x₂+x₃+x₄≤2, first add two pairs of input bits, then add their two-bit results. The following are all binary gates of the types in §2.1:

x₁+x₂+x₃+x₄≤2では、まず入力を二つずつ足し、その2ビットの結果どうしを足します。以下はすべて§2.1の二入力ゲートです。

$$
\begin{aligned}
a_0&\leftrightarrow x_1\oplus x_2,&a_1&\leftrightarrow x_1\land x_2,\\
b_0&\leftrightarrow x_3\oplus x_4,&b_1&\leftrightarrow x_3\land x_4,\\
s_0&\leftrightarrow a_0\oplus b_0,&c&\leftrightarrow a_0\land b_0,\\
t&\leftrightarrow a_1\oplus b_1,&s_1&\leftrightarrow t\oplus c,\\
u&\leftrightarrow a_1\land b_1,&v&\leftrightarrow t\land c,\\
s_2&\leftrightarrow u\lor v.&&
\end{aligned}
$$

These gates enforce x₁+x₂+x₃+x₄=4s₂+2s₁+s₀. The desired comparison is just the following two clauses:

これらのゲートによりx₁+x₂+x₃+x₄=4s₂+2s₁+s₀となります。求める比較は次の2節です。

$$
\neg s_2\ \land\ (\neg s_1\lor\neg s_0).
$$

They allow the binary sums 000, 001 and 010, and reject 011 (three) and 100 (four). Expanding each gate with §2.1 and conjoining these two clauses gives the actual CNF; the arithmetic equality is an explanation of its meaning, not an additional SAT primitive.

二進和000、001、010を許し、011（3）と100（4）を拒否します。各ゲートを§2.1で展開し、この2節とすべてANDで結んだものが実際のCNFです。算術の等式はその意味の説明であり、SATに別の演算を追加するわけではありません。

### 2.5 Vertex Cover: variables and clauses / Vertex Coverの変数数と節数

In the [Vertex Cover entry](docs/en/vertex-cover-solution-size.md), the unweighted kernel has r≤2k vertices and a reduced budget k′. Assign one selection bit x_v to each kernel vertex. Every kernel edge uv contributes the binary clause x_u∨x_v directly, using no auxiliary variable. Add the circuit above for ∑_v x_v≤k′. For a simple kernel graph,

[Vertex Coverの項目](docs/ja/vertex-cover-solution-size.md)で用いる非重み付きカーネルはr≤2k頂点と残りの予算k′を持ちます。各頂点に選択ビットx_vを一つ置き、各辺uvには二項節x_u∨x_vを直接置きます。辺ごとの補助変数は不要です。∑_v x_v≤k′には上の回路を加えます。単純グラフなので、

$$
N=r+O(r)=O(k),\qquad
M=|E'|+O(r)=O(k^2),\qquad
\text{width}\le3.
$$

The general-density SAT assumption permits polynomially many clauses: substituting these counts gives A(k)L^{O(1)}, including polynomial-time kernelization. Here the linear quantity is the variable count; the clause count may be quadratic. A B(k) bound cannot be read from the sparse assumption when |E′|=Θ(k²). Applying the forced-variable padding of §5 in that case would introduce Θ(k²) variables, losing the O(k) variable bound.

一般密度のSATの仮定は多項式個の節を許すため、上の変数数と節数を代入すると、多項式時間のカーネル化も含めてA(k)L^{O(1)}を得ます。線形なのは変数数であり、節数は二次になり得ます。|E′|=Θ(k²)なら、疎なSATの仮定からB(k)をそのまま得ることはできません。この場合に§5の固定変数による密度調整をするとΘ(k²)個の変数を加えることになり、O(k)変数という評価を失います。

Unique gate extension does not make an earlier decision kernel counting-preserving.

ゲート値への拡張が一意でも、その前の判定カーネルが計数を保存するとは限りません。

## 3. Finite-obstruction vertex deletion / 有限障害型の頂点削除

Suppose a fixed finite family of forbidden (induced or noninduced) graphs has a genuine unweighted O(k)-vertex kernel for its vertex-deletion problem. Use one deletion bit per kernel vertex. For each forbidden copy S add a clause saying that at least one vertex in S is deleted. Add the linear cardinality circuit. If the largest forbidden graph has h vertices, the width is at most max(h,3), the variable count is O(k), and the clause count is polynomial for fixed h.

固定された有限族の禁止グラフに対する頂点削除問題が、同じ非重み付き問題へのO(k)頂点カーネルを持つとします。各頂点に削除変数を置き、各禁止コピーSについて少なくとも一頂点を削除する節を置き、線形サイズの個数制約を加えます。最大の禁止グラフがh頂点なら、幅はmax(h,3)以下、変数O(k)、節数は固定hに対して多項式です。誘導・非誘導のいずれにも使えます。

Use the fixed-width SAT theorem at that width. Replacing every longer constant-width clause by a chain of 3-clauses can add as many variables as there are clauses and destroy the desired bound.

この幅のままSAT定理を使います。各節を3節の鎖に分解すると節数に比例した補助変数が必要になり、目的の変数数を失う場合があります。

The same construction on the original n-vertex graph gives an exact-algorithm consequence without requiring a kernel. There are O(n) variables and O(n^h+n) clauses of width at most max(h,3). Enumerating all forbidden copies takes polynomial time for fixed h. Every deletion set corresponds to exactly one assignment, including the unique cardinality-gate extension when a size bound is imposed. Thus A(n)L^{O(1)} suffices to count all valid deletion sets or those of a specified size, find the minimum size, and recover an optimum set. This is a consequence measured by n; the A(k) decision consequence still requires the stated linear kernel. No counting claim is transferred through an ordinary decision kernel.

同じ構成を元のn頂点グラフに直接使えば、カーネルを必要としない厳密アルゴリズムの帰結を得ます。変数はO(n)個、節はO(n^h+n)個、幅はmax(h,3)以下で、hが固定なら全禁止コピーの列挙は多項式時間です。各削除集合は一つの割当てに対応し、サイズ制約を課した場合も個数回路への拡張は一意です。したがってA(n)L^{O(1)}で、有効な削除集合全体や指定サイズの集合を数え、最小サイズを求め、最適集合を復元できます。これはnを尺度にした帰結です。kを尺度にしたA(k)の判定上界には、引き続き上記の線形カーネルが必要であり、通常の判定カーネルを通して計数の帰結を主張するものではありません。

## 4. Small extension witnesses / 小さい延長不能証人

For a fixed-domain problem, keep only the variables of a small modulator when every failed extension has a constant-size witness in the modulator. Enumerate the witness assignments and forbid them by bounded-arity constraints. This gives O(k) variables even when the whole input has many vertices. The fixed-q coloring examples rely on the cited NO-certificate lemmas; existence of a polynomial-time extension test alone is not enough.

固定領域の問題で、modulatorへの割当てが延長不能なとき、その不能性がmodulator上の定数サイズの部分割当てで証明できるなら、それらを全て禁止する制約を作れます。入力全体が大きくても変数をO(k)に保てます。固定qの彩色の項目では引用したNO証明の補題を使います。延長可能性の多項式時間判定だけでは、この性質は導けません。

The resulting Boolean count usually counts boundary assignments, not their extensions. Counting requires separate treatment.

このBoolean式の計数は通常、境界への割当て数を数え、拡張数は数えません。計数版は別途扱う必要があります。

## 5. Counting-preserving density padding / 計数を保存する密度調整

Suppose an encoding has N variables and M clauses with N+M=O(s), but no direct ratio bound M≤DN is recorded. Add M new variables z_i and the M unit clauses ¬z_i. Then

$$
N'=N+M,\qquad M'=2M\le2N'.
$$

Every satisfying assignment has exactly one extension. The clause width stays fixed, the counts are unchanged, and the sparse SAT assumption applies with D=2 and N'=O(s). The bound is B(s) times a polynomial in L. This applies to the exact counting and optimization encodings below. Adding free variables is another valid option only if their known multiplicity is corrected for counting; forced variables avoid that extra correction.

符号化でN変数・M節、N+M=O(s)と分かっていても、M≤DNという比の評価が直接ない場合があります。このとき新しいM変数z_iと、それらを0に固定するM個の単位節¬z_iを加えると、上式のとおりN'=N+M、M'=2M≤2N'になります。各充足割当ての拡張は一意で、節幅は固定のまま、解の個数も変わりません。D=2として疎なSATの仮定を使い、B(s)にLの多項式を掛けた上界を得ます。以下の計数・最適化の符号化にも使えます。自由変数を加える方法では計数の多重度補正が必要ですが、0に固定すれば補正を避けられます。

## 6. Direct parsimonious encodings / 解の個数を保存する直接符号化

For a fixed domain [q] and maximum arity r, use q one-hot Boolean variables per CSP variable. For every forbidden tuple of a constraint, add the clause that forbids selecting that tuple. Each constraint contributes at most q^r clauses of width at most r, while the exactly-one clauses have width at most max(q,2). Thus N=qn and M=O_{q,r}(n+m), without one auxiliary variable per constraint. Every original assignment has exactly one one-hot representation. Unused original variables must be retained or their q-fold contributions recorded. False nullary constraints and other trivial cases are handled directly.

領域[q]と最大arity rを固定したCSPでは、元の各変数にq個のone-hot変数を置きます。各制約の禁止タプルごとに、その選択を禁止する節を作ります。一制約から高々q^r個、幅r以下の節を作り、exactly-oneの節幅はmax(q,2)以下です。制約ごとに補助変数を置かず、N=qn、M=O_{q,r}(n+m)となります。元の割当てとone-hot表現は一対一です。使われない元変数も残すか、そのq倍の寄与を記録します。偽の0項制約などの自明な場合は直接処理します。

For Independent Set, one input bit per vertex and a binary clause ¬x_u∨¬x_v per edge give a bijection. Adding the cardinality circuit from Section 2 preserves that bijection and adds only O(n) variables. For fixed-q coloring the same one-hot representation and edge conflicts suffice. Consequently these families have A(n) bounds even when m is large, and B(n) when m=O(n). Fixed q and r are essential; a nonuniform dependence hidden in O_{q,r} cannot be used with growing q or r.

独立集合では一頂点一ビットと各辺の二項節¬x_u∨¬x_vで一対一対応を得ます。第2節の個数回路を加えても対応を保ち、追加変数はO(n)個です。固定q彩色もone-hot表現と各辺の衝突禁止で扱えます。したがってこれらはmが大きくてもA(n)、m=O(n)ならB(n)の上界を持ちます。qとrが固定であることは不可欠で、O_{q,r}に隠れる依存をqやrが増える場合にそのまま使えません。

## 7. Local degree and neighborhood constraints / 局所的な次数・近傍制約

For a chosen edge set F, sum its incident selection bits at each vertex v using O(deg_G(v)) gates. Clip the input degree interval to [0,deg_G(v)] and compare against its endpoints with O(log(deg_G(v)+1)) gates. Empty intervals reject the instance; check isolated vertices before removing them. The total is O(m) gates because the sum of all degrees is 2m. Full gate equivalences give a unique extension, hence an O(m)-variable, O(m)-clause, width-3 counting encoding. This covers matchings, b-matchings, perfect matchings, edge covers, f-factors and 2-factors through their degree intervals. It imposes no global connectivity requirement.

選択する辺集合Fについて、各頂点vに接続する選択ビットをO(deg_G(v))ゲートで加算します。入力された次数区間を[0,deg_G(v)]と交差させ、O(log(deg_G(v)+1))ゲートで両端と比較します。空の区間なら解なしとし、孤立点は条件を確認してから除きます。次数の総和は2mなので、全体でO(m)ゲートです。全ゲートを同値で符号化すれば拡張は一意となり、O(m)変数・O(m)節・幅3の計数用符号化を得ます。局所次数の区間の指定によりmatching、b-matching、perfect matching、edge cover、f-factor、2-factorを扱えます。ただし全体の連結性は課しません。

For Dominating Set, each vertex requires an OR over its closed neighborhood. Bounded-fan-in OR circuits have a total of O(n+m) gates, since the sum of closed-neighborhood sizes is n+2m. Require every output to be true and encode gates by equivalences; do not use a merely equisatisfiable clause splitting when counting. Cardinality constraints add O(n) gates. This direct route gives B(n+m), and therefore B(n) on sparse graphs without a maximum-degree assumption. Section 9 gives a different route using the difference of two exact counts: it has O(n) variables even on dense graphs and yields A(n). Both bounds remain available.

Dominating Setでは、各頂点の閉近傍に選択された頂点があることをOR回路で表します。閉近傍の大きさの総和はn+2mなので、入力数を定数に制限した回路の総ゲート数はO(n+m)です。各出力を真に固定し、ゲートを同値で符号化します。計数では、充足可能性だけを保存する節の分割では不十分です。個数制約の追加はO(n)ゲートであり、この直接的な方法からB(n+m)、疎グラフでは最大次数の制限なしにB(n)を得ます。第9節では2回の正確な計数の差を用いる別の方法を示します。密なグラフでも変数をO(n)個に保ち、A(n)を得られます。両方の上界を使えます。

## 8. Exact optimization and witnesses / 厳密最適化と解の復元

For an unweighted integer objective bounded by a polynomial in the input length, first obtain a test for feasibility at a threshold. With a direct fixed-width CNF encoding, ordinary satisfiability is sufficient. For the signed-count construction in Section 9, use the test Z₀−Z₁>0, requiring two exact #SAT counts per threshold. Binary search needs logarithmically many threshold queries; a linear scan also introduces only a polynomial factor. Once the optimum is known, fix the original choice bits one by one, retaining a value whenever a feasibility query permits an optimum extension. Unit clauses do not increase the number of choice variables, and the polynomially many queries can be made sequentially in polynomial space. Preserving satisfiability suffices for a direct decision encoding; the signed-count route requires the count preservation proved in Section 9.

入力長の多項式で上から抑えられる非重み付き整数目的関数では、まず閾値を満たす実行可能解の存在を判定する方法を用意します。直接の固定幅CNF符号化なら、通常の充足可能性判定で十分です。第9節の符号付き計数を使う場合は、閾値ごとに2回の正確な#SAT計数を行い、Z₀−Z₁>0かを判定します。二分探索の問い合わせ回数は対数個で、線形走査でも多項式因子だけの増加です。最適値が分かったら、元の選択ビットを順に固定し、最適解への延長が残る値を選ぶことで解を復元できます。単位節の追加は選択変数数を増やさず、多項式回の問い合わせは逐次実行すれば多項式領域です。直接の判定用符号化なら充足可能性の保存で十分ですが、符号付き計数の方法には第9節で示す計数の保存が必要です。

For fixed-domain, fixed-arity Max-CSP, compute one satisfaction bit for each constraint by a constant-size Boolean circuit, then sum these m bits with Section 2. Including the qn one-hot inputs gives O_{q,r}(n+m) variables and clauses. This yields the exact B(n+m) bound for Max-CSP, including fixed-width MaxSAT and Max-Cut. It is a separate catalog deduction from any source theorem that offers additive approximation measured in n. A general B(n) exact bound does not follow for dense inputs.

固定領域・固定arityのMax-CSPでは、各制約を満たすかどうかを定数サイズのBoolean回路で一ビットにし、そのmビットを第2節の回路で加算します。qn個のone-hot入力と合わせ、変数と節はO_{q,r}(n+m)個です。固定幅MaxSATやMax-Cutを含め、厳密なB(n+m)上界を得ます。これは、nを尺度にした加法近似を与える元論文の定理とは別の、カタログ側の導出です。密な入力に対する厳密なB(n)上界までは導けません。

Binary weights require a separate bit-cost audit. A sum of n choices with b-bit weights can need O(nb) Boolean gates; a generating polynomial can have degree proportional to the weight values. Neither quantity belongs in a polynomial factor while claiming an unchanged exponent without further argument. Likewise local degree-two constraints yield a 2-factor, not necessarily a Hamiltonian cycle.

二進重みはビットコストを別途調べます。bビットの重みを持つn個の選択の和はO(nb)ゲートを必要とし得て、生成多項式の次数は重みの値に比例し得ます。追加の議論なしに、その分を多項式因子に押し込めて指数を変えずに済ませることはできません。同様に、局所的な次数2の制約から得られるのは2-factorであり、Hamiltonian cycleとは限りません。

## 9. Parity inclusion-exclusion / 偶奇による包含排除

This section records a catalog deduction from the exact-counting assumptions. Grouping inclusion-exclusion terms by parity reduces the computation to at most two #SAT calls. Both calls return exact integers; knowing only their parities, or only whether their formulas are satisfiable, is insufficient.

この節は、正確な計数を行う仮定からのカタログ側の導出です。包含排除の項を偶数側と奇数側にまとめ、高々2回の#SAT呼出しで計算します。各呼出しでは正確な整数値を求めます。その値の偶奇だけ、あるいは式が充足可能かどうかだけでは不十分です。

### 9.1 Avoiding explicitly described bad events / 明示された禁止事象を避ける

Let x comprise p Boolean variables, and let R(x) be a base CNF of fixed width w₀. We want to count assignments satisfying R while avoiding t bad events E₁(x),…,Eₜ(x). Suppose every E_i is explicitly represented as a CNF of fixed width w over the same base variables. Introduce a selector y_i for each event and form

xをp個のBoolean変数、R(x)を固定幅w₀の基本条件とします。Rを満たし、t個の禁止事象E₁(x),…,Eₜ(x)をすべて避ける割当てを数えます。各E_iが同じ基本変数上の固定幅wのCNFとして明示されているとします。事象ごとに選択変数y_iを置き、次を作ります。

$$
\Psi(x,y)=R(x)\ \land\
\bigwedge_{i=1}^{t}\ \bigwedge_{C\in E_i}(\neg y_i\lor C).
$$

Thus y_i=1 is allowed exactly when E_i(x) holds. For a fixed base assignment x, the allowed selector sets are all subsets of I(x)={i:E_i(x) holds}. Compute selector parity using z₀=0 and the gate equivalences

y_i=1を許すのはE_i(x)が成立するときだけです。基本割当てxを固定すると、許される選択集合はI(x)={i:E_i(x)が成立する}のすべての部分集合です。z₀=0とし、次のゲート同値で選択個数の偶奇を計算します。

$$
z_i\leftrightarrow(z_{i-1}\oplus y_i),\qquad 1\le i\le t.
$$

Use all four XOR clauses from §2.1 for each gate, substituting the initial constant. Let Z₀ and Z₁ be the numbers of satisfying assignments after forcing z_t=0 and z_t=1, respectively. The XOR outputs are uniquely determined by y. Consequently,

各ゲートに§2.1のXORの4節をすべて使い、初期値の定数を代入します。z_t=0に固定した式の充足割当て数をZ₀、z_t=1に固定した式の個数をZ₁とします。XORの出力はyから一意に決まるので、

$$
Z_0-Z_1
=\sum_{x:R(x)}\ \sum_{S\subseteq I(x)}(-1)^{|S|}
=\#\{x:R(x)\text{ and }I(x)=\varnothing\}.
$$

If I(x) is empty, its only subset contributes +1. If it is nonempty, toggling any fixed member pairs every even subset with an odd subset, so the contribution is zero. For t=0, set Z₀=#R and Z₁=0 and omit the parity circuit.

I(x)が空なら、唯一の部分集合である空集合が+1を寄与します。空でなければ、固定した一要素を入れる・外す操作で偶数サイズと奇数サイズの部分集合を対にでき、寄与は0になります。t=0では偶奇回路を作らず、Z₀=#R、Z₁=0とします。

Writing M_R for the number of base clauses and |E_i| for the number of clauses describing E_i, each query has

基本条件の節数をM_R、E_iを記述する節数を|E_i|と書くと、各問い合わせは

$$
N=p+O(t),\qquad
M=M_R+\sum_i|E_i|+O(t),\qquad
\text{width}\le\max(w_0,w+1,3).
$$

The widths must be fixed and the descriptions polynomial in the original input length L. Under these conditions, two calls give A(p+t)L^{O(1)} time and polynomial space. Moreover Z₀+Z₁≤2^{p+t}, so the counts and their difference use O(p+t+1) bits; the exact subtraction introduces only polynomial work. The corresponding reported-formalization profile follows by the same variable substitution.

幅は固定し、各記述の大きさは元入力長Lの多項式以下とします。この条件の下で、2回の呼出しによりA(p+t)L^{O(1)}時間・多項式領域を得ます。またZ₀+Z₁≤2^{p+t}なので、個数と差を表すのはO(p+t+1)ビットで足り、正確な減算に必要な追加時間も多項式です。公開された形式化範囲のプロファイルにも同じ変数数を代入できます。

Additional side conditions may be included in R. If they use auxiliary gates, count those variables in p and ensure a unique extension for each intended base assignment. A merely existential encoding of E_i with arbitrary auxiliary assignments does not meet the stated hypothesis: its multiplicities can invalidate the cancellation. Retain unused base variables or explicitly correct for their multiplicity.

追加の条件はRに含められます。補助ゲートを使う場合は、その変数もpに含め、数えたい元の割当てごとに拡張が一意であることを確保します。任意の補助割当てを許す存在量化だけのE_iの符号化は、上の仮定を満たしません。その多重度によって打ち消しが崩れる可能性があります。使われない基本変数も残すか、その多重度を明示的に補正します。

### 9.2 Arbitrary-width CNF with few clauses / 節数の少ない任意幅CNF

Consider an arbitrary-width CNF F on p declared variables with q clauses C_i. Its bad event for clause i is

p個の宣言変数とq個の節C_iを持つ任意幅CNF Fを考えます。第i節に対する禁止事象は

$$
E_i(x)=\neg C_i(x)=\bigwedge_{\ell\in C_i}\neg\ell.
$$

This is a conjunction of unit clauses regardless of the length of C_i. Its activation therefore uses only the binary clauses (¬y_i∨¬ℓ), one per literal occurrence. With R=true and the parity circuit above, each query has

C_iがどれだけ長くても、E_iは単位節のANDです。したがって、その選択条件は各リテラル出現につき二項節(¬y_i∨¬ℓ)を一つ置くだけです。R=trueとして上の偶奇回路を加えると、各問い合わせは

$$
N=O(p+q),\qquad M=O(p+q+\operatorname{occ}(F)),\qquad
\text{width}\le3,
$$

where occ(F) is the total number of literal occurrences. Their difference is exactly #F, including the contribution of unused declared variables. Hence the general-density assumption gives A(p+q)L^{O(1)}; in particular, q=O(p) gives A(p)L^{O(1)} even when clauses are long. This does not assert an A(p) bound with arbitrarily many clauses. Already fixed-width clauses may instead stay in R and need no selectors; the resulting width is the maximum of their width and three.

ここでocc(F)はリテラルの総出現数です。2つの個数の差は、使われない宣言変数の寄与も含めて正確に#Fです。一般密度の仮定からA(p+q)L^{O(1)}を得て、特にq=O(p)なら、長い節を許してもA(p)L^{O(1)}です。節数を無制限に増やした場合のA(p)を主張するものではありません。最初から固定幅の節はRに残せば選択変数が不要で、その場合の最終的な幅は元の固定幅と3の大きい方です。

### 9.3 Dominating sets and maximal independent sets / 支配集合と極大独立集合

Let G be a simple undirected graph with n vertices and m edges. A choice x_v=1 means that v is selected. For each vertex v, the bad event is that its closed neighborhood N[v] contains no selected vertex. The activated clauses are

Gをn頂点・m辺の単純無向グラフとし、x_v=1はvを選ぶことを表します。各頂点vについて、閉近傍N[v]に選択頂点がないことを禁止事象にします。選択条件の節は

$$
\Psi_G(x,y)=
\bigwedge_{v\in V(G)}\ \bigwedge_{u\in N[v]}
(\neg y_v\lor\neg x_u).
$$

For a fixed x, the eligible selector vertices are precisely its undominated vertices. The even-minus-odd count therefore contributes one for a dominating set and zero for every other vertex set. There are 2n choice and selector variables, O(n) parity-gate variables, and n+2m activated clauses. Thus each query has N=O(n), M=O(n+m) and width at most three. The difference counts all dominating sets in A(n)L^{O(1)} time, including on dense graphs. On sparse graphs, Section 5 also gives B(n); the direct B(n+m) construction in Section 7 remains valid.

xを固定すると、選択可能なyの頂点は、ちょうどxで支配されない頂点です。したがって偶数側から奇数側を引くと、支配集合は1、それ以外の頂点集合は0を寄与します。元の選択変数と事象の選択変数は計2n個、偶奇ゲートの変数はO(n)個、選択条件の節はn+2m個です。各問い合わせはN=O(n)、M=O(n+m)、節幅3以下となり、密なグラフも含めてA(n)L^{O(1)}で全支配集合を数えられます。疎グラフでは第5節からB(n)も得られ、第7節の直接的なB(n+m)の構成も引き続き有効です。

Add a cardinality circuit for ∑_v x_v=s or ∑_v x_v≤s to R to count dominating sets of exactly or at most s vertices. Fixing some original choices with unit clauses is also allowed. These additions require O(n) variables and clauses and have unique gate extensions. Isolated vertices need not be removed: if one is unselected, its bad event holds and its contribution cancels. The empty graph has the single empty dominating set, as the t=0 case requires.

Rに∑_v x_v=sまたは∑_v x_v≤sの個数回路を加えれば、ちょうどs頂点または高々s頂点の支配集合を数えられます。元の選択の一部を単位節で固定することもできます。追加はO(n)変数・節で、ゲートの拡張は一意です。孤立頂点を除く必要はなく、孤立頂点を選ばなければその禁止事象が成立して寄与が打ち消されます。頂点数0のグラフでは空の支配集合が一つあり、t=0の場合の扱いと一致します。

Adding the binary independence clauses ¬x_u∨¬x_v for every edge uv restricts the base assignments to independent sets. An independent set dominates G exactly when it is maximal under inclusion. The same two counts therefore count maximal independent sets. Complementation gives a bijection with minimal vertex covers, with |C|=n−|I|, so their total and size-specific counts have the same A(n) bound. Here maximal and minimal refer to inclusion, not to maximum or minimum cardinality.

各辺uvに二項節¬x_u∨¬x_vを加えれば、基本割当てを独立集合に制限できます。独立集合がGを支配することは、集合包含に関して極大であることと同値です。したがって同じ2回の計数で極大独立集合を数えられます。補集合との対応は極小頂点被覆への全単射で、|C|=n−|I|です。その総数やサイズ別の個数にも同じA(n)上界を得ます。ここでの極大・極小は集合包含に関するもので、頂点数の最大・最小とは区別します。

### 9.4 Feasibility, optimization and reconstruction / 存在判定・最適化・解の復元

For any of these restrictions, feasibility is the positivity of the difference Z₀−Z₁. Satisfiability of either query alone does not generally decide it. Apply Section 8 with the condition ∑x≤s to find a minimum dominating set or a minimum independent dominating set. The at-most threshold is monotone even though the existence of a maximal independent set of exactly s vertices need not be. After obtaining the optimum, fix each x_v in turn and keep a value for which the difference remains positive under the optimum-size bound. There are O(log(n+1)) threshold tests and O(n) reconstruction tests, each using two sequential #SAT calls. All added conditions preserve O(n) variables, polynomial encoding length and polynomial space.

これらの追加条件を課した場合も、実行可能解があることは差Z₀−Z₁が正であることに対応します。どちらか一方の式の充足可能性だけでは、一般には判定できません。∑x≤sという条件で第8節の手順を使えば、最小支配集合や最小独立支配集合を求められます。ちょうどs頂点の極大独立集合が存在するかどうかは単調でなくても、高々s頂点という閾値なら単調です。最適値が分かったら、そのサイズ上限の下で差が正のままになる値を残し、x_vを一つずつ固定して解を復元します。閾値の判定はO(log(n+1))回、復元の判定はO(n)回で、各回につき#SATを2回、逐次実行します。追加条件を含めてもO(n)変数、多項式の符号長、多項式領域を保ちます。
