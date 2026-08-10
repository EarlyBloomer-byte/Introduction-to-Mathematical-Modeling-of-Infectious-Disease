# Sections 3.1–3.2 — Practice Problems

## Conceptual (5)

1. Give a real-world (non-pendulum) example of a system that is stable but not asymptotically
   stable, and explain why it fits that category.
2. Explain, in your own words, why "asymptotically stable" is a strictly stronger requirement than
   "stable" — i.e. why every asymptotically stable equilibrium is automatically stable, but not
   vice versa.
3. Theorem 3.2.1 only concludes LOCAL asymptotic stability from linearization. Explain why this
   theorem alone cannot tell you anything about the size of the basin of attraction.
4. Explain why $F(x)=f(x)-Ax$ satisfies $F(0)=0$ and $\partial F/\partial x(0)=0$ — connect this
   back to the definition of the Jacobian and Taylor's theorem.
5. Why does the book bother giving the Routh–Hurwitz criteria at all, rather than just always
   computing eigenvalues directly?

## Computational (5)

6. For $A=\begin{pmatrix}-2&1\\0&-3\end{pmatrix}$, compute tr(A) and det(A), and determine
   stability using the 2×2 Routh–Hurwitz condition.
7. For the same $A$ as Q6, compute the eigenvalues directly and confirm they match the
   Routh–Hurwitz prediction.
8. For $A=\begin{pmatrix}-1&0&0\\0&-2&1\\0&-1&-1\end{pmatrix}$, compute tr(A), det(A), and $a_2$
   (sum of 2×2 principal minors), and apply the 3×3 Routh–Hurwitz condition.
9. Compute the Jacobian of $f(x,y)=(-x+y^2,\ -y+x^2)$ at the equilibrium $(0,0)$, and determine
   its stability using Routh–Hurwitz.
10. A linear system has eigenvalues $\lambda_1=-1+2i$ and $\lambda_2=-1-2i$. Is the origin
    asymptotically stable? Would trajectories spiral or move directly toward the origin?

## Coding Exercises (2)

11. Write a function `is_asymptotically_stable(A)` that works for BOTH 2×2 and 3×3 matrices,
    dispatching to the correct Routh–Hurwitz formula based on the matrix's shape. Test it against
    `numpy.linalg.eigvals`-based direct verification on at least 5 matrices of each size.
12. Write code that generates the undamped/damped oscillator comparison (Block 1) but for THREE
    damping levels ($c=0.1, 0.3, 1.0$), and quantify (e.g. via time to reach within 1% of the
    origin) how damping strength affects convergence speed.

## Visualization Exercise (1)

13. For the damped oscillator with $c=0.3$, plot the distance from the origin, $\sqrt{x^2+v^2}$,
    as a function of time on a LOG y-axis. What shape does the curve have, and what does that
    shape tell you about the *rate* of convergence (exponential vs. some other rate)?

## Challenge Problem (1)

14. Theorem 3.2.1 says linear asymptotic stability implies NONLINEAR asymptotic stability nearby.
    It does NOT claim the converse. Construct (or look up) an example of a nonlinear system whose
    linearization at an equilibrium has a zero eigenvalue (making Theorem 3.2.1/3.2.2
    inconclusive), but which is nevertheless asymptotically stable when you analyze the full
    nonlinear system directly. (Hint: consider $x'=-x^3$ in one dimension — what does linearization
    say, and what does direct analysis of the nonlinear equation say?)

*Full worked solutions: `../solutions/section_3_1_3_2_solutions.md`.*
