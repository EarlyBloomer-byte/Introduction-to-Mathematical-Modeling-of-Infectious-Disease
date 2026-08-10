# Section 2.1 — Practice Problems

## Conceptual (5)

1. Explain, biologically, why the tangency of the vector field on the $SR$-plane (Property 1's
   first check) means "no infection at the start implies no infection ever" in this particular
   model. Would this still be true if the model had a term for infectious immigrants entering
   from outside the population?
2. Property 5 shows an epidemic always ends with some susceptibles left over. Explain why this
   does *not* contradict the idea of "herd immunity" — what is herd immunity actually claiming,
   if not "100% will be infected eventually"?
3. Explain in your own words why $I_{max}$ occurring exactly at $S=\rho$ is a *stronger* statement
   than just "the epidemic peaks somewhere in the middle."
4. The final-size equation (2.7) can be used in two directions (predictive and inferential).
   Describe a realistic public-health scenario for each direction.
5. Why does the Threshold Theorem's approximation get worse as $\nu=S_0-\rho$ grows? Point to the
   specific step in the derivation where this limitation originates.

## Computational (5)

6. If $\beta=0.0006$ and $\gamma=0.15$, compute $\rho$.
7. Using $\rho$ from Q6, if $S_0=800$, is this above or below threshold? Compute
   $\mathcal{R}_0=S_0/\rho$.
8. For the first integral $\varphi(S,I)=I+S-\rho\ln S$, with $\rho=250$, $S_0=600$, $I_0=10$,
   compute the constant $C=\varphi(S_0,I_0)$.
9. Using $C$ from Q8, compute $I_{max}$ using $I_{max}=C-\rho+\rho\ln\rho$ (this is (2.6)
   evaluated at $S=\rho$, rearranged).
10. If a real outbreak had $S_0=10{,}000$ and ended with $S_\infty=6{,}000$ known from data, use
    equation (2.8) to estimate $\rho$, then estimate $\mathcal{R}_0=S_0/\rho$.

## Coding Exercises (2)

11. Write a function `final_size(S0, rho)` that numerically solves the transcendental final-size
    equation (2.7) for $S_\infty$ using `scipy.optimize.brentq` (or R's `uniroot`), and test it
    against at least 3 different $(S_0,\rho)$ pairs, comparing to a full ODE simulation's
    long-run $S(t)$ as in the notebook.
12. Write a function `threshold_theorem_estimate(rho, nu)` that returns the approximate
    $R_\infty\approx2\nu$ estimate, and a second function `exact_final_size_R(rho, nu)` that
    computes the exact $R_\infty$ via the final-size equation (using $S_0=\rho+\nu$). Plot the
    ratio `exact/approx` as a function of $\nu$ for $\nu$ ranging from 1 to 500 (holding $\rho$
    fixed), and describe the trend.

## Visualization Exercise (1)

13. Recreate Figure 2.1 (the family of epidemic curves) but change $\rho$ (e.g. try both a small
    and a large value) while keeping the same set of $(S_0,I_0)$ starting points. How does
    changing $\rho$ alone reshape the family of curves?

## Challenge Problem (1)

14. The Threshold Theorem derivation used a *second-order* Taylor expansion of $e^{-R/\rho}$
    (Step 2 in Block 4). Redo the derivation conceptually using a *third-order* Taylor expansion
    instead (you do not need to solve the resulting cubic ODE in closed form — that generally
    isn't possible in elementary functions). Discuss: would you expect the resulting approximation
    of $R_\infty$ to be more accurate than the second-order version, and why might the book have
    chosen to stop at second order despite this?

*Full worked solutions: `../solutions/section_2_1_solutions.md`.*
