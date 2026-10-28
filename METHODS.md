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

For Dominating Set, each vertex requires an OR over its closed neighborhood. Bounded-fan-in OR circuits have a total of O(n+m) gates, since the sum of closed-neighborhood sizes is n+2m. Require every output to be true and encode gates by equivalences; do not use a merely equisatisfiable clause splitting when counting. Cardinality constraints add O(n) gates. This gives B(n+m), and therefore B(n) on sparse graphs without a maximum-degree assumption.

Dominating Setでは、各頂点の閉近傍に選択された頂点があることをOR回路で表します。閉近傍の大きさの総和はn+2mなので、入力数を定数に制限した回路の総ゲート数はO(n+m)です。各出力を真に固定し、ゲートを同値で符号化します。計数では、充足可能性だけを保存する節の分割では不十分です。個数制約の追加はO(n)ゲートであり、B(n+m)、疎グラフでは最大次数の制限なしにB(n)を得ます。

## 8. Exact optimization and witnesses / 厳密最適化と解の復元

For an unweighted objective bounded by a polynomial in the input length, build a fixed-width encoding of feasibility at a threshold. Binary search needs logarithmically many threshold queries; a linear scan also introduces only a polynomial factor. Once the optimum is known, fix the original choice bits one by one, retaining a value whenever a feasibility query permits an optimum extension. Unit clauses do not increase the number of choice variables, and their polynomially many queries can be made sequentially in polynomial space. Unique extensions are needed when counting, but ordinary satisfiability is sufficient for these optimization queries.

入力長の多項式で上から抑えられる非重み付き目的関数では、閾値を満たす実行可能解の存在を固定幅CNFで表します。二分探索の問い合わせ回数は対数個で、線形走査でも多項式因子だけの増加です。最適値が分かったら、元の選択ビットを順に固定し、最適解への延長が残る値を選ぶことで解を復元できます。単位節の追加は選択変数数を増やさず、多項式回の問い合わせは逐次実行すれば多項式領域です。計数には拡張の一意性が必要ですが、この最適化の問い合わせには充足可能性の保存で十分です。

For fixed-domain, fixed-arity Max-CSP, compute one satisfaction bit for each constraint by a constant-size Boolean circuit, then sum these m bits with Section 2. Including the qn one-hot inputs gives O_{q,r}(n+m) variables and clauses. This yields the exact B(n+m) bound for Max-CSP, including fixed-width MaxSAT and Max-Cut. It is a separate catalog deduction from any source theorem that offers additive approximation measured in n. A general B(n) exact bound does not follow for dense inputs.

固定領域・固定arityのMax-CSPでは、各制約を満たすかどうかを定数サイズのBoolean回路で一ビットにし、そのmビットを第2節の回路で加算します。qn個のone-hot入力と合わせ、変数と節はO_{q,r}(n+m)個です。固定幅MaxSATやMax-Cutを含め、厳密なB(n+m)上界を得ます。これは、nを尺度にした加法近似を与える元論文の定理とは別の、カタログ側の導出です。密な入力に対する厳密なB(n)上界までは導けません。

Binary weights require a separate bit-cost audit. A sum of n choices with b-bit weights can need O(nb) Boolean gates; a generating polynomial can have degree proportional to the weight values. Neither quantity belongs in a polynomial factor while claiming an unchanged exponent without further argument. Likewise local degree-two constraints yield a 2-factor, not necessarily a Hamiltonian cycle.

二進重みはビットコストを別途調べます。bビットの重みを持つn個の選択の和はO(nb)ゲートを必要とし得て、生成多項式の次数は重みの値に比例し得ます。追加の議論なしに、その分を多項式因子に押し込めて指数を変えずに済ませることはできません。同様に、局所的な次数2の制約から得られるのは2-factorであり、Hamiltonian cycleとは限りません。
