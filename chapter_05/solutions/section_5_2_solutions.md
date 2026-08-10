# Section 5.2 — Worked Solutions

## Conceptual

**1.** Every population-level epidemic model in Chapters 1–2 has infectious individuals leave
their compartment purely through recovery or death — there is no mechanism for the infectious
class to grow "on its own." Here, infected T cells retain most of their normal cellular function,
including the ability to divide (the $\nu_2y(1-(x+y)/K)$ term) — so once a population of infected
cells exists, it can maintain or even grow itself through ordinary cell division, completely
independent of whether any NEW cells are being infected via contact. This is a fundamentally
different biological mechanism from anything in the population-level models, and it's exactly
what allows a chronic state to persist even when $\sigma$ (which only governs the RATE of NEW
infections) is small.

**2.** A saddle point's stable manifold is the special (lower-dimensional, here 1-dimensional)
set of points that, followed forward in time, converge exactly TO the saddle rather than being
repelled from it. It divides the plane into (at least) two regions, since points on either side of
this manifold get pushed toward the saddle briefly but then diverge away from each other along the
saddle's unstable direction, ending up at different final destinations. Without a saddle (or some
other kind of separating structure), two stable equilibria alone wouldn't automatically imply
which points go where — the saddle's stable manifold is exactly the "boundary line" needed to
partition the plane into well-defined basins of attraction for each of the two stable equilibria.

**3.** $\mathcal{R}_0$ still answers a well-defined, meaningful biological question — "on average,
does a single introduced infected cell (population), starting from the disease-free state,
initially grow or decay" — it's a statement about LOCAL behavior near $P_0$ specifically. What
changes in the backward-bifurcation case is that $\mathcal{R}_0<1$ no longer implies GLOBAL
elimination from every starting condition — it only implies $P_0$ remains locally stable (small
enough perturbations still die out), while larger perturbations (like an already-substantial
existing infection) can persist. $\mathcal{R}_0$'s original, narrower meaning (local stability of
$P_0$) remains completely intact and correctly computed — it's the broader intuition ("$\mathcal{
R}_0<1$ means elimination, period") that turns out to be unreliable here.

**4.** If a real infection is already established at a level past the saddle's stable manifold
(i.e., already in the "chronic" basin) when an intervention pushes $\mathcal{R}_0$ just below 1,
the intervention will NOT be sufficient to eliminate the infection — the population will simply
settle into the chronic equilibrium anyway, despite $\mathcal{R}_0<1$ technically holding. Policy
based on backward-bifurcation-prone diseases may need to push $\mathcal{R}_0$ considerably further
below 1 (specifically, below $\mathcal{R}_0(\sigma_0)$, not just below 1) to guarantee elimination
regardless of starting conditions — a substantially more demanding, and more expensive, target
than the naive "just get below 1" heuristic that works for every forward-bifurcation model in this
book.

**5.** If periodic orbits could exist, a "bistable" picture might actually be incomplete — instead
of two coexisting equilibria, the true long-run picture might include additional attracting
periodic orbits (sustained oscillations) as a third (or more) possible outcome, and some
trajectories that appear to be heading toward one of the two equilibria in a finite-time simulation
might actually be circling a periodic orbit not yet resolved by the simulation's time window.
Ruling out periodic orbits via Dulac's criterion is what allows Poincaré–Bendixson to guarantee
EVERY trajectory converges to an equilibrium — making the two-equilibria bistable picture the
complete and final classification, not just the most commonly observed behavior in a finite set of
simulations.

## Computational

**6.** Since $\sigma=0.05<\sigma_c\approx0.185$, and this notebook's numerically-located
bifurcation diagram shows exactly **2** chronic equilibria throughout $[0,\sigma_c)$ for this
parameter set, we'd expect **2** chronic equilibria at $\sigma=0.05$ — confirmed directly by
computation: $(18.37, 602.81)$ and $(617.88, 45.23)$.

**7.** $\mathcal{R}_0(0.05)=\frac{1}{0.484}\left[0.05\times0.000862\times836.68+0.866\times
\left(1-\frac{836.68}{1405.3}\right)\right]\approx\mathbf{0.798}$.

**8.** Yes, $\mathcal{R}_0(0.05)\approx0.798<1$. And yes — bistability still occurs: direct
classification confirms one of the two chronic equilibria found in Q6 is a **stable node**
$(18.37,602.81)$ and the other is a **saddle** $(617.88,45.23)$, with the disease-free equilibrium
$P_0$ also stable — the full three-equilibria bistable topology, present at $\sigma=0.05$ just as
it was at $\sigma=0.1$.

