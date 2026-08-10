# Section 2.5 — Worked Solutions

## Conceptual

**1.** Every earlier model in this chapter had disease transmission happening *within* a single
homogeneously-mixing population (human-to-human contact). Malaria's parasite cannot transmit
directly between humans at all — it strictly requires passing through a mosquito as an
intermediate host. This means the model *must* track two separate, distinct populations (with
their own separate compartment counts, birth/death rates, etc.) and describe transmission as
crossing *between* them rather than within one — a structurally different kind of model, not just
a variation on the same S/I/R template.

**2.** $b_1$ (mosquito→human transmission probability) and $b_2$ (human→mosquito transmission
probability) describe transmission happening at two different biological "interfaces" — human
immune/physiological response to a mosquito bite injecting parasites is a completely different
biological process from a mosquito's gut/salivary gland successfully picking up and later
transmitting parasites from a blood meal. There's no a priori reason these two very different
biological processes should have the same probability of success, so the model correctly keeps
them as two independent parameters.

**3.** The total number of "bite events" happening per unit time must be internally consistent
regardless of which side you count them from: counting bites "per mosquito" ($a$, summed over all
$V$ mosquitoes) gives $aV$ total bites; counting bites "per human" ($\tilde a$, summed over all
$H$ humans) gives $\tilde aH$ total bites. Since both are just two different ways of counting the
*same* underlying set of biting events, they must be equal: $aV=\tilde aH$.

**4.** In a single-population model, "who gets infected next" and "who infected them" both refer
to the *same* population — the chain is one step. In malaria, a human case doesn't directly cause
another human case; it causes mosquito infections, which *then* cause the next round of human
infections. So counting "how many onward infections does one case eventually cause" requires
following the chain through *two* distinct stages (human→mosquito, then mosquito→human) before
arriving back at a comparable quantity (new human cases) — this is precisely why the heuristic
$\mathcal{R}_0$ derivation needed two sequential steps (Block 2, Steps 1–4) rather than one.

**5.** "Homogeneous of degree 1" is an *exact equality* under scaling: $f(\lambda x)=\lambda f(x)$
for all $\lambda>0$. "Strictly sublinear" is a *strict inequality*: $F(\lambda x)>\lambda F(x)$ for
$0<\lambda<1$. The original 4D system (2.48) is exactly homogeneous because every term is either a
simple linear term (like $\mu_hH$, $-\mu_hS_h$) or a bilinear/quotient term where the total
population $H$ or $V$ appears in exactly the right power to preserve the scaling exactly. Once
reduced to the $(x,y)$ prevalence system (2.50), though, new terms like $(1-x)$ and $(1-y)$ appear
(from the $s_h=1-i_h$ substitution) — and $(1-\lambda x)$ is NOT simply $\lambda$ times $(1-x)$;
it's *strictly greater* than $\lambda(1-x)$ for $0<\lambda<1,\ x>0$ (direct algebra:
$1-\lambda x > \lambda - \lambda x = \lambda(1-x)$ since $1>\lambda$). This "saturation" term is
exactly what turns exact homogeneity into strict sublinearity upon reduction.

## Computational

**6.** $\mathcal{R}_0=\frac{a^2mb_1b_2}{\gamma_1\gamma_2}=\frac{0.25^2\times3\times0.4\times0.5}
{0.12\times0.08}=\frac{0.0625\times3\times0.2}{0.0096}=\frac{0.0375}{0.0096}\approx\mathbf{3.906}$.

**7.** $\mathcal{R}_0\approx3.906>1$ — **endemic**.

**8.** Halving $m$ to $1.5$: $\mathcal{R}_0=\frac{0.25^2\times1.5\times0.4\times0.5}{0.0096}
\approx\mathbf{1.953}$ — still above 1 (still endemic), roughly half of Q6's value (as expected,
since $\mathcal{R}_0$ is exactly linear in $m$).

**9.** Solving $\frac{a^2\times3\times0.4\times0.5}{0.0096}=1$ for $a$:
$a^2=\frac{0.0096}{0.6}=0.016\Rightarrow a=\sqrt{0.016}\approx\mathbf{0.1265}$ — about half of
Q6's original $a=0.25$.

**10.** Halving $m$ only brought $\mathcal{R}_0$ from 3.906 down to 1.953 (still well above the
disease-free threshold), while roughly halving $a$ (from 0.25 to 0.1265) brought $\mathcal{R}_0$
all the way down to exactly 1. This is a direct, numerically confirmed consequence of formula
(2.52): since $\mathcal{R}_0\propto a^2$ but only $\mathcal{R}_0\propto m^1$, cutting $a$ in half
cuts $\mathcal{R}_0$ to roughly a *quarter* of its value ($0.5^2=0.25$), while cutting $m$ in half
cuts $\mathcal{R}_0$ to exactly *half* — the squared dependence on $a$ makes biting-rate
reductions dramatically more powerful, percentage for percentage, than mosquito-population
reductions.

## Coding Exercises

