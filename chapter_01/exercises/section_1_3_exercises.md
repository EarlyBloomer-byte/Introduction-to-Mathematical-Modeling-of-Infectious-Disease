# Section 1.3 — Practice Problems

## Conceptual (5)

1. Which of the seven hypotheses would you have to drop to model a sexually transmitted infection
   where transmission requires a specific type of contact rather than any random contact? Which
   hypothesis is most directly about *that*?
2. Explain in your own words why $N(t)=S(t)+I(t)+R(t)$ being constant is called a *conservation
   law* rather than just an *assumption* — where exactly does the proof come from?
3. Why can $S(t)$ never increase in this model, but $I(t)$ and $R(t)$ are not similarly
   constrained?
4. In the threshold argument, why is *continuity* of $S(t)$ specifically invoked in Case (ii)
   (Step 5), rather than just checking the sign at $t=0$ and stopping there?
5. Explain the difference between $R(0)$ (as used in this section) and $\mathcal{R}_0$ (the basic
   reproduction number). Why do you think the book bothers to flag this distinction explicitly?

## Computational (5)

6. If $\lambda = 0.0004$ and $\gamma = 0.25$, compute the threshold value $\gamma/\lambda$.
7. Using the threshold from Q6, will an epidemic occur if $S_0 = 500$? If $S_0=700$? Justify using
   the sign argument from Block 3, not simulation.
8. Suppose $\lambda$ doubles (transmission becomes twice as easy) while $\gamma$ stays the same.
   What happens to the threshold value $\gamma/\lambda$? Does an epidemic become more or less
   likely for a fixed $S_0$?
9. The average infectious period is $1/\gamma$. If $\gamma = 0.2$ (per day), what is the average
   number of days someone remains infectious?
10. Verify by direct substitution that $I(t)=0$ for all $t$ (i.e. $I_0=0$) is an equilibrium of
    the system (all three derivatives are zero), regardless of $S_0$. What does this mean
    biologically?

## Coding Exercises (2)

11. Modify the `solve_ivp` (or `deSolve`) code to compute the **final epidemic size** — the total
    number of people who were ever infected, i.e. $R(\infty) \approx R(t_{\text{end}})$ for a
    large enough $t_{\text{end}}$ — for the "above threshold" scenario in the notebook. What
    fraction of $S_0$ ended up infected?
12. Write a function `simulate_and_classify(S0, I0, lam, gamma, t_max=200)` that integrates the
    Kermack–McKendrick ODEs and returns `"epidemic"` if $I(t)$ ever exceeds $I_0$ during the
    simulation, else `"no epidemic"`. Test it against at least 3 combinations of parameters that
    should give different classifications, and confirm your function's output matches the
    analytical threshold prediction $S_0 \gtrless \gamma/\lambda$ in each case.

## Visualization Exercise (1)

13. Produce a plot of *peak $I(t)$* (y-axis) versus $S_0$ (x-axis), holding $\lambda, \gamma, I_0$
    fixed, sweeping $S_0$ across a range that spans both sides of the threshold. Describe the
    shape of the curve near the threshold.

## Challenge Problem (1)

14. The threshold argument in Block 3 only used the *sign* of $I'(t)$ near $t=0$ to conclude
    whether an epidemic "occurs." It did not compute how *large* the epidemic gets. Investigate
    numerically: for a fixed $\gamma/\lambda$ threshold, does the *peak* size of $I(t)$ depend only
    on how far $S_0$ is above the threshold (i.e. $S_0 - \gamma/\lambda$), or does it depend on
    $S_0$ and $\gamma/\lambda$ in some other combination? Design and run a small numerical
    experiment (varying $S_0$ while holding $\gamma/\lambda$ fixed at two different values by also
    scaling $\lambda,\gamma$ together) to investigate, and report what you find.

*Full worked solutions: `../solutions/section_1_3_solutions.md`.*
