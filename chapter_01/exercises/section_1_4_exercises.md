# Section 1.4 — Practice Problems

## Conceptual (5)

1. Explain why "proportional exit rate" and "exponentially distributed residence time" are really
   the *same* assumption stated two different ways. Which framing is more intuitive to you, and why?
2. What is the *memoryless property*, in plain words, and why does it make ODE modeling
   mathematically convenient (as opposed to needing to track "how long has this individual been
   infectious")?
3. Explain, using the Michaelis–Menten/Monod analogy, why saturating incidence eventually becomes
   *independent* of $I$ for very large $I$, while mass-action incidence never does.
4. Why does the text argue that standard/proportionate incidence is more appropriate for a rural
   population, while bilinear (mass-action) incidence suits a dense urban population?
5. Explain the difference between incubation period, latent period, and infectious period. Give an
   example of a disease (real or hypothetical) where the latent period is *shorter* than the
   incubation period, and explain the public-health significance of that ordering.

## Computational (5)

6. If the mean infectious period is 5 days, what is $\gamma$?
7. For the saturating incidence form $\beta IS/(K+I)$ with $\beta=0.001$, $S=600$, $K=50$: compute
   the incidence rate at $I=10$, $I=50$, and $I=500$. At which value of $I$ does the incidence rate
   equal exactly half of its saturating maximum $\beta S$?
8. For the demographic model with $b=0.03$, $d=0.02$ (equal across compartments), and $N_0=1000$,
   compute $N(10)$ using the closed-form solution $N(t)=N_0e^{(b-d)t}$.
9. For the logistic growth model with $b=0.05$, $d=0.01$, $K=2000$, compute the carrying capacity
   $(b-d)K$.
10. If the mean latent period is 4 days, what is $\kappa$? If the mean infectious period is 6
    days, what is $\gamma$? What is the total expected time from infection to recovery
    (assuming these two stages happen back-to-back)?

## Coding Exercises (2)

11. Modify the ODE-vs-DDE comparison code to try three different values of $\omega$ (fixed
    residence time) while keeping the mean the same in the ODE case ($1/\gamma$ fixed). Does the
    DDE's peak $I(t)$ change as $\omega$ changes, holding the mean fixed? (It shouldn't, since
    $\omega$ *is* the mean in the fixed-duration case — but confirm this numerically to be sure
    you haven't mixed up which parameter is which.)
12. Write a function `saturating_incidence(I, S, beta, K)` and confirm numerically that
    `saturating_incidence(I, S, beta, K) / (beta * S) -> 1` as `I` becomes very large (e.g. test
    at I = 10, 100, 1000, 100000 and print the ratio each time).

## Visualization Exercise (1)

13. Plot the survival function $G(t)=e^{-\gamma t}$ for three different values of $\gamma$
    (e.g. 0.1, 0.2, 0.5) on the same axes. Which curve represents the *shortest* mean infectious
    period? Label the mean ($1/\gamma$) on each curve.

## Challenge Problem (1)

14. The text shows that a Heaviside/delta residence-time distribution leads to a delay
    differential equation (1.14)-(1.15). Consider an intermediate case: a **gamma distribution**
    with shape parameter $k=2$ and rate $2\gamma$ (chosen so the mean is still $1/\gamma$, matching
    the exponential case, but with less variability). This can be implemented as two sequential
    exponential stages, each with rate $2\gamma$ (a "linear chain trick"), i.e. add an intermediate
    compartment $I_1 \to I_2$ each with exit rate $2\gamma$, and treat $I=I_1+I_2$ as "infectious."
    Implement this as a 4-compartment ODE system ($S, I_1, I_2, R$) and compare its epidemic curve
    to both the plain exponential ($I$ alone, rate $\gamma$) and the fixed-delay ($\omega=1/\gamma$)
    cases from Block 1. Where does the gamma-distributed curve fall relative to the other two, in
    terms of peak height and sharpness?

*Full worked solutions: `../solutions/section_1_4_solutions.md`.*
