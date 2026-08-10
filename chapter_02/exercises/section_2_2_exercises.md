# Section 2.2 — Practice Problems

## Conceptual (5)

1. Explain why the SIS model only needs ONE differential equation to fully describe its dynamics,
   while the SIR model of Sec. 2.1 needs two (even after eliminating $R$ via conservation).
2. In your own words, define "asymptotically stable equilibrium" and "unstable equilibrium" using
   the arrows-on-a-line picture from phase-line analysis.
3. Why can a 1D autonomous ODE's solution never oscillate or overshoot its limit, while a 2D
   system's solution (like SIR's) can trace out a more complex curve?
4. Explain, using the "S(t) stays on one side of $\rho$" argument, exactly why an SIS model cannot
   produce a rise-and-fall epidemic curve — don't just state the conclusion, explain the mechanism.
5. What is a transcritical bifurcation, and what specifically "exchanges" at the bifurcation point
   that would NOT happen in, say, a bifurcation where equilibria simply appear or disappear in
   pairs without changing which one is stable?

## Computational (5)

6. For $\beta=0.0008$, $\gamma=0.25$, $N_0=500$: compute $\rho$ and $\mathcal{R}_0$. Is this
   above or below threshold?
7. Using Q6's parameters, compute the endemic equilibrium $(S^*,I^*)$, if it exists.
8. If $N_0=1000$ and $\rho=1000$ exactly (i.e. $\mathcal{R}_0=1$ exactly), what does Proposition
   2.2.1 predict for $I(t)$ as $t\to\infty$, for any $I_0$ between 0 and $N_0$? (Hint: think about
   what happens to the two equilibria at exactly the bifurcation value.)
9. For $\beta=0.0005$, $\gamma=0.2$, compute $\rho$. Then find the smallest integer $N_0$ for which
   the disease becomes endemic (i.e. $\mathcal{R}_0>1$).
10. Suppose you observe an SIS-type disease reach a stable endemic level of $I^*=150$ in a
    population of $N_0=600$. Using $I^*=N_0-\rho$, back out $\rho$, and then, if you know
    $\gamma=0.2$, back out $\beta$.

## Coding Exercises (2)

11. Write a function `sis_equilibria(beta, gamma, N0)` that returns both equilibria
    $(I_1^*, I_2^*)$ as a tuple, along with a string describing which one is stable ("disease-free"
    or "endemic"). Test it on at least 3 parameter combinations spanning both regimes.
12. Modify the SIR-vs-SIS comparison code to test a THIRD case where $\mathcal{R}_0$ is very close
    to 1 (e.g. $N_0$ just barely above $\rho$) for the SIS model. How does the *time* it takes to
    approach the endemic equilibrium compare to a case where $N_0$ is far above $\rho$? (This
    previews a phenomenon called "critical slowing down" near a bifurcation point.)

## Visualization Exercise (1)

13. Recreate the bifurcation diagram (Block 4) but plot $S^*$ (rather than $I^*$) as a function of
    $N_0$. Describe how this version of the diagram looks different, and explain why $S^*$'s
    stable branch is "flat" for $N_0>\rho$ (Hint: recall $S^*=\rho$ is fixed on the endemic branch).

## Challenge Problem (1)

14. Consider a "spontaneous recovery loss" variant of the SIS model where susceptibles also lose
    "susceptibility" at some very small rate $\mu$ (representing, hypothetically, natural
    death/removal of susceptibles from the model, unrelated to disease) — i.e. add a $-\mu S$ term
    to the $S$-equation and a $+\mu S$... no wait, to keep $N$ conserved you'd need a matching
    $+\mu S$ birth term elsewhere. Instead, explore conceptually: if you wanted to add demography
    (births and deaths) to the SIS model while keeping $N_0$ exactly constant (as the book does in
    Sec. 2.3 for the SIR case), what structural change would you need to make to the $S$-equation,
    and would you expect the phase-line analysis technique from this section to still apply
    directly, or would the system need to become 2-dimensional again? Explain your reasoning.

*Full worked solutions: `../solutions/section_2_2_solutions.md`.*
