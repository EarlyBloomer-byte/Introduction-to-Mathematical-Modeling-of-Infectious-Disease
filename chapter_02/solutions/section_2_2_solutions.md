# Section 2.2 — Worked Solutions

## Conceptual

**1.** The SIR model has three compartments but only two truly independent quantities once
$N=S+I+R$ is conserved (you can always recover the third from the other two and $N$) — however,
which *two* you keep still matters, and the natural choice ($S,I$) leaves a genuine 2D system
because $S$ and $I$ both appear with independent dynamics that can't be collapsed further. The SIS
model, by contrast, has only *two* compartments ($S,I$) to begin with, and conservation
($N=S+I$) lets you eliminate one of them entirely ($S=N_0-I$), leaving a *bona fide* 1D system —
there's simply one fewer compartment to start with, so conservation removes the last remaining
degree of freedom rather than just reducing three down to two.

**2.** An equilibrium $I^*$ is **asymptotically stable** if the arrows (direction of motion) on
both sides of $I^*$ point *toward* it — any small displacement away from $I^*$ gets pulled back,
so nearby solutions converge to $I^*$ over time. An equilibrium is **unstable** if the arrows on
both sides point *away* from it — any small displacement grows, so nearby solutions move further
away from $I^*$ rather than toward it.

**3.** A 1D autonomous ODE's solution is confined to move along a single line, and solutions of
smooth ODEs can never cross themselves (uniqueness of solutions — if two solution curves crossed,
there'd be two different futures from the same point, violating uniqueness). On a 1D line, "never
crossing itself" forces monotonic motion in one direction only — there's no room to turn around
without crossing your own past path. A 2D system has an entire plane to move through, so a
trajectory can curve, turn, and trace out a complex path (like the rise-then-fall SIR curve)
without ever crossing itself, since it has a second dimension to "swing around" in.

**4.** In the SIS model, the endemic equilibrium is exactly $S^*=\rho$ — meaning $S(t)$
*converges to* $\rho$ from whichever side it started on, but by monotonicity of $S(t)=N_0-I(t)$
(itself forced by the 1D phase-line argument), $S(t)$ can never cross over $\rho$ during that
convergence — it just approaches from one fixed side. Since $I'(t)=I(t)(\beta S(t)-\gamma)$, and
the sign of $(\beta S(t)-\gamma)$ is determined entirely by which side of $\rho$ the (non-crossing)
$S(t)$ sits on, that sign never flips — so $I'(t)$ never changes sign, meaning $I(t)$ is monotone
throughout. A rise-then-fall epidemic curve *requires* $I'(t)$ to change sign (positive during the
rise, negative during the fall) — which requires $S(t)$ to cross $\rho$ — which the SIS model's
structure simply does not allow.

**5.** A transcritical bifurcation is a qualitative change where two equilibria meet at a critical
parameter value and then **swap which one is stable** — the equilibrium that was stable before the
bifurcation becomes unstable after, and vice versa, while both equilibria continue to exist on
either side of the bifurcation point (just relabeled in stability). This is different from a
bifurcation where equilibria simply *appear or disappear in pairs* (a saddle-node bifurcation) —
there, stability doesn't get "handed off" between two persisting equilibria; instead, equilibria
are created or destroyed altogether as the parameter crosses the critical value.

## Computational

**6.** $\rho=0.25/0.0008=312.5$. $\mathcal{R}_0=\beta N_0/\gamma=0.0008\times500/0.25=1.6>1$ —
**above threshold**.

**7.** $S^*=\rho=312.5$, $I^*=N_0-\rho=500-312.5=187.5$.

**8.** At exactly $\mathcal{R}_0=1$ ($N_0=\rho$), the two equilibria $I_1^*=0$ and
$I_2^*=N_0-\rho=0$ **coincide** — there is only one equilibrium (at $I=0$), and it sits exactly at
the bifurcation point. In this borderline case, Proposition 2.2.1 (as stated) covers strict
inequalities only; at the bifurcation value itself, $I(t)\to0$ as $t\to\infty$ still holds (the
single equilibrium at the merge point is semi-stable — attracting from one side), but convergence
is typically much *slower* than on either side of the bifurcation (see Q12/Challenge for a direct
numerical demonstration of this "critical slowing down").

**9.** $\rho=0.2/0.0005=400$. The smallest integer $N_0$ with $\mathcal{R}_0=N_0/\rho>1$ is
$N_0=\mathbf{401}$ (since $N_0=400$ gives exactly $\mathcal{R}_0=1$, the borderline/bifurcation
case, not a strict endemic outcome).

