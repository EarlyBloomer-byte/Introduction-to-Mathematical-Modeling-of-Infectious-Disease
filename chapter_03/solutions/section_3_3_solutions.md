# Section 3.3 — Worked Solutions

## Conceptual

**1.** $\dot V\le0$ (non-strict) only guarantees $V$ never *increases* along trajectories — this
keeps trajectories from wandering away (stability), but says nothing about whether they actually
make *progress* toward the origin; $V$ could stay perfectly flat forever along some trajectory
(e.g. a circular orbit), which is consistent with $\dot V\le0$ but clearly not convergence.
Requiring $\dot V$ strictly negative (except at the origin) forces $V$ to keep *strictly*
decreasing everywhere except exactly at the equilibrium, which is exactly the extra ingredient
needed to guarantee trajectories are continually making progress toward — and therefore eventually
reach — the origin.

**2.** Theorem 3.3.2 requires $\dot V$ to be negative *definite*, i.e. strictly negative
*everywhere except exactly at the origin*. Here $\dot V=-cv^2=0$ whenever $v=0$, which includes
many points other than the origin (e.g. $(5,0)$) — so $\dot V$ fails to be negative definite in
the strict sense the theorem demands, even though (as later analysis, e.g. LaSalle, confirms) the
origin genuinely is asymptotically stable. This is precisely why a *stronger* tool (LaSalle) is
needed to close the gap Theorem 3.3.2 alone leaves open.

**3.** An omega-limit set is "the set of points a trajectory keeps coming back arbitrarily close to,
no matter how far out in time you look" — informally, where the trajectory ends up settling
(possibly a single point, possibly a more complicated set like a periodic orbit). It must be
invariant because if a trajectory keeps returning near some point $p$ in its omega-limit set,
then (by the deterministic, unique-solutions nature of the ODE) the entire future trajectory
starting from $p$ must ALSO keep getting revisited arbitrarily closely — pushing that whole future
trajectory into the omega-limit set too, which is exactly what "invariant" means.

