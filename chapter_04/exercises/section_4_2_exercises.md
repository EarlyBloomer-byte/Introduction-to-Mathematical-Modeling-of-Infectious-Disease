# Section 4.2 — Practice Problems

## Conceptual (5)

1. Explain why equation (4.8) is "generally still nonlinear in $\theta$" even though it came from
   setting derivatives equal to zero (a step that often produces linear equations in simpler
   calculus problems).
2. In your own words, describe what "linearizing $f$" means at each Gauss–Newton step, and why
   this is different from linearizing $SSE$ itself.
3. Why does Gauss–Newton need an INITIAL GUESS $\theta^{(0)}$, while Sec. 4.1's linear least
   squares does not need one?
4. Explain why the log-linearized solution and the direct Gauss–Newton solution for Example 4
   don't need to agree exactly, even though both are "least-squares" fits of the same underlying
   exponential model to the same data.
5. What practical risk does Gauss–Newton have that ordinary linear least squares does not?
   (Think about what could go wrong during the iteration process.)

## Computational (5)

6. For $f(x,\theta)=\theta_1\sin(x)+\theta_2\cos(x)$, is this model linear or nonlinear in
   $\theta$? Justify your answer by checking whether $\partial f/\partial\theta_j$ depends on
   $\theta$.
7. For $f(x,\theta)=\sin(\theta x)$, compute $\partial f/\partial\theta$. Does it depend on
   $\theta$? Is this a linear or nonlinear least-squares problem?
8. Using the book's Jacobian formula for $f(t,a)=a_0e^{a_1t}$, compute $J$ (both columns) at
   $t=2$, $a_0=2$, $a_1=0.5$.
9. If $\Delta y=(0.5,-0.3,0.2)^T$ and $J=\begin{pmatrix}1&2\\1&3\\1&4\end{pmatrix}$, compute
   $J^TJ$ and $J^T\Delta y$ (the ingredients of one Gauss–Newton normal system).
10. Using Q9's results, solve for $\Delta\theta$.

## Coding Exercises (2)

11. Modify the from-scratch `gauss_newton` function to also record the SSE at each iteration
    (not just $\theta$), and plot SSE vs. iteration number for Example 4. Confirm SSE decreases
    monotonically (or very nearly so) toward its minimum.
12. Write a function that runs Gauss–Newton from SEVERAL different initial guesses
    $\theta^{(0)}$ (e.g. $(0.5,0.5)$, $(5,0.1)$, $(1,2)$) for Example 4's data, and reports whether
    all of them converge to the same final answer. Discuss what you observe — does the starting
    point matter for this particular problem?

## Visualization Exercise (1)

13. Create a contour plot of $SSE(a_0,a_1)$ for Example 4's data over a grid of $(a_0,a_1)$ values
    (e.g. $a_0\in[1,4]$, $a_1\in[0.3,0.9]$), and overlay the Gauss–Newton iteration path from the
    notebook on top of it. Does the path move in the direction of decreasing SSE at each step?

## Challenge Problem (1)

14. The book's Gauss–Newton formula (4.11) does not include any "step size" control — it always
    takes the FULL Newton step $\theta^{(k+1)}=\theta^{(k)}+\Delta\theta$. A common practical
    improvement is "damped" Gauss–Newton: $\theta^{(k+1)}=\theta^{(k)}+\lambda\Delta\theta$ for
    some $0<\lambda\le1$. Modify your Gauss–Newton implementation to accept a damping factor
    $\lambda$, and test $\lambda=1.0$ (undamped), $\lambda=0.5$, and $\lambda=0.2$ on Example 4's
    data. Does damping change the FINAL answer? Does it change how many iterations are needed?
    Discuss when you might want to use damping even though it slows convergence for this
    particular (well-behaved) example.

*Full worked solutions: `../solutions/section_4_2_solutions.md`.*
