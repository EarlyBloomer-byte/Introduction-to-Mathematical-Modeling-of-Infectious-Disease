# Section 4.3 — Worked Solutions

## Conceptual

**1.** "Apples to apples" means: only ever compare a model's prediction of a SPECIFIC quantity to
data that actually measures that SAME quantity. A concrete violation: if hospital records report
new cases per week (incidence, roughly $-S'(t)$ or the flow INTO $I$) but a modeler mistakenly
fits those numbers directly against the model's $I(t)$ (the current PREVALENCE, i.e. how many are
sick right now, not how many newly got sick) — these are related but genuinely different
quantities, and fitting one against the other would systematically bias every parameter estimate.

**2.** The differential equation's solution $x(t,\theta)$ has no simpler closed form in general —
for a new trial $\theta$, there is no way to "reuse" the numerical integration performed for a
different $\theta$, because the entire trajectory (not just its endpoint) depends on $\theta$ in a
way that isn't captured by any cached partial computation. Each SSE evaluation is therefore an
entirely fresh numerical integration from $t=0$ (or wherever the initial condition is set) forward
to every observation time.

**3.** Compute the SSE at ANY set of parameters you have independent reason to consider plausible
(from prior literature, related outbreaks, or just biological plausibility bounds) and compare it
to the SSE the optimizer reports as its "best" fit. If a plausible independent guess achieves a
MUCH lower SSE than the optimizer's reported "converged" answer, that's a strong signal the
optimizer found a local, not global, minimum — exactly the check performed in this section (SSE
at the fitted point was ~7287, while SSE at a plausible/true-parameter guess was only ~164, a
massive red flag detectable without ever knowing the true generating parameters in a real
application).

**4.** For the demography-free model, summing the equations gives $dN/dt=0$ identically —
TRUE regardless of $\lambda,\gamma$'s specific values, so $N(t)$ is a constant for every possible
parameter choice, carrying zero information about which $\lambda,\gamma$ generated the data. For
Sec. 2.3's model WITH demography ($S'=b-\lambda IS-bS$, etc.), summing instead gives
$dN/dt=b(N_0-N)$ in general (or more precisely, depends on the specific birth/death structure) —
$N(t)$ is NOT automatically constant, and its trajectory DOES depend on the disease parameters
(through how many individuals move through $I$ and experience any disease-associated dynamics) —
so in that model, $N(t)$ data genuinely COULD help constrain a fit, unlike in this section's
example.

**5.** A "stiff" ODE is one where the solution has widely different timescales — here, a very
large $\lambda$ makes the epidemic unfold almost instantaneously (a very fast timescale) while the
overall observation window remains comparatively long (a slow timescale). An explicit method like
RK45 must take steps small enough to resolve the FASTEST timescale accurately (for numerical
stability, not just accuracy) even during the "boring" slow parts — forcing enormous numbers of
tiny steps and making integration extremely (sometimes practically infinitely) slow. LSODA
automatically detects this situation and switches to an implicit method, which remains numerically
stable with much larger steps regardless of how fast the fast timescale is — fixing the problem
qualitatively, not just by a constant speed factor.

## Computational

**6.** $N'=S'+I'+R'=(-\lambda IS)+(\lambda IS-\gamma I)+(\gamma I)$. The $-\lambda IS$ and
$+\lambda IS$ terms cancel; the $-\gamma I$ and $+\gamma I$ terms cancel. $N'=0$ — exactly, for
ANY values of $\lambda,\gamma,S,I$, confirming $N(t)$ is constant along every solution regardless
of parameter values.

**7.** $N'=S'+I'+R'=(b-\lambda IS-bS)+(\lambda IS-\gamma I-bI)+(\gamma I-bR)$. The $-\lambda IS$
and $+\lambda IS$ cancel; the $-\gamma I$ and $+\gamma I$ cancel; what remains is
$N'=b-bS-bI-bR=b(1-S-I-R)=b(1-N)$ (using the normalized-population convention from Sec. 2.3) —
**NOT** automatically zero; it depends on how far $N$ currently is from its equilibrium value
(here, 1), and while it doesn't directly depend on $\lambda,\gamma$ in this particular
formulation, the actual TRAJECTORY of $N(t)$ getting there is shaped by how quickly individuals
move through $I$, which does depend on $\gamma$ — so for genuinely more complex demography
structures (e.g. disease-induced mortality with $I$-specific death rates), $N(t)$'s trajectory can
depend on the disease parameters directly.

**8.** You should conclude the "converged" result is very likely a **spurious local minimum**, not
the true best fit — a properly converged global optimum cannot be beaten by an independent guess
achieving a 10x lower SSE. The appropriate response is to re-run the optimization from multiple
starting points (including, ideally, near the better-performing guess) and take the genuinely
lowest-SSE result found.

