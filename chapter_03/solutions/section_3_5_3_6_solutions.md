# Sections 3.5–3.6 — Worked Solutions

## Conceptual

**1.** In 1D, the Jacobian at an equilibrium is just the single number $f'(\bar x)$ — a $1\times1$
matrix. The Routh–Hurwitz-style stability condition "all eigenvalues have negative real part"
collapses, for a $1\times1$ matrix, to simply "this one number is negative" — exactly
Proposition 3.5.1's criterion. So the 1D phase-line criterion isn't a separate idea at all; it's
the linearization/eigenvalue machinery of Sec. 3.2 in its simplest possible case, where there's
only one eigenvalue and it equals the derivative itself.

**2.** The Poincaré–Bendixson argument fundamentally relies on the fact that a continuous curve
confined to a bounded 2D region, forbidden from crossing itself, has very limited "room" to
behave — it either has to spiral into a point or settle into a periodic loop; there's no third
option available in the plane. In 3D (or higher), a bounded, non-self-intersecting curve has vastly
more room to maneuver — it can wind around in complex, never-repeating patterns without ever
crossing itself (this is exactly what happens in chaotic attractors like the Lorenz attractor,
mentioned in the text) — so the "must be periodic" conclusion simply doesn't follow anymore once
there's a third dimension to twist through.

**3.** If the fenced region contained an equilibrium, the trapped trajectory could simply spiral
into (or otherwise converge onto) that equilibrium instead of tracing out a periodic loop — an
equilibrium is itself a perfectly valid, "boring" kind of limit set (a single resting point), and
its presence gives the trapped trajectory an alternative destination besides a periodic orbit. Only
by explicitly excluding equilibria from the trapping region does the theorem force the "must loop
forever" conclusion, since resting is no longer possible anywhere in the trap.

**4.** For a vector field $f=(P,Q)$, the divergence is defined as
$\text{div }f=\frac{\partial P}{\partial u}+\frac{\partial Q}{\partial v}$ — literally the sum of
the diagonal entries of the Jacobian matrix $\partial f/\partial x=\begin{pmatrix}\partial
P/\partial u&\partial P/\partial v\\\partial Q/\partial u&\partial Q/\partial v\end{pmatrix}$.
The trace of any matrix is, by definition, the sum of its diagonal entries — so
$\text{tr}(\partial f/\partial x)=\partial P/\partial u+\partial Q/\partial v$, which is exactly
the divergence. These are the same quantity computed two different (but algebraically identical)
ways.

**5.** Bendixson's plain criterion checks the sign of $\text{div }f$ directly. Sec. 2.3/2.5's
argument instead checks the sign of $\text{div}(\alpha f)$ for some cleverly chosen scalar
multiplier function $\alpha(x,y)$ — a strictly more flexible tool. You need the multiplier version
specifically when the PLAIN divergence is not sign-definite (changes sign somewhere in the region
of interest) — exactly the situation in Sec. 2.3, where $\partial P/\partial S+\partial Q/\partial
I=\beta(S-I)-2b-\gamma$ changes sign depending on whether $S>I$ or $S<I$, but multiplying by
$\alpha=1/I$ produces the sign-definite $-\beta-b/I$ instead.

## Computational

**6.** $f(x)=x(x-2)(x+3)$, equilibria at $x=0,2,-3$. $f'(x)=3x^2+2x-6$ (expanding and
differentiating). At $x=0$: $f'(0)=-6<0$ → **stable**. At $x=2$: $f'(2)=12+4-6=10>0$ →
**unstable**. At $x=-3$: $f'(-3)=27-6-6=15>0$ → **unstable**.

**7.** $J(0,0)=\begin{pmatrix}0&1\\-1&\mu\end{pmatrix}=\begin{pmatrix}0&1\\-1&2\end{pmatrix}$ for
$\mu=2$. $\text{tr}(J)=2>0$, $\det(J)=(0)(2)-(1)(-1)=1>0$. Since trace is positive, by
Routh–Hurwitz the origin is **unstable** (same conclusion as $\mu=1$, and in fact for ANY
$\mu>0$, since $\text{tr}(J)=\mu>0$ always in this family).

