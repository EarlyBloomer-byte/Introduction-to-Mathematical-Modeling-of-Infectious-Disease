# Section 1.1 — Worked Solutions

## Conceptual

**1.** An outbreak is any rise in case counts above the usual baseline in a short time — it's the
broader, more general term. An epidemic is a *fast, widespread* case of that rise. So every
epidemic is also technically an outbreak (it started as one), but not every outbreak becomes an
epidemic — most stay small and local. Outbreak ⊇ epidemic, not the other way around.

**2.** Example: seasonal influenza-like circulation in a large city, or malaria in parts of
sub-Saharan Africa. For a disease to stay endemic rather than dying out, the rate at which new
susceptible individuals appear (births, waning immunity, migration) must roughly balance the rate
at which infections are "used up" (recovery, death, or full immunity) — enough fresh susceptibles
must keep entering the population to sustain a low, steady trickle of new infections indefinitely,
rather than either exploding (epidemic) or running out of fuel (dying out).

**3.**
- *Scale mismatch*: a disease affecting millions across a continent can't be reproduced at
  bench-top scale in a lab.
- *Ethics/feasibility*: you cannot ethically infect thousands of people with a real pathogen just
  to observe spread.
- *Incomplete data*: asymptomatic carriers are invisible to surveillance systems, so case counts
  systematically undercount transmission.
- *Single-sample problem*: a real outbreak is one enormous, un-repeatable "sample," which breaks
  the large-independent-samples assumption most classical statistics relies on.