**4.** If $G$ were not positively invariant, a trajectory starting inside $G$ could eventually
leave $G$ — at which point all the guarantees built on "$\dot V\le0$ throughout $G$" simply stop
applying (since $\dot V$'s sign was only established *inside* $G$), and the trajectory's ultimate
fate becomes unknown. Positive invariance is exactly the guarantee that a trajectory starting in
$G$ can never escape the region where the Lyapunov argument is valid, which is what allows the
local conclusion (near the equilibrium) to be extended into a genuinely global one (throughout
all of $G$).

**5.** "V is negative definite everywhere" is a *uniform, region-wide* condition — it must hold
at literally every point in the neighborhood. "There exists a sequence $x_n\to0$ with
$V(x_n)<0$" is a much weaker, *existence-only* condition — it only requires finding *some*
points, arbitrarily close to the origin, where $V$ happens to be negative; it says nothing about
$V$'s sign at other nearby points. This weaker condition is exactly enough to guarantee that
*some* trajectories starting arbitrarily close to the origin get pushed away (instability) without
needing to control $V$'s sign everywhere in the neighborhood.

## Computational

**6.** $V(x,y)=x^2+y^2$ is positive definite (standard sum of squares, zero only at the origin).
$\dot V=2x\cdot(-x)+2y\cdot(-2y)=-2x^2-4y^2$ — negative definite (strictly negative except at the
origin). **Yes**, Theorem 3.3.2 applies directly: the origin is asymptotically stable (as expected,
since this system is just two independent, trivially stable linear decays).

**7.** $\dot V=2x\cdot y+2y\cdot(-x)=2xy-2xy=0$ **everywhere**. Since $\dot V\equiv0\le0$, Theorem
3.3.1 applies (the origin is **stable**), but $\dot V$ is nowhere negative (let alone negative
definite), so Theorem 3.3.2 does NOT apply — consistent with this system being a pure rotation
(circular orbits, exactly the "center" behavior from Sec. 3.1's undamped-oscillator example):
**stable but not asymptotically stable**.

**8.** At $(x,v)=(2,0)$: $\dot V=-c(0)^2=0$, so yes, this point **is** in $\{\dot V=0\}$. But is it
in the largest invariant subset $K$? Checking whether the system stays there: at $(2,0)$,
$x'=v=0$ but $v'=-x-cv=-2-0=-2\ne0$ — so the trajectory immediately starts moving (specifically,
$v$ starts decreasing from 0), meaning it does NOT stay at $(2,0)$ forever. So $(2,0)$ is **in**
$\{\dot V=0\}$ but **NOT in** the invariant subset $K$ — exactly illustrating why $K$ (just the
origin) is strictly smaller than the full set $\{\dot V=0\}$ (the entire line $v=0$).

**9.** Starting from $I'=\beta IS-\gamma I-bI$ (the $I$-equation of Sec. 2.3's model): factor out
$I$: $I'=I(\beta S-\gamma-b)$. Since $L(S,I)=I$, $\dot L=\text{grad }L\cdot(S',I')
=(0,1)\cdot(S',I')=I'=I(\beta S-\gamma-b)$ — a direct, one-line substitution confirming the
Lyapunov derivative of $L=I$ is exactly the $I$-equation's right-hand side itself (since $L$
depends only on $I$, its gradient is $(0,1)$, which simply "picks out" the $I'$ equation).

**10.** On the segment $I=0$, the $S$-equation becomes $S'=b-\beta(0)S-bS=b-bS=b(1-S)$ — a simple
linear ODE. Separating variables: $\frac{dS}{1-S}=b\,dt \Rightarrow -\ln|1-S|=bt+C \Rightarrow
1-S(t)=(1-S_0)e^{-bt} \Rightarrow S(t)=1-(1-S_0)e^{-bt}$. As $t\to\infty$, $e^{-bt}\to0$ (since
$b>0$), so $S(t)\to1$ for **any** starting value $S_0\in[0,1]$ — confirming the claim directly
from the closed-form solution, not just simulation.

## Coding Exercises

**11.** Python:
```python
import numpy as np

def check_lyapunov_conditions(V_func, Vdot_func, region_points, origin=(0,0), tol=1e-9):
    V_ok, Vdot_ok = True, True
    for p in region_points:
        Vp = V_func(*p)
        if np.allclose(p, origin, atol=tol):
            if abs(Vp) > tol:
                V_ok = False
        elif Vp < -tol:
            V_ok = False
        if Vdot_func(*p) > tol:
            Vdot_ok = False
    return V_ok, Vdot_ok
```
Tested on the damped oscillator's energy function across 500 random sample points plus the origin
itself: both conditions report **True** — $V$ is positive definite and $\dot V\le0$ everywhere
sampled, consistent with the analytical result.

**12.** Simulating from two points on the segment ($S_0=0.2$ and $S_0=0.6$, both with $I_0=0$):
in both cases, `max|I(t)|` along the entire trajectory stays **exactly 0** (to floating-point
precision) — confirming the segment truly is invariant (a trajectory starting on it never leaves
it) — while $S(t)$ still climbs toward 1 in both cases ($S(t_{end})\approx0.892$ and
$\approx0.946$ respectively, still climbing at the simulation's endpoint, consistent with the
$e^{-bt}$ decay from Q10 which approaches but never exactly reaches 1 in finite time). This
confirms both halves of the LaSalle argument: the segment is invariant, AND no point other than
$P_0$ itself is "permanently at rest" within it — exactly why $K$ collapses to the single point
$P_0$ rather than the whole segment.

## Visualization Exercise

**13.** Plotting circular level curves of $V(x,v)=\frac12(x^2+v^2)$ (concentric circles centered
at the origin, radius $=\sqrt{2V}$) together with a spiraling damped-oscillator trajectory
starting at $(1,0)$: the trajectory visibly crosses through smaller and smaller circles as it
spirals inward, never once crossing back out to a larger circle than it was previously on — a
direct visual confirmation that $V$ (proportional to squared distance from the origin) is
monotonically non-increasing along the trajectory, exactly as $\dot V\le0$ requires.

## Challenge Problem

**14.** For $x'=x,\ y'=y$: with $V=x^2+y^2$, $\dot V=2x\cdot x+2y\cdot y=2x^2+2y^2\ge0$ — this is
POSITIVE (semi-)definite, not negative, so it does **not** fit Theorem 3.3.3's hypothesis (2),
which requires $\dot V\le0$. Trying instead $V=-(x^2+y^2)$: this $V$ takes negative values
*everywhere* except the origin (certainly along any sequence $x_n\to0$, satisfying hypothesis (1)
of Theorem 3.3.3 immediately), and $\dot V=-2x\cdot x-2y\cdot y=-2x^2-2y^2\le0$, with equality only
at the origin — satisfying hypothesis (2) exactly. So Theorem 3.3.3 **does** apply with this
sign-flipped $V$, correctly concluding the origin is **unstable** — matching the obvious fact that
$x(t)=x_0e^t,\ y(t)=y_0e^t$ diverges to infinity for any nonzero starting point. The lesson: when
a natural "energy" function like $x^2+y^2$ doesn't fit a theorem's hypotheses as stated, it's
often worth checking whether flipping its sign makes it fit a *different* (but related) theorem —
here, the instability theorem specifically wants a function that's negative near (but not at) the
origin, exactly the opposite sign convention from the stability theorems.
