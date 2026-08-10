# Section 4.2 — Worked Solutions

## Conceptual

**1.** Setting a derivative to zero only produces a LINEAR equation in the unknowns if that
derivative is itself linear in those unknowns. Here, $\partial SSE/\partial\theta_j$ involves
$\partial f/\partial\theta_j(x_i,\theta)$, which can be an arbitrarily complicated function of
$\theta$ whenever $f$ itself depends nonlinearly on $\theta$ (e.g. $f=a_0e^{a_1t}$, where
$\partial f/\partial a_1=a_0te^{a_1t}$ still contains $\theta$ inside an exponential) — so the
"set derivative to zero" step doesn't automatically simplify things the way it would for, say,
a plain quadratic function.

**2.** Linearizing $f$ means replacing $f(x_i,\theta)$ with its first-order Taylor approximation
around the CURRENT estimate $\theta^{(k)}$ — a plane (or hyperplane) that matches $f$'s value and
slope exactly at $\theta^{(k)}$ but is only approximately correct nearby. This is different from
linearizing $SSE$ itself: $SSE$ is already a smooth (indeed, quadratic-in-the-residuals) function,
and directly Taylor-expanding IT would produce a different, less useful approximation. By
linearizing the MODEL $f$ first and only THEN forming the (now linear-in-$\Delta\theta$) normal
equations, Gauss–Newton cleverly reuses all of Sec. 4.1's linear least-squares machinery at each
step, which would not work if $SSE$ were linearized directly instead.

**3.** Gauss–Newton is fundamentally an ITERATIVE, local method — it needs a starting point to
linearize around, and each step only refines that estimate incrementally. Sec. 4.1's linear least
squares, by contrast, is solved by ONE single matrix equation (the normal equations) that has a
closed-form (or at least directly computable) solution with no iteration and no dependence on any
starting guess at all — a fundamentally different (non-iterative, globally-exact-for-the-linear-
model) computational structure.

**4.** The log-linearized approach minimizes $\sum_i[\ln x_i - (\ln a_0+a_1t_i)]^2$ — squared
errors measured in LOG space. Direct Gauss–Newton minimizes $\sum_i[x_i-a_0e^{a_1t_i}]^2$ —
squared errors measured in the ORIGINAL space. These are genuinely different objective functions
(a given deviation counts differently depending on whether you measure it before or after taking a
logarithm), so their minimizers need not coincide — they're solving two different (though closely
related) optimization problems, not the same problem via two different methods.

**5.** Gauss–Newton's iteration can fail to converge, converge to the wrong (local, not global)
minimum, or converge very slowly, depending on the initial guess $\theta^{(0)}$ and how
well-behaved the model is — none of which can happen with Sec. 4.1's linear least squares, whose
normal equations (when uniquely solvable per Theorem 4.1.2) give the exact global minimum in one
step, with no dependence on any starting guess and no risk of the iteration "getting stuck"
somewhere suboptimal.

## Computational

**6.** LINEAR in $\theta$: $\partial f/\partial\theta_1=\sin(x)$ and
$\partial f/\partial\theta_2=\cos(x)$ — both are functions of $x$ alone, with NO dependence on
$\theta_1$ or $\theta_2$. This is exactly the "linear combination of known basis functions"
structure from Sec. 4.1 ($f_0(x)=\sin x$, $f_1(x)=\cos x$), so this is a linear least-squares
problem despite $f$ itself being a nonlinear (trigonometric) function of $x$.

**7.** $\partial f/\partial\theta=x\cos(\theta x)$ — this DOES depend on $\theta$ (through the
$\cos(\theta x)$ factor), so this is a **nonlinear** least-squares problem, requiring Gauss–Newton
or a similar iterative method.

**8.** Column 0 ($\partial f/\partial a_0=e^{a_1t}$): at $t=2,a_1=0.5$, $e^{1}=e\approx2.7183$.
Column 1 ($\partial f/\partial a_1=a_0te^{a_1t}$): at $t=2,a_0=2,a_1=0.5$,
$2\times2\times e^{1}=4e\approx10.8731$.

