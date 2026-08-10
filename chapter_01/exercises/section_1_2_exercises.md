# Section 1.2 — Practice Problems

## Conceptual (5)

1. Explain why a transfer diagram is drawn *before* any equations are written, rather than after.
2. In your own words, explain the difference between a bookkeeping equation like
   $\Delta S(t) = \text{in} - \text{out}$ and a differential equation like $S'(t) = \dots$. What
   operation turns one into the other?
3. The text calls "removal" a general term for death *or* out-migration. Why might a modeler want
   to lump these two very different biological events into one term?
4. Suppose a modeler adds a fourth compartment, $V$ (vaccinated), to the S-I-R structure. Without
   writing equations, describe in words what new arrows the transfer diagram would need.
5. Why does the text say that "how you write the rate terms" (not the limiting process itself) is
   where biological hypotheses enter the model?

## Computational (5)

6. Using the symbolic derivative check from the notebook, verify by hand (using ordinary calculus
   rules) that $\frac{d}{dt}\left[1000e^{-0.05t} + 50\cos t\right] = -50e^{-0.05t} - 50\sin t$.
   Which two derivative rules did you need?
7. For the same $S(t) = 1000e^{-0.05t}+50\cos t$, compute the difference quotient
   $\frac{S(5.01)-S(5)}{0.01}$ by hand (a calculator is fine) and compare it to the true derivative
   at $t=5$. How many digits of agreement do you get?
8. If a compartment has in-flow rate $a(t)=3$ (constant) and out-flow rate $b(t)=0.1\,X(t)$, write
   the differential equation for $X(t)$, then find its equilibrium value (where $X'(t)=0$).
9. Using the general balance equation template from Block 2, write out (in words, not necessarily
   symbols) what the balance equation for compartment $I$ would look like if there were also an
   "external immigration of infectious individuals" arrow entering $I$ directly.
10. If $\Delta t = 0.5$ gives a difference quotient of $12.3$ and $\Delta t = 0.05$ gives $11.98$,
    and the pattern continues shrinking the error roughly by a factor of 10 each time $\Delta t$
    shrinks by a factor of 10, estimate the true derivative to 3 significant figures.

## Coding Exercises (2)

11. Extend the Python (or R) convergence-check code to test a *different* function, e.g.
    $S(t) = 500 t e^{-0.1t}$, and confirm the difference quotient still converges to the correct
    symbolic derivative as $\Delta t \to 0$.
12. Write a function `finite_difference_derivative(f, t0, dt)` that returns the difference
    quotient $\frac{f(t_0+dt)-f(t_0)}{dt}$ for an arbitrary function `f`. Test it against a known
    derivative (e.g. $f(t)=t^2 \Rightarrow f'(t)=2t$) at several points.

## Visualization Exercise (1)

13. Recreate the transfer-diagram figure but for a model with *only* S and I (no R compartment,
    i.e. infected individuals never recover, they just stay infectious forever). What changes
    about the diagram compared to Figure 1.1?

## Challenge Problem (1)

14. The text derives the *limit* of a difference quotient using ordinary single-variable calculus.
    Suppose instead the "arrival rate of new susceptibles" itself depended explicitly on time in a
    discontinuous way (e.g. a step function representing a one-time mass vaccination event).
    Discuss, conceptually, what would go wrong with the Step 3 limiting argument at the moment of
    discontinuity, and what a modeler might do about it (e.g. splitting the time domain into
    intervals). You do not need to solve any equations — a clear qualitative discussion is enough.

*Full worked solutions: `../solutions/section_1_2_solutions.md`.*
