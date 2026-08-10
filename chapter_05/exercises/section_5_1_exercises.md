# Section 5.1 — Practice Problems

## Conceptual (5)

1. Explain why $\mathcal{R}_0=\frac{\lambda\kappa}{(\kappa+\mu)(\gamma+\mu)}$ reduces to Sec.
   2.3's $\mathcal{R}_0=\beta/(b+\gamma)$ in the limit $\kappa\to\infty$ (instantaneous
   progression through the latent stage). What does $\kappa\to\infty$ mean biologically?
2. Why does adding a latent compartment $E$ NOT require any fundamentally new stability-analysis
   technique beyond what Chapter 3 already developed?
3. Explain, using the "competing exponential clocks" idea, why $\kappa/(\kappa+\mu)$ is the
   probability of surviving the latent period.
4. Why is it significant that the SAME Lyapunov function structure ($L=\kappa E+(\kappa+\mu)I$)
   that proves global stability of $P_0$ also, via the same $L$, proves uniform persistence when
   $\mathcal{R}_0>1$? What does this tell you about the relationship between these two theorems?
5. Explain why the one eigenvalue $p_1=-\mu$ at $P_0$ can be read off immediately, without doing
   any Routh-Hurwitz calculation.

## Computational (5)

6. For $\lambda=0.4$, $\kappa=0.15$, $\gamma=0.2$, $\mu=0.02$: compute $\mathcal{R}_0$.
7. Using Q6's parameters, compute the endemic equilibrium $S^*$.
8. If the mean latent period is 8 days and the mean infectious period is 5 days, and $\mu$
   (natural birth/death rate) corresponds to a 70-year lifespan, estimate $\kappa$, $\gamma$, and
   $\mu$ in units of 1/days.
9. Using Q8's parameters and $\lambda=0.5$, compute $\mathcal{R}_0$.
10. Compare Q9's $\mathcal{R}_0$ to what it would be if $\mu=0$ (ignoring demography's effect on
    the latent-period survival probability). How much does that survival factor matter
    numerically here?

## Coding Exercises (2)

11. Write a function `seir_R0(lam, kappa, gamma, mu)` implementing (5.7), and a function
    `seir_equilibrium(lam, kappa, gamma, mu)` that returns $P^*$ if $\mathcal{R}_0>1$ or `None`
    otherwise. Test both against this notebook's random parameter sweep.
12. Modify the global-convergence simulation code to sweep $\lambda$ across a range spanning
    $\mathcal{R}_0=1$, and plot the long-run $I^*$ (estimated from a long simulation) as a
    function of $\mathcal{R}_0$. Confirm the transcritical-bifurcation shape (flat at 0 below
    threshold, rising above it) matches the pattern seen in Sec. 2.2/2.3's simpler models.

## Visualization Exercise (1)

13. Recreate the SEIR transfer diagram (Figure 5.1) showing all four compartments and the six
    labeled arrows (birth into S, S→E, E→I, I→R, and death out of every compartment).

## Challenge Problem (1)

14. The book's proof of Theorem 5.1.2 for $P_0$ factors the characteristic polynomial into
    $(p+\mu)$ times a quadratic. Verify this factorization symbolically (using sympy or by hand)
    by computing the full $3\times3$ characteristic polynomial $\det(pI-J(P_0))$ directly and
    confirming it equals $(p+\mu)[p^2+(\kappa+\gamma+2\mu)p+(\kappa+\mu)(\gamma+\mu)-\lambda\kappa]$.

*Full worked solutions: `../solutions/section_5_1_solutions.md`.*
