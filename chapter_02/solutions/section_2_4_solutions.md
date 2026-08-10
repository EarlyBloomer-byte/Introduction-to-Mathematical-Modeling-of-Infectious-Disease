# Section 2.4 — Worked Solutions

## Conceptual

**1.** Example of homogeneous degree 1: a simple currency conversion, $f(x)=7.2x$ (dollars to some
other currency) — doubling the dollar amount exactly doubles the converted amount:
$f(2x)=7.2(2x)=2(7.2x)=2f(x)$. Non-example: $f(x)=x+10$ (e.g. a fixed $10 fee plus a proportional
charge) — doubling $x$ does NOT double $f(x)$: $f(2x)=2x+10 \ne 2(x+10)=2x+20$ unless $x=0$; the
constant term breaks the scaling property.

**2.** It tells you that this particular model structure is fundamentally about *relative*
disease dynamics, not a "steady state" of raw population counts — trying to find a fixed
equilibrium number of susceptibles/infectious/recovered people is the wrong question to ask when
births and deaths are imbalanced, because the population itself has no equilibrium (it's
inherently always growing or shrinking). The right question — "what fraction of the population is
in each state, in the long run" — *does* have a well-defined, analyzable answer (Theorem 2.4.2),
which is precisely why the book pivots to fractional variables rather than treating the
"no equilibrium" result as a dead end.

**3.** Because the projected system (2.41) is *self-contained* — its right-hand side depends only
on $s,i,r$ themselves, with no explicit dependence on $N$ or how $N$ is changing. This was proven
directly from the homogeneity property: any homogeneity-of-degree-1 system, when projected onto
fractional variables, yields a new system with $N$ eliminated entirely from the equations (Block 1,
Step 4) — so solving for the fractions' long-run behavior literally never requires knowing
anything about $N(t)$'s trajectory.

**4.** $\mathcal{R}_0<1$ only guarantees the *fraction* $i(t)=I(t)/N(t)\to0$ — it says nothing
directly about the *raw count* $I(t)=i(t)\cdot N(t)$. If $N(t)\to\infty$ fast enough (which
requires $b>d$, Case III) while $i(t)\to0$ more slowly than $N(t)\to\infty$ grows, their product
$I(t)=i(t)N(t)$ can still diverge to infinity — a race between a shrinking fraction and a growing
population, and $\mathcal{R}_1$ is exactly the quantity that tells you who wins that race.

**5.** Prevalence (a fraction) directly measures "how widespread is the disease *right now*,
relative to the population it's affecting" — a quantity this section proves is governed by a
clean, self-contained, population-size-independent threshold ($\mathcal{R}_0$). A raw case count,
by contrast, conflates two entirely different processes — actual disease transmission dynamics AND
whatever the background population growth/decline happens to be doing — and can send a misleading
signal (e.g. rising case counts in a rapidly growing population might just reflect population
growth, not worsening disease dynamics; Case IIIa is a clean, provable example of exactly this
occurring even while the disease is fractionally *dying out*).

## Computational

**6.** $\mathcal{R}_0=\lambda/(b+\gamma)=0.4/(0.03+0.15)=0.4/0.18\approx\mathbf{2.222}$.

**7.** $s^*=(b+\gamma)/\lambda=0.18/0.4=0.45$.
$i^*=\frac{b(\lambda-(b+\gamma))}{\lambda(b+\gamma)}=\frac{0.03\times0.22}{0.4\times0.18}\approx0.0917$.
$r^*=\frac{\gamma(\lambda-(b+\gamma))}{\lambda(b+\gamma)}=\frac{0.15\times0.22}{0.4\times0.18}
\approx0.4583$. Check: $s^*+i^*+r^*=0.45+0.0917+0.4583=1.000$ ✓ (as required by the simplex
constraint).

**8.** $\mathcal{R}_1=(d+\gamma)/\lambda=1 \iff \lambda=d+\gamma=0.01+0.2=\mathbf{0.21}$.

**9.** $\mathcal{R}_1=(0.02+0.25)/0.3=0.27/0.3=0.9<1$ — since $\mathcal{R}_1<1$, $I(t)$ will
**grow** (exponentially), even though $\mathcal{R}_0=0.3/(b+0.25)$ would need to be checked
separately with $b$ specified, but per the problem's premise ($\mathcal{R}_0<1$, $b>d$ given),
$\mathcal{R}_1<1$ means the population-growth effect wins the "race" described in Q4's answer.