**9.** The positive eigenvalue ($\approx+0.043$ at $\sigma=0.1$) means that ALONG the
corresponding eigendirection (the saddle's unstable manifold), any nonzero perturbation from the
saddle grows exponentially at that rate — trajectories starting exactly on the unstable manifold
(but off the saddle itself) move AWAY from the saddle, never toward it, at a rate proportional to
$e^{0.043t}$. This is the mathematical signature of instability along that specific direction, in
contrast to the negative eigenvalue's direction (the stable manifold), along which nearby
trajectories DO converge to the saddle.

**10.** Since $x_0$ (the disease-free equilibrium) doesn't depend on $\mu_2$ at all (it's
determined entirely by $\lambda,\nu_1,\mu_1,K$), and $\mathcal{R}_0(\sigma)=\frac{1}{\mu_2}[\ldots]$
has $\mu_2$ appearing ONLY in the denominator (with a $\mu_2$-independent numerator), increasing
$\mu_2$ makes $\mathcal{R}_0$ **decrease** — directly confirming the biologically sensible
expectation that a treatment increasing the infected-cell death rate should reduce the disease's
reproductive capacity.

## Coding Exercises

**11.** Python:
```python
import numpy as np

def classify_equilibrium(x, y, sigma, params):
    lam, K, nu1, nu2, mu1, mu2, beta = params
    dPdx = nu1*(1-(2*x+y)/K) - mu1 - beta*y
    dPdy = -nu1*x/K - beta*x
    dQdx = sigma*beta*y - nu2*y/K
    dQdy = sigma*beta*x + nu2*(1-(x+2*y)/K) - mu2
    eig = np.linalg.eigvals(np.array([[dPdx, dPdy], [dQdx, dQdy]]))
    if np.all(eig.real < 0):
        return "stable node"
    elif np.any(eig.real < 0) and np.any(eig.real > 0):
        return "saddle"
    else:
        return "unstable"
```
Tested on all three equilibria at $\sigma=0.1$: $P_0=(836.68,0)$ → `"stable node"`;
$(18.32,604.14)$ → `"stable node"`; $(684.38,31.24)$ → `"saddle"` — matching Block 3's findings
exactly.

**12.** Sweeping a $8\times8$ grid of starting points (64 total, filtered to those inside $\Gamma$)
at $\sigma=0.1$: 6 converge to disease-free, 44 converge to chronic infection — the basin split is
NOT 50/50 (the chronic basin is considerably larger in this parameter regime), and the boundary
between the two colored regions in a scatter plot passes near the saddle's location
$(684.38,31.24)$, consistent with the saddle's stable manifold forming the actual basin boundary.

## Visualization Exercise

**13.** Plotting $\mathcal{R}_0(\sigma)=\frac{1}{\mu_2}[\sigma\beta x_0+\nu_2(1-x_0/K)]$ for
$\sigma\in[0,1]$: it's a straight line (linear in $\sigma$), starting at $\mathcal{R}_0(0)\approx
0.766$ and increasing steadily. At the numerically-located $\sigma_c\approx0.18521$, the plotted
line gives $\mathcal{R}_0(\sigma_c)\approx0.99996$ — matching the theoretical claim
$\mathcal{R}_0(\sigma_c)=1$ to within numerical precision (the tiny residual gap is exactly the
bisection search's stopping tolerance from Block 2, not a real discrepancy).

## Challenge Problem

**14.** Searching over $\nu_2$ (holding $\mu_2=0.484$ fixed) reveals a clean qualitative
transition: for $\nu_2=0.5$ (barely above $\mu_2$), there are **0** chronic equilibria across the
ENTIRE tested $\sigma$ range up to 0.3 — infected cells simply cannot sustain themselves via
proliferation alone when their growth rate is too close to their death rate. For $\nu_2=0.6$, a
genuinely clean version of the book's own four-case pattern emerges:

| $\sigma$ | 0.001–0.175 | 0.18–0.185 | 0.185–0.3 (tested up to) |
|---|---|---|---|
| equilibrium count | **0** | transition (≈$\sigma_0$, between 0.18 and 0.185) | **2** |

confirming a genuine lower threshold $\sigma_0\approx0.18$–$0.185$ DOES exist for this modified
parameter set — the "0 chronic equilibria for small $\sigma$" regime shown in the book's own
Figure 5.2(a) is recovered once $\nu_2$ is brought close enough to (but still above) $\mu_2$. This
directly confirms the conceptual answer to Q1: the persistent low-$\sigma$ chronic branch found
with the original parameters ($\nu_2=0.866$, comfortably above $\mu_2=0.484$) was a direct
consequence of infected cells having a strong proliferative advantage even without new
transmission; narrowing that advantage (smaller $\nu_2-\mu_2$ gap) removes the persistent branch
and recovers the cleaner textbook bifurcation picture with a genuine $\sigma_0>0$.

---

# This closes the final solutions file of the book.

Every section from 1.1 through 5.2 — the complete textbook — has now been rewritten, derived,
explained, visualized, implemented in Python and R, numerically verified, and equipped with
practice problems and full worked solutions.
