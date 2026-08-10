# Section 2.1 — Worked Solutions

## Conceptual

**1.** The tangency $dI/dt|_{I=0}=0$ means: if $I=0$ at some instant, $I$'s rate of change is also
exactly zero at that instant, so $I$ cannot spontaneously become positive — the only way to "enter"
the $I>0$ region is to have started there. Biologically, this reflects that in this simple closed
model, new infections can only come from existing infectious individuals ($\beta IS$ requires
$I>0$ to be nonzero) — there's no other mechanism to create a case. This would **not** remain true
if the model added a term for infectious immigrants (e.g. $+m$ for a constant immigration rate of
infectious individuals into $I$): then $dI/dt|_{I=0}=m>0$, and the vector field would point *into*
the $I>0$ region even when $I=0$ — infection could appear "from nowhere" (from the model's
internal perspective), exactly matching the real-world possibility of imported cases.

**2.** Herd immunity claims that once enough of a population is immune, the *chain of
transmission* breaks — an infectious individual's expected number of new infections drops below 1,
so the epidemic can no longer sustain itself, even though susceptible individuals still exist.
That is exactly what Property 5 shows mathematically: $I(t)\to0$ (transmission chains break) while
$S(\infty)>0$ (untouched individuals remain). Herd immunity is a claim about the epidemic *process
stopping*, not about *everyone eventually getting infected* — those are different claims, and the
math here supports the former, not the latter.

**3.** "Peaks somewhere in the middle" is a vague qualitative statement that could be true of many
different functional shapes. "$I_{max}$ occurs exactly at $S=\rho$" is a *precise, checkable,
model-specific prediction*: it tells you exactly which value of $S$ to look for, it holds exactly
(not just approximately) along every single trajectory regardless of $S_0,I_0$, and it connects a
purely epidemiological quantity ($I_{max}$, relevant to hospital capacity) to a purely
parameter-derived quantity ($\rho=\gamma/\beta$) — a genuinely falsifiable, quantitative claim
rather than a qualitative observation.

**4.** *Predictive*: before an outbreak (or early in one), a public health agency estimates
$\beta,\gamma$ (hence $\rho$) from contact-tracing and clinical data, then uses (2.7) to forecast
$S_\infty$/$R_\infty$ — informing how many hospital beds, vaccine doses, or antivirals to
stockpile. *Inferential*: after an outbreak has run its course, an agency has reliable counts of
$S_0$ (initial population at risk) and $S_\infty$ (from serosurveys or case totals) but poor direct
measurements of the transmission rate $\beta$; equation (2.8) lets them back out $\rho$ (and hence
$\beta=\gamma/\rho$, given a known $\gamma$ from clinical data on infectious duration) — useful for
characterizing how transmissible a *new* pathogen was, after the fact.

**5.** The approximation's accuracy degrades because Step 2 of the derivation Taylor-expands
$e^{-R/\rho}$ only to *second order*, which is only a good approximation when $R/\rho$ stays small.
As $\nu=S_0-\rho$ grows, the epidemic infects more people, so $R(t)$ (and ultimately $R_\infty$)
grows larger relative to $\rho$, making the truncated Taylor series (which drops all
third-order-and-higher terms) increasingly inaccurate — exactly what the numerical comparison table
in the notebook shows (error growing from 0.4% at $\nu=5$ to over 25% at $\nu=400$).

## Computational

**6.** $\rho=\gamma/\beta = 0.15/0.0006 = 250$.

**7.** $\mathcal{R}_0 = S_0/\rho = 800/250 = 3.2 > 1$, so **above threshold** — an epidemic occurs.

**8.** $C = I_0+S_0-\rho\ln S_0 = 10+600-250\ln(600) \approx 10+600-1599.23 \approx \mathbf{-989.23}$.

**9.** $I_{max} = C-\rho+\rho\ln\rho = -989.23-250+250\ln(250) \approx -989.23-250+1383.36
\approx \mathbf{141.13}$.

**10.** $\rho \approx \frac{10000-6000}{\ln(10000)-\ln(6000)} = \frac{4000}{\ln(10/6)} \approx
\frac{4000}{0.5108} \approx \mathbf{7830.5}$. Then $\mathcal{R}_0 \approx 10000/7830.5 \approx
\mathbf{1.28}$ — a comparatively low reproduction number, consistent with a substantial fraction
(60%) of the initial susceptible population remaining uninfected.

## Coding Exercises