**9.** $J^TJ=\begin{pmatrix}1&1&1\\2&3&4\end{pmatrix}\begin{pmatrix}1&2\\1&3\\1&4\end{pmatrix}
=\begin{pmatrix}3&9\\9&29\end{pmatrix}$. $J^T\Delta y=\begin{pmatrix}1&1&1\\2&3&4\end{pmatrix}
\begin{pmatrix}0.5\\-0.3\\0.2\end{pmatrix}=\begin{pmatrix}0.4\\0.9\end{pmatrix}$.

**10.** Solving $\begin{pmatrix}3&9\\9&29\end{pmatrix}\Delta\theta=\begin{pmatrix}0.4\\0.9
\end{pmatrix}$ gives $\Delta\theta\approx(\mathbf{0.5833},\ \mathbf{-0.15})$.

## Coding Exercises

**11.** Tracking SSE alongside $\theta$ at each iteration for Example 4 (starting from
$\theta^{(0)}=(1,1)$):

| iteration | SSE |
|---|---|
| 0 (initial) | 8649.46 |
| 1 | 326.64 |
| 2 | 26.21 |
| 3 | 0.563 |
| 4 | 0.2795 |
| 5 | 0.27949 (essentially converged) |
| 6 | 0.27949 |

SSE decreases **monotonically** at every single iteration, dropping by roughly an order of
magnitude (or more) per step initially, then leveling off smoothly as the iterates approach the
minimum — exactly the behavior expected of a well-behaved Gauss–Newton run.

**12.** Testing three very different starting points — $(0.5,0.5)$, $(5,0.1)$, and $(1,2)$ — ALL
converge to the identical final answer $(a_0,a_1)\approx(2.4309,\ 0.6364)$ (matching to at least 5
decimal places). For this particular problem, the starting point does not affect the final answer
— the SSE surface is well-behaved enough (essentially a single, wide basin) that Gauss–Newton
finds the same global minimum regardless of where it starts, at least across this range of
reasonable starting guesses.

## Visualization Exercise

**13.** Plotting SSE contours over the $(a_0,a_1)$ grid shows a single elongated, bowl-shaped
valley with its minimum at $(2.431,0.636)$; overlaying the Gauss–Newton iteration path (from
Q11/the main notebook) shows the path moving from the starting point $(1,1)$ through successively
lower-SSE contour lines at every step, converging into the bottom of the valley by iteration 3–4 —
visually confirming that each Gauss–Newton step genuinely descends the SSE surface, not just
formally "solves an equation" that happens to relate to it.

## Challenge Problem

**14.** Testing damping factors $\lambda=1.0,0.5,0.2$ starting from $\theta^{(0)}=(1,1)$:

| $\lambda$ | final $(a_0,a_1)$ | iterations to converge (within $10^{-4}$) |
|---|---|---|
| 1.0 (undamped) | $(2.4309,\ 0.6364)$ | ~4 |
| 0.5 | $(2.4309,\ 0.6364)$ | ~17 |
| 0.2 | *(not yet converged after 20 iterations — still at $(2.374,\ 0.645)$)* | >20 |

Damping does **not** change the final answer (when it does converge, $\lambda=0.5$ reaches the
identical solution as $\lambda=1.0$) — it only changes HOW MANY iterations are needed, and smaller
$\lambda$ needs substantially more iterations (over 4× more for $\lambda=0.5$, and still not fully
converged at $\lambda=0.2$ within 20 iterations). For this particular well-behaved problem,
damping is purely a cost with no benefit — undamped (full-step) Gauss–Newton is strictly better.
Damping becomes genuinely useful in problems where the undamped step is a *poor* local
approximation (highly curved or badly-scaled SSE surfaces, or starting points far from the
minimum) — there, a full undamped step can badly overshoot or even diverge, and taking a smaller,
damped step (sometimes combined with adaptively adjusting $\lambda$ itself, as the
Levenberg–Marquardt algorithm used by `scipy.optimize.curve_fit`/`minpack.lm` does automatically)
trades some speed for substantially improved reliability.