**9.** In principle, you need at least $p\ge m$ observation times (at least as many independent
data points as parameters) for the problem to not be underdetermined — exactly analogous to Sec.
4.1's requirement of at least $m+1$ points to fit $m+1$ coefficients. Having MORE observation
times than this bare minimum helps by averaging out noise (each additional independent data point
adds more information competing against the same amount of random measurement error, generally
improving estimation precision) and by better constraining the model's behavior across its full
dynamic range (e.g. capturing both the rise and fall of an epidemic curve, not just a few points
clustered in one region).

**10.** Yes, generally — since $N(t)$ for such a model actually varies with $\theta$ (Q7's
answer), including it as an additional observable gives the optimizer genuinely independent
information beyond what $I(t)$ alone provides, which should generally improve (or at worst not
hurt) estimation accuracy — in contrast to THIS section's demography-free example, where $N(t)$
was analytically guaranteed to carry zero information regardless of $\theta$, so including it
neither helped nor hurt.

## Coding Exercises

**11.** Python:
```python
import numpy as np
from scipy.optimize import minimize

def multi_start_fit(sse_func, starting_guesses, method="Nelder-Mead"):
    results = [minimize(sse_func, g, method=method,
                         options={"xatol": 1e-8, "fatol": 1e-8, "maxiter": 3000})
               for g in starting_guesses]
    best = min(results, key=lambda r: r.fun)
    sses = [r.fun for r in results]
    agree = (max(sses) - min(sses)) < 1.0  # small tolerance -> "different starts agree"
    return best, agree
```
Applied to this section's SIR problem: returns the best result
$(\hat\lambda,\hat\gamma)\approx(0.01028,0.10320)$ with `agree=False` — correctly flagging that
the different starting guesses did NOT all agree (confirming the local-minimum issue really is
present and detectable this way).

**12.** Re-running with `width=0.3` (3× the original noise level): the multi-start fit still
recovers reasonable parameters ($\hat\lambda\approx0.0109,\hat\gamma\approx0.109$, close to the
true $0.01,0.1$), but with a noticeably higher final SSE (~1428 vs ~159, roughly proportional to
the increased noise variance, as expected) and still `agree=False` (some starting points still get
trapped in the same style of local minimum). More noise makes the ABSOLUTE fit worse (higher SSE,
somewhat less precise parameter recovery) but did not, in this test, change whether the specific
local-minimum trap from the bad starting guess occurs — that trap appears to be more a structural
feature of the SSE landscape near $(\lambda,\gamma)=(8,0.02)$ than something noise-level-dependent.

## Visualization Exercise

**13.** Plotting $SSE(\lambda,\gamma)$ with $\lambda$ on a log scale reveals two separated
low-SSE regions: a narrow, deep valley near the true parameters $(0.01,0.1)$, and a second,
shallower (higher-SSE, but still locally-minimal) basin out near very large $\lambda$ — visually
confirming the SSE landscape genuinely has (at least) two distinct local minima, with the
optimizer's starting point $(8,0.02)$ sitting closer to (and within the basin of attraction of)
the spurious large-$\lambda$ minimum rather than the true one — exactly explaining why Nelder-Mead
converges there instead of to the global optimum.

## Challenge Problem

**14.** Testing 7 different random seeds (all other settings unchanged), starting from
$(\lambda,\gamma)=(8,0.02)$ every time:

| seed | fitted $(\lambda,\gamma)$ | SSE (fitted) | SSE (at true params) | trapped? |
|---|---|---|---|---|
| 1 | (525.6, 6.96) | 5529.3 | 254.7 | **YES** |
| 2 | (533.8, 7.15) | 6891.9 | 392.2 | **YES** |
| 3 | (160.8, 2.08) | 6443.3 | 354.1 | **YES** |
| 4 | (23556, 0.0845) | 6067.2 | 235.9 | **YES** |
| 5 | (251120, 0.135) | 11535.5 | 300.4 | **YES** |
| 10 | (45795, 0.109) | 7471.0 | 230.6 | **YES** |
| 20 | (2316.8, 31.0) | 5187.1 | 635.0 | **YES** |

**Finding: this is NOT an artifact of one unlucky random seed — starting from $(8,0.02)$ gets
trapped in a spurious local minimum for EVERY single tested seed (7 out of 7)**, though the
specific trap location varies noticeably (sometimes large $\lambda$ with small $\gamma$, sometimes
large $\gamma$ too). This is a genuine, robust, structural feature of the SSE landscape near that
particular starting point for this model — not noise-dependent bad luck. This is an important,
somewhat sobering finding: the book's own suggested starting guess is, on this evidence,
systematically a poor choice for this particular model and optimizer combination, reinforcing —
even more strongly than a single example could — exactly why the book's own advice to use multiple
starting guesses isn't just a theoretical best practice but a practical necessity here.
