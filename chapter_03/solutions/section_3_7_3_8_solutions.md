# Sections 3.7–3.8 — Worked Solutions

## Conceptual

**1.** "$I(t)>0$ for all $t$" only requires $I$ to never hit exactly zero — it's fully consistent
with $I(t)\to0$ as $t\to\infty$ (approaching but never touching zero), which most people would
NOT call a persisting, robustly endemic disease. Uniform persistence explicitly rules this out by
requiring $\liminf_{t\to\infty}I(t)>\epsilon_0$ for some FIXED positive $\epsilon_0$ — a genuine
floor the trajectory can never dip below in the long run, not just "never exactly zero." Example:
$I(t)=1/t$ for $t\ge1$ satisfies $I(t)>0$ always, but $\liminf I(t)=0$ — persistent in the weak
sense, but NOT uniformly persistent.

**2.** Because the boundary is exactly where "the disease disappears" (or some other compartment
vanishes) would first manifest — if trajectories starting in the interior can approach the
boundary, disease-related quantities are heading toward zero. By understanding whether the
boundary's own invariant structures (like the disease-free equilibrium $P_0$) attract or repel
nearby interior trajectories, you learn everything needed about whether interior trajectories can
get trapped near "disease disappears" states — without needing to solve the full interior
dynamics directly.

**3.** In a system $x'=f(x)$ with Metzler Jacobian, increasing one coordinate $x_j$ (holding others
fixed) can only ever *help* (increase or leave unchanged) the growth rate of every other coordinate
$x_i$ ($i\ne j$), since $\partial f_i/\partial x_j\ge0$. This is exactly the algebraic
condition that prevents one trajectory's coordinate from "reaching over" and suppressing another's
growth in a way that could let a smaller trajectory (in the componentwise order) overtake a larger
one — the mathematical content of "no leapfrogging" is precisely that cross-derivatives never
point the wrong way.

**4.** A matrix's associated directed graph has an edge $i\to j$ whenever $a_{ij}\ne0$; the matrix
is irreducible exactly when this graph is *strongly connected* (every vertex can reach every other
vertex by following directed edges). A REDUCIBLE matrix, by contrast, has some subset of
coordinates that can be reached from the rest but never "feed back" — the system effectively
splits into a one-way cascade rather than being fully interconnected. Irreducibility strengthens
Perron-Frobenius's conclusion because full interconnectedness prevents the dominant eigenvalue's
eigenvector from being forced to zero on some coordinates (which CAN happen in a reducible system,
where some "downstream" coordinates might not even participate in the dominant growth mode) —
full connectivity is exactly what guarantees every coordinate of the eigenvector is strictly
positive, not just non-negative.

**5.** Strong monotonicity alone guarantees ORDER is preserved (bigger initial conditions stay
bigger) but says nothing about trajectories actually CONVERGING to a single point — without
sublinearity, trajectories could still grow without bound or fail to settle down at all. Strict
sublinearity alone (without monotonicity) would give a kind of "diminishing returns" structure but
without the order-preservation, there'd be no way to sandwich an arbitrary trajectory between
simpler, well-understood ones to pin down its limiting behavior. Both properties working together
are what let the proof technique (bounding trajectories between comparison trajectories that are
forced to converge to the same point by sublinearity) actually work.

## Computational

**6.** Uniform persistence requires $\mathcal{R}_0=\beta/(b+\gamma)>1 \iff \beta>b+\gamma=
0.03+0.15=\mathbf{0.18}$.

**7.** Off-diagonal entries: $3\ge0$ and $1\ge0$ — **yes, Metzler**. Eigenvalues: $-1$ and $-5$
(computed directly), so $s(A)=\max(-1,-5)=\mathbf{-1}<0$ — **stable**.

**8.** Off-diagonal entries: $A_{12}=-3$ (negative!) and $A_{21}=1$. Since $A_{12}<0$, this matrix
is **NOT Metzler** — a single negative off-diagonal entry disqualifies it, regardless of the other
entries' signs.

**9.** **Yes**, a strongly connected directed graph is the exact definition of irreducibility — so
a Metzler matrix with a strongly connected associated graph is automatically irreducible. If the
graph instead had two disconnected "islands" (no directed paths connecting the two groups in both
directions), the matrix would be **reducible** — this is exactly the block-triangular structure
described in the text ($PAP^T=\begin{pmatrix}A_1&0\\A_2&A_3\end{pmatrix}$ for some permutation
$P$), confirmed computationally in Q12 below for a concrete example.

**10.** Trying $x=(1,1)$: $Ax=\begin{pmatrix}-3+1\\2-5\end{pmatrix}=\begin{pmatrix}-2\\-3
\end{pmatrix}$ — both components strictly negative, so $x=(1,1)>0$ works directly, confirming
Theorem 3.8.3's condition (4)/(5) holds for this (confirmed stable, via eigenvalues
$\approx-2.27,-5.73$, both negative) matrix.

## Coding Exercises