**11.** Python:
```python
def malaria_R0(a, m, b1, b2, gamma1, gamma2):
    return a**2 * m * b1 * b2 / (gamma1 * gamma2)

def malaria_equilibrium(a, m, b1, b2, gamma1, gamma2):
    R0 = malaria_R0(a, m, b1, b2, gamma1, gamma2)
    if R0 <= 1:
        return None
    x_star = (a**2*m*b1*b2 - gamma1*gamma2) / (a*b2*(a*m*b1 + gamma1))
    y_star = (a**2*m*b1*b2 - gamma1*gamma2) / (a*m*b1*(a*b2 + gamma2))
    return (x_star, y_star)
```
Tested against the notebook's example parameter set ($a=0.3,b_1=0.5,b_2=0.3,
\gamma_1=0.1,\gamma_2=0.05$) at two values of $m$:
- $m=2.0$: $\mathcal{R}_0=5.40$, equilibrium $(x^*,y^*)\approx(0.611,\ 0.524)$ (a valid endemic
  case not shown in the notebook's own figure, added here as an extra test point)
- $m=6.0$: $\mathcal{R}_0=16.20$, equilibrium $(x^*,y^*)\approx(0.844,\ 0.603)$ — matching the
  "above threshold" panel in the notebook's phase-portrait figure exactly. (The notebook's
  "below threshold" panel uses $m=0.3$, giving $\mathcal{R}_0=0.81<1$ and no biologically
  meaningful equilibrium — `malaria_equilibrium` correctly returns `None` for that case.)

**12.** SymPy confirms both off-diagonal Jacobian entries are manifestly non-negative by
inspection: $J_{12}=amb_1(1-x)$ — since $a,m,b_1>0$ (all rate/probability parameters are
positive by definition) and $1-x\ge0$ for $x\in[0,1]$ (the feasible region $\Gamma$ restricts $x$
to exactly this interval), the product of a positive constant and a non-negative factor is
non-negative. Identically, $J_{21}=ab_2(1-y)\ge0$ for the same reason applied to $y\in[0,1]$. This
is a genuinely *symbolic* (algebraic-form) proof, not just a numerical spot-check — it holds
literally everywhere in $\Gamma$ by inspection of the sign of each factor, complementing (and
explaining *why*) the 2000-point numerical check in the main notebook found zero violations.

## Visualization Exercise

**13.** Plotting $\mathcal{R}_0(a)=\frac{a^2\times3\times0.4\times0.5}{0.0096}=62.5a^2$ for
$a\in[0,0.5]$: the curve is a clean **upward-opening parabola** through the origin (since
$\mathcal{R}_0\propto a^2$, with no linear or constant term) — visibly **convex**, curving upward
increasingly steeply as $a$ grows. It crosses $\mathcal{R}_0=1$ at $a\approx0.1265$ (matching Q9).
The convexity directly explains $a$'s outsized effect: near small $a$, small increases in $a$
produce only small increases in $\mathcal{R}_0$, but the *rate* of increase itself keeps growing —
so proportional reductions in $a$ (like a 20% cut) remove a disproportionately large chunk of
$\mathcal{R}_0$ compared to the same proportional cut applied to a linearly-appearing parameter
like $m$, whose $\mathcal{R}_0(m)$ curve would instead be a straight line through the origin.

## Challenge Problem

**14.** Using Q6's parameters ($\mathcal{R}_0\approx3.906$): a 20% cut in $a$ (to $a=0.2$) gives
$\mathcal{R}_0=0.2^2\times3\times0.4\times0.5/0.0096\approx\mathbf{2.500}$ — a
**36.0% reduction**. A 20% cut in $m$ (to $m=2.4$) gives
$\mathcal{R}_0=0.25^2\times2.4\times0.4\times0.5/0.0096\approx\mathbf{3.125}$ — only a **20.0%
reduction** (exactly matching the 20% cut, since $\mathcal{R}_0$ is exactly linear in $m$). Under
the simplified assumption of equal cost, **bednets (cutting $a$) are substantially more
effective per dollar** — nearly double the percentage reduction in $\mathcal{R}_0$ for the same
20% parameter cut.

Real-world caveat: this comparison assumes a "20% cut" costs the same for both interventions and
captures the *complete* picture — neither is generally true. Bednet distribution and larval
control have very different cost structures (per-household distribution and behavior-dependent
compliance for bednets, versus environmental/geographic-dependent labor costs for larval control),
so "20% cut" may not be equally expensive to achieve for both. Moreover, comparing interventions
purely by $\mathcal{R}_0$-reduction ignores co-benefits not captured in this single number —
bednets also reduce other insect-borne disease exposure and provide protection immediately upon
distribution, while larval control may have longer-term environmental effects (reducing future
mosquito population growth, not just current numbers) that a single-timepoint $\mathcal{R}_0$
comparison doesn't capture. Real intervention decisions require weighing cost-effectiveness
data, logistics, and multiple outcome measures together — $\mathcal{R}_0$ sensitivity is a
genuinely useful starting point (and correctly identifies where the "quadratic leverage" lies
mathematically), but it is not a complete policy analysis by itself.
