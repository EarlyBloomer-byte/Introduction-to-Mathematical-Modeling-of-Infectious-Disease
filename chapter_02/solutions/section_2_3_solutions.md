# Section 2.3 — Worked Solutions

## Conceptual

**1.** Adding demography means infectious individuals now leave the $I$ compartment via *two*
independent routes: recovery (rate $\gamma$) and death (rate $b$, since death removes individuals
from every compartment equally in this model). The *total* rate individuals leave $I$ is therefore
$\gamma+b$, not just $\gamma$ — so the "effective infectious period" is $1/(\gamma+b)$, shorter
than the pure recovery-driven $1/\gamma$ from Sec. 2.1/2.2. Since $\mathcal{R}_0$ is (transmission
rate) × (effective infectious period), the denominator correctly becomes $b+\gamma$ instead of
just $\gamma$.

**2.** A triangular matrix's eigenvalues are always exactly its diagonal entries — this is a basic
linear algebra fact (the characteristic polynomial of a triangular matrix factors immediately into
$(\lambda-a_{11})(\lambda-a_{22})\cdots$, since the determinant of a triangular matrix minus
$\lambda I$ is just the product of its diagonal entries minus $\lambda$). $J(P^*)$ has no such
special triangular structure — both diagonal AND off-diagonal entries are generally nonzero and
depend on $S^*,I^*$, so its eigenvalues require solving a genuine quadratic characteristic
equation (or, as the book does, using the more efficient Routh–Hurwitz trace/determinant shortcut
instead of solving for eigenvalues explicitly).

**3.** "Hyperbolic" means every eigenvalue of the Jacobian has strictly nonzero real part; a
non-hyperbolic equilibrium has at least one eigenvalue with real part *exactly* zero. Linearization
theory (specifically, the Hartman–Grobman theorem, which underlies why linearization is valid at
all) only guarantees the linearized system accurately captures local behavior when the equilibrium
is hyperbolic — with a zero eigenvalue, the linear approximation's prediction ("neither growing nor
decaying" along that eigendirection) is exactly balanced on a knife's edge, and the *nonlinear*
terms (ignored by linearization) become decisive in determining the true local behavior — which is
exactly why a different tool (Lyapunov functions) is needed at $\mathcal{R}_0=1$.

**4.** A Lyapunov function proves *where trajectories end up* without ever computing *what the
trajectories actually look like* along the way — it sidesteps solving the differential equations
entirely. This is fundamentally easier because checking a single inequality ($\dot L\le0$
everywhere) is a algebra/calculus problem, while solving a nonlinear ODE system explicitly is
generally impossible in closed form — the Lyapunov approach converts an intractable "solve the
equations" problem into a tractable "verify one inequality" problem.

**5.** If periodic orbits could exist, a trajectory might get trapped circling forever around one
of them instead of converging to $P^*$ — the disease level would oscillate indefinitely rather
than settling to a fixed endemic level. The Poincaré–Bendixson theorem's guarantee ("any bounded 2D
trajectory converges to either an equilibrium or a periodic orbit") is only useful for concluding
convergence *to the equilibrium* if periodic orbits have been *ruled out* first — otherwise the
theorem would leave open the possibility of never-ending oscillation, and the "global stability of
$P^*$" conclusion simply wouldn't follow.

## Computational

**6.** $\mathcal{R}_0=0.3/(0.01+0.15)=0.3/0.16=1.875>1$ — **yes**, $P^*$ is in the feasible region.

**7.** $S^*=(b+\gamma)/\beta=0.16/0.3\approx0.5333$.
$I^*=\frac{b[\beta-(b+\gamma)]}{\beta(b+\gamma)}=\frac{0.01\times(0.3-0.16)}{0.3\times0.16}=
\frac{0.0014}{0.048}\approx\mathbf{0.02917}$.

**8.** $\text{tr}(J(P^*))=-b/S^*=-0.01/0.5333\approx\mathbf{-0.01875}$ (negative, as required).
$\det(J(P^*))=\beta I^*S^*=0.3\times0.02917\times0.5333\approx\mathbf{0.004667}$ (positive, as
required). Both Routh–Hurwitz conditions ($\text{tr}<0$, $\det>0$) hold, confirming $P^*$ is
locally asymptotically stable.

**9.** Bifurcation occurs at $\mathcal{R}_0=1 \iff \beta=b+\gamma=0.02+0.2=\mathbf{0.22}$.

