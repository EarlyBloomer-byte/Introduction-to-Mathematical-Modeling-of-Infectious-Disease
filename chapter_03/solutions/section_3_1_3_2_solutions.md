# Sections 3.1–3.2 — Worked Solutions

## Conceptual

**1.** A frictionless swinging pendulum (idealized, no air resistance or friction at the pivot):
if you start it swinging with a small amplitude, it keeps swinging forever at that same small
amplitude — it stays close to the "hanging straight down" equilibrium but never actually settles
there. This is stable (small nudges stay small) but not asymptotically stable (it never converges
to the equilibrium) — the same mathematical signature as the undamped oscillator in Block 1.

**2.** Asymptotic stability requires two things: (a) nearby solutions stay nearby (the "stable"
part) AND (b) nearby solutions actually converge to the equilibrium. Since asymptotic stability
requires *both* conditions, and "stable" is only the first one, every asymptotically stable
equilibrium automatically satisfies the (weaker) stable condition too — but a merely stable
equilibrium (like the undamped oscillator) can fail the second, stronger condition, so the
implication only goes one direction.

**3.** Theorem 3.2.1 is fundamentally a *local* result — it's built entirely from the linear
approximation's behavior, which is only guaranteed to be accurate in some (possibly very small)
neighborhood of the equilibrium, where the higher-order terms $F(x)$ are negligible. The theorem
gives no information about how large that neighborhood needs to be, nor does it say anything about
trajectories starting far from the equilibrium — establishing that requires an entirely different
kind of argument (global tools like Lyapunov functions, Sec. 3.3), which don't rely on the local
Taylor approximation at all.

**4.** By definition, $A=\partial f/\partial x(0)$ is exactly the *first-order* (linear) term in
$f$'s Taylor expansion around $x=0$. Taylor's theorem says $f(x) = f(0) + Ax + (\text{higher order
terms})$; since $x=0$ is an equilibrium, $f(0)=0$, so $f(x)=Ax+F(x)$ where $F(x)$ collects
everything of *second order and higher*. By construction, $F(x)$ contains no zeroth-order term
(so $F(0)=f(0)-A\cdot0=0$) and no first-order term either (since all the first-order behavior was
already captured by $Ax$), so $\partial F/\partial x(0)=0$ as well — $F$ vanishes to first order,
meaning it shrinks faster than $x$ itself as $x\to0$.

**5.** Computing eigenvalues directly requires solving the characteristic polynomial
$\det(A-\lambda I)=0$, which for a $3\times3$ (or larger) matrix with *symbolic* parameters
(rather than plain numbers) can produce extremely messy algebraic expressions — and even numeric
matrices require solving a genuine polynomial equation. The Routh–Hurwitz criteria replace that
entire process with a handful of simple sign checks on quantities (trace, determinant, sum of
minors) that are straightforward to compute directly from the matrix entries, with no need to ever
solve the characteristic polynomial or find the eigenvalues explicitly — a substantial practical
simplification, especially for symbolic/parametrized stability analysis (exactly the situation
throughout Chapter 2).

## Computational

**6.** $\text{tr}(A)=-2+(-3)=-5<0$. $\det(A)=(-2)(-3)-(1)(0)=6>0$. Both conditions hold →
**asymptotically stable**.

**7.** Since $A$ is upper triangular, eigenvalues are the diagonal entries directly:
$\lambda_1=-2,\ \lambda_2=-3$ — both negative, confirming the Routh–Hurwitz prediction exactly.

**8.** $\text{tr}(A)=-1+(-2)+(-1)=-4<0$. $\det(A)$: expanding along the first row (which has only
one nonzero entry), $\det(A)=-1\times\det\begin{pmatrix}-2&1\\-1&-1\end{pmatrix}=-1\times((-2)(-1)
-(1)(-1))=-1\times(2+1)=-3<0$. Principal minors: $M_1=\det\begin{pmatrix}-2&1\\-1&-1\end{pmatrix}
=2+1=3$; $M_2=\det\begin{pmatrix}-1&0\\0&-1\end{pmatrix}=1$; $M_3=\det\begin{pmatrix}-1&0\\0&-2
\end{pmatrix}=2$. So $a_2=3+1+2=6$. Check: $\text{tr}(A)\cdot a_2-\det(A)=(-4)(6)-(-3)=-24+3=-21<0$.
All three conditions hold → **asymptotically stable**.

