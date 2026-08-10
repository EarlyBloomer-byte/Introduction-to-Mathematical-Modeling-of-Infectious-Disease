# Section 4.1 — Worked Solutions

## Conceptual

**1.** With $n$ data points and $m+1<n$ unknown coefficients, requiring the curve to pass through
every point EXACTLY gives $n$ equations in only $m+1$ unknowns — an overdetermined linear system.
Geometrically, the achievable curves form an $(m+1)$-dimensional subspace $\text{col}(A)$ sitting
inside the full $n$-dimensional space of "all possible data vectors" — since $m+1<n$, this
subspace is a strictly lower-dimensional slice of the full space, and the actual data vector $b$
will almost never happen to land exactly inside that lower-dimensional slice (much like a random
point in 3D space almost never lands exactly on a specific 2D plane).

**2.** For non-negative quantities, $u<v \iff u^2<v^2$ (squaring is a strictly increasing function
on non-negative numbers) — so the SAME $x$ minimizes both $\|b-Ax\|$ and $\|b-Ax\|^2$. We prefer
the squared version because $\|b-Ax\|^2=\sum_i[\ldots]^2$ is a smooth polynomial (differentiable
everywhere), while $\|b-Ax\|$ itself involves a square root that's not differentiable at zero —
working with the squared version keeps the minimization problem purely algebraic/calculus-friendly
throughout.

**3.** "Linear" refers to the fact that the unknowns $a_0,a_1,a_2$ each appear to the first power,
multiplied by KNOWN functions of $x$ ($1,x,x^2$) and added together — exactly the structure
$y=a_0f_0(x)+a_1f_1(x)+a_2f_2(x)$ from Problem (3) in Block 1. The resulting *curve*
$y=a_2x^2+a_1x+a_0$ is indeed nonlinear as a function of $x$ (it's a parabola, not a straight
line), but that's a completely separate question from whether the fitting PROBLEM (solving for
the coefficients) is linear — and it is, since the normal equations remain a straightforward
linear system in $(a_0,a_1,a_2)$.

**4.** In $x=a_0e^{a_1t}$, the parameter $a_1$ appears INSIDE an exponential — it does not enter
as a simple linear combination of known functions of $t$ multiplied by unknown coefficients. There
is no way to write this model in the form $x=c_0f_0(t)+c_1f_1(t)$ for known basis functions
$f_0,f_1$ and unknowns $c_0,c_1$ (since $a_1$ itself controls the *shape* of the exponential, not
just its scale) — so the direct linear least-squares machinery doesn't apply until the logarithm
transformation converts it into a genuinely linear form ($\ln x=\ln a_0+a_1t$).

**5.** If two of the "basis functions" you chose happened to be proportional to each other — e.g.
mistakenly including both $f_1(x)=x$ and $f_2(x)=2x$ as separate basis functions in the same
fit — the corresponding two columns of $A$ would be scalar multiples of each other (linearly
dependent), and infinitely many coefficient combinations $(a_1,a_2)$ would give the exact same
fitted curve (e.g. $(a_1,a_2)=(1,0)$ and $(a_1,a_2)=(0,0.5)$ produce identical predictions) — the
least-squares solution would not be unique in this case.

## Computational

**6.** $A=\begin{pmatrix}1&1\\1&2\\1&3\end{pmatrix}$, $b=(2,3,5)^T$. Solving
$A^TA\hat x=A^Tb$ gives $\hat x=(\hat a_0,\hat a_1)=(\mathbf{1/3},\ \mathbf{3/2})$, i.e.
$y=\frac13+\frac32x$.

**7.** $A=\begin{pmatrix}1&0&0&0\\1&1&1&1\\1&2&4&8\\1&3&9&27\\1&4&16&64\end{pmatrix}$,
$b=(1,2,5,10,17)^T$ (columns of $A$ correspond to $1,x,x^2,x^3$ evaluated at each $x$-value).

