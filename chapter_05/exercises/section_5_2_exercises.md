# Section 5.2 — Practice Problems

## Conceptual (5)

1. Explain why the term $\nu_2y(1-(x+y)/K)$ in the $y$-equation — infected-cell self-replication —
   is exactly what allows a chronic equilibrium to persist even at very low $\sigma$, in a way that
   has no analogue in any of the population-level epidemic models from Chapters 1–2.
2. In your own words, explain what a saddle point's "stable manifold" is, and why its existence is
   essential for genuine bistability (as opposed to just having two equilibria, which alone
   wouldn't be enough).
3. Why is $\mathcal{R}_0<1$ still a MEANINGFUL quantity to compute in a backward-bifurcation
   model, even though it no longer guarantees disease elimination by itself?
4. Explain the practical, real-world implication of backward bifurcation for a public health
   intervention that only aims to push $\mathcal{R}_0$ just below 1.
5. Why does ruling out periodic orbits (Block 4) matter for the completeness of this section's
   bistability claim — what would be different if periodic orbits COULD exist in this model?

## Computational (5)

6. Using this notebook's parameters, if $\sigma=0.05$ (well below $\sigma_c\approx0.185$), how
   many chronic equilibria would you expect, based on the numerically-located bifurcation diagram?
7. Compute $\mathcal{R}_0(\sigma)$ at $\sigma=0.05$ using formula (5.21) and this notebook's
   parameters ($x_0\approx836.68$).
8. Is $\mathcal{R}_0(0.05)<1$? Given the equilibrium count from Q6, does bistability still occur
   at $\sigma=0.05$?
9. Using the eigenvalues reported for the saddle point in Block 3
   ($\approx-0.214,\ +0.0428$), what does the POSITIVE eigenvalue tell you about the local
   behavior of trajectories exactly on the unstable manifold through that saddle?
10. If a hypothetical treatment could increase $\mu_2$ (the infected-cell death rate, e.g. via
    antiretroviral therapy), would you expect $\mathcal{R}_0(\sigma)$ to increase or decrease for
    fixed $\sigma$? Use formula (5.21) to justify your answer.

## Coding Exercises (2)

11. Write a function `classify_equilibrium(x, y, sigma, params)` that computes the Jacobian
    eigenvalues at a given point and returns `"stable node"`, `"saddle"`, or `"unstable"` based on
    their signs. Test it on all three equilibria found in Block 3.
12. Modify the bistability simulation to sweep MANY starting points on a grid (e.g. 20×20 across
    the feasible region) at $\sigma=0.1$, color each by its final outcome (disease-free vs.
    chronic), and produce a scatter plot showing the approximate basin boundary. Does the boundary
    look like it passes near the saddle point?

## Visualization Exercise (1)

13. Plot $\mathcal{R}_0(\sigma)$ (formula 5.21) as a function of $\sigma\in[0,1]$ using this
    notebook's parameters, and mark the numerically-located $\sigma_c\approx0.185$ on the plot.
    Confirm $\mathcal{R}_0(\sigma_c)$ is close to (or exactly) 1.

## Challenge Problem (1)

14. This section found that, for the specific parameter set used, chronic equilibria persist even
    as $\sigma\to0$ (rather than the "clean" 0→1→2→1 pattern with a lower threshold $\sigma_0$
    shown in the book's own Figure 5.2). Design a numerical experiment to search for a DIFFERENT
    parameter set (varying $\nu_2$ relative to $\mu_2$, in particular) where the equilibrium count
    genuinely starts at 0 for small $\sigma$. (Hint: think about what has to be true for infected
    cells to NOT be able to sustain themselves via pure proliferation alone — i.e., what if
    $\nu_2<\mu_2$, so infected cells have a "natural" per-capita disadvantage even before
    accounting for population limits?) Report what you find.

*Full worked solutions: `../solutions/section_5_2_solutions.md`.*
