# Section 1.2 — Worked Solutions

## Conceptual

**1.** The transfer diagram fixes the *qualitative structure* of the model (which compartments
exist, which transitions are possible) before any quantitative commitment is made. It's much
easier to check "did I forget a transition?" by inspecting a picture with arrows than by staring
at a system of equations — and every arrow in the diagram becomes exactly one term in the
eventual equations, so getting the diagram right first makes the algebra close to mechanical.

**2.** The bookkeeping equation gives the *total* net change over a finite chunk of time
$\Delta t$ — a lump sum. The differential equation gives the *instantaneous rate* of change at a
single moment $t$. The operation connecting them is dividing by $\Delta t$ and then taking the
limit $\Delta t \to 0$ — precisely the definition of a derivative.

**3.** Both death and out-migration have the identical *mathematical* effect on the compartment
being modeled: the individual leaves the tracked population and never returns (within this model).
Since the model only cares about compartment counts, not the fate of individuals once they leave,
lumping them into one "removal" rate keeps the model simpler without losing anything the model
actually uses.

**4.** New arrows needed: (i) from $S$ into $V$ (vaccination), and possibly (ii) from $V$ back
into $S$ if vaccine-induced immunity can wane. Whether $V$ also needs its own "removal" arrow
(death/migration) mirrors the same logic already applied to $S$, $I$, $R$.

**5.** The limiting process (bookkeeping → divide by $\Delta t$ → take the limit) is purely a
mathematical/calculus operation — it works identically no matter what the flows represent. The
*content* of the model — whether new infections happen at rate $\lambda I S$ or something else
entirely — is a separate judgment call about how disease actually spreads, informed by biology,
not by calculus. That's why the text treats "write the rate as a function of state" as a distinct,
final step, separate from the derivation itself.

## Computational

**6.** $\frac{d}{dt}\left[1000e^{-0.05t}\right] = 1000 \cdot (-0.05) e^{-0.05t} = -50e^{-0.05t}$
(chain rule: derivative of $e^{kt}$ is $ke^{kt}$). $\frac{d}{dt}[50\cos t] = -50\sin t$ (standard
derivative of cosine). Sum: $-50e^{-0.05t}-50\sin t$, matching the target expression. Rules used:
the **chain rule** (for the exponential) and the **derivative of cosine**.

**7.** $S(5.01)=1000e^{-0.2505}+50\cos(5.01)$, $S(5)=1000e^{-0.25}+50\cos(5)$. Computing:
$\frac{S(5.01)-S(5)}{0.01} \approx 8.944$, while the true derivative $S'(5) \approx 9.006$. That's
agreement to about **1 significant figure** — a fairly coarse $\Delta t=0.01$ combined with the
oscillatory $\cos t$ term (whose second derivative is large-ish here) leaves a visible error;
compare to the notebook's table, which shows the error shrinking further as $\Delta t$ keeps
decreasing.

**8.** $X'(t) = a(t) - b(t) = 3 - 0.1X(t)$. At equilibrium, $X'(t)=0 \Rightarrow 3-0.1X=0
\Rightarrow X = 30$.

**9.** In words: "rate of change of $I$ = incidence rate (transfer in from $S$) + external
immigration rate of infectious individuals − transfer rate into $R$ (recovery) − removal rate from
$I$." The new "external immigration" term is simply added alongside the existing in-flow term,
exactly like the "new susceptibles" in-flow was added to the $S$ equation.

**10.** Using Richardson-style reasoning: if the error at $\Delta t=0.5$ is roughly 10× the error
at $\Delta t=0.05$, and both difference quotients bracket the true value from the same side (here,
decreasing toward it), a good estimate is
$\text{true} \approx v_2 - \frac{v_1-v_2}{9} = 11.98 - \frac{12.3-11.98}{9} \approx 11.98 - 0.0356
\approx \mathbf{11.9}$ (3 significant figures). (The "9" comes from $10^1-1$, the standard
Richardson extrapolation factor for an error that shrinks linearly in $\Delta t$.)

## Coding Exercises

**11.** Python:
```python
import sympy as sp
t, dt = sp.symbols('t dt', positive=True)
S_expr2 = 500 * t * sp.exp(-0.1 * t)
dq2 = (S_expr2.subs(t, t + dt) - S_expr2) / dt
limit2 = sp.limit(dq2, dt, 0)
deriv2 = sp.diff(S_expr2, t)
print(sp.simplify(limit2 - deriv2) == 0)   # -> True
```
This confirms the limiting argument is not special to the original example function — it holds for
any differentiable $S(t)$, exactly as claimed in Step 3 of the derivation.

**12.** Python:
```python
def finite_difference_derivative(f, t0, dt):
    return (f(t0 + dt) - f(t0)) / dt

f = lambda t: t**2
for dt in [1, 0.1, 0.01, 0.001]:
    approx = finite_difference_derivative(f, 3.0, dt)
    print(dt, approx, "true:", 2*3.0)
# approx -> 2*3.0 = 6.0 as dt shrinks
```

## Visualization Exercise

**13.** With only $S$ and $I$ (no recovery, infected individuals stay infectious forever): the
diagram has two boxes instead of three. The single arrow "new infections" ($S\to I$) remains. The
arrow "recovery" ($I\to R$) is deleted entirely, and so is "loss of immunity" ($R\to S$), since
there is no $R$ box to connect to. $I$ becomes an **absorbing compartment** — once someone enters
it, nothing but "removal" (death/migration) ever gets them out. This is a simpler, more extreme
model than SIR — sometimes used for diseases with no recovery in the modeled timescale (e.g. some
chronic infections).

## Challenge Problem

**14.** At the exact instant of a discontinuous jump (e.g. a step-function mass vaccination event
at some time $t^*$), the function describing the "arrival rate of new susceptibles" is not
continuous at $t^*$, and therefore not differentiable there either — the two-sided limit
$\lim_{\Delta t \to 0}\frac{S(t^*+\Delta t)-S(t^*)}{\Delta t}$ does not exist in the ordinary sense
(the limit from the left and from the right disagree, since the flow rate itself jumps). So Step 3
of the derivation — replacing the difference quotient with a derivative — breaks down exactly at
$t^*$. In practice, modelers handle this by splitting the time domain into pieces at each
discontinuity ($[0,t^*)$ and $(t^*,\infty)$, say), solving the smooth ODE system separately on each
piece, and using the compartment values just before $t^*$ (updated instantaneously for the
vaccination event itself) as the initial condition for the next piece. This is exactly how
real-world interventions (vaccination campaigns, quarantine start/end dates) are typically modeled
— as a sequence of smooth ODE segments glued together at event times, rather than as one equation
valid for all $t$.