**11.** Python:
```python
import numpy as np
from scipy.integrate import solve_ivp

def is_uniformly_persistent_numerically(rhs_func, params, R0_func, n_trials=10,
                                          t_tail=(400,600), threshold=1e-4):
    rng = np.random.default_rng(0)
    R0 = R0_func(params)
    t_eval = np.linspace(*t_tail, 100)
    min_vals = []
    for _ in range(n_trials):
        S0 = rng.uniform(0.001, 0.999)
        I0 = rng.uniform(0.001, 1 - S0 - 0.001) if S0 < 0.999 else 0.001
        sol = solve_ivp(rhs_func, (0, t_tail[1]), [S0, I0], args=params,
                         t_eval=t_eval, rtol=1e-10, atol=1e-10)
        S, I = sol.y
        gap = 1 - S - I
        min_vals.append(min(S.min(), I.min(), gap.min()))
    return (min(min_vals) > threshold), R0, min(min_vals)
```
Tested on Sec. 2.3's model: the $\mathcal{R}_0=2.27>1$ case returns `(True, 2.27, 0.0509)` (all
ten random trials' tails stay above 0.0509, comfortably bounded away from zero); the
$\mathcal{R}_0=0.68<1$ case returns `(False, 0.68, ~7.5e-24)` — essentially zero, correctly
detecting the failure of persistence.

**12.** Python (using `scipy.sparse.csgraph.connected_components`):
```python
import numpy as np
from scipy.sparse.csgraph import connected_components
from scipy.sparse import csr_matrix

def is_metzler(A):
    A = np.asarray(A)
    n = A.shape[0]
    return all(A[i,j] >= 0 for i in range(n) for j in range(n) if i != j)

def is_irreducible(A):
    A = np.asarray(A)
    adj = (np.abs(A) > 1e-12).astype(int)
    np.fill_diagonal(adj, 0)
    n_components, _ = connected_components(csr_matrix(adj), directed=True, connection='strong')
    return n_components == 1
```
Tested on 3 matrices:
- $\begin{pmatrix}-1&2\\3&-1\end{pmatrix}$: Metzler ✓, irreducible ✓ (fully 2-way connected).
- $\begin{pmatrix}-1&2&0\\0&-1&0\\0&3&-1\end{pmatrix}$: Metzler ✓ but **reducible** — vertex 1
  (0-indexed) has no outgoing edges to 0 or 2, so the graph isn't strongly connected (a "downstream"
  coordinate that nothing flows back from — exactly the block-triangular structure from Q9).
- $\begin{pmatrix}-1&-2\\3&-1\end{pmatrix}$: **not Metzler** (negative off-diagonal entry), so
  irreducibility isn't even checked.

## Visualization Exercise

**13.** Sweeping $\beta$ across $\mathcal{R}_0=1$ and plotting the estimated $\liminf I(t)$: for
$\mathcal{R}_0\le1$, the estimated floor sits at essentially exactly 0 (flat along the x-axis).
Right at and above $\mathcal{R}_0=1$, the floor lifts off from 0 — but the transition is
**continuous, not abrupt**: near the threshold the endemic floor starts very small (recall the
"critical slowing down" phenomenon from the Sec. 2.2 exercises — the SIS model's analogous
transcritical bifurcation showed the same gradual liftoff), then grows more steeply further above
threshold. This continuous-but-initially-slow transition is the generic signature of a
transcritical bifurcation, consistent with everything else observed about this model's threshold
behavior throughout Chapters 2–3.

## Challenge Problem

**14.** The $S$-axis ($I=0$): substituting $I=0$ into the model gives $S'=b-bS=b(1-S)$ and
$I'=0$ — so a trajectory starting with $I=0$ stays at $I=0$ forever (confirmed directly: if
$I(0)=0$, then $I'(0)=\beta(0)S-\gamma(0)-b(0)=0$, and by uniqueness of solutions, $I(t)\equiv0$
for all $t$). **The $S$-axis IS positively invariant.** Now consider $S+I=1$ (i.e. $R=0$ in the
full 3-compartment picture): differentiating, $(S+I)'=S'+I'=(b-\beta IS-bS)+(\beta IS-\gamma I-bI)
=b-bS-bI-\gamma I=b(1-S-I)-\gamma I$. On the line $S+I=1$ (so $1-S-I=0$), this becomes
$(S+I)'=-\gamma I$, which is **strictly negative** whenever $I>0$ — meaning a trajectory starting
exactly on the line $S+I=1$ (with $I>0$) immediately moves to $S+I<1$, LEAVING the line. **This
boundary piece is NOT positively invariant** (except at the single point $S=1,I=0$, where $I=0$
makes the derivative zero too). This exactly illustrates the text's warning: different pieces of
$\partial D$ can have genuinely different invariance properties within the very same model — the
$S$-axis traps trajectories, but the $S+I=1$ line does not. Theorem 3.7.1's framework handles this
correctly because it doesn't require the ENTIRE boundary to be invariant — it only needs the
*largest compact invariant set* $M$ *within* the boundary to be identified and analyzed (which, for
this model, turns out to be just the single point $\{P_0\}$, sitting on the doubly-boundary corner
where the always-invariant $S$-axis meets the never-invariant-except-there line $S+I=1$) — a
carefully general enough formulation to handle boundaries that are a patchwork of invariant and
non-invariant pieces, rather than requiring uniform behavior across the whole boundary.
