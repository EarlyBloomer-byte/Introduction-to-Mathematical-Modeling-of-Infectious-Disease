# Section 2.3 — Practice Problems

## Conceptual (5)

1. Explain why $\mathcal{R}_0=\beta/(b+\gamma)$ has $b+\gamma$ in the denominator, rather than
   just $\gamma$ as in Sec. 2.1/2.2. What does adding $b$ to the denominator represent
   biologically?
2. Explain, without doing any algebra, why $J(P_0)$ being upper triangular makes finding its
   eigenvalues trivial, while $J(P^*)$ generally is not triangular.
3. What does it mean for an equilibrium to be "non-hyperbolic," and why does that make
   linearization inconclusive exactly at $\mathcal{R}_0=1$?
4. In your own words, explain what a Lyapunov function is trying to prove, and why proving
   $\dot L\le0$ everywhere is a fundamentally different (and often much easier) task than solving
   the differential equations directly.
5. Why does ruling out periodic orbits (Bendixson–Dulac) matter for proving global stability of
   $P^*$? What would go wrong with the global-stability conclusion if periodic orbits *could*
   exist?

## Computational (5)

6. For $b=0.01$, $\beta=0.3$, $\gamma=0.15$: compute $\mathcal{R}_0$. Is $P^*$ in the feasible
   region?
7. Using Q6's parameters, compute $S^*$ and $I^*$.
8. Compute $\text{tr}(J(P^*))$ and $\det(J(P^*))$ using the formulas (2.27)-(2.28) for Q6's
   parameters, and confirm both Routh–Hurwitz conditions hold.
9. For $b=0.02$, $\gamma=0.2$: find the exact value of $\beta$ at which the system undergoes its
   bifurcation (i.e. $\mathcal{R}_0=1$).
10. If $b\to0$ (no demography at all), what does $\mathcal{R}_0=\beta/(b+\gamma)$ reduce to? Does
    this match the threshold quantity from Sec. 2.1/2.2?

## Coding Exercises (2)

11. Write a function `local_stability_P0(b, beta, gamma)` that computes the Jacobian at $P_0$,
    finds its eigenvalues using `numpy.linalg.eigvals`, and returns `True`/`False` for stability.
    Test it against the analytical prediction ($\mathcal{R}_0<1$) across at least 5 parameter
    combinations.
12. Extend the phase-portrait code to add a 6th starting point placed very close to $P_0$ when
    $\mathcal{R}_0>1$ (e.g. $(0.999, 0.001)$). Confirm numerically that even this near-$P_0$
    starting point still eventually diverges away toward $P^*$ (consistent with $P_0$ being
    unstable/a saddle in this regime), rather than getting stuck near $P_0$.

## Visualization Exercise (1)

13. Plot the real part of both eigenvalues of $J(P_0)$ as a function of $\mathcal{R}_0$ (sweep
    $\beta$ across a range spanning $\mathcal{R}_0<1$ and $>1$, holding $b,\gamma$ fixed). At what
    $\mathcal{R}_0$ value does the relevant eigenvalue cross zero? Does this match Block 2's
    analytical claim?

## Challenge Problem (1)

14. The book's Bendixson–Dulac calculation used the Dulac multiplier $\alpha(S,I)=1/I$. Try
    recomputing $\frac{\partial(\alpha P)}{\partial S}+\frac{\partial(\alpha Q)}{\partial I}$
    using instead $\alpha(S,I)=1$ (i.e. no multiplier at all — just check
    $\partial P/\partial S+\partial Q/\partial I$ directly). Does this simpler choice also give a
    sign that's constant (all one sign) throughout the feasible region? If not, explain why the
    book bothered introducing the $1/I$ multiplier at all — what does it accomplish that plain
    $\partial P/\partial S+\partial Q/\partial I$ does not?

*Full worked solutions: `../solutions/section_2_3_solutions.md`.*
