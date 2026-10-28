# Complexity-Theoretic Consequences

[日本語](COMPLEXITY.ja.md) · [Home](README.md) · [FPT](FPT.md) · [Exact algorithms](EXACT.md) · [Assumptions and sources](ASSUMPTIONS.md)

Updated: 2026-10-28. **Status: draft.** Codex carried out most research, derivation, and writing. No human verification is recorded. The statements below are conditional on the specified SAT assumptions; source attribution and mechanical checks do not certify the underlying theorem or these deductions. Inclusion is not a novelty claim.

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

These class consequences are companion material, outside the catalog's 23 algorithm-entry and 9 candidate counts. Here logarithms are base two; small inputs are handled separately. A different fixed logarithm base changes parameters only by bounded factors.

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

## 7. Primary sources and locations

Full bibliographic metadata also appears in the [shared source register](ASSUMPTIONS.md#source-register). The locators below identify the statements used in this note.

| Reference | Version and locator |
| --- | --- |
| [SAT26: OpenAI, The Exponential Time Hypothesis is False][sat26] | October 25, 2026 revision, Git commit 42907c9e999cc00d6abe7a50bfc1023250edd1b9; Theorem 1.1 and reduction warning (p. 2); Corollary 1.4 (p. 3); Sections 1.1.1–1.1.2; Appendix C.1 (pp. 51–52). |
| [FG04: Flum–Grohe, Parameterized Complexity and Subexponential Time][fg04] | Bulletin of the EATCS 84 (2004), 71–100; Definition 4.2, Corollary 4.3, Theorem 4.4; Theorem 5.3, Corollaries 5.6, 5.8, 5.9; Proposition 5.10 and following discussion; Section 7. |
| [CG06: Chen–Grohe, An Isomorphism between Subexponential and Parameterized Complexity Theory][cg06] | ECCC TR06-011 (2006); Section 2.1; Definition 13 (printed p. 7), Theorem 14 (p. 8). Numbering follows this report. |
| [M08: Müller, Parameterized Randomization][mueller08] | November 2008 dissertation; Example 3.2.17 (printed pp. 125–126); Appendix A.2 (p. 195). |
| [DF95: Downey–Fellows, Fixed-Parameter Tractability and Completeness II][df95] | Author-hosted article; Introduction (Dominating Set), Corollary 3.2 (Clique). |
| [Marx10: Can You Beat Treewidth?][marx10] | Theory of Computing 6 (2010), 85–112; Theorem 1.5, p. 89. |
| [IPZ01: Which Problems Have Strongly Exponential Complexity?][ipz01] | JCSS 63(4) (2001), 512–530; Sections 1.1.1–1.1.4. |
| [Vadhan07: Basic Derandomization Techniques][vadhan07] | CS225 Lecture 5, February 15, 2007; Section 2, p. 2, Definition 1 and the following hierarchy statement. |
| [Williams10: Improving Exhaustive Search Implies Superpolynomial Lower Bounds][williams10] | Author full version dated May 4, 2010; Theorem 6.1, printed pp. 20–21 (PDF pp. 21–22); journal version SICOMP 42(3) (2013), 1218–1244. |

[sat26]: https://github.com/openai/math/blob/42907c9e999cc00d6abe7a50bfc1023250edd1b9/preprints/The-Exponential-Time-Hypothesis-is-False-October-25-2026/paper.pdf
[fg04]: https://www.uni-ulm.de/fileadmin/website_uni_ulm/iui.inst.190/Mitarbeiter/toran/beatcs/column84.pdf
[cg06]: https://eccc.weizmann.ac.il/report/2006/011/download
[mueller08]: https://d-nb.info/993356915/34
[df95]: https://homepages.ecs.vuw.ac.nz/~downey/publications/2.pdf
[marx10]: https://toc.cs.uchicago.edu/articles/v006a005/v006a005.pdf
[ipz01]: https://cseweb.ucsd.edu/~russell/ipz.pdf
[vadhan07]: https://people.seas.harvard.edu/~salil/cs225/spring07/lecnotes/lec5.pdf
[williams10]: https://people.csail.mit.edu/rrw/improved-algs-lbs2.pdf
