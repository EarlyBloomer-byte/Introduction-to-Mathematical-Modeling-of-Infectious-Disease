# Sections 3.7–3.8 — Practice Problems

## Conceptual (5)

1. Explain the difference between "$I(t)$ stays positive for all $t$" (a weaker property) and
   "uniform persistence" (as defined by (3.18)). Give an example of a hypothetical $I(t)$ that
   satisfies the first but not the second.
2. Why does Theorem 3.7.1 focus on the dynamics of the BOUNDARY $\partial D$ rather than directly
   analyzing the interior?
3. Explain why a Metzler matrix's off-diagonal non-negativity is exactly the algebraic condition
   needed for "no leapfrogging" (order-preservation) in the corresponding system of ODEs.
4. What does it mean for a nonnegative matrix to be "irreducible," in terms of its associated
   directed graph? Why does irreducibility strengthen the Perron-Frobenius conclusion (simple
   eigenvalue, strictly positive eigenvector) rather than just a non-negative one?
5. Explain why Theorem 3.8.5 requires BOTH strong monotonicity AND strict sublinearity — what
   could go wrong with only one of the two conditions?

## Computational (5)

6. For Sec. 2.3's model with $b=0.03$, $\gamma=0.15$: find the range of $\beta$ for which the
   model is uniformly persistent (i.e. $\mathcal{R}_0>1$).
7. Is the matrix $A=\begin{pmatrix}-2&3\\1&-4\end{pmatrix}$ Metzler? Compute its stability
   modulus $s(A)$ (via its eigenvalues) and determine whether it's stable.
8. Is the matrix $A=\begin{pmatrix}-2&-3\\1&-4\end{pmatrix}$ Metzler? (Careful — check ALL
   off-diagonal entries.)
9. For a $3\times3$ Metzler matrix with a strongly connected associated directed graph, is it
   irreducible? What if the graph had two disconnected "islands" of vertices?
10. Using Theorem 3.8.3's equivalence (1)⟺(4): if $A=\begin{pmatrix}-3&1\\2&-5\end{pmatrix}$ is
    stable, find (by inspection or solving) a strictly positive vector $x>0$ such that $Ax<0$
    (componentwise).

## Coding Exercises (2)

11. Write a function `is_uniformly_persistent_numerically(rhs_func, params, R0_func, n_trials=10,
    t_tail=(400,600))` that, given a model's right-hand side, parameters, and a function computing
    $\mathcal{R}_0$, simulates several random initial conditions and returns whether ALL
    trajectories' tails stay above a common small threshold (a numerical proxy for uniform
    persistence). Test it on Sec. 2.3's model for both an $\mathcal{R}_0>1$ and $\mathcal{R}_0<1$
    case.
12. Write a function `is_metzler(A)` that checks whether a given matrix has all non-negative
    off-diagonal entries, and a function `is_irreducible(A)` that checks strong connectivity of
    the associated directed graph (hint: you can use `scipy.sparse.csgraph.connected_components`
    with `connection='strong'`, treating nonzero entries as edges). Test both on at least 3
    example matrices, including one reducible Metzler matrix (e.g. a block upper-triangular one).

## Visualization Exercise (1)

13. For Sec. 2.3's model, create a plot showing $\liminf I(t)$ (estimated as the minimum of $I(t)$
    over a late time window, e.g. $t\in[400,600]$) as a function of $\mathcal{R}_0$, sweeping
    $\beta$ across a range that crosses $\mathcal{R}_0=1$. Describe the shape near the threshold —
    does persistence "turn on" smoothly or abruptly as $\mathcal{R}_0$ crosses 1?

## Challenge Problem (1)

14. Sec 3.7 notes the boundary $\partial D$ "may not be positively invariant for most epidemic
    models." Consider Sec. 2.3's model restricted to the $S$-axis ($I=0$): is this axis positively
    invariant (i.e., does a trajectory starting with $I=0$ stay at $I=0$ forever)? Now consider the
    line $S+I=1$ (i.e. $R=0$, no demography deaths having occurred yet, if we imagine the full 3D
    system): is THIS boundary piece positively invariant? Explain the difference, and discuss why
    Theorem 3.7.1's framework is stated carefully enough to handle boundaries with mixed invariance
    properties (some invariant pieces, some not).

*Full worked solutions: `../solutions/section_3_7_3_8_solutions.md`.*