**4.** Treating it as linear implies you'd build one model, validate it once, and be done. But
Step 6 explicitly feeds back into Step 1: validation against real data (Step 5) routinely reveals
that some assumptions were wrong or too simple, which forces you to revise them and rebuild.
Since biological knowledge, data quality, and available mathematical theory are all limited (the
text's own three caveats), a single pass essentially never produces a finished, fully validated
model — refinement is the normal case, not the exception.

**5.** The text's own reasoning (Block 4) directly contradicts this: more realistic models have
more parameters, and with a fixed, often small amount of disease surveillance data, more
parameters are *harder* to pin down reliably — leading to *greater* uncertainty in predictions,
not less. The bootstrap demonstration in the notebook shows this numerically: a 7-parameter curve
fit to 8 data points swings wildly under resampling, while a 2-parameter line stays stable. "Closer
to the truth in principle" doesn't mean "better supported by the data you actually have."

## Computational

**6.** By hand: `above_baseline=True` → not "no event". Check `crosses_continents` next: it's
`True` → return `"pandemic"` immediately (the function never even needs to look at
`persists_long_term` or `fast_widespread`, because `crosses_continents` is checked before them).
Code confirms: `classify_disease_event(True, False, True, True) == "pandemic"`.

**7.** $\binom{50}{2} = \frac{50 \times 49}{2} = 1{,}225$ pairs.
$\binom{5000}{2} = \frac{5000 \times 4999}{2} = 12{,}497{,}500$ pairs.
Ratio of pair counts: $12{,}497{,}500 / 1{,}225 \approx 10{,}202.04$.
Ratio of population sizes: $5000/50 = 100$.
These are **not** equal — the pair count grows roughly with the *square* of the population size
(since $n(n-1)/2 \approx n^2/2$ for large $n$), so a 100× increase in population gives
roughly a $100^2 = 10{,}000$× increase in potential contact pairs — matching the
$\approx 10{,}202$ we computed (the small difference from exactly 10,000 comes from the $-1$ term,
which matters less as $n$ grows).

**8.** With exactly 8 data points and a degree-7 polynomial (which has 8 free coefficients), the
polynomial can be made to pass through *every single point exactly* — a unique interpolating
polynomial exists (assuming distinct $x$-values). This isn't "fitting" in any predictive sense
at all — it's pure interpolation with zero residual error and, typically, wild oscillation between
the data points (Runge's phenomenon). It represents the absolute extreme of the realism/tractability
trade-off: the model looks "perfect" on the training data and is essentially guaranteed to
generalize terribly.

**9.** By hand: $N(15) = 5 \cdot e^{0.3 \times 15} = 5 \cdot e^{4.5}$. Since $e^{4.5} \approx
90.017$, $N(15) \approx 5 \times 90.017 \approx 450.09$. This matches the
`deterministic_final` value computed in the verification code cell (which uses the exact same
formula), confirming the closed-form and the code agree.

**10.** The coefficient of variation should shrink even further, getting very close to zero. The
underlying reason (from Block 5's intuition) is a law-of-large-numbers effect: as the starting
population $N_0$ grows, individual-level randomness gets averaged over more and more independent
"individuals," so the *relative* size of random fluctuations shrinks — even though the *absolute*
fluctuations may still grow. Concretely, the coefficient of variation scales roughly like
$1/\sqrt{N_0}$ for this kind of process (verified numerically in Q14 below), so multiplying $N_0$
by 10,000 (from 500 to 5,000,000) should shrink the CV by roughly a factor of 100.

## Coding Exercises

**11.** Python:
```python
def classify_disease_event_v2(above_baseline, fast_widespread, persists_long_term,
                                crosses_countries, crosses_continents):
    if not above_baseline:
        return "no event"
    if crosses_continents:
        return "pandemic"
    if crosses_countries and fast_widespread:
        return "regional epidemic"
    if persists_long_term:
        return "endemic"
    if fast_widespread:
        return "epidemic"
    return "outbreak"
```
R (mirrors the same branching order):
```r
classify_disease_event_v2 <- function(above_baseline, fast_widespread, persists_long_term,
                                       crosses_countries, crosses_continents) {
  if (!above_baseline) return("no event")
  if (crosses_continents) return("pandemic")
  if (crosses_countries && fast_widespread) return("regional epidemic")
  if (persists_long_term) return("endemic")
  if (fast_widespread) return("epidemic")
  return("outbreak")
}
```
The flow diagram would need one new decision node ("Crosses countries?") inserted between the
"Crosses continents?" node and the "Spreads fast?" node, with a new terminal box "REGIONAL
EPIDEMIC" branching off it.

**12.** Python:
```python
def severity_summary(cases_over_time):
    import numpy as np
    arr = np.asarray(cases_over_time)
    return {
        "total_cases": int(arr.sum()),
        "peak_daily_cases": int(arr.max()),
        "peak_day_index": int(arr.argmax()),
    }

test_cases = [3, 5, 12, 40, 85, 60, 30, 10, 4]
print(severity_summary(test_cases))
# -> {'total_cases': 249, 'peak_daily_cases': 85, 'peak_day_index': 4}
```
`total_cases` answers question (1a) — total number who may need medical care. `peak_daily_cases`
answers question (1b) — the maximum number infected at any given time (relevant to hospital
capacity). `peak_day_index` contributes to question (2) — when the epidemic will peak.

## Visualization Exercise

**13.** Looping the Block 4 bootstrap procedure over polynomial degree 1 through 6 (2 through 7
parameters) on the *same* 8-point dataset gives (actual numbers from running the experiment):

| degree | parameters | avg. bootstrap spread |
|---|---|---|
| 1 | 2 | 0.487 |
| 2 | 3 | 1.282 |
| 3 | 4 | 2.699 |
| 4 | 5 | 4.712 |
| 5 | 6 | 9.643 |
| 6 | 7 | 9.966 |

The relationship is **monotonically increasing** — uncertainty never goes back down as complexity
grows. It's roughly gradual up to degree 3–4, then rises sharply between degree 4 and 5 (from
~4.7 to ~9.6, more than doubling), before nearly leveling off approaching the
fully-interpolating limit at degree 7 (8 parameters for 8 points, see Q8). A simple line plot of
`degree` (x-axis) vs. `spread` (y-axis) makes this sharp knee clearly visible — that knee is the
practical "don't go past this complexity with this little data" warning the text is making
qualitatively.

## Challenge Problem

**14.** Design: for a grid of candidate $N_0$ values, simulate the birth process many times (e.g.
300–400 repetitions), compute the coefficient of variation (std/mean of the final population at
$t_{max}$) for each $N_0$, and find the smallest $N_0$ where CV drops below 0.05.

Running this experiment for $r = 0.3$, $t_{max} = 15$ gives (actual simulation output):

| $N_0$ | CV |
|---|---|
| 50 | 0.138 |
| 100 | 0.096 |
| 200 | 0.070 |
| 300 | 0.057 |
| 400 | 0.052 |
| 500 | 0.045 |
| 700 | 0.039 |
| 1000 | 0.031 |

So the CV crosses below 0.05 somewhere between $N_0 = 400$ and $N_0 = 500$ — call the threshold
**$N_0 \approx 450$** for this particular growth rate.

The data closely follows a $1/\sqrt{N_0}$ scaling law: e.g.
$0.138 / \sqrt{500/50} \approx 0.138/3.16 \approx 0.0437$, close to the observed CV of 0.045 at
$N_0=500$. This scaling is a general feature of this class of stochastic process (variance grows
linearly in $N_0$ while the mean also grows linearly, but relative fluctuation still shrinks like
$1/\sqrt{N_0}$), not specific to $r=0.3$. Changing $r$ changes *how fast* the population grows
(and slightly changes the multiplicative constant in front of the $1/\sqrt{N_0}$ term, since faster
growth means less "time" for randomness to average out before $t_{max}$), but it does **not**
change the qualitative $1/\sqrt{N_0}$ shrinkage pattern — the threshold $N_0$ would shift somewhat
for a different $r$, but the same asymptotic law governs it.
