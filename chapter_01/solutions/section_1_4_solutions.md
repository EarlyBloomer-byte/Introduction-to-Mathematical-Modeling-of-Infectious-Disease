# Section 1.4 — Worked Solutions

## Conceptual

**1.** They are the same assumption because one implies the other exactly: if exit is
proportional ($rN$), the fraction remaining is $e^{-rt}$, which is precisely the survival function
of an exponential distribution; conversely, if residence time is exponentially distributed, the
population remaining after time $t$ is $N_0e^{-rt}$, whose derivative gives exactly $-rN(t)$ — a
proportional exit rate. Which framing is "more intuitive" is a matter of taste: the proportional-
rate framing is closer to how a modeler naturally writes an equation; the distribution framing is
closer to how a statistician or someone thinking about individual-level biology would describe it.

**2.** Memorylessness means: given that an individual has *already* remained in a compartment for
some time $s$, the probability they remain for an *additional* time $t$ is exactly the same as if
they had just entered — past duration carries no information about future duration. This makes ODE
modeling convenient because the model only needs to track *how many* individuals are currently in
each compartment, never *how long* each one has already been there — if the past mattered, you'd
need to track each individual's "age since infection," which is exactly what forces the
integro-differential/delay formulations in this section.

**3.** In $\beta IS/(K+I)$, dividing numerator and denominator by $I$ gives
$\beta S / (K/I + 1)$; as $I\to\infty$, $K/I\to 0$, so the expression approaches $\beta S$ — a
constant with respect to $I$. In mass-action $\beta IS$, there is no such denominator term to
"absorb" growth in $I$ — the expression is linear in $I$ and grows without bound as $I\to\infty$.
The saturating form encodes a hard biological limit (an infectious individual can only make so
many contacts per unit time, however many other infectious people exist); mass action has no such
limit built in.

**4.** The book's argument hinges on population *density*, not population size per se. In a rural
setting, as the population grows, towns tend to expand outward, keeping density roughly constant —
so the *rate* an individual can find susceptible contacts, per capita, is roughly independent of
total population size (standard incidence divides by $N$ to reflect this). In a dense city
confined in space, growth increases density directly, so absolute contact opportunities grow with
$N$ — which is exactly what bilinear/mass-action incidence (no division by $N$) captures.

**5.** Incubation period = infection → symptom onset. Latent period = infection → becoming
infectious. Infectious period = the duration of being contagious. A disease where latent period is
*shorter* than incubation period means a host becomes contagious *before* showing any symptoms —
e.g., this is a well-documented pattern for several respiratory viruses. The public-health
significance is severe: symptom-based screening (temperature checks, "stay home if you feel sick")
systematically misses this pre-symptomatic infectious window, since by the time symptoms trigger
isolation, transmission may already have been happening for some time.

## Computational

**6.** Mean $= 1/\gamma = 5 \Rightarrow \gamma = 1/5 = 0.2$.

**7.** At $I=10$: $\frac{0.001\times10\times600}{50+10}=\frac{6}{60}=0.100$.
At $I=50$: $\frac{0.001\times50\times600}{50+50}=\frac{30}{100}=0.300$.
At $I=500$: $\frac{0.001\times500\times600}{50+500}=\frac{300}{550}\approx0.545$.
The saturating maximum is $\beta S = 0.001\times600=0.6$; half of that is $0.3$, which occurs
exactly at $I=50=K$ — this is a general Michaelis–Menten fact: **the half-saturation point always
occurs exactly at $I=K$** (by definition, that's what the constant $K$ means in this functional
form), confirmed here numerically since $I=50$ gave exactly $0.300 = 0.6/2$.

**8.** $N(10) = 1000 \cdot e^{0.01\times10} = 1000\cdot e^{0.1} \approx \mathbf{1105.17}$.

**9.** $(b-d)K = (0.05-0.01)\times2000 = 0.04\times2000 = \mathbf{80}$.

**10.** $\kappa = 1/4 = 0.25$ per day. $\gamma=1/6\approx0.1667$ per day. Total expected time from
infection to recovery $= 4+6=\mathbf{10}$ days (sum of the two independent mean stage durations,
since they occur sequentially).

