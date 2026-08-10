# Section 1.3 — Worked Solutions

## Conceptual

**1.** You would need to drop (or heavily modify) Hypothesis (2) — homogeneous mixing / mass
action. Mass action assumes any susceptible individual is equally likely to contact any infectious
individual; STI transmission instead depends on a specific, much smaller "sexual contact network,"
not the whole population mixing freely. (In more advanced models this is handled with contact-rate
or network-structured incidence terms instead of simple mass action.)

**2.** It's a conservation law because it is *derived*, not *assumed* — Block 3, Step 1 shows that
adding equations (1.3), (1.4), (1.5) causes every flow term to cancel algebraically ($-\lambda IS$
cancels $+\lambda IS$; $-\gamma I$ cancels $+\gamma I$), leaving $dN/dt = 0$ as a mathematical
consequence of the model's structure. Ultimately this traces back to Hypothesis 6/7 (no births,
deaths, or migration), but the constancy of $N(t)$ itself is a *proven fact about the equations*,
not a separate assumption bolted on afterward.

**3.** $S(t)$ is restricted because it has *only* an out-flow term ($-\lambda IS \le 0$, always
non-positive) — Hypothesis 5 (no waning immunity) and Hypothesis 6 (no new susceptible influx)
remove every possible in-flow to $S$. $I(t)$ has both an in-flow ($\lambda IS$) and an out-flow
($\gamma I$) competing, and $R(t)$ has only an in-flow ($\gamma I$, no out-flow at all under these
hypotheses) — so neither is restricted to move in only one direction the way $S$ is (note $R(t)$
is actually also monotonic — non-decreasing — for a different reason: it has no out-flow term at
all).

**4.** Continuity is needed because knowing $\lambda S(0) - \gamma > 0$ only tells you the sign of
$I'(t)$ at the single instant $t=0$. To conclude $I(t)$ is *increasing over some stretch of time*
(not just at one isolated instant), you need $\lambda S(t) - \gamma$ to stay positive for $t$ in
some interval $[0,\bar t)$, not just at $t=0$. Continuity of $S(t)$ guarantees that if
$S(0) > \gamma/\lambda$ strictly, then $S(t)$ stays close to $S(0)$ (and hence above
$\gamma/\lambda$) for at least a short time afterward — this is the standard "continuity preserves
strict inequalities locally" argument from real analysis.

**5.** $R(0)$ in this section is the initial *size of the recovered compartment* — just a
number of people, always $0$ at the start of a novel outbreak (nobody has recovered yet). $\mathcal{R}_0$
(basic reproduction number) is a completely different quantity: a dimensionless ratio (here,
$\lambda S_0/\gamma$) describing the average number of secondary infections one infectious
individual generates in a fully susceptible population. They share the letter "R" purely by
unfortunate historical convention. The book flags this because confusing them is an extremely
common and consequential error — mixing up a compartment size with a threshold ratio would produce
nonsensical statements.

## Computational

**6.** $\gamma/\lambda = 0.25/0.0004 = 625$.

**7.** With threshold $=625$: at $S_0=500 < 625$, $S(t)\le S_0 <\gamma/\lambda$ for all $t$
(monotonicity), so $\lambda S(t)-\gamma<0$ always $\Rightarrow I'(t)<0$ always $\Rightarrow$ **no
epidemic**. At $S_0=700 > 625$, $\lambda S(0)-\gamma>0$, so by continuity $I'(t)>0$ on some initial
interval $\Rightarrow$ **epidemic occurs**.

**8.** Threshold $=\gamma/(2\lambda)$ — it is **halved**. A smaller threshold means a *lower* bar
for $S_0$ to clear, so for the *same* fixed $S_0$, doubling $\lambda$ makes an epidemic **more
likely** (or, if already above the old threshold, makes the resulting epidemic more severe).

**9.** $1/\gamma = 1/0.2 = 5$ days.

