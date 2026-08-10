# Section 5.1 — Worked Solutions

## Conceptual

**1.** As $\kappa\to\infty$, $\frac{\kappa}{\kappa+\mu}\to1$ — the "survives the latent period"
probability approaches certainty, and $\mathcal{R}_0\to\frac{\lambda}{\gamma+\mu}$, exactly Sec.
2.3's formula. Biologically, $\kappa\to\infty$ means the mean latent period $1/\kappa\to0$ —
individuals become infectious essentially instantly upon infection, which is precisely the "no
latency" assumption (Hypothesis 4) built into every model before Sec. 5.1 — confirming the SEIR
model correctly reduces to the simpler SIR model in the appropriate limit.

**2.** Because the mathematical STRUCTURE of the problem — a smooth ODE system with equilibria,
Jacobians, Lyapunov functions, and boundary dynamics — doesn't change when a new compartment is
added; only the SIZE of the matrices and the specific algebraic formulas grow. Every theorem in
Chapter 3 (linearization, Routh-Hurwitz for $n\times n$, Lyapunov/LaSalle, uniform persistence) was
already stated in general dimension, not just for 2×2/3×3 systems — Sec. 5.1 is simply the first
place the book exercises that generality on a genuinely 3-or-4-dimensional example.

**3.** An exposed individual faces two "competing clocks": one clock ticking toward progression
to infectious (rate $\kappa$, mean time $1/\kappa$) and another ticking toward death (rate $\mu$,
mean time $1/\mu$) — exactly the same competing-exponential-clocks setup from Sec. 1.4.1's
residence-time discussion, just with two competing EXITS instead of one exit and one "staying"
option. For two independent exponential clocks with rates $\kappa$ and $\mu$, the probability the
FIRST clock (rate $\kappa$) fires before the second is the standard ratio
$\frac{\kappa}{\kappa+\mu}$ — a well-known fact about competing exponential races.

**4.** It shows the two theorems are not independent, separately-proven facts but two DIFFERENT
consequences drawn from the SAME underlying calculation ($L'=(\kappa+\mu)(\gamma+\mu)I
(\mathcal{R}_0S-1)$) applied in two different regimes: when $\mathcal{R}_0\le1$, the same sign
argument (using $S\le1$) shows $L'\le0$ everywhere, giving global stability of $P_0$; when
$\mathcal{R}_0>1$, the SAME formula instead shows $L'>0$ near $P_0$ (since $S$ near 1 there makes
$\mathcal{R}_0S-1>0$), showing $P_0$ REPELS into the interior — exactly what uniform persistence's
proof needs. One piece of algebra, two complementary conclusions depending on which side of the
threshold you're examining.

**5.** Because the Jacobian $J(P_0)$ has a special structure at this specific equilibrium: the
$R$-equation (or, in the reduced 3D system, effectively the $S$-row) decouples from $E,I$ in a way
that leaves $-\mu$ as a directly-readable diagonal entry with zero coupling to the other rows in
the relevant column — exactly the same "triangular structure at the disease-free equilibrium"
phenomenon already seen in Sec. 2.3's simpler model, where $J(P_0)$ was exactly upper triangular.

## Computational

**6.** $\mathcal{R}_0=\frac{0.4\times0.15}{(0.15+0.02)(0.2+0.02)}=\frac{0.06}{0.0374}\approx
\mathbf{1.604}$.

**7.** $S^*=\frac{(0.17)(0.22)}{0.06}=\frac{0.0374}{0.06}\approx\mathbf{0.6233}$.

**8.** $\kappa=1/8=\mathbf{0.125}$/day. $\gamma=1/5=\mathbf{0.2}$/day.
$\mu=1/(70\times365)\approx\mathbf{0.0000391}$/day.

**9.** $\mathcal{R}_0=\frac{0.5\times0.125}{(0.125+0.0000391)(0.2+0.0000391)}\approx
\frac{0.0625}{0.02501}\approx\mathbf{2.499}$.

