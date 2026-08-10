# Sections 3.5–3.6 — Practice Problems

## Conceptual (5)

1. Explain why Proposition 3.5.1's stability criterion ("$f'(\bar x)<0\Rightarrow$ stable") is
   really just the 1D special case of the Routh–Hurwitz / linearization ideas from Sec. 3.2.
2. What makes the Poincaré–Bendixson Theorem specifically a 2D result? Why can't the same
   trapping-region argument guarantee a periodic orbit in 3D?
3. Explain, using the "bug in a fenced donut" analogy, why the fence (trapping region) must
   contain NO equilibria for the Poincaré–Bendixson conclusion to apply.
4. Why is $\text{div }f=\text{tr}(\partial f/\partial x)$ true? Connect this to the definition of
   both quantities.
5. Explain in your own words why Bendixson's criterion (Theorem 3.6.5) can be seen as a special
   case of Sec. 2.3/2.5's more general Bendixson–Dulac argument, and under what circumstance you'd
   need the more general (multiplier-based) version instead of the plain one.

## Computational (5)

6. For the equation $x'=x(x-2)(x+3)$, find all equilibria and use $f'$ at each to determine
   stability.
7. For the van der Pol oscillator with $\mu=2$ (instead of $\mu=1$), compute the Jacobian at the
   origin, its trace and determinant, and confirm the origin is still unstable.
8. Using Liouville's formula, if $\int_0^T\text{div }f(p(t))\,dt=-6$, what is the second Floquet
   multiplier $\lambda_2$? Is the orbit orbitally asymptotically stable?
9. For a 2D system with $\text{div }f(x,y)=3x^2+2y^2+1$, can Bendixson's criterion rule out
   periodic orbits in all of $\mathbb{R}^2$? Explain using the sign of this expression.
10. For the logistic equation with $r=0.5$, $K=200$, compute $f'(0)$ and $f'(K)$, and state which
    equilibrium is stable.

## Coding Exercises (2)

11. Write a function `phase_line_stability(f_prime, equilibria)` that, given a function computing
    $f'(x)$ and a list of equilibrium points, returns a dictionary/list mapping each equilibrium to
    "stable" or "unstable" based on the sign of $f'$ there. Test it on the logistic equation's two
    equilibria.
12. Numerically verify Liouville's formula for the VAN DER POL system instead of the $r'=r(1-r^2)$
    system: integrate $\text{div }f=\mu(1-x^2)$ along the (numerically computed, not exact) limit
    cycle over one period, and compare $e^{\int\text{div }f\,dt}$ to a directly-computed monodromy
    matrix eigenvalue (you'll need to numerically find the limit cycle's period first, e.g. by
    detecting when the trajectory returns close to a reference point).

## Visualization Exercise (1)

13. Create a plot of $\text{div }f(x,y)=\mu(1-x^2)$ for the van der Pol system as a function of
    $x$ alone (it doesn't depend on $y$), for $x\in[-3,3]$. Shade or mark the region where it's
    positive vs. negative. Relate this to where, roughly, you'd expect the limit cycle to spend
    more/less time (Hint: think about where trajectories are being pushed outward vs. pulled
    inward).

## Challenge Problem (1)

14. The book's own exercise asks: construct a planar system where the unit circle consists
    ENTIRELY of equilibria, and every non-equilibrium trajectory's omega-limit set is the unit
    circle. (Hint: think in polar coordinates — you want $r'=g(r)$ with $g(1)=0$ but ALSO
    $g\equiv0$ in some sense on the circle itself... actually, reconsider: you want every POINT on
    r=1 to be an equilibrium, which is different from the r'=r(1-r^2) example where only the
    circle as a whole is invariant. Try $\theta'=0$ combined with an appropriate $r'=g(r)$.) Write
    down your system, and briefly explain why Poincaré–Bendixson's "no equilibria in the
    omega-limit set" hypothesis is essential for ruling out exactly this kind of example (where the
    limit set is a circle but NOT a periodic orbit, since nothing on it is actually moving).

*Full worked solutions: `../solutions/section_3_5_3_6_solutions.md`.*
