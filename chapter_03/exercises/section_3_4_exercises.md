# Section 3.4 — Practice Problems

## Conceptual (5)

1. Explain why a LINEAR pendulum (small-angle approximation, $x''+x=0$) does NOT have the
   phase-drift problem that the nonlinear pendulum has. (Hint: what do you know about the period
   of a linear/simple harmonic oscillator regardless of amplitude?)
2. In your own words, explain the difference between "orbital stability" and "orbital
   asymptotic stability." Which one applies to the pendulum's nested orbits, and which applies to
   the $r'=r(1-r^2)$ limit cycle?
3. Why is it structurally GUARANTEED (true for every smooth system with a periodic orbit, not just
   special examples) that one Floquet multiplier always equals exactly 1?
4. Explain why Floquet theory needs to linearize ALONG the entire periodic orbit $p(t)$, rather
   than just at a single fixed point the way Sec. 3.2's equilibrium linearization does.
5. In the $r'=r(1-r^2)$ example, what would it mean physically/dynamically if the SECOND Floquet
   multiplier had modulus greater than 1 instead of less than 1? Would $r=1$ still be a periodic
   orbit? Would it still be a limit cycle?

## Computational (5)

6. For the $r'=r(1-r^2)$ system, the second Floquet multiplier was found to be approximately
   $e^{-4\pi}$. Compute this value to 3 significant figures.
7. A different radial system has $r'=r(1-r^2/4)$ (i.e. a limit cycle at $r=2$ instead of $r=1$).
   Linearizing $r'=g(r)$ at $r=2$: compute $g'(2)$ (this determines the analogous radial Floquet
   exponent for this system).
8. If a periodic orbit in a 3-dimensional system has Floquet multipliers $1$, $0.3$, and $1.2$,
   is the orbit orbitally asymptotically stable? Explain which multiplier(s) determine your answer.
9. If a periodic orbit's Floquet exponents (eigenvalues of $L$, not multipliers) are $0$ and $-0.5$,
   with period $T=4$, compute the corresponding Floquet multipliers ($e^{\lambda T}$ for each
   exponent $\lambda$).
10. True or false, with justification: "If all Floquet multipliers of a periodic orbit have
    modulus less than 1, the orbit is orbitally asymptotically stable." (Careful — re-read Theorem
    3.4.1 part (3) and Theorem 3.4.2 before answering.)

## Coding Exercises (2)

11. Write a function `compute_floquet_multipliers(jacobian_func, period, orbit_func, n_dim)` that,
    given a function returning the Jacobian at a point on the orbit, the orbit's period, a function
    giving the orbit's position at time $t$, and the system's dimension, integrates the linearized
    system from the identity matrix over one period and returns the eigenvalues of the resulting
    monodromy matrix. Test it on the $r'=r(1-r^2)$ example and confirm it reproduces the notebook's
    result.
12. Modify the pendulum simulation to measure the ACTUAL periods of the two orbits (amplitude 0.5
    and 0.7) numerically (e.g. by detecting when $v$ crosses zero with $x>0$, twice, and measuring
    the time between crossings). Confirm the larger-amplitude orbit has a longer period, and
    relate the period DIFFERENCE to how quickly $|p(t)-q(t)|$ grows in your Block 1 plot.

## Visualization Exercise (1)

13. Create a plot showing the SAME limit cycle example ($r'=r(1-r^2)$) but starting from FOUR
    points all on a small circle of radius 0.1 around a single point near the limit cycle (i.e. a
    tiny cluster of nearby initial conditions). Plot all four trajectories together. Do they
    stay clustered together as they approach the limit cycle, spread apart, or something else?
    Relate your observation to the SIZE of the (non-trivial) Floquet multiplier.

## Challenge Problem (1)

14. The book states a "generalization to higher dimensional systems was developed by
    J.S. Muldowney" for estimating Floquet multipliers without solving the full linearized system
    explicitly (this connects to the "compound matrices" technique, briefly relevant in more
    advanced treatments). Without needing to know the technical details, discuss: why might
    directly computing the monodromy matrix (as done numerically in this notebook) become
    increasingly impractical for very high-dimensional systems (e.g. a 20-compartment epidemic
    model), and what general computational/mathematical strategies (based on what you've learned
    in this book so far — think about how Chapter 2 handled high-dimensional-feeling problems)
    might help make the problem more tractable?

*Full worked solutions: `../solutions/section_3_4_solutions.md`.*
