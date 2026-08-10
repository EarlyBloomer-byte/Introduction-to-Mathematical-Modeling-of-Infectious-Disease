# Section 2.5 — Practice Problems

## Conceptual (5)

1. Explain, in your own words, why malaria needs a fundamentally different modeling structure
   (two populations, cross-infection) than every model earlier in this chapter.
2. Why are $b_1$ and $b_2$ modeled as two DIFFERENT probabilities, rather than a single shared
   transmission probability? Give a biological reason they might genuinely differ.
3. Explain the bite-conservation relation $aV=\tilde aH$ in your own words — why must these two
   quantities be equal?
4. In the heuristic $\mathcal{R}_0$ derivation, explain why the argument has to "chain through"
   two full stages (human→mosquito, then mosquito→human) rather than just one, unlike the
   single-population models earlier in the chapter.
5. Explain the difference between "homogeneous of degree 1" (Sec. 2.4) and "strictly sublinear"
   (this section). Why does the Ross–MacDonald model end up strictly sublinear rather than exactly
   homogeneous once reduced to the $(x,y)$ system, even though the ORIGINAL 4D system (2.48) is
   exactly homogeneous of degree 1?

## Computational (5)

6. For $a=0.25$, $b_1=0.4$, $b_2=0.5$, $\gamma_1=0.12$, $\gamma_2=0.08$, $m=3$: compute
   $\mathcal{R}_0$.
7. Using Q6's parameters, is the disease endemic or does it die out?
8. If mosquito control efforts cut $m$ (mosquitoes per human) in half, and everything else stays
   the same as Q6, what is the new $\mathcal{R}_0$?
9. Bednets reduce the biting rate $a$. Using Q6's original parameters, find the value of $a$ that
   would bring $\mathcal{R}_0$ down to exactly 1 (holding all other parameters fixed at Q6's
   values).
10. Compare your answers to Q8 and Q9: cutting the mosquito population in half only partially
    reduces $\mathcal{R}_0$ (from Q6 to Q8), while reducing the biting rate can bring
    $\mathcal{R}_0$ all the way to 1. Using the formula (2.52), explain why $a$ has a
    disproportionately large effect compared to $m$.

## Coding Exercises (2)

11. Write a function `malaria_R0(a, m, b1, b2, gamma1, gamma2)` implementing formula (2.52), and
    a companion function `malaria_equilibrium(a, m, b1, b2, gamma1, gamma2)` that returns
    $(x^*,y^*)$ if $\mathcal{R}_0>1$, or `None`/`NA` otherwise. Test both against the notebook's
    two example parameter sets ($m=2$ and $m=6$).
12. Write code that verifies the Metzler property symbolically (not just numerically) — i.e. use
    `sympy` (or R's symbolic tools) to show that both off-diagonal Jacobian entries,
    $amb_1(1-x)$ and $ab_2(1-y)$, are manifestly non-negative for all $(x,y)\in\Gamma$ by
    inspecting their algebraic form directly (hint: what do you know about the signs of $a,m,b_1,
    b_2$, and about $1-x$ and $1-y$ within $\Gamma$?).

## Visualization Exercise (1)

13. Create a plot of $\mathcal{R}_0$ as a function of $a$ (holding $m,b_1,b_2,\gamma_1,\gamma_2$
    fixed at Q6's values), for $a$ ranging from 0 to 0.5. Mark the point where $\mathcal{R}_0=1$.
    Describe the shape of the curve (is it linear? convex? concave?) and connect this shape to why
    $a$ has an outsized effect on $\mathcal{R}_0$ compared to a linear parameter like $m$.

## Challenge Problem (1)

14. The book states that reducing $a$ (via bednets, say) reduces $\mathcal{R}_0$ "quadratically
    faster" than reducing $m$ (via larval control), because $a$ appears squared in (2.52) while
    $m$ appears only linearly. Suppose a public health budget can either (i) cut $a$ by 20% via
    bednets, or (ii) cut $m$ by 20% via larval control — assume both interventions cost the same.
    Using formula (2.52), compute the percentage reduction in $\mathcal{R}_0$ each option achieves
    (starting from Q6's parameter values), and determine which intervention is more effective per
    dollar, under this simplified assumption. Then discuss one real-world reason this
    "more effective on paper" conclusion might not directly translate into "better real-world
    policy" (hint: think about what other factors — cost scaling, feasibility, other benefits —
    aren't captured by comparing $\mathcal{R}_0$ reduction alone).

*Full worked solutions: `../solutions/section_2_5_solutions.md`.*