**11.** Testing against three different $(S_0,\beta,\gamma,I_0)$ combinations, comparing the
formula's $S_\infty$ to a long-run ODE simulation:

| $S_0$ | $\rho$ | formula $S_\infty$ | simulated $S_\infty$ | difference |
|---|---|---|---|---|
| 600 | 400.0 | 250.31 | 242.30 | 8.01 |
| 1000 | 333.3 | 59.52 | 59.09 | 0.43 |
| 2000 | 1250.0 | 716.04 | 690.33 | 25.71 |

```python
import numpy as np
from scipy.optimize import brentq

def final_size(S0, rho):
    def resid(S_inf):
        return (S0 - S_inf) - rho * (np.log(S0) - np.log(S_inf))
    return brentq(resid, 1e-6, S0 - 1e-6)
```
The differences are larger exactly when $I_0$ is a non-negligible fraction of $S_0$ (e.g. the
first and third rows used $I_0=5$ and $I_0=20$ respectively against smaller/larger $S_0$) — a
direct, numerically confirmed illustration of the "$I_0\approx0$" caveat baked into the final-size
equation's derivation (Block 3, Step 2).

**12.** Holding $\rho=400$ fixed and varying $\nu$:

| $\nu$ | exact $R_\infty$ | approx $2\nu$ | ratio (exact/approx) |
|---|---|---|---|
| 1 | 2.00 | 2 | 0.9992 |
| 10 | 19.84 | 20 | 0.9918 |
| 50 | 96.15 | 100 | 0.9615 |
| 100 | 185.69 | 200 | 0.9284 |
| 250 | 425.83 | 500 | 0.8517 |
| 500 | 768.08 | 1000 | 0.7681 |

The ratio starts near 1 (approximation essentially exact) and **decreases monotonically** as $\nu$
grows, confirming the approximation systematically *overestimates* the true final size once the
excess above threshold is no longer small relative to $\rho$ — worth remembering as a rule of
thumb: the Threshold Theorem approximation is a conservative *overestimate* of severity, not an
underestimate, in this regime.

## Visualization Exercise

**13.** Recreating Figure 2.1 with a **smaller** $\rho$ (say $\rho=100$ instead of $400$, same
$(S_0,I_0)$ starting points as the notebook): the vertical threshold line shifts far to the left,
so essentially all the example trajectories now start well above threshold, and their peaks
($I_{max}$, occurring at $S=\rho=100$) are reached much earlier (at much smaller $S$) and are
noticeably **higher** — since a smaller $\rho$ means a smaller "cost" of getting sick before the
population's susceptible pool depletes below threshold, letting the epidemic run further before
turning around. With a **larger** $\rho$ (say $\rho=700$), some of the same starting points now
fall *below* threshold entirely (their curves never rise, only decline from $I_0$) — since a larger
$\rho$ means fewer initial conditions clear the higher bar needed to trigger an outbreak at all.
In short: changing $\rho$ alone shifts *where* the family's shared peak-line sits, and can flip
individual trajectories between "epidemic" and "no epidemic" without changing $S_0,I_0$ at all.

## Challenge Problem

**14.** A third-order Taylor expansion would be
$e^{-R/\rho}\approx1-\frac{R}{\rho}+\frac{R^2}{2\rho^2}-\frac{R^3}{6\rho^3}$, giving a **cubic**
(rather than quadratic) ODE for $R(t)$ after substitution into (2.13). In general, cubic
(and higher) polynomial ODEs of this form do **not** have solutions expressible in elementary
functions (no closed-form "cubic analogue" of the $\tanh$ solution exists in general) — they would
typically require elliptic functions or purely numerical solution. You *would* expect the resulting
approximation of $R_\infty$ to be more accurate (each additional Taylor term reduces the truncation
error, at least for R/ρ not too large), but the practical cost is steep: losing the clean, portable,
easily-communicated closed-form $\tanh$ solution and its simple $R_\infty\approx2\nu$ takeaway. This
is likely exactly why the book (and the original 1927 paper) stopped at second order — the
second-order approximation is the *highest* order that still yields a fully closed-form,
easy-to-state, and (crucially) *historically enormously influential* result; trading a small amount
of extra accuracy for the loss of that simplicity and communicability would defeat the purpose of
deriving a memorable threshold theorem in the first place. This is a recurring theme across applied
mathematics: the "best" model is not always the most accurate one, but the one that best balances
accuracy against interpretability and usability — the same tension raised all the way back in
Sec. 1.1's discussion of the realism/tractability trade-off.