**8.** $A^TA=\begin{pmatrix}1&1&1\\1&2&3\end{pmatrix}\begin{pmatrix}1&1\\1&2\\1&3\end{pmatrix}
=\begin{pmatrix}3&6\\6&14\end{pmatrix}$. $A^Tb=\begin{pmatrix}1&1&1\\1&2&3\end{pmatrix}
\begin{pmatrix}2\\3\\5\end{pmatrix}=\begin{pmatrix}10\\23\end{pmatrix}$.

**9.** Solving $\begin{pmatrix}3&6\\6&14\end{pmatrix}\hat x=\begin{pmatrix}10\\23\end{pmatrix}$
gives $\hat x=(\hat a_0,\hat a_1)=(\mathbf{1/3},\ \mathbf{3/2})$ — matching Q6 exactly (as it
should, since this is the same data and design matrix as Q6, just presented as a direct
normal-equations exercise).

**10.** With a single basis function $f_0(x)=\ln x$ and no additive constant, $A$ is the single
column $(\ln x_1,\ldots,\ln x_n)^T$. The normal equation $A^TA\hat a_0=A^Tb$ becomes the scalar
equation $\left(\sum_i(\ln x_i)^2\right)\hat a_0=\sum_i(\ln x_i)y_i$, giving the closed form
$$\hat a_0=\frac{\sum_i(\ln x_i)y_i}{\sum_i(\ln x_i)^2}.$$

## Coding Exercises

**11.** Python:
```python
import numpy as np

def least_squares_fit(x_data, y_data, basis_funcs):
    x_data = np.asarray(x_data, dtype=float)
    y_data = np.asarray(y_data, dtype=float)
    A = np.column_stack([f(x_data) for f in basis_funcs])
    return np.linalg.solve(A.T @ A, A.T @ y_data)

basis = [lambda x: np.ones_like(x), lambda x: x, lambda x: x**2]
coeffs = least_squares_fit([2,-1,6,4], [1,5,2,-1], basis)
# -> [2.9719, -1.9089, 0.2826] -- exactly reproduces Example 2
```

**12.** Computing the total squared error (4.1) at the optimal point and several nearby points:

| $(a_0,a_1)$ | total squared error |
|---|---|
| **$(2/7,\ 5/14)$ — optimal** | **0.07143** |
| $(0.2, 0.35)$ | 0.1350 |
| $(0.3, 0.36)$ | 0.0752 |
| $(0.25, 0.34)$ | 0.1452 |
| $(0.28, 0.38)$ | 0.1400 |

The optimal point's error (0.07143) is strictly smaller than every nearby tested point, directly
confirming numerically that the least-squares solution genuinely minimizes the total squared
error — even the closest competitor tested, $(0.3, 0.36)$, has noticeably higher error.

## Visualization Exercise

**13.** Plotting $d(2/7, a_1)$ for $a_1\in[0,0.7]$ (holding $a_0=2/7$ fixed at its optimal value)
produces a smooth upward-opening parabola in $a_1$ (since $d$ is a quadratic function of the
coefficients), with its minimum occurring exactly at $a_1=5/14\approx0.357$ — visually confirming
that the pair $(2/7, 5/14)$ found by solving the normal equations really is a joint minimum of the
error surface, not just a solution to an arbitrary linear system.

## Challenge Problem

**14.** Trying to set up a design matrix for $y=a_0+a_1x+a_1^2x^2$: the model is
NOT of the form $y=c_0f_0(x)+c_1f_1(x)$ for independent unknowns $c_0,c_1$ multiplying KNOWN
functions of $x$ — because the *same* parameter $a_1$ appears in two different places (once to the
first power in the linear term, once squared in the quadratic term), the model's dependence on
$a_1$ is itself nonlinear (doubling $a_1$ does not double the quadratic term's contribution — it
quadruples it). There is no way to build a design matrix whose columns are fixed, known functions
of $x$ alone that, multiplied by independent linear coefficients, reproduce this model — the
coefficient of $x^2$ is rigidly *tied* to the coefficient of $x$ through the relationship
"(coefficient of $x$)$^2$", which no linear combination can express. This requires genuinely
**nonlinear least squares** — iterative methods that directly minimize the sum of squared errors
as a nonlinear function of $(a_0,a_1)$, without the shortcut of solving a single linear system —
exactly the subject of Section 4.2.
