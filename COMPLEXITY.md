# Complexity-Theoretic Consequences

[日本語](COMPLEXITY.ja.md) · [Home](README.md) · [FPT](FPT.md) · [Exact algorithms](EXACT.md) · [Assumptions and sources](ASSUMPTIONS.md)

Updated: 2026-10-29. **Overall status: draft.** Codex carried out most research, derivation, and writing. Section-specific reviews are recorded in Section 9; no human verification is recorded. Algorithmic consequences depend on the specified SAT assumptions. Unconditional class facts, conditional deductions, and existing conjectures are distinguished explicitly. Source attribution and mechanical checks do not certify the underlying theorem or these deductions. Inclusion is not a novelty claim.

## 1. Scope and main consequences

Either [sat26-general](ASSUMPTIONS.md#sat26-general) or [sat26-reported-formal](ASSUMPTIONS.md#sat26-reported-formal) supplies an explicit deterministic fixed-width SAT algorithm with time $2^{o_{\mathrm{eff}}(N)}L_{\mathrm{CNF}}^{O(1)}$. The effective little-oh means a computable rate of convergence. Positivity of the exact count decides satisfiability. These are alternative sufficient premises for the qualitative consequences, not assumptions that must be combined. The quantitative circuit statement in Section 6.2 specifically uses [sat26-sparse](ASSUMPTIONS.md#sat26-sparse).

| Topic | What the assumed result supports | Scope |
| --- | --- | --- |
| Exponential-time hypotheses | ETH, SETH, Gap-ETH, and NSETH fail; exact counting also refutes #ETH | Source-stated consequences: [SAT26, Sections 1.1.1–1.1.2][sat26] |
| Parameterized classes | $\mathrm{FPT}=\mathrm{M}[1]$ | Apply the established M-hierarchy correspondence; Section 2 |
| Concrete complete problems | The miniaturized decision problems in Section 3 become FPT | The parameter and padding are part of each problem definition |
| W hierarchy | No equality with W[1] or W[2] is established here | Hardness reductions remain valid; an ETH-based lower bound loses its premise |
| Classical classes | Neither P versus NP nor NP versus EXP is settled by the displayed SAT upper bound | Section 5 explains the polynomial-size blowup |
| Circuit complexity | $\mathrm{E}^{\mathrm{NP}}\not\subseteq\mathrm{SIZE}(O(n))$ | Williams's existing theorem; Section 6 separates the manuscript's stronger claim |
| XP | $\mathrm{FPT}\subsetneq\mathrm{XP}$ is unconditional; some individual XP bounds can improve | Section 7 separates class-wide claims from small-encoding transfers |
| Logarithmic pathwidth | Clique becomes FPT for the normalized width of a supplied decomposition | A catalog deduction; Section 7 contrasts XNLP-complete Independent Set and Dominating Set |
| XNLP and space | No class-wide FPT simulation or SPSC refutation is established by the transfers checked here | Section 7 states the additional time–space bounds that would suffice |

These class consequences are companion material, outside the catalog's 35 algorithm-entry and 10 candidate counts. Here logarithms are base two; small inputs are handled separately. A different fixed logarithm base changes parameters only by bounded factors.

## 2. The conditional equality FPT = M[1]

We use the M hierarchy of [Flum–Grohe, Definition 4.2][fg04]. Its first level is the union, over fixed clause widths $d$, of FPT many-one downward closures of **p-log-SAT on $d$-CNF**. For a formula with $N$ variables and combinatorial formula size $S$, this problem has parameter $\lceil N/\log S\rceil$. The formula representation permits repeated clauses and hence padding. $S$ is distinct from the bit length used to encode variable names.

Their correspondence and hierarchy inclusions give, under either qualitative SAT premise above,

$$
\boxed{\mathrm{FPT}=\mathrm{M}[1]}
\subseteq\mathrm{W}[1]\subseteq\mathrm{M}[2]\subseteq\mathrm{W}[2].
$$

See [FG04, Corollary 4.3, Theorem 4.4, and Section 7][fg04]. The argument below explains the needed algorithmic direction. The conclusion applies to problems **in M[1]**, including its complete problems; M[1]-hardness alone is not an algorithmic upper bound. These are decision classes, with no claim about a collapse of parameterized counting classes.

### Why the polynomial exponent becomes independent of the parameter

For a decision problem $Q$ with declared measure $\nu(x)$, use the miniaturization convention of [Chen–Grohe, Definition 13][cg06]:

$$
\text{input }(x,1^b),\qquad b\ge\max\{2,|x|_{\mathrm{bits}}\},\qquad
\kappa=\left\lceil\frac{\nu(x)}{\log b}\right\rceil.
$$

The question remains whether $x\in Q$. The integer $b$ is supplied **in unary**; the padded input length is $\Theta(|x|_{\mathrm{bits}}+b)$. The measure $\nu$ must be specified separately from bit length.

For variable-mini-$d$-SAT with fixed $d$, $\nu=N$ and $L=|x|_{\mathrm{bits}}$. Write the assumed SAT time as $2^{Na(N)}L^c$, with $a(N)\to0$ effectively. For each $\kappa\ge1$, choose a computable threshold $H(\kappa)$ such that $a(N)\le1/\kappa$ for $N\ge H(\kappa)$. Then

$$
N\ge H(\kappa)\quad\Longrightarrow\quad
2^{Na(N)}\le 2^{N/\kappa}\le b.
$$

For $N<H(\kappa)$, brute force costs at most $2^{H(\kappa)}L^{O(1)}$. Combining the cases yields $f(\kappa)(L+b)^{O(1)}$ with polynomial exponent independent of $\kappa$. The zero-variable case is trivial. Taking $b=S$ gives the same argument for p-log-SAT; there $S$ is at most a polynomial in the encoded input length, and no separate padding string is required. This is a direct explanation of the relevant direction of [CG06, Theorem 14][cg06], not a practical bound on $f$.

More generally, the same threshold argument works for an FPT reduction producing fixed-width SAT with at most $h(\kappa)\log L$ variables and encoded length $g(\kappa)L^c$, for computable $h,g$ and a constant $c$. The width must be a fixed constant independent of the input and parameter. Counting only witness bits while omitting auxiliary variables does not meet this premise.

## 3. Known complete problems and their exact parameters

Every row below includes the unary padding $1^b$ defined above. $N$ is the number of formula variables and $n$ the number of graph vertices. A requested solution size $r$ is ordinary input, **not the parameter**.

All completeness statements in this table are **under FPT Turing reductions**. This qualification does not replace the many-one closure in the class definition. The facts follow from [FG04, Theorem 5.3, Corollaries 5.6 and 5.8, Proposition 5.10 and the following discussion][fg04].

| Miniaturized decision problem | Parameter $\kappa$ |
| --- | --- |
| Variable-mini-3SAT: is the 3-CNF satisfiable? | $\lceil N/\log b\rceil$ |
| Vertex-mini-$q$-Colorability, fixed $q\ge3$: is the graph $q$-colorable? | $\lceil n/\log b\rceil$ |
| Vertex-mini-Independent Set: is there an independent set of size at least $r$? | $\lceil n/\log b\rceil$ |
| Vertex-mini-Clique: is there a clique of size at least $r$? | $\lceil n/\log b\rceil$ |
| Vertex-mini-Vertex Cover: is there a vertex cover of size at most $r$? | $\lceil n/\log b\rceil$ |

Vertex-mini-Clique and vertex-mini-Independent Set are related by graph complementation, preserving $n$; increase the unary padding to a polynomial in $b$ if necessary to cover the new explicit encoding length. The measure matters: **mini-Clique with total graph size $n+m$ in the numerator is already FPT**, as recorded in [FG04, Corollary 5.9][fg04]. The table therefore does not use an unqualified “Mini-Clique” label.

Unary padding is included in the running-time input length. These conclusions do not give an FPT algorithm for ordinary Clique parameterized by $r$.

For p-log-3SAT, one must likewise retain the paddable formula syntax. Restricting to duplicate-free canonical fixed-width formulas gives $S=N^{O(1)}$; then $\lceil N/\log S\rceil=\kappa$ implies $N=O(\kappa\log(\kappa+2))$, so brute force is already FPT. This is a catalog observation about a changed problem definition, not the cited completeness result.

### A normalized solution-size parameter for Vertex Cover

[Müller, Example 3.2.17, printed pp. 125–126][mueller08] records the following problem as M[1]-complete:

$$
\text{input }(G,r),\quad 0\le r\le n,\qquad
\kappa=\left\lceil\frac{r}{\log\max\{2,\|G\|\}}\right\rceil.
$$

The question is whether there is a vertex cover of cardinality $r$, equivalently at most $r$ on this input range. The source's structure size $\|G\|$ is $\Theta(n+m)$ for graphs (Appendix A.2, p. 195), not simply the vertex count or encoded bit length. There is no separate unary-padding input. We regularize the logarithm only for tiny graphs.

The cited example states completeness and the equivalence with ETH failing, but does not specify a completeness reduction type in that passage; we do not add one. Under the present SAT assumptions this reparameterization is FPT. Ordinary Vertex Cover was already FPT in $r$; the consequence concerns the smaller parameter $r/\log\|G\|$.

## 4. W-hardness and quantitative lower bounds

For the usual solution-size parameter, Clique is W[1]-complete and Dominating Set is W[2]-complete; see [Downey–Fellows, Introduction and Corollary 3.2][df95]. Their reduction statements do not depend on ETH and remain valid. A hardness statement alone establishes no class separation, however, and this note does not infer either $\mathrm{FPT}=\mathrm{W}[1]$ or $\mathrm{FPT}=\mathrm{W}[2]$.

A tempting direct route illustrates the obstacle. Selecting $r$ graph vertices uses $r\lceil\log n\rceil$ binary bits. But forbidding a nonedge between two selected positions produces a clause of width up to $2\lceil\log n\rceil$. The fixed-width SAT premise does not apply. A conversion to 3-CNF introduces auxiliary variables that must be counted. This diagnoses that encoding; it is not a lower bound on every possible encoding.

Separately, [Marx, Theorem 1.5][marx10] states that an $f(r)n^{o(r)}$ algorithm for Clique would refute ETH. If ETH is false, this implication remains a valid theorem but no longer excludes that algorithm on the basis of ETH. It does **not** supply the algorithm. Lower bounds proved from other hypotheses must be assessed under their actual premises.

Finally, a speedup from $2^{O(\kappa)}L^{O(1)}$ to $2^{o(\kappa)}L^{O(1)}$ improves an algorithm already in FPT. It is a different conclusion from making the M[1]-complete reparameterizations FPT.

## 5. Why this does not establish NP versus EXP

**The catalog does not record $\mathrm{NP}\ne\mathrm{EXP}$ as a consequence established by these SAT upper bounds.** We use

$$
\mathrm{EXP}=\bigcup_{d\ge1}\mathrm{DTIME}(2^{n^d}),\qquad
\mathrm{SUBEXP}=\bigcap_{\varepsilon>0}\mathrm{DTIME}(2^{n^\varepsilon}).
$$

This definition of SUBEXP is stronger than merely admitting a $2^{o(n)}$ running time. [Vadhan, Lecture 5, Section 2][vadhan07] states these class definitions and the relevant time-hierarchy separation.

An NP language with input length $n$ may reduce to a 3-CNF with $N=\Theta(n^d)$ variables and polynomial encoded length. Substituting into the general manuscript profile gives

$$
2^{O\!\left(n^d\log\log n/\sqrt{\log n}\right)}n^{O(1)}.
$$

For $d>1$, this displayed guarantee is not $2^{o(n)}$. The degree depends on the language and reduction. Thus the argument gives no sufficiently small deterministic time class containing all of NP. The manuscript explicitly notes the reduction-size obstruction on p. 2, before Corollary 1.3; see also [IPZ01, Section 1.1.4][ipz01].

A much stronger hypothetical bound would suffice:

$$
\text{3SAT in }2^{N^{o(1)}}L_{\mathrm{CNF}}^{O(1)}
\quad\Longrightarrow\quad
\mathrm{NP}\subseteq\mathrm{SUBEXP}\subsetneq\mathrm{EXP}.
$$

Indeed, every fixed-degree polynomial reduction preserves this form because $(n^d)^{o(1)}=n^{o(1)}$. The final strict containment follows from the deterministic time hierarchy [Vadhan, Section 2][vadhan07]. The single-algorithm bound displayed here is a sufficient condition; the intersection definition of SUBEXP may use a different algorithm for each $\varepsilon$.

The current exponent $N\log\log N/\sqrt{\log N}=N^{1-o(1)}$ is much larger than $N^{o(1)}$. This compares stated upper-bound guarantees, not lower bounds on every possible SAT algorithm. We are identifying a failure of this inference, **not proving logical independence** between ETH and NP versus EXP.

Faster algorithms can in principle prove this kind of separation: if P = NP, then the known strict inclusion P $\subsetneq$ EXP immediately gives NP $\subsetneq$ EXP. The present bound does not provide such a sufficient class-wide improvement.

## 6. Circuit lower bounds

### 6.1. The qualitative consequence of Williams's theorem

**Provenance: existing theorem applied to the assumed SAT algorithm. Status: draft.**

[Williams, Theorem 6.1][williams10] proves

$$
\neg\mathrm{ETH}
\quad\Longrightarrow\quad
\mathrm{E}^{\mathrm{NP}}\not\subseteq\mathrm{SIZE}(O(n)).
$$

Here $\mathrm{E}^{\mathrm{NP}}$ is deterministic $2^{O(n)}$ time with an NP oracle, and $\mathrm{SIZE}(O(n))$ denotes nonuniform linear-size Boolean circuits over bounded-fan-in AND, OR, and NOT gates. The pinned manuscript records this application as Corollary C.1.

Only the decision guarantee is needed. A circuit with $n$ inputs and $s$ gates has a 3-CNF encoding with $O(n+s)$ variables and clauses. Thus linear-size circuits admit the subexponential SAT algorithm used in Williams's argument. Either general or reported-formal SAT assumption suffices for this qualitative consequence. It is not an NP-versus-EXP separation or a superpolynomial circuit lower bound for NP.

### 6.2. The manuscript's quantitative corollary

**Provenance: source-corollary. Status: draft. Dependency: sat26-sparse and the argument in Appendix C.1.**

[SAT26, Corollary 1.4, p. 3][sat26] states that some language $A\in\mathrm{E}^{\mathrm{NP}}$ and constant $c>0$ satisfy

$$
C_A(n)>c\,n\sqrt{\log n}\qquad\text{for infinitely many }n.
$$

$C_A(n)$ is the minimum number of **noninput gates**, allowing arbitrary Boolean gates of fan-in at most two and unrestricted fan-out. The proof is in Appendix C.1, pp. 51–52. This is an explicitly source-stated conditional corollary; the catalog has not independently verified that proof or its local-reduction ingredients. We do not attribute this sharper quantitative bound to the weaker reported-formalization profile.

<a id="xp-xnlp"></a>

## 7. XP, XNLP, and simultaneous time–space bounds

The SAT assumptions give substantial improvements for some problems, while leaving other algorithmic questions unresolved by the transfers checked here. This section separates unconditional class structure, conditional algorithms, and the additional progress that would suffice for broader consequences. Its scoped review record is in Section 9.

As in the algorithm catalog, write

$$
A(s)=2^{O(s\log\log s/\sqrt{\log s})},\qquad
B(s)=2^{O(s/\sqrt{\log s})},
$$

with bounded logarithms regularized. These are the manuscript's general-density and fixed-density profiles, respectively; $B$ requires the corresponding density or padding argument. The reported-formalization profile remains a separate assumption. Throughout, $L$ is the full original input bit length, including any supplied decomposition, and parameter-dependent functions are computable.

### 7.1. An unconditional boundary: FPT is strictly contained in XP

**Unconditional background, using strongly uniform classes.** Let $L$ denote the full encoded input length. FPT requires a single algorithm with running time $f(k)L^c$, where $f$ is computable and $c$ is a constant independent of $k$. XP permits a computable parameter-dependent exponent, for example $L^{g(k)}+g(k)$. Under these conventions,

$$
\mathrm{FPT}\subsetneq\mathrm{XP}
$$

holds unconditionally; see [Downey, Definition 1 and Section 2][downey12] and the uniform XP definition in [Chen–Grohe, Definition 11][cg06].

The time-hierarchy argument is useful here. A bounded-machine-acceptance problem can have slices simulating arbitrary deterministic polynomial-time machines, with the permitted polynomial degree increasing with $k$, while remaining in XP. If this problem had an FPT algorithm with a fixed polynomial exponent $c$, fixing a sufficiently large parameter would simulate all languages at a larger polynomial degree within degree $c$. The deterministic time hierarchy excludes this. The multiplicative factor $f(k)$ is only a constant on that fixed slice.

Thus the conditional equality $\mathrm{M}[1]=\mathrm{FPT}$ in Section 2 does not erase the outer boundary with XP. In particular, a problem complete for XP under FPT reductions is unconditionally outside FPT. This statement concerns XP-completeness, which must not be substituted for XNLP-completeness. The remaining questions concern particular problems and intermediate classes, not an ETH-based conjecture that FPT and XP differ.

### 7.2. A sufficient transfer condition, stronger than an $L^{o(k)}$ time bound

**Existing transfer principle, applied to the specified SAT assumptions.** [Chen–Flum, Proposition 43][cf08] proves that $2^{o_{\mathrm{eff}}(k\log L)}L^{O(1)}$ time implies FPT. Here the exponent is a single effectively sublinear function of the whole product $k\log L$; their Section 2 defines the required computable convergence rate. This is a sufficient condition, not a characterization asserted here for every FPT problem.

For applying the pinned SAT algorithms, the following concrete version allows a computable change of parameter. Suppose an input of length $L$ and parameter $k$ can be solved using fixed-width SAT queries such that:

- One constant clause width $w$ works for all inputs and parameter values.
- Each query has $N\le a h(k)\log_2(L+2)$ Boolean variables, for a constant $a$ and a computable function $h$. This counts **all** choice, verification, cardinality, and Tseitin variables.
- Query construction, one plus the length of every explicit CNF, the number of queries, and the other computation are bounded by $f(k)(L+2)^c$, after enlarging a computable $f$ and a fixed constant $c$ if needed. Queries may be adaptive.

Then either [sat26-general](ASSUMPTIONS.md#sat26-general) or [sat26-reported-formal](ASSUMPTIONS.md#sat26-reported-formal) yields an FPT algorithm. Only decision is needed: a positive exact SAT count decides a query.

To verify the uniform bound, write the SAT running time as $2^{g(N)}(L_{\mathrm{CNF}}+1)^d$, where $g(N)=o_{\mathrm{eff}}(N)$ and $d$ is constant for the fixed width. Set $H(k)=\max\{1,\lceil a h(k)\rceil\}$. Choose a computable threshold $T(k)\ge1$ for which

$$
N\ge T(k)\quad\Longrightarrow\quad g(N)\le N/H(k).
$$

For such queries, $2^{g(N)}\le 2^{N/H(k)}\le L+2$. For $N<T(k)$, the same factor is at most the computable parameter-only bound

$$
F_0(k)=\max_{0\le s<T(k)}2^{g(s)},
$$

with bounded cases regularized. Thus every query costs at most $(F_0(k)+L+2)f(k)^d(L+2)^{cd}$. Multiplying by the FPT number of calls and including construction gives $F(k)(L+2)^{c'}$ for computable $F$ and a constant $c'$ independent of $k$. Both explicit exponent profiles in the assumption register have the required effective convergence. A possibly enormous $F$ is permitted; this is a tractability statement, not a practical parameter dependence.

This condition must be distinguished from $L^{o(k)}=2^{o(k)\log L}$. For example, $L^{\sqrt{k}}$ has an exponent sublinear in $k$, but that displayed bound cannot be bounded by $F(k)L^c$ with a single constant $c$. More strongly, [Chen–Grohe, the first step of the proof of Lemma 18][cg06] constructs, for every XP problem, an FPT-equivalent reparameterization solvable in $L^{O(\sqrt{k})}$ time. Such a bound therefore does not imply FPT, even for XP-complete problems. We use $2^{o_{\mathrm{eff}}(k\log L)}$ only in the stronger whole-product sense above.

The conditions on the reduction are substantial. An $L^{h(k)}$ construction or query count is not FPT merely because the final SAT call is fast. Counting $h(k)\log L$ witness bits also does not bound the number of variables required to verify the witness in a fixed-width CNF. The polynomial-space SAT guarantee gives an $F_1(k)L^{O(1)}$ space bound for the stated construction; it does not give an $f(k)\log L$ space bound. These are limits of this transfer argument, not impossibility results for other reductions or algorithms.

### 7.3. XNLP: simultaneous nondeterministic time and space

The definitions and hardness facts in this subsection are unconditional. With original input bit length $L$ and parameter $k$, XNLP consists of decision problems solvable nondeterministically with the simultaneous bounds

$$
T\le f(k)L^c,\qquad S\le f(k)\log L,
$$

for a computable $f$ and a constant $c$. It satisfies $\mathrm{XNLP}\subseteq\mathrm{XP}$: there are at most $L^{g(k)}$ configurations, and deterministic reachability through them gives an XP algorithm. See [BGNS24, Sections 2.2–2.4 and Lemma 1][bgns24].

The completeness convention is **parameterized logspace (pl) reduction**: the output parameter is computably bounded by the input parameter, and the reduction uses deterministic space $g(k)+O(\log L)$. An XNLP-hard problem is W[$t$]-hard for every fixed positive $t$ [BGNS24, Lemma 2][bgns24]. This does not assert literal inclusions $\mathrm{W}[t]\subseteq\mathrm{XNLP}$ under ordinary FPT reductions.

The relevant possible collapse is $\mathrm{XNLP}\subseteq\mathrm{FPT}$. The reverse inclusion $\mathrm{FPT}\subseteq\mathrm{XNLP}$ would imply $\mathrm{P}=\mathrm{NL}$ by assigning a constant parameter to polynomial-time problems [BGNS24, Lemma 3][bgns24]. Thus we do not write $\mathrm{FPT}=\mathrm{XNLP}$. If XNLP were contained in FPT, its hardness consequence above would imply $\mathrm{W}[t]=\mathrm{FPT}$ for every fixed $t$.

A workspace bound controls what must be stored at one moment. It does not bound all nondeterministic guesses made during the computation, or all variables needed to describe the computation in a SAT tableau. Consequently, $f(k)\log L$ workspace alone does not supply a fixed-width SAT encoding with $f(k)\log L$ variables. A transfer would have to count all variables, including those recording successive stages and checking their consistency.

### 7.4. A positive example: Log-Pathwidth Clique

**Catalog deduction from the stated SAT assumptions (2026-10-29 JST).** This application concerns decision, the maximum clique size, and reconstruction of a maximum clique. It does not assert novelty or a new completeness classification.

The input is a simple undirected graph $G$ with $n$ explicitly represented vertices, an explicitly supplied valid path decomposition $B_1,\ldots,B_t$, and, for decision, an integer target $r$. For $n\ge1$, let

$$
p=\max_i|B_i|-1,\qquad s=p+1,\qquad
\kappa=\left\lceil\frac{p+1}{\log_2(n+2)}\right\rceil.
$$

Thus $p$ is the width of the **supplied decomposition**, and every bag has at most $p+1$ vertices. Let $L$ be the full encoded input length, including the decomposition; with explicit vertex and bag records, $n,t\le L$. The decomposition can be checked in polynomial time. We do not compute a minimum-width decomposition. The target $r$ is ordinary input, not the parameter.

[BGNS24, Section 4.2][bgns24] uses the parameter $\alpha=\lceil p/\log_2 n\rceil$ for $n\ge2$. Our regularization is an equivalent parameterization: $\alpha\le2\kappa$ and $\kappa\le2\alpha+1$. Inputs with $n=0$ or $n=1$ are answered directly, with maximum clique sizes zero and one respectively. If $p=0$ and $n\ge1$, validity of the decomposition implies that the graph is edgeless, so its maximum clique size is one. Decision targets outside $0\le r\le p+1$ are immediate; the empty clique is allowed.

**Why it suffices to inspect bags.** The indices of bags containing a fixed vertex form an interval. For a clique, any two such intervals intersect, because every edge is present together in a bag. A finite collection of pairwise intersecting intervals has a common index: its largest left endpoint is no larger than its smallest right endpoint. Hence every clique is contained in a single bag. The source makes this observation in the discussion immediately before [BGNS24, Proposition 19, printed p.20][bgns24]. Consequently,

$$
\omega(G)=\max_{1\le i\le t}\omega(G[B_i]).
$$

**The fixed-width encoding for one bag.** For a bag $B$ of size $b\le p+1$, introduce one selection variable $x_v$ per vertex. For every nonedge $uv$ in $G[B]$, add $\neg x_u\lor\neg x_v$. Add the condition $\sum_{v\in B}x_v\ge r$ using the balanced binary adder and comparator in [METHODS, Section 2](METHODS.md). This uses $O(b)$ gates in total; encoding all gate equivalences uses clauses of width at most three. Empty bags and impossible thresholds are handled directly. Including all auxiliary variables, every nontrivial query has

$$
N=O(p+1),\qquad M=O((p+1)^2),\qquad w\le3.
$$

Local variable names give encoded length $O((p+1)^2\log(p+2))$. Constructing all queries costs only a polynomial in $L$. The possibly quadratic clause count is allowed by the general-density assumption; a linear variable count does not assert linear density.

One threshold decision uses at most $t\le L$ SAT calls. The manuscript's general profile therefore gives

$$
A(p+1)L^{O(1)}
=2^{O\!\left((p+1)\log\log(p+1)/\sqrt{\log(p+1)}\right)}L^{O(1)},
$$

with logarithms regularized for bounded values. Independently, the reported-formalization profile gives

$$
2^{O\!\left((p+1)(\log\log(p+1))^2/\sqrt{\log(p+1)}\right)}L^{O(1)}.
$$

These are alternative conditional guarantees, with polynomial space under their respective assumptions. They are not interchangeable quantitative bounds.

**Optimization and a witness.** Binary search over $0,\ldots,p+1$ finds $\omega(G)$ using $O(\log(p+2))$ threshold tests. Locate and retain a bag that admits the optimum, and fix its original selection bits one at a time, retaining a value whenever a SAT query admits an extension with that optimum threshold. At most $p+1$ further calls reconstruct an actual maximum clique in that bag. Altogether there are $O(t\log(p+2)+p+1)$ calls; unit clauses for the fixed bits preserve the displayed encoding bounds. The calls are sequential, so the same time forms and polynomial-space guarantee apply.

**Why this is FPT in the normalized width.** Write either displayed exponential factor as $2^{g(s)}$, where $g(s)=o_{\mathrm{eff}}(s)$. Since $s\le\kappa\log_2(n+2)$, choose a computable threshold $T(\kappa)$ with $g(s)\le s/\kappa$ for all $s\ge T(\kappa)$. In that range,

$$
2^{g(s)}\le2^{s/\kappa}\le n+2\le L+2.
$$

For $s<T(\kappa)$, the exponential factor is bounded by a computable function of $\kappa$ alone. Multiplying by the polynomial construction and query costs gives $F(\kappa)L^{O(1)}$ for decision, optimization, and reconstruction. Thus ordinary single-exponential dependence on the supplied width is replaced here by effective subexponential dependence, which makes this smaller normalized parameter FPT.

This bag-local argument has a precise scope. A clique can occur in several bags, so adding their counts would overcount; no counting aggregation is claimed. [BGNS24, Theorems 17 and 18, printed pp.17–18][bgns24] instead proves Log-Pathwidth Dominating Set and Log-Pathwidth Independent Set XNLP-complete under parameterized logspace reductions. If either of those problems admitted a deterministic $2^{o_{\mathrm{eff}}(p)}L^{O(1)}$ algorithm with the supplied width $p$, the same threshold argument and their completeness would imply $\mathrm{XNLP}\subseteq\mathrm{FPT}$. The catalog's current $A(n)L^{O(1)}$ algorithms for those problems concern the total number of vertices and do not establish an $A(p)L^{O(1)}$ bound. A supplied decomposition may have small width and many bags, so replacing $n$ by $p$ requires a new argument. No separation or logical independence follows from the absence of that argument here.

### 7.5. XP time and small space must hold simultaneously

The **Slice-wise Polynomial Space Conjecture (SPSC)** asserts that an XNLP-complete problem has no deterministic algorithm simultaneously using

$$
T\le L^{f(k)},\qquad S\le g(k)L^c,
$$

where $f,g$ are computable and the constant $c$ is independent of $k$ and $L$. This is the XP-time/FPT-space formulation in [PW16, Conjecture 2 and Corollary 13][pw16]; see also [BGNS24, Conjectures 30–31][bgns24]. The exponent in the space bound must be fixed. A separate XP-time algorithm and a separate FPT-space algorithm do not establish this simultaneous guarantee.

The current SAT assumptions do not establish such a simulation by the following two generic substitutions:

1. **Construct an XP-size formula.** If an explicit reduction produces a formula of length $L'=L^{h(k)}$, a polynomial-space guarantee in that length supplies only $(L')^{O(1)}=L^{O(h(k))}$ space. This displayed bound is XP space; it does not establish $g(k)L^c$ with a fixed $c$. The construction time and SAT running time must also be included.
2. **Construct an FPT-size computation tableau.** Unfolding an XNLP computation of FPT running time can give a fixed-width formula with $N+M\le F(k)L^d$ for a fixed $d$. Polynomial space then becomes FPT space. However, direct substitution gives a running-time guarantee of the form $A(F(k)L^d)F(k)L^{O(1)}$, which does not establish a polynomial dependence on $L$ for each fixed $k$. Thus this route supplies no XP-time guarantee. Even the sparse profile $B(F(k)L^d)$ does not fix this issue.

These are limitations of the displayed guarantees, not lower bounds on every possible encoding or algorithm.

For a concrete comparison, take either Independent Set or Dominating Set on an explicitly represented $n$-vertex graph with a supplied path decomposition of width $p$. Include that decomposition in $L$ and use the regularized parameter from Section 7.4,

$$
\kappa=\left\lceil\frac{p+1}{\log_2(n+2)}\right\rceil.
$$

Both decision problems are XNLP-complete for this parameter, equivalently up to bounded factors and tiny cases to the source's logarithmic pathwidth parameter [BGNS24, Section 4.2, Theorems 17–18][bgns24]; [Linear-XNLP25, Theorem 6][linear-xnlp25]. For **either** problem, the following statements have different strengths:

| Bound for the problem with a supplied decomposition | Space bound | Consequence under parameter $\kappa$ |
| --- | --- | --- |
| Current general-input transfer: $A(n)L^{O(1)}$ | $L^{O(1)}$ | This substitution does not establish XP time or FPT time in $\kappa$; the graph may be large even when $\kappa$ is constant. |
| Hypothetical $2^{O(p)}L^{O(1)}$ time | $p^{O(1)}L^{O(1)}$ | XP time and FPT space in $\kappa$; this would refute SPSC. |
| Hypothetical $2^{o_{\mathrm{eff}}(p)}L^{O(1)}$ time | No additional space premise needed for this implication | FPT in $\kappa$; by completeness, $\mathrm{XNLP}\subseteq\mathrm{FPT}$. |

For the second row, $p\le\kappa\log_2(n+2)$ turns the time bound into $(n+2)^{O(\kappa)}L^{O(1)}$, while the displayed space bound has a polynomial exponent independent of $\kappa$. For the third row, apply the effective-threshold argument of Section 7.2 with $p\le\kappa\log_2(n+2)$; bounded $p$ is absorbed into a computable function of $\kappa$. Completeness transfers the resulting algorithm through pl-reductions. The relationship between normalized pathwidth and simultaneous time–space bounds also appears in [PW16, Theorems 14–15][pw16].

The second and third rows are **sufficient hypothetical breakthroughs**, not algorithms obtained from the present SAT assumptions. In contrast, Section 7.4 actually obtains the effective subexponential dependence on $p+1$ for Clique by restricting the problem to individual bags.

### 7.6. Interpreting the remaining obstacles

There are three distinct kinds of statements here:

- **An unconditional class separation.** Section 7.1's $\mathrm{FPT}\subsetneq\mathrm{XP}$ concerns the full classes. It does not identify a particular W-hard or XNLP-complete problem as being outside FPT.
- **Hardness theorems and conditional barriers.** The cited reduction-based hardness theorems remain valid. Deducing the absence of an algorithm requires the corresponding noncollapse assumption or a time–space conjecture such as SPSC. These assumptions must be stated separately.
- **Limits of the present transfer.** Counting all auxiliary variables, controlling clause width, and obtaining simultaneous time and space bounds remain necessary. Failure to meet those conditions establishes that the proposed route is insufficient; it is not an impossibility theorem for the problem.

Under the pinned SAT assumptions, ETH-dependent lower bounds lose their hypothesis. Their implications remain mathematical theorems, but no longer exclude algorithms on that basis; an algorithm must still be constructed. This distinction applies equally to finer exponents within XP.

Polynomial space for an explicit SAT formula also does not give a logarithmic-space simulation in the original input length. XL and XNL use deterministic and nondeterministic space $f(k)\log L$, respectively, without the additional FPT-time requirement of XNLP [BGNS24, Section 2.2][bgns24]; [PW16, Section 3][pw16]. We establish neither $\mathrm{XNLP}\subseteq\mathrm{XL}$ nor $\mathrm{XL}=\mathrm{XNL}$.

The remaining obstacles are therefore substantial: some concern finding algorithms, and others concern preserving a specified resource bound through a reduction. This discussion does not prove logical independence between the SAT assumptions and any of the unresolved class or space questions.

## 8. Primary sources and locations

Full bibliographic metadata also appears in the [shared source register](ASSUMPTIONS.md#source-register). The locators below identify the statements used in this note.

| Reference | Version and locator |
| --- | --- |
| [SAT26: OpenAI, The Exponential Time Hypothesis is False][sat26] | October 25, 2026 revision, Git commit 42907c9e999cc00d6abe7a50bfc1023250edd1b9; Theorem 1.1 and reduction warning (p. 2); Corollary 1.4 (p. 3); Sections 1.1.1–1.1.2; Appendix C.1 (pp. 51–52). |
| [FG04: Flum–Grohe, Parameterized Complexity and Subexponential Time][fg04] | Bulletin of the EATCS 84 (2004), 71–100; Definition 4.2, Corollary 4.3, Theorem 4.4; Theorem 5.3, Corollaries 5.6, 5.8, 5.9; Proposition 5.10 and following discussion; Section 7. |
| [CG06: Chen–Grohe, An Isomorphism between Subexponential and Parameterized Complexity Theory][cg06] | ECCC TR06-011 (2006); Section 2.1; Definitions 11 and 13 (printed p.7), Theorem 14 (p.8), first step of the proof of Lemma 18 (p.9). Numbering follows this report. |
| [M08: Müller, Parameterized Randomization][mueller08] | November 2008 dissertation; Example 3.2.17 (printed pp. 125–126); Appendix A.2 (p. 195). |
| [DF95: Downey–Fellows, Fixed-Parameter Tractability and Completeness II][df95] | Author-hosted article; Introduction (Dominating Set), Corollary 3.2 (Clique). |
| [Marx10: Can You Beat Treewidth?][marx10] | Theory of Computing 6 (2010), 85–112; Theorem 1.5, p. 89. |
| [IPZ01: Which Problems Have Strongly Exponential Complexity?][ipz01] | JCSS 63(4) (2001), 512–530; Sections 1.1.1–1.1.4. |
| [Vadhan07: Basic Derandomization Techniques][vadhan07] | CS225 Lecture 5, February 15, 2007; Section 2, p. 2, Definition 1 and the following hierarchy statement. |
| [Williams10: Improving Exhaustive Search Implies Superpolynomial Lower Bounds][williams10] | Author full version dated May 4, 2010; Theorem 6.1, printed pp. 20–21 (PDF pp. 21–22); journal version SICOMP 42(3) (2013), 1218–1244. |
| [Downey12: A Basic Parameterized Complexity Primer][downey12] | Author-hosted 38-page version of the 2012 chapter; Definition 1 (PDF p.5) and Section 2 (PDF p.8). Strongly uniform FPT and the unconditional strict inclusion FPT ⊊ XP. |
| [CF08: Chen–Flum, Subexponential Time and Fixed-Parameter Tractability: Exploiting the Miniaturization Mapping][cf08] | Author preprint dated August 20, 2008; Section 2 (printed p.4), Proposition 43 (pp.26–27). Effective little-o and the FPT consequence of effective subexponential time in k log L. |
| [BGNS24: Bodlaender–Groenland–Nederlof–Swennenhuis, Parameterized problems complete for nondeterministic FPT time and logarithmic space][bgns24] | Information and Computation 300 (2024), 105195; Sections 2.2–2.3 (printed p.4), Section 2.4, Lemmas 1–3 (p.5); Section 4.2, Theorems 17–18 (pp.17–18), Clique discussion before Proposition 19 (p.20). Section 5.1, Conjectures 30–31 (p.31). Journal numbering; the repository PDF adds one cover page. |
| [Linear-XNLP25: Bodlaender–Groenland–Jacob–Jaffke–Lima, XNLP-Completeness for Parameterized Problems on Graphs with a Linear Structure][linear-xnlp25] | Algorithmica 87 (2025), 465–506; first online November 4, 2024. Introduction, Conjecture 1 and Footnote 2; Theorem 6; Sections 3.1–3.2. |
| [PW16: Pilipczuk–Wrochna, On Space Efficiency of Algorithms Working on Structural Decompositions of Graphs][pw16] | STACS 2016, LIPIcs 47, Article 57. Conjecture 2 (printed p.57:3); Section 3, Theorem 12, Corollary 13, Theorems 14–15 (pp.57:8–57:9). Numbering follows the STACS version. |

## 9. Scoped review record

- **Date:** 2026-10-29 (JST).
- **Status and reviewer:** `ai-audited` for Section 7 only; reviewer type AI, independent Codex agent `transfer_audit`.
- **Scope:** the English and Japanese Section 7, including class definitions and reduction conventions, the effective-threshold transfer, all-variable encoding counts and the supplied-width Clique algorithm, optimization and witness calls, normalized-parameter equivalence, the simultaneous time–space implications, and their primary-source locators in Section 8 and the shared source register. No substantive mathematical or translation errors were identified in this review.
- **Limits:** this does not record human verification, verification of the assumed SAT theorem, or a renewed audit of Sections 2–6. The note's overall status remains `draft`. Mechanical generation and preservation checks are separate from this mathematical review.

The exact reviewed Section 7 in each file is the UTF-8 substring beginning with `<a id="xp-xnlp"></a>` and ending immediately before the following `## 8` heading, including the intervening final blank lines. Its fingerprints are:

| File and scope | Bytes | SHA-256 |
| --- | ---: | --- |
| `COMPLEXITY.md`, Section 7 | 19,225 | `4cd67cb0739e917ecd7468590813ca7a6090a69383d5cda2dc068041c5697551` |
| `COMPLEXITY.ja.md`, Section 7 | 23,308 | `9b99786939cb217b50c3c5fb700f19ccc6cfa3fa80fa52c07dd004da8003ce63` |

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
