# Section 4.3 — Practice Problems

## Conceptual (5)

1. Explain the "apples to apples" principle in your own words, and describe a concrete mistake a
   modeler could make by violating it.
2. Why does every SSE evaluation for an ODE-based model require solving the ODE system from
   scratch, rather than reusing some cached computation from a previous parameter guess?
3. This section found that a single optimizer run from a reasonable-looking starting guess
   actually landed in a spurious local minimum. Explain how you can tell, PURELY from the
   optimizer's own output (without knowing the true parameters), that a fit might be suspect.
   (Hint: think about what information you DO have access to in a real fitting problem.)
4. Explain why the basic (demography-free) SIR model's $N(t)\equiv$const property means the
   $N(t)$ data added no fitting information — would this still be true for the Sec. 2.3 model
   (SIR with demography)? Why or why not?
5. Why did switching from RK45 to LSODA fix the optimizer hanging, rather than just making it
   somewhat faster? Connect your answer to what "stiffness" means for an ODE solver.

## Computational (5)

6. For the basic SIR model, verify algebraically (not numerically) that $dN/dt=0$ by adding the
   three equations $S'=-\lambda IS$, $I'=\lambda IS-\gamma I$, $R'=\gamma I$.
7. If a demography-including model instead has $S'=b-\lambda IS-bS$, $I'=\lambda IS-\gamma I-bI$,
   $R'=\gamma I-bR$, compute $dN/dt$ by adding all three equations. Is it still zero regardless of
   $\lambda,\gamma$?
8. Suppose an optimizer reports a "converged" fit with SSE=500, but you separately compute the SSE
   at a set of parameters you have good independent reason to believe are close to correct, and
   get SSE=50. What should you conclude about the "converged" result?
9. If you have $p$ observation times and are fitting $m$ parameters, what is the minimum number of
   observation times needed in principle for the fitting problem to be well-posed (not
   underdetermined)? Does having MORE observation times than this minimum help, and if so, how?
10. For a model where $N(t)$ genuinely varies with $\theta$ (e.g. the Sec. 2.3 demography model),
    would you expect fitting with BOTH $I(t)$ and $N(t)$ to generally outperform fitting with
    $I(t)$ alone? Explain your reasoning using this section's findings as a contrast case.

## Coding Exercises (2)

11. Write a function `multi_start_fit(sse_func, starting_guesses, method="Nelder-Mead")` that
    runs the optimizer from each starting guess in a list, and returns the best (lowest-SSE)
    result along with a flag indicating whether the different starting guesses actually agreed
    (i.e. whether local-minima issues were detected). Test it on this section's SIR fitting
    problem.
12. Modify the data-generation code to use a LARGER noise level (e.g. `width=0.3` instead of
    `0.1`), re-run the multi-start fit, and report how the parameter recovery accuracy changes.
    Does more noise make the local-minimum trap MORE or LESS likely to occur from the same bad
    starting guess?

## Visualization Exercise (1)

13. Create a 2D contour or heatmap of $SSE(\lambda,\gamma)$ over a grid spanning both the true
    parameters AND the spurious local minimum found in this section (e.g. $\lambda\in[0.001,100]$
    on a log scale, $\gamma\in[0.01,1]$), and mark both the true parameters and the spurious
    minimum on the plot. Does the plot make it visually clear why Nelder-Mead could get trapped?

## Challenge Problem (1)

14. Design a numerical experiment to test whether the specific spurious local minimum found in
    this section ($\lambda\approx35317$) is a genuine feature of the SSE landscape for ANY noisy
    dataset generated from this model, or an artifact of this specific random seed's noise
    realization. (Hint: regenerate the synthetic data with several different random seeds, keeping
    everything else the same, and check whether starting from $(8, 0.02)$ reliably lands in a
    similar bad region each time, or only sometimes.) Run your experiment and report what you find.

*Full worked solutions: `../solutions/section_4_3_solutions.md`.*