**10.** As $b\to0$: $\mathcal{R}_0=\beta/(b+\gamma)\to\beta/\gamma$ — exactly matching the
threshold quantity $\mathcal{R}_0=\beta N_0/\gamma$ from Sec. 2.1/2.2 with $N_0=1$ (recall this
model normalizes total population to 1). This is a reassuring consistency check: turning off
demography entirely should recover the earlier, simpler models exactly, and it does.

## Coding Exercises

**11.** Python:
```python
import numpy as np

def local_stability_P0(b, beta, gamma):
    J = np.array([[-b, -beta], [0, beta - (b + gamma)]])
    eig = np.linalg.eigvals(J)
    return bool(np.all(eig.real < 0))
```
Tested against 5 parameter combinations, comparing to the analytical prediction
$\mathcal{R}_0<1$:

| $b$ | $\beta$ | $\gamma$ | $\mathcal{R}_0$ | predicted stable | actual (eigvals) | match |
|---|---|---|---|---|---|---|
| 0.02 | 0.10 | 0.20 | 0.455 | True | True | ✓ |
| 0.02 | 0.30 | 0.20 | 1.364 | False | False | ✓ |
| 0.01 | 0.05 | 0.10 | 0.455 | True | True | ✓ |
| 0.05 | 0.50 | 0.30 | 1.429 | False | False | ✓ |
| 0.02 | 0.2196 | 0.20 | 0.998 | True | True | ✓ |

Every case matches exactly, including the near-bifurcation case ($\mathcal{R}_0=0.998$, very close
to 1 but still just below).

**12.** Starting at $(S_0,I_0)=(0.999,0.001)$ — extremely close to $P_0=(1,0)$ — with
$\mathcal{R}_0>1$ ($b=0.02,\beta=0.5,\gamma=0.2$), integrating out to $t=600$ gives a final state
of approximately $(0.440,0.0509)$, which matches the endemic equilibrium
$P^*=(0.440,\ 0.0509)$ almost exactly. So even a trajectory starting *extremely* close to the
disease-free equilibrium still eventually diverges away and converges to $P^*$ instead — direct,
concrete numerical confirmation that $P_0$ is genuinely unstable (a saddle) when
$\mathcal{R}_0>1$: proximity to an unstable equilibrium is not enough to keep a trajectory there.

## Visualization Exercise

**13.** Sweeping $\beta$ (holding $b=0.02,\gamma=0.2$ fixed) and plotting the non-trivial
eigenvalue $\lambda_2=\beta-(b+\gamma)$ of $J(P_0)$ against $\mathcal{R}_0=\beta/(b+\gamma)$: the
numerical sweep finds the zero-crossing at $\mathcal{R}_0\approx0.993$ on a moderately coarse
50-point grid — consistent with (and converging toward, on a finer grid) the exact analytical
crossing at $\mathcal{R}_0=1$ (equivalently $\beta=b+\gamma=0.22$), confirming Block 2's claim that
$\lambda_2$ changes sign precisely at the bifurcation value. (The other eigenvalue, $\lambda_1=-b$,
never crosses zero — it stays at $-0.02$ regardless of $\beta$, confirming it plays no role in the
bifurcation.)

## Challenge Problem

**14.** Without any multiplier ($\alpha=1$), direct computation gives
$\frac{\partial P}{\partial S}+\frac{\partial Q}{\partial I} = \beta(S-I)-2b-\gamma$ — this
expression is **not** sign-definite: since $S$ and $I$ can each range independently within the
feasible region, $\beta(S-I)$ can be positive (when $S>I$) or negative (when $S<I$), so this simple
sum can be positive in some parts of the region and negative in others — it does *not* rule out
periodic orbits by itself. With the Dulac multiplier $\alpha=1/I$, the expression simplifies to
exactly $-\beta-b/I$ — manifestly negative *everywhere* in the interior of the feasible region
(where $I>0$), regardless of the specific values of $S$ or $I$. This is precisely what the
multiplier accomplishes: it's not just an arbitrary trick, but a deliberately chosen rescaling that
converts a sign-indefinite expression into a sign-definite one, which is exactly the condition the
Bendixson–Dulac criterion requires to rule out periodic orbits. Finding a useful Dulac multiplier
is generally more art than algorithm — but $1/I$ (or $1/(SI)$, or similar) is a common, often
effective choice for compartmental epidemic models specifically because it tends to cancel the
problematic mixed $\beta IS$-type terms that make the unmultiplied expression sign-indefinite.