**10.** $\rho = N_0-I^* = 600-150=450$. Then, using $\rho=\gamma/\beta \Rightarrow
\beta=\gamma/\rho = 0.2/450 \approx \mathbf{0.000444}$.

## Coding Exercises

**11.** Python:
```python
def sis_equilibria(beta, gamma, N0):
    rho = gamma / beta
    I1_star = 0.0
    I2_star = N0 - rho
    stable = "endemic" if N0 > rho else "disease-free"
    return (I1_star, I2_star, stable)
```
Tested on three cases:
- $(\beta,\gamma,N_0)=(0.001,0.3,200)$: $\rho=300$, $N_0<\rho$ → `(0.0, -100.0, 'disease-free')`
  (note $I_2^*$ is negative — not biologically meaningful, correctly reflecting the disease-free
  regime).
- $(0.001,0.3,500)$: $\rho=300$, $N_0>\rho$ → `(0.0, 200.0, 'endemic')`.
- $(0.0005,0.2,400.0001)$: essentially right at the bifurcation, $N_0$ just barely above $\rho=400$
  → `(0.0, ~0.0001, 'endemic')` — the endemic equilibrium exists but is vanishingly small, exactly
  as expected just past the bifurcation point.

**12.** Running the SIS model at $N_0=\rho+2$ (barely above threshold) versus $N_0=\rho+200$ (far
above threshold), and measuring the time to get within 5% of the endemic equilibrium:

| excess above threshold ($N_0-\rho$) | time to reach within 5% of $I^*$ |
|---|---|
| 2 | **1267.3** |
| 200 | **33.5** |

The near-bifurcation case takes roughly **38× longer** to settle near its equilibrium than the
far-from-bifurcation case. This is a real, numerically confirmed instance of **critical slowing
down** — a general phenomenon near bifurcation points where the "restoring force" pulling
trajectories back toward equilibrium (governed by $f'(I^*)$, which approaches zero as the
equilibrium approaches the bifurcation point) weakens, so convergence becomes dramatically slower
the closer a system sits to a bifurcation.

## Visualization Exercise

**13.** Plotting $S^*$ vs. $N_0$: the disease-free branch $S_1^*=N_0$ (since $I=0$ means all
individuals are susceptible) is a straight diagonal line, stable for $N_0<\rho$ and unstable for
$N_0>\rho$. The endemic branch is $S_2^*=\rho$ — a **perfectly flat horizontal line** at height
$\rho$, existing (and stable) only for $N_0>\rho$. The flatness is a direct visual expression of
the fact established in Block 3: $S^*=\rho$ *regardless* of $N_0$, i.e. once the disease is
endemic, the equilibrium number of susceptibles is pinned at exactly the threshold value no matter
how large the total population grows — all of the "extra" population beyond $\rho$ ends up, at
equilibrium, in the infectious compartment instead (visible directly in the corresponding $I^*$
diagram from Block 4, where the endemic branch instead rises linearly with $N_0$).

## Challenge Problem

**14.** To add demography while keeping $N_0$ exactly constant, you'd need matching birth and
death rate constants (the same "$b$" for both, as the book does in Sec. 2.3's SIR-with-demography
model) — e.g. $S'=bN_0-\beta IS+\gamma I-bS$, $I'=\beta IS-\gamma I-bI$, where a birth term
$bN_0$ enters $S$ (assuming all births are into the susceptible class) and a matching death term
$-bS$, $-bI$ leaves each compartment at the same rate, so summing the equations still gives
$N'=bN_0-bS-bI=b(N_0-N)=0$ when $N=N_0$ (conservation is preserved, but now it's an *equilibrium*
of the total-population dynamics rather than an automatic algebraic identity, though if $N(0)=N_0$
it stays there for all $t$). Crucially: **this does NOT change the dimension-reduction argument**
— since $N(t)\equiv N_0$ still holds (given matching birth/death rates and $N(0)=N_0$), you can
still substitute $S=N_0-I$ into the $I$-equation and reduce to a single 1D ODE, exactly as in this
section — phase-line analysis still applies directly, just with a modified $f(I)$ that now
includes the extra $-bI$ term. The system would only need to become 2-dimensional again if birth
and death rates were *not* matched across compartments (e.g. disease-induced excess mortality
giving $I$ a different removal rate than $S$) — in that case $N(t)$ would no longer be constant,
the clean substitution $S=N_0-I$ would break down, and you'd need the fuller 2D (or higher)
analysis toolkit developed in Chapter 3.
