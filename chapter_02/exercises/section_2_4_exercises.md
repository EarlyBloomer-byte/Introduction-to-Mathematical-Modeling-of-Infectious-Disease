# Section 2.4 — Practice Problems

## Conceptual (5)

1. Give an everyday (non-epidemiology) example of a function that is homogeneous of degree 1, and
   one that is NOT (explain why not).
2. Explain why the model (2.31) having "no biologically relevant equilibrium unless $b=d$" doesn't
   mean the model is useless or broken — what does it actually tell you?
3. Explain, in your own words, why fractional/proportional dynamics can be fully understood
   without knowing whether $N(t)$ is growing, shrinking, or constant.
4. In Case IIIa, explain how it's possible for $\mathcal{R}_0<1$ (suggesting "no epidemic") and
   yet $I(t)\to\infty$ at the same time. What has to be true about $N(t)$ for this to happen?
5. Why is disease prevalence (a fraction/percentage) often a more meaningful public-health
   statistic than a raw case count, in a population that isn't demographically stable? Use this
   section's results to justify your answer.

## Computational (5)

6. For $\lambda=0.4$, $b=0.03$, $\gamma=0.15$: compute $\mathcal{R}_0$ for the projected system.
7. Using Q6's parameters, compute the endemic equilibrium fractions $(s^*,i^*,r^*)$.
8. For $b=0.04$, $d=0.01$, $\gamma=0.2$: find the value of $\lambda$ at which $\mathcal{R}_1=1$
   exactly.
9. For $\lambda=0.3$, $d=0.02$, $\gamma=0.25$: compute $\mathcal{R}_1$. If $b>d$ and
   $\mathcal{R}_0<1$, will $I(t)$ grow or decay?
10. If $b=0.05$ and $d=0.02$, what is the growth rate of $N(t)$ (i.e. the exponent in
    $N(t)=N_0e^{(b-d)t}$)? After how many time units does $N(t)$ double?

## Coding Exercises (2)

11. Write a function `check_homogeneity(f, x, scale_factor)` that numerically checks whether a
    given vector-valued function `f` satisfies `f(scale_factor * x) == scale_factor * f(x)` (within
    a numerical tolerance), and test it on both the model's right-hand side and a function you
    construct that is deliberately NOT homogeneous of degree 1 (e.g. add a constant term).
12. Modify the Case IIIa simulation code to sweep $\lambda$ across a range that crosses
    $\mathcal{R}_1=1$ (holding $b,d,\gamma$ fixed, with $\mathcal{R}_0<1$ throughout), and plot the
    long-run growth/decay rate of $I(t)$ (estimated via a linear fit to $\log I(t)$ over the tail
    of the simulation) as a function of $\lambda$. Confirm the sign change happens at the predicted
    $\mathcal{R}_1=1$ point.

## Visualization Exercise (1)

13. Plot $N(t)$ for all three demographic cases (b=d, b<d, b>d) on the SAME axes (log scale on the
    y-axis), starting from the same $N_0$. Describe the three qualitatively different shapes.

## Challenge Problem (1)

14. Consider generalizing to a function homogeneous of degree $\delta\ne1$ (as in the book's own
    Exercise 1 for this section): $f(\lambda x)=\lambda^\delta f(x)$. Redo the Euler Identity
    derivation (differentiate both sides with respect to $\lambda$, then set $\lambda=1$) for this
    more general case, and state the resulting identity. Then check: is
    $g(x_1,x_2)=x_1^2+x_2^2$ homogeneous of some degree $\delta$? If so, what is $\delta$, and does
    your generalized Euler Identity correctly predict $\sum_i \frac{\partial g}{\partial x_i}x_i$
    when you compute it directly by hand?

*Full worked solutions: `../solutions/section_2_4_solutions.md`.*