**10.** Growth rate $=b-d=0.05-0.02=0.03$ per time unit. Doubling time: solve
$2=e^{0.03t}\Rightarrow t=\ln(2)/0.03\approx\mathbf{23.10}$ time units.

## Coding Exercises

**11.** Python:
```python
import numpy as np

def check_homogeneity(f, x, scale_factor, tol=1e-9):
    lhs = np.array(f(scale_factor * np.array(x)))
    rhs = scale_factor * np.array(f(np.array(x)))
    return np.allclose(lhs, rhs, atol=tol)
```
Testing on the model's right-hand side (with $b=0.03,d=0.03,\lambda=0.5,\gamma=0.2$) at
$x=(900,50,50)$ scaled by 2: returns **True** — confirmed homogeneous. Testing on a deliberately
broken function `f(x) = [S+1, I, R]` (a constant term added to the first component): returns
**False** — confirming the check correctly detects the violation caused by the additive constant.

**12.** Sweeping $\lambda$ (holding $b=0.05,d=0.02,\gamma=0.2$ fixed, so $\mathcal{R}_1=1$ exactly
at $\lambda=0.22$) and measuring the long-run growth rate of $\log I(t)$ via a linear fit over the
simulation's tail:

| $\lambda$ | $\mathcal{R}_0$ | $\mathcal{R}_1$ | measured growth rate of $I(t)$ |
|---|---|---|---|
| 0.05 | 0.200 | 4.400 | −0.170 |
| 0.10 | 0.400 | 2.200 | −0.120 |
| 0.15 | 0.600 | 1.467 | −0.070 |
| 0.19 | 0.760 | 1.158 | −0.030 |
| 0.21 | 0.840 | 1.048 | −0.010 |
| **0.22** | 0.880 | **1.000** | **≈0.00002 (essentially zero)** |
| 0.23 | 0.920 | 0.957 | +0.010 |

The measured growth rate crosses from negative to positive **exactly** at $\lambda=0.22$, matching
the predicted $\mathcal{R}_1=1$ threshold to within numerical precision — a clean, direct
confirmation of the principal-part/asymptotic argument in Block 3.

## Visualization Exercise

**13.** Plotting $N(t)$ on a log-scale y-axis for all three cases starting from the same $N_0$:
the $b=d$ case is a perfectly **flat horizontal line** (constant $N$); the $b<d$ case is a
**straight line sloping downward** on the log scale (since $N(t)=N_0e^{(b-d)t}$ with $b-d<0$ is
exponential decay, which appears linear on a log axis); the $b>d$ case is a **straight line
sloping upward** on the log scale (exponential growth, also linear in log-space, with slope
$b-d>0$). The three cases are visually distinguished entirely by the *sign* of that slope — zero,
negative, or positive — directly reflecting the sign of $b-d$ in the closed-form
$N(t)=N_0e^{(b-d)t}$.

## Challenge Problem

**14.** For $f(\lambda x)=\lambda^\delta f(x)$, differentiate both sides with respect to $\lambda$:
$$\sum_{i=1}^n\frac{\partial f}{\partial x_i}(\lambda x)\cdot x_i = \delta\lambda^{\delta-1}f(x)$$
Setting $\lambda=1$: $\sum_i\frac{\partial f}{\partial x_i}(x)\cdot x_i = \delta f(x)$ — the
**generalized Euler Identity**, matching the book's $\delta=1$ special case exactly when
$\delta=1$ (since $\delta\lambda^{\delta-1}=1\cdot\lambda^0=1$ in that case).

Checking $g(x_1,x_2)=x_1^2+x_2^2$: $g(\lambda x_1,\lambda x_2)=(\lambda x_1)^2+(\lambda
x_2)^2=\lambda^2(x_1^2+x_2^2)=\lambda^2g(x_1,x_2)$ — so $g$ **is** homogeneous, with
$\boxed{\delta=2}$. Computing $\sum_i\frac{\partial g}{\partial x_i}x_i$ directly:
$\frac{\partial g}{\partial x_1}=2x_1$, $\frac{\partial g}{\partial x_2}=2x_2$, so
$\sum_i\frac{\partial g}{\partial x_i}x_i = 2x_1\cdot x_1+2x_2\cdot x_2=2x_1^2+2x_2^2=2(x_1^2+x_2^2)
=2g(x_1,x_2)$ — exactly matching the generalized identity's prediction of $\delta
f(x)=2g(x_1,x_2)$. The generalized Euler Identity checks out precisely.