**8.** $\lambda_2=e^{-6}\approx0.00248$. Since $0<\lambda_2<1$, **yes**, orbitally asymptotically
stable (Theorem 3.4.2 / Poincaré's condition).

**9.** $\text{div }f(x,y)=3x^2+2y^2+1 \ge 1 > 0$ for **all** $(x,y)$ (since $3x^2\ge0$ and
$2y^2\ge0$ always) — strictly positive everywhere, hence sign-definite. **Yes**, Bendixson's
criterion rules out periodic orbits anywhere in all of $\mathbb{R}^2$ (which is simply connected).

**10.** $f'(N)=r-\frac{2r}{K}N$. $f'(0)=r=0.5>0$ → **unstable**. $f'(K)=r-2r=-r=-0.5<0$ →
**stable**. So $N=K=200$ is the stable equilibrium (as always for the logistic equation with
$r>0$).

## Coding Exercises

**11.** Python:
```python
def phase_line_stability(f_prime, equilibria):
    result = {}
    for eq in equilibria:
        fp = f_prime(eq)
        result[eq] = "stable" if fp < 0 else "unstable"
    return result

r, K = 0.3, 100
fprime_logistic = lambda N: r - 2*r*N/K
print(phase_line_stability(fprime_logistic, [0, K]))
# -> {0: 'unstable', 100: 'stable'}
```
Matches the analytical prediction exactly.

**12.** Numerically locating the van der Pol limit cycle's period (via peak-detection on a
settled trajectory) gives period $\approx6.663$. Integrating $\text{div }f=\mu(1-x^2)$ along that
numerically-found orbit over one period gives $\approx-7.058$, so Liouville's formula predicts
$\lambda_2=e^{-7.058}\approx8.602\times10^{-4}$. Independently computing the monodromy matrix by
integrating the linearized system along the same numerically-found orbit gives eigenvalues
$\approx\{8.601\times10^{-4},\ 1.00006\}$ — matching Liouville's prediction to about 4 significant
figures (small residual discrepancy attributable to the period being estimated numerically via
peak-detection rather than known in exact closed form, unlike the $r'=r(1-r^2)$ case). This
confirms Poincaré's stability condition applies just as well to a limit cycle with no closed-form
solution, not just the exactly-solvable example from the main notebook.

## Visualization Exercise

**13.** Plotting $\text{div }f=\mu(1-x^2)$ against $x\in[-3,3]$: it's a downward-opening parabola,
positive for $-1<x<1$ and negative for $|x|>1$, crossing zero exactly at $x=\pm1$. Physically:
positive divergence means nearby trajectories are locally *expanding* (being pushed apart/outward)
— this happens for $|x|<1$, consistent with the unstable origin repelling trajectories from the
center. Negative divergence means local *contraction* — for $|x|>1$, consistent with the
"nonlinear damping" term pulling trajectories back inward once they swing out far enough. The limit
cycle itself must cross both regions during each cycle (it can't stay entirely within $|x|<1$ or
entirely within $|x|>1$, since it's a closed loop enclosing the origin) — spending part of each
cycle being pushed outward and part being pulled back inward, netting to exactly zero total change
in "size" per period (consistent with it being a stable, non-growing, non-shrinking periodic orbit).

## Challenge Problem

**14.** A valid construction, in polar coordinates: $r'=-(r-1)^3$, $\theta'=(r-1)^2$. At $r=1$:
both $r'=0$ and $\theta'=0$ for every $\theta$ — so **every point on the unit circle is a genuine
equilibrium** (the vector field vanishes there entirely, not just its radial component). For
$r\ne1$: $r'\ne0$ pulls $r$ back toward 1 (verified numerically: starting at $r_0=1.5$, $r(t)$
decreases toward 1, though very slowly — algebraically like $1/\sqrt{t}$ rather than exponentially,
since the linearization of $r'=-(r-1)^3$ at $r=1$ is itself degenerate/zero, similar in spirit to
the Sec. 3.2 exercise on $x'=-x^3$), while $\theta'=(r-1)^2\ne0$ keeps rotating. Because $r(t)-1$
decays only algebraically (not exponentially), $\int_0^\infty\theta'(t)\,dt=\int_0^\infty(r(t)-1)^2
dt$ **diverges** — confirmed numerically here: $\theta(t)$ keeps growing without bound
($\theta\approx6.56$ radians by $t=10^6$ and still climbing), meaning the trajectory winds around
the origin infinitely many times as $t\to\infty$, getting ever closer to (but never exactly
reaching) every angle on the circle. So its omega-limit set is the **entire unit circle** — not a
single point, and not a genuine periodic orbit either (since nothing on the circle is actually
moving — it's a continuum of equilibria, not a closed trajectory). This is exactly the
pathological case Poincaré–Bendixson's "no equilibria in the omega-limit set" hypothesis is
designed to exclude: without that hypothesis, "the omega-limit set is a closed curve" could
describe this equilibrium-filled circle just as easily as a genuine periodic orbit, and the
theorem's strong conclusion (a periodic orbit, i.e. something actually *in motion*) would be false
without explicitly ruling this case out.