## Coding Exercises

**11.** Running the windowed-sum DDE simulation at $\omega=3,5,8$ (with $\lambda=0.0005$,
$S_0=550$, $I_0=5$ fixed) gives real, distinct peak values:

| $\omega$ | peak $I(t)$ |
|---|---|
| 3 | 20.11 |
| 5 | 84.25 |
| 8 | 228.61 |

So the DDE's peak $I(t)$ changes *substantially* with $\omega$ — this makes sense once you notice
the exercise's premise needs care: $\omega$ **is** the mean residence time in the fixed-duration
case (there's no separate "shape" parameter to vary independently of the mean, unlike, say, a
gamma distribution). So changing $\omega$ *is* changing the mean infectious period itself, not
holding it fixed — a longer fixed infectious period ($\omega=8$) naturally produces a much larger
epidemic (more total transmission opportunity per infected individual) than a shorter one
($\omega=3$). This is a useful "gotcha": in the fixed-duration case, you cannot vary residence-time
*shape* independently of its *mean*, unlike with more flexible distributions (see the gamma
distribution challenge problem below, where shape and mean genuinely can be separated).

**12.** Python:
```python
def saturating_incidence(I, S, beta, K):
    return beta * I * S / (K + I)

beta, S, K = 0.001, 600, 50
for I in [10, 100, 1000, 100000]:
    ratio = saturating_incidence(I, S, beta, K) / (beta * S)
    print(I, ratio)
# I=10:     ratio=0.167
# I=100:    ratio=0.667
# I=1000:   ratio=0.952
# I=100000: ratio=0.9995
```
The ratio climbs steadily toward 1 as $I$ grows, confirming
$\text{saturating\_incidence}(I,S,\beta,K)/(\beta S)\to 1$ as claimed.

## Visualization Exercise

**13.** Plotting $G(t)=e^{-\gamma t}$ for $\gamma\in\{0.1,0.2,0.5\}$: the curve for $\gamma=0.5$
decays fastest (mean $1/0.5=2$ — **shortest** mean infectious period), the curve for $\gamma=0.1$
decays slowest (mean $1/0.1=10$ — longest), and $\gamma=0.2$ (mean $5$) sits in between. Marking
the mean on each curve: at $t=1/\gamma$, every exponential survival curve has dropped to exactly
$e^{-1}\approx0.368$ — this is a nice universal landmark for eyeballing the mean directly off a
survival-function plot, regardless of which $\gamma$ it belongs to.

## Challenge Problem

**14.** Implementing the two-stage "linear chain trick" (Erlang-2 distribution: $I_1\to I_2$, each
stage with exit rate $2\gamma$, so the *total* mean time spent as "infectious" across both stages
is still $1/\gamma$, same as the plain exponential case) and comparing peak $I=I_1+I_2$ against
the plain exponential and fixed-delay ($\omega=1/\gamma=5$) cases from Block 1, using identical
$\lambda,\gamma,S_0,I_0$ throughout:

| Residence-time model | peak $I(t)$ |
|---|---|
| Exponential (plain ODE, Block 1) | 27.62 |
| **Erlang-2 (this problem)** | **36.72** |
| Fixed delay $\omega=1/\gamma$ (DDE, Block 1) | 84.25 |

**Finding:** the gamma/Erlang-distributed case falls **strictly between** the two extremes, closer
to the exponential end. This matches the underlying intuition precisely: the exponential
distribution has the *most* variability (relatively many individuals recover very quickly, a few
take a very long time) for a given mean, while the fixed-delay distribution has *zero* variability
(everyone takes exactly the same time). The Erlang-2 distribution has intermediate variability
(less spread than exponential, since it's built from two sequential exponential stages, but still
some spread, unlike the fixed-delay case) — and its epidemic curve sits, correspondingly, between
the two. This demonstrates a general principle worth remembering: **reducing variability in the
residence-time distribution (while holding the mean fixed) tends to produce sharper, larger
epidemic peaks** — synchronizing recoveries concentrates the "infectious window" more tightly in
time, giving the disease a more concentrated opportunity to spread before hosts recover.