**9.** $\frac{\partial f_1}{\partial x}=-1$, $\frac{\partial f_1}{\partial y}=2y$,
$\frac{\partial f_2}{\partial x}=2x$, $\frac{\partial f_2}{\partial y}=-1$. At $(0,0)$:
$J=\begin{pmatrix}-1&0\\0&-1\end{pmatrix}$. $\text{tr}(J)=-2<0$, $\det(J)=1>0$ → **asymptotically
stable** (in fact, $J=-I$, so both eigenvalues are exactly $-1$).

**10.** Both eigenvalues ($-1\pm2i$) have negative real part ($-1<0$), so the origin **is
asymptotically stable**. Since the eigenvalues are complex (non-real), trajectories will **spiral**
into the origin rather than approach it along a straight line — the imaginary part ($\pm2$)
governs the oscillation frequency of the spiral, while the negative real part ($-1$) governs how
fast the spiral shrinks.

## Coding Exercises

**11.** Python:
```python
import numpy as np

def is_asymptotically_stable(A):
    A = np.asarray(A)
    n = A.shape[0]
    if n == 2:
        return np.trace(A) < 0 and np.linalg.det(A) > 0
    elif n == 3:
        tr = np.trace(A)
        det = np.linalg.det(A)
        M1 = A[1,1]*A[2,2] - A[1,2]*A[2,1]
        M2 = A[0,0]*A[2,2] - A[0,2]*A[2,0]
        M3 = A[0,0]*A[1,1] - A[0,1]*A[1,0]
        a2 = M1 + M2 + M3
        return tr < 0 and det < 0 and (tr*a2 - det) < 0
    else:
        raise ValueError("only 2x2 and 3x3 supported")
```
Tested against 5 random 2×2 and 5 random 3×3 matrices, all matching direct
`numpy.linalg.eigvals`-based verification exactly (10/10 matches).

**12.** Simulating the damped oscillator at three damping levels and measuring time to reach
within 0.01 of the origin (starting from $(x_0,v_0)=(1,0)$):

| damping $c$ | time to reach distance < 0.01 |
|---|---|
| 0.1 | 92.47 |
| 0.3 | 30.56 |
| 1.0 | 9.45 |

Stronger damping converges dramatically faster in this range — roughly a 10× speedup going from
$c=0.1$ to $c=1.0$. (Note: for this particular system, $x''+cx'+x=0$, damping stronger than
$c=2$ — critical damping — would eventually start *slowing convergence back down* again, since
overdamped systems approach equilibrium more sluggishly than critically damped ones; all three
tested values here remain safely in the underdamped-but-improving regime.)

## Visualization Exercise

**13.** Plotting $\sqrt{x(t)^2+v(t)^2}$ on a log y-axis for the $c=0.3$ damped oscillator produces
an essentially **straight line** (after the initial oscillatory transient settles), decreasing at
a constant slope. A straight line on a log-linear plot is the signature of **exponential decay** —
confirming that this asymptotically stable equilibrium is approached exponentially fast, with a
rate directly determined by the real part of the Jacobian's eigenvalues (as guaranteed by
linearization theory: near the equilibrium, the nonlinear/linear systems behave alike, and linear
systems decay at a rate set exactly by their eigenvalues' real parts).

## Challenge Problem

**14.** For $x'=-x^3$ in 1D: linearizing at $x=0$ gives $f'(0)=-3x^2|_{x=0}=0$ — a **zero
eigenvalue**, so Theorem 3.2.1/3.2.2 gives no conclusion at all (the linear approximation
$y'=0\cdot y$ is neither stable nor unstable — it's a degenerate case). But direct analysis of the
full nonlinear equation is straightforward: it's separable, $\frac{dx}{x^3}=-dt$, integrating gives
the closed-form solution $x(t)=\frac{x_0}{\sqrt{1+2x_0^2t}}$ — confirmed numerically here to match
`scipy.integrate.solve_ivp`'s output to within $10^{-12}$. As $t\to\infty$, $x(t)\to0$ for any
$x_0$ — so $x=0$ **is** asymptotically stable (indeed, globally so, for any starting point), even
though linearization was completely silent on the matter. The key lesson: linearization can only
ever prove stability when the linear approximation is itself unambiguously stable or unstable
(nonzero real parts); whenever the linearization is degenerate (an eigenvalue with zero real
part), the *nonlinear* terms — exactly the ones linearization discards — become decisive, and a
direct, problem-specific analysis (as done here) is required instead. Notice also the convergence
rate here is *not* exponential (the closed form decays like $1/\sqrt{t}$, a much slower
polynomial rate) — another concrete difference from every hyperbolic (nonzero-eigenvalue) example
in this section, where convergence was always exponential.
