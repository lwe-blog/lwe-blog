---
layout: post
title: "Deterministic Sparsification"
authorid: chenyang
---

<div style='display:none;'><script type='math/tex'>
\newcommand{\paren}[1]{\left( #1 \right)}
\newcommand{\dotp}[1]{\left\langle #1 \right\rangle }
</script></div>

Let $G$ be a dense graph. A sparse graph $H$ is a sparsifier of $G$
approximation of $G$ that preserves certain properties such as quadratic forms
of its Laplacian. This post will formally define spectral sparsification, then
present the intuition behind the deterministic construction of spectral
sparsifiers by Baston, Spielman and Srivastava [[BSS08](#BSS08)].

Bencz&uacute;r and Karger [[BK96](#BK96)] introduced the cut sparsifier, which
ensures that the value of all cuts in $H$ approximates that of all cuts in $G$:

> <a name="def:cut-sp"></a>**Definition 1 (Cut Sparsification):** A weighted
> undirected graph $H = (V, E_H)$ is an $\epsilon$-cut sparsifier of a weighted
> undirected graph $G = (V, E_G)$ if for all $S \subset V$,
>
> $$
> (1-\epsilon)E_G(S, V / S) \le E_H(S, V / S) \le (1+\epsilon)E_G(S, V / S)
> $$
>
> Where $E_G$ and $E_H$ are the sums of edge weights crossing the cuts in $G$ and
> $H$ respectively.

Spielman and Teng [[ST08](#ST08)] introduced another notion of graph sparsification
with the quadratic form of the Laplacian:

> <a name="def:spectral-sp"></a>**Definition 2 (Spectral Sparsification):** A
> weighted undirected graph $H = (V, E_H)$ is an $\epsilon$-spectral sparsifier
> of a weighted undirected graph $G = (V, E_G)$ if for all $x \in
> \mathbb{R}^{|V|}$,
>
> $$
> (1-\epsilon)x^T L_G x \le x^T L_H x \le (1+\epsilon) x^T L_G x
> $$
>
> Where $L_G$ and $L_H$ are the graph Laplacians of $G$ and $H$ respectively.

<!-- Add uses of sparsifiers -->
<!-- Add why it's important for H to be weighted? -->

Note that this stronger than cut sparsification, as we can fix $x$ to be the
indicator vectors of cuts to obtain [definition 1](#def:cut-sp). Also note that
this notion of sparsifiers also provides bounds on Laplacian eigenvalues, and
thus spectral sparsifiers of complete graphs are also expanders. These
sparsifiers can be constructed in a randomized manner by sampling edges
proportional to their effective resistance [[SS08](#SS08)], but in this post we
will focus on a deterministic construction presented in [[BSS08](#BSS08)], as
stated more precisely in the following theorem:

> <a name="thm:sparsifier"></a>**Theorem 1:** For every $d > 1$, every undirected
> graph $G = (V, E)$ on $n$ vertices contains a weighted subgraph $H = (V, F,
> \tilde{w})$ with $\lceil d(n-1) \rceil$ edges that satisfies:
>
> $$
> x^T L_G x \le x^T L_H x \le \left(\frac{d+1+2\sqrt d}{d+1-2\sqrt d}\right) x^T L_G x \quad \forall x \in \mathbb{R}^{|V|}
> $$

## Preliminaries
The Laplacian $L$ of a graph can be seen as a linear transformation relating the
flow and demand in an electrical flow on the graph where each edge has unit
resistance. Let $B \in \mathbb{R}^{m \times n}$ be the vertex-edge incidence
matrix and $W$ is a diagonal matrix of edge weights, then $L = B^TWB$. $L$ also
has a pseudoinverse which acts like an actual inverse for all vectors
$x \bot \mathbb 1$, resulting from solving an electrical flow on $G$.

Let $\kappa = \frac{d+1+2\sqrt d}{d+1-2\sqrt d}$, we assume that the graph is
connected thus $x \bot \mathbf{1}$, and perform a transformation on the
condition in [Theorem 1](#thm:sparsifier):

$$
\begin{align*}
& x^T L_G x \le x^T L_H x \le \kappa x^T L_G x & \forall x \bot \mathbf{1} \\
\iff & 1 \le \frac{x^T L_H x}{x^T L_G^{-1/2} L_G^{-1/2} x} \le \kappa & \forall x \bot \mathbf{1} \\
\iff & 1 \le \frac{y^T L_G^{-1/2} L_H L_G^{-1/2} y}{y^T y} \le \kappa & \forall y \in \text{im}(L_G)
\end{align*}
$$

Let $b_{e = (u, v)} = \mathbf{1}_u - \mathbf{1}_v$ be a row of incidence matrix
$B$ , $s_e$ be the weight of edge $e$ in $E_H$ and $A \succeq B$ when $A - B$ is
a psd matrix. Then the above condition can be rewritten as:

<a name="eq:sparse-approx"></a>
$$
\begin{align}
  I \preceq \sum_{e \in E_H} L_G^{-1/2} b_e^T s_e b_e L_G^{-1/2} \preceq \kappa I
\end{align}
$$

We then define a vector $v_e = L_G^{-1/2}b_e^T$ for each $e \in E_G$. Notice
that over all edges of $G$, the rank 1 matrices formed by $v_eV_e^T$ sum to the
identity matrix:

$$
\sum_{e\in E_G} v_e v_e^T = \sum_{e\in E_G} L_G^{-1/2}b_e^T b_e L_G^{-1/2} = L_G^{-1/2} B^T B L_G^{-1/2} = L_G^{-1/2} L_G L_G^{-1/2} = I
$$

Then [equation 1](#eq:sparse-approx) can be interpreted as choosing a sparse
subset of the edges in $G$, as well as weights $s_e$, so that the matrix
obtained by summing over the edges of $H$, $\sum_{e \in E_H} s_e v_ev_e^T$, has
a low condition number (ratio between the largest and smallest eigenvalues):

$$
I \preceq \sum_{e \in E_H} s_e v_ev_e^T \preceq \kappa I
$$

If we can find such a sparse set of edges and weights, then we have proved
[Theorem 1](#thm:sparsifier). In [[SS08](#SS08)] this was done by randomly
sampling these rank-1 matrices based on their effective resistances of their
corresponding edges, using a distribution that has the identity matrix as the
expectation. Convergence is shown using a matrix concentration inequality. The
construction in [[BSS08](#BSS08)] deterministically chooses each $v_e$ and
$s_e$, bounding the increase in $\kappa$ in each step using barrier
functions. One useful lemma for this procedure is:

> <a name="lem:matrix-det"></a>**Lemma 1 (Matrix Determinant Lemma):** If $A$ is
> nonsingular and $v$ is a vector, then:
>
> $$
> \det(A + vv^T) = \det(A)(1 + v^TA^{-1}v)
> $$

## Main Proof

Recall from the previous section the main theorem that need to be proved is:

> <a name="thm:rank1approx"></a>**Theorem 2:**
> Suppose $d > 1$ and $v_1, \cdots, v_m$ are vectors in $\mathbb{R}^n$ with
> $$
> \sum_{i \le m} v_i v_i^T = I.
> $$
> Then there exist scalars $s_i > 0$ with $|\{i: s_i \ne 0 \}| \le dn$ so that
> $$
> I \preceq \sum_{i \le m} s_i v_iv_i^T \preceq \left(\frac{d+1+2\sqrt d}{d+1-2\sqrt d}\right) I
> $$

This is equivalent to bounding the ratio of $\lambda_{\min}$ and
$\lambda_{\max}$ of the matrix $\sum_{i \le m} s_i v_iv_i^T$.

We start with a matrix $A = 0$, and build it by adding rank-1 updates
$s_ev_ev_e^T$. One interesting fact is that for any vector $v$, the eigenvalues
of $A$ and $A + vv^T$ interlace. Consider the characteristic polynomial of $A +
vv^T$:

$$
p_{A + vv^T}(x) = \det(I - A - vv^T) = p_A(x) \paren{1 - \sum_j \frac{\dotp{v,u_j}^2}{x - \lambda_j}}
$$

Which can be written in terms of the characteristic polynomial of $A$ using
[Lemma 1](#lem:matrix-det). $u_j$ are the eigenvectors of $A$.  Let $\lambda$ be
a zero of $p_{A + vv^T}(x)$. It can either:

1. Be a zero of $p_A(x)$, so $\lambda$ is equal to an eigenvalue $\lambda_i$
  of $A$, and the corresponding eigenvector $u_i$ is orthogonal to $v$. In this
  case, this eigenvalue doesn't move.
2. Strictly interlace with the old eigenvalues. This happens when
  $p_A(\lambda) \ne 0$ and
  $$
  \sum_j \frac{\dotp{v,u_j}^2}{x - \lambda_j} = 1
  $$
  This can be interpreted with a physical model. Consider $n$ positive charges
  arranged vertically with the $j$-th charge's position corresponding to the
  $j$-th eigenvalue of $A$, and its charge is $\dotp{v, u_j}^2$. The points
  where the electric potential is 1 are the new eigenvalues. Since between any
  two charges the potential changes direction from $+ \infty$ to $- \infty$,
  there has to be a point between every two charges where the potential is 1,
  thus the new eigenvalues strictly interlace the old ones.

To get some intuition, we see what happens when we sample $v_i$ uniformly
randomly. Since $\sum_j v_jv_j^T = I$, $\mathbb{E}\_v[\dotp{v, u}^2]$ is constant
for any normalized vector $u$. Therefore adding the average $v$ increases the
charges by the same amount in the physical model, causing the new eigenvalues to
all increase by the same amount. Informally, we expect all the eigenvalues to
"march forward" at similar rates with each $vv^T$ added, so $\lambda_{\max} /
\lambda_{\min}$ is bounded.

We construct a sequence of matrices $A^{(0)}, \cdots, A^{(q)}$ by adding rank-1
updates $t vv^T$. To bound the condition number after each update, we create two
barriers $l < \lambda_{\min}(A) < \lambda_{\max}(A) < u$ so that the eigenvalues
of $A$ lie between them. $\Phi_l(A)$ and $\Phi^u(A)$ are defined as the
potentials at the barriers respectively:

$$
\Phi_l(A) := \sum_i \frac{1}{\lambda_i - l}, \quad \Phi^u(A) := \sum_i \frac{1}{u - \lambda_i}
$$

The crucial step is to show that there exists a $v\_i$ and $t$ so that we can
add $t v_i v_i^T$ to $A$, so that each barrier is shifted by a constant, and the
potentials at each barrier doesn't change. We will sketch out the proof briefly,
readers can pursue the details in [[BSS08](#BSS08)].

Let constants $\delta_U$ and $\delta_L$ be the maximum amount each barrier can
increase each round, and constants $\epsilon_U = \Phi^{u_0}(A^{(0)})$ and
$\epsilon_L = \Phi_{l_0}(A^{(0)})$ be the initial potentials at each
barrier. The first lemma shows that if $t$ is not too large, adding $t vv^T$ to
$A$ and shifting the upper barrier by $\delta_U$ will not increase the upper
potential $\Phi^u$.

> **Lemma 2 (Upper Barrier Shift):** Suppose $\lambda_{\max}(A) < u$, and $v$ is
> any vector. If
> $$
> U_A(v) := v^T \paren{\frac{((u + \delta_U)I - A)^{-2}}{\Phi^u(A) - \Phi^{u+ \delta_U}(A)}
> + ((u + \delta_U)I - A)^{-1}} v \le \frac{1}{t}
> $$
> Then:
> $$
> \Phi^{u+ \delta_U}(A + tvv^T) \le \Phi^{u}(A) \quad \text{and} \quad
>     \lambda_{\max}(A + tvv^T) < u + \delta_U
> $$

The second lemma shows that if $t$ is not too small, adding $t vv^T$ to $A$ and
shifting the lower barrier by $\delta_L$ will not increase the lower potential
$\Phi^u$.

> **Lemma 3 (Lower Barrier Shift):** Suppose $\lambda_{\min}(A) > l$, $\Phi_l(A)
> \le 1/\delta_L$ and $v$ is any vector. If
> $$
> L_A(v) := v^T \paren{\frac{(A - (l + \delta_L)I)^{-2}}{\Phi_{l+ \delta_L}(A) - \Phi_l(A)}
> + ((A - (l + \delta_L)I)^{-1}} v \ge \frac{1}{t} > 0
> $$
> Then:
> $$
> \Phi_{l+ \delta_L}(A + tvv^T) \le \Phi_{l}(A) \quad \text{and} \quad
> \lambda_{\min}(A + tvv^T) > l + \delta_L
> $$

Finally, it can be shown that there exists a $t$ and $v_i$ that satisfy the
conditions of the above lemmas.

> **Lemma 3 (Both Barriers):** If $\lambda_{\max}(A) < u$, $\lambda_{\min}(A) >
> l$, $\Phi^u(A) \le \epsilon_U$, $\Phi_l(A) \le \epsilon_L$, and $\epsilon_U$ ,
> $\epsilon_L$, $\delta_U$, $\delta_L$ satisfy:
> $$
> 0 \le \frac{1}{\delta_U} + \epsilon_U \le \frac{1}{\delta_L} + \epsilon_L,
> $$
> then there exists a $v_i$ and positive $t$ for which
> $$
> L_A(v_i) \ge \frac{1}{t} \ge U_A(v_i) \quad \text{and} \quad l + \delta_L <
> \lambda_{\min}(A + t v_iv_i^T) < \lambda_{\max}(A + t v_iv_i^T) < u + \delta_U
> $$

This is proved by an averaging argument relating the behavior of vector $v$ to
the behavior of the expected vector, showing that
$$
\sum_{i \le m} (L_A(v_i) - U_A(v_i)) \ge 0,
$$
therefore there exists a $i$ for which there is a gap between
$L_A(v_i) - U_A(v_i)$. Choosing the constants carefully, we can get the required
bound on the condition number.

## Extensions

TODO

## References

<a name="BK96">[BK96]</a>
A. A. Bencz&uacute;r and D. R. Karger. Approximating s-t minimum cuts in
$\tilde{O}(n^2)$ time. In *STOC '96*, pages 47-55, 1996.

<a name="BSS08">[BSS08]</a>
J. Baston, D. A. Spielman and N. Srivastava. Twice-Ramanujan
Sparsifiers. Available at <http://arxiv.org/abs/0808.0163>, 2008.

<a name="SS08">[SS08]</a>
D. A. Spielman and N. Srivastava. Graph Sparsification by Effective
Resistances. In *STOC '08*, pages 563-568, 2008.

<a name="ST08">[ST08]</a>
D. A. Spielman and S.-H. Teng. Spectral Sparsification of Graphs. Available at
<http://arxiv.org/abs/0808.4134>, 2008.