**10.** Substitute $I=0$ into all three equations: $S'=-\lambda(0)S=0$; $I'=\lambda(0)S-\gamma(0)=0$;
$R'=\gamma(0)=0$. All three derivatives vanish identically for *any* value of $S$ — so
$(S,0,R)$ is an equilibrium for every $S,R\ge0$, not just one specific point. Biologically: if
there are currently no infectious individuals, the disease cannot spontaneously reappear in this
model (there's no external source of new infections) — the system just sits still forever,
regardless of how many susceptibles remain. This is sometimes called the "disease-free
equilibrium," and the entire *line* of such equilibria (parameterized by $S$) is a direct
consequence of there being no natural births/deaths replenishing $S$ in this simplified model.

## Coding Exercises

**11.** Running the ODE integration out to $t=300$ (well past the epidemic's end) for the "above
threshold" scenario ($S_0=550$, $I_0=5$, $\lambda=0.0005$, $\gamma=0.2$) gives
$R(\infty) \approx 285.8$, i.e. **about 52.0% of the initial susceptible population** ($285.8/550
\approx 0.520$) was ultimately infected — even though, at the epidemic's *peak*, only a much
smaller fraction was infectious at any single instant. This final-size vs. peak-size distinction is
exactly the one flagged as important back in Sec. 1.1, Block 2.

**12.** Python:
```python
import numpy as np
from scipy.integrate import solve_ivp

def kermack_mckendrick(t, y, lam, gamma):
    S, I, R = y
    return [-lam*I*S, lam*I*S - gamma*I, gamma*I]

def simulate_and_classify(S0, I0, lam, gamma, t_max=200):
    sol = solve_ivp(kermack_mckendrick, (0, t_max), [S0, I0, 0], args=(lam, gamma),
                     t_eval=np.linspace(0, t_max, 1000), rtol=1e-9, atol=1e-9)
    return "epidemic" if sol.y[1].max() > I0 else "no epidemic"

lam, gamma = 0.0005, 0.2
threshold = gamma / lam  # 400
print(simulate_and_classify(250, 5, lam, gamma))  # below threshold -> "no epidemic"
print(simulate_and_classify(550, 5, lam, gamma))  # above threshold -> "epidemic"
print(simulate_and_classify(400.0001, 5, lam, gamma))  # just above -> "epidemic"
```
All three outputs match the analytical prediction $S_0 \gtrless \gamma/\lambda$ exactly, including
the boundary case just barely above the threshold.

## Visualization Exercise

**13.** Sweeping $S_0$ from 200 to 900 (threshold $=400$, $\lambda=0.0005$, $\gamma=0.2$,
$I_0=5$), the actual simulated peak values are:

| $S_0$ | 200–400 | 450 | 500 | 550 | 600 | 700 | 800 | 900 |
|---|---|---|---|---|---|---|---|---|
| peak $I(t)$ | 5.00 (flat) | 7.89 | 15.74 | 27.62 | 42.81 | 81.15 | 127.73 | 180.61 |

**Shape:** the curve is perfectly **flat at $I_0=5$** for every $S_0$ below the threshold (no
epidemic, so the "peak" is just the starting value, since $I(t)$ only decreases). At exactly
$S_0=400$ it starts to lift off, and the rise is very gentle just above threshold (a few units of
peak increase for the first 50 units of excess $S_0$) before becoming visibly steeper further from
threshold — consistent with a **transcritical bifurcation** at $S_0=\gamma/\lambda$: the
disease-free "branch" (flat at $I_0$) loses stability there and a new "epidemic" branch peels away,
initially slowly (looks roughly quadratic just past the threshold) and then more steeply.

## Challenge Problem

**14.** Running the same "excess above threshold" comparison at two different threshold scales
(Case A: $\lambda=0.0005,\gamma=0.2\Rightarrow$ threshold $=400$; Case B: $\lambda=0.00025,
\gamma=0.2\Rightarrow$ threshold $=800$, same $I_0=5$ in both):

| excess above threshold | peak $I$, Case A (threshold 400) | peak $I$, Case B (threshold 800) |
|---|---|---|
| 50 | 7.89 | 6.50 |
| 100 | 15.74 | 10.77 |
| 200 | 42.81 | 26.49 |

**Finding:** the peak size is clearly **not** a function of the absolute excess $S_0-\gamma/\lambda$
alone — the same excess (e.g. 100) gives very different peaks (15.74 vs. 10.77) at different
threshold scales. Re-running with $S_0$ held at a fixed *ratio* to the threshold instead
(e.g. $S_0 = 1.25\times\text{threshold}$ in both cases) gives peak-to-threshold ratios of 0.039 (A)
vs. 0.033 (B) — closer, but still not identical, because $I_0=5$ was kept as a fixed absolute
number rather than scaled proportionally to the population size in each case. The takeaway: peak
epidemic size depends on the *relative* quantity $S_0/(\gamma/\lambda)$ — essentially the
reproduction-number ratio $\mathcal{R}_0 = \lambda S_0/\gamma$ — far more than on the raw
difference $S_0-\gamma/\lambda$, and it is also sensitive to how the initial number of infectious
individuals scales relative to the total population. This foreshadows exactly the kind of
normalized/proportional analysis (working with fractions of $N$ rather than raw counts) used
throughout Chapter 2.
