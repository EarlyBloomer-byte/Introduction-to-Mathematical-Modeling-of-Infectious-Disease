# Section 3.4 — Worked Solutions

## Conceptual

**1.** A linear (simple harmonic) oscillator's period is a constant, $T=2\pi\sqrt{m/k}$
(or, for $x''+x=0$, exactly $T=2\pi$), **completely independent of amplitude** — every orbit,
regardless of how big or small, takes exactly the same time to complete one cycle. Since every
nearby periodic solution shares the *identical* period, two nearby solutions started at slightly
different amplitudes stay perfectly in phase forever — there's no possibility of one "lapping"
the other, so $|p(t)-q(t)|$ stays small for all time whenever the initial conditions are close.
This is exactly the special, simplifying feature that breaks down once nonlinearity (like
$\sin x$ instead of $x$) makes the period amplitude-dependent.

**2.** Orbital stability says nearby *orbits* (as sets/curves) stay geometrically close to the
target orbit forever — but doesn't require them to converge onto it. Orbital *asymptotic*
stability adds that requirement: nearby orbits must actually merge into (converge onto) the target
orbit as $t\to\infty$. The pendulum's nested orbits are orbitally stable (they stay close to each
other, nested, forever) but NOT orbitally asymptotically stable (they never merge — each stays on
its own separate nested loop forever). The $r'=r(1-r^2)$ system's unit circle IS orbitally
asymptotically stable — it's a genuine limit cycle, and every tested nearby trajectory was shown
to spiral onto it, not just stay nearby.