**10.** With $\mu=0$: $\mathcal{R}_0=\lambda/\gamma=0.5/0.2=2.5$. Ratio of Q9's answer to this:
$2.499/2.5\approx0.9995$ — the demographic correction changes $\mathcal{R}_0$ by less than
**0.05%** here, because $\mu\approx0.0000391$ is utterly dwarfed by $\kappa=0.125$ and
$\gamma=0.2$ (a 70-year lifespan is vastly longer than an 8-day latent period or 5-day infectious
period) — confirming that for acute, fast-resolving diseases in a population with normal
demography, the "survives latency" correction to $\mathcal{R}_0$ is usually negligible in
practice, even though it's structurally present in the formula.

## Coding Exercises

**11.** Python:
```python
def seir_R0(lam, kappa, gamma, mu):
    return lam * kappa / ((kappa + mu) * (gamma + mu))

def seir_equilibrium(lam, kappa, gamma, mu):
    R0 = seir_R0(lam, kappa, gamma, mu)
    if R0 <= 1:
        return None
    S_star = (kappa + mu) * (gamma + mu) / (lam * kappa)
    I_star = mu * (1 - S_star) / (lam * S_star)
    E_star = (gamma + mu) / kappa * I_star
    return (S_star, E_star, I_star)
```
Tested with $(\lambda,\kappa,\gamma,\mu)=(0.4,0.15,0.2,0.02)$: returns $\mathcal{R}_0\approx1.604$
and equilibrium $(S^*,E^*,I^*)\approx(0.6233,\ 0.0443,\ 0.0302)$, matching Q6/Q7 exactly.

**12.** Sweeping $\lambda$ across the $\mathcal{R}_0=1$ threshold and plotting the long-run
$I^*$: the curve is flat at $I^*=0$ for $\mathcal{R}_0\le1$, then rises continuously (starting
gently, then more steeply) for $\mathcal{R}_0>1$ — the same transcritical-bifurcation shape
observed in Sec. 2.2's SIS model and Sec. 2.3's demography model, confirming the SEIR model's
$\mathcal{R}_0=1$ threshold is structurally the identical kind of bifurcation, just now embedded
in a 3-dimensional (reduced) state space instead of 1D or 2D.

## Visualization Exercise

**13.** The transfer diagram has four boxes ($S,E,I,R$) in a row, with: an incoming arrow into
$S$ labeled $\mu$ (births, all into $S$); an arrow $S\to E$ labeled $\lambda IS$ (new infections,
still latent); an arrow $E\to I$ labeled $\kappa E$ (progression to infectious); an arrow
$I\to R$ labeled $\gamma I$ (recovery); and a downward "removal" arrow labeled $\mu$ leaving EACH
of the four boxes (background death, proportional to each compartment's own size) — six labeled
arrows in total, directly matching the six terms appearing across equations (5.1).

## Challenge Problem

**14.** Computing $\det(pI-J(P_0))$ directly for
$J(P_0)=\begin{pmatrix}-\mu&0&-\lambda\\0&-(\kappa+\mu)&\lambda\\0&\kappa&-(\gamma+\mu)
\end{pmatrix}$ and expanding symbolically confirms it equals EXACTLY
$(p+\mu)\left[p^2+(\kappa+\gamma+2\mu)p+(\kappa+\mu)(\gamma+\mu)-\lambda\kappa\right]$ — the
difference between the two expanded polynomials simplifies to exactly zero. This factorization is
possible precisely because the first column of $J(P_0)-pI$ has zeros in rows 2 and 3 — a direct
consequence of $S$ not appearing in the $E$ or $I$ equations' linearization at $P_0$ (since
$\partial(\lambda IS)/\partial S=\lambda I=0$ at $I=0$) — confirming algebraically why one
eigenvalue ($p=-\mu$) separates out cleanly, exactly as claimed in the answer to Conceptual Q5.
