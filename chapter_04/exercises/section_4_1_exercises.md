# Section 4.1 — Practice Problems

## Conceptual (5)

1. Explain why an overdetermined system ($n$ data points, fewer than $n$ unknown coefficients)
   generally has no EXACT solution, using a geometric argument (think about dimensions).
2. Why is minimizing $\|b-Ax\|$ equivalent to minimizing $\|b-Ax\|^2$? Why might we prefer to work
   with the squared version?
3. Explain why fitting $y=a_2x^2+a_1x+a_0$ is called a "LINEAR" least-squares problem even though
   the resulting curve is a (nonlinear) parabola.
4. In Example 3, why couldn't we directly apply ordinary linear least squares to
   $x=a_0e^{a_1t}$ without first taking a logarithm?
5. Theorem 4.1.2 says the least-squares solution is unique exactly when $A$'s columns are linearly
   independent. Give an example (in words) of a curve-fitting setup where this would FAIL (i.e.
   columns of $A$ are linearly dependent).

## Computational (5)

6. Find the least-squares line $y=a_0+a_1x$ for the data points $(1,2), (2,3), (3,5)$.
7. Write out the matrix $A$ and vector $b$ (but don't solve) for fitting a cubic
   $y=a_3x^3+a_2x^2+a_1x+a_0$ to the data points $(0,1),(1,2),(2,5),(3,10),(4,17)$.
8. For the normal equations $A^TA\hat x=A^Tb$ with
   $A=\begin{pmatrix}1&1\\1&2\\1&3\end{pmatrix}$, $b=\begin{pmatrix}2\\3\\5\end{pmatrix}$, compute
   $A^TA$ and $A^Tb$ explicitly (by hand or by showing your matrix multiplication steps).
9. Using Q8's normal equations, solve for $\hat x=(\hat a_0,\hat a_1)$.
10. For $y=a_0\ln x$ (a single-parameter model, no additive constant), set up the appropriate
    (1-column) design matrix $A$ for data points $(x_1,y_1),\ldots,(x_n,y_n)$, and write the
    resulting normal equation. What is $\hat a_0$ in closed form?

## Coding Exercises (2)

11. Write a function `least_squares_fit(x_data, y_data, basis_funcs)` that takes data and a LIST
    of basis functions (e.g. `[lambda x: 1, lambda x: x, lambda x: x**2]` for a quadratic fit),
    constructs the design matrix $A$, and returns the least-squares coefficients via the normal
    equations. Test it by reproducing Example 2's quadratic fit.
12. Write code that computes the TOTAL SQUARED ERROR $d(\hat a_0,\hat a_1)$ (equation 4.1) for
    Example 1's fitted line, and confirm it is indeed smaller than the total squared error for at
    least 3 other nearby (but not optimal) choices of $(a_0,a_1)$ that you pick yourself — directly
    demonstrating that the least-squares solution really does minimize the error, not just solve
    some abstract equation.

## Visualization Exercise (1)

13. For Example 1's data, create a plot of the total squared error $d(a_0,a_1)$ as a function of
    $a_1$ alone (holding $a_0$ fixed at its optimal value $2/7$), for $a_1$ ranging over
    $[0,0.7]$. Confirm the minimum occurs exactly at $a_1=5/14$.

## Challenge Problem (1)

14. Suppose you have reason to believe $y$ depends on $x$ through the model $y=a_0+a_1x+a_1^2x^2$
    (note: the SAME parameter $a_1$ appears in both the linear and quadratic terms — a genuinely
    nonlinear-in-parameters model, unlike the quadratic fit in Example 2 where $a_1$ and $a_2$ are
    independent). Explain why the standard linear least-squares machinery of this section CANNOT
    be directly applied to fit $a_0,a_1$ for this model (hint: try setting up the design matrix —
    what goes wrong?). What kind of method (relevant to the next section) would be needed instead?

*Full worked solutions: `../solutions/section_4_1_solutions.md`.*