**3.** Because $y(t)=p'(t)$ (the velocity/derivative of the periodic solution itself) can always be
shown, purely by differentiating the defining relation $p'(t)=f(p(t))$ using the chain rule, to be
a solution of the linearized system (3.7) — and since $p(t)$ is periodic, so is its derivative
$p'(t)$, and it's nonconstant (as long as the orbit isn't a fixed equilibrium in disguise). By
Theorem 3.4.1 part (3), a nonconstant periodic solution existing is *equivalent to* some Floquet
multiplier equaling exactly 1 — so this always happens, for any smooth system with a genuine
(nonconstant) periodic orbit, no special conditions needed.

**4.** A single equilibrium point is a single, fixed location — the Jacobian there is a single,
constant matrix, valid everywhere "nearby" in every direction. A periodic orbit is instead an
entire moving curve; the local linear behavior of the system genuinely differs at different points
along that curve (different $p(t)$ values generally give different Jacobians $\partial f/\partial
x(p(t))$). So a single, fixed Jacobian cannot capture the orbit's stability — the linearization
must be evaluated *along the whole orbit*, producing a genuinely time-varying (periodic-in-time)
linear system, which is exactly what Floquet theory is built to analyze.

**5.** If the second Floquet multiplier had modulus greater than 1, nearby trajectories would be
pushed AWAY from $r=1$ after each loop rather than pulled toward it — $r=1$ would still technically
be a periodic orbit (the exact solution $r(t)=1$ for all $t$ still solves the equation regardless
of nearby behavior), but it would be an UNSTABLE periodic orbit rather than a limit cycle — nearby
trajectories would spiral away instead of spiraling in. (Note this can't actually happen for THIS
specific system, since $g'(1)$ for $g(r)=r(1-r^2)$ is fixed at $-2<0$ by the equation's specific
form — but it's a genuine possibility for other systems with different radial dynamics.)

## Computational

**6.** $e^{-4\pi}\approx\mathbf{3.487\times10^{-6}}$ — an extremely small number, reflecting very
strong (fast) attraction onto the limit cycle.

**7.** $g(r)=r(1-r^2/4)=r-r^3/4$, so $g'(r)=1-\frac{3r^2}{4}$. At $r=2$:
$g'(2)=1-\frac{3(4)}{4}=1-3=\mathbf{-2}$ — negative, confirming $r=2$ is an attracting radius for
this system too (same qualitative structure as the $r=1$ example, just relocated and rescaled).

**8.** **Yes**, orbitally asymptotically stable. Theorem 3.4.2 requires the multipliers OTHER than
the guaranteed "1" to have modulus less than 1 — here those are $0.3$ (modulus $0.3<1$ ✓) and
$1.2$... **wait** — modulus $1.2>1$, which means this orbit is actually **NOT** orbitally
asymptotically stable (one of the non-trivial multipliers exceeds modulus 1, indicating growth in
that direction). This is a deliberately tricky example: don't just check that *a* multiplier
equals 1 and stop there — every OTHER multiplier must also individually satisfy modulus $<1$.

**9.** Floquet multiplier $=e^{\lambda T}$ for each exponent $\lambda$. For $\lambda=0$:
$e^{0\times4}=e^0=\mathbf{1}$ (consistent with the always-guaranteed multiplier, corresponding to
exponent exactly 0). For $\lambda=-0.5$: $e^{-0.5\times4}=e^{-2}\approx\mathbf{0.1353}$.

**10.** **False, but subtly.** The precise statement (Theorem 3.4.2) is that the orbit is orbitally
asymptotically stable if the multipliers OTHER THAN the always-present "1" all have modulus less
than 1. Since one multiplier is *always* exactly 1 (modulus exactly 1, not less than 1), the
literal statement "ALL Floquet multipliers have modulus less than 1" can in fact **never be true**
for any periodic orbit — the correct and careful statement always excludes that one guaranteed
trivial multiplier from the count.

## Coding Exercises

**11.** Python:
```python
import numpy as np
from scipy.integrate import solve_ivp

def compute_floquet_multipliers(jacobian_func, period, orbit_func, n_dim):
    def lin_rhs(t, Y_flat):
        Y = Y_flat.reshape(n_dim, n_dim)
        pt = orbit_func(t)
        J = jacobian_func(*pt)
        return (J @ Y).flatten()
    Y0 = np.eye(n_dim).flatten()
    sol = solve_ivp(lin_rhs, (0, period), Y0, rtol=1e-12, atol=1e-12)
    monodromy = sol.y[:, -1].reshape(n_dim, n_dim)
    return np.linalg.eigvals(monodromy)
```
Applied to the $r'=r(1-r^2)$ example: returns `[3.487e-06, 1.0]`, exactly reproducing the
notebook's directly-computed result.

**12.** Measuring the period numerically (via zero-crossing detection, $x$ crossing 0 with $v>0$):
amplitude-0.5 orbit has period $\approx6.383$; amplitude-0.7 orbit has period
$\approx6.481$ — the larger-amplitude orbit's period is indeed **longer** (by about 0.098 time
units), confirming the physical fact that a pendulum released from a wider swing takes longer per
cycle. Over the 60-time-unit simulation window (roughly 9.4 periods), that period difference
accumulates to a phase lag of around $9.4\times0.098\approx0.92$ time units — comparable to a
significant fraction of a full period, which is exactly why $|p(t)-q(t)|$ was observed to grow
large periodically in Block 1: the two pendulums gradually drift out of sync by almost a full
swing's worth of phase over the course of the simulation.

## Visualization Exercise

**13.** Starting four points on a tiny circle of radius 0.1 around a point near (but not exactly
on) the limit cycle: all four trajectories rapidly **converge together AND onto the limit
cycle simultaneously** — the small cluster shrinks in size as all four points get pulled toward
$r=1$, while also all ending up close to each other (since they're all being pulled toward the
same one-dimensional curve). This directly reflects the tiny magnitude of the non-trivial Floquet
multiplier ($\approx3.5\times10^{-6}$): the "off-orbit" direction contracts *extremely* fast (much
faster than one period), so any small cluster of nearby points collapses onto the limit cycle
almost immediately, well before completing even a single full loop.

## Challenge Problem

**14.** Directly computing the monodromy matrix requires integrating an $n\times n$ matrix ODE
(i.e., $n^2$ coupled scalar equations) over one period — for $n=20$, that's 400 coupled equations,
which becomes computationally expensive (though not necessarily intractable with modern
computers) and numerically delicate (errors can compound over long integrations, and eigenvalues
of large matrices can be sensitive to small numerical errors, especially when multipliers are
close together in magnitude). Strategies suggested by techniques already seen in this book: (a)
the **homogeneous-system dimension-reduction** trick from Sec. 2.4 — if the high-dimensional
system has extra structure (like a conservation law or degree-1 homogeneity), reducing to fewer
effective dimensions before doing Floquet analysis would shrink the monodromy matrix substantially;
(b) exploiting **sparsity or block structure** in the Jacobian (many realistic epidemic models
have Jacobians where most compartments only interact with a few neighbors, not all 20 with each
other) could make the linear algebra dramatically cheaper; (c) as the book itself hints, dedicated
techniques (Muldowney's compound-matrix approach) reformulate the problem to avoid needing every
individual Floquet multiplier — often only the *sign* of the dominant multiplier's modulus
relative to 1 is actually needed for a stability conclusion, not its precise numerical value,
and specialized methods can sometimes establish that sign more cheaply than full monodromy-matrix
computation.
