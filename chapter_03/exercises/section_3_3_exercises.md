# Section 3.3 — Practice Problems

## Conceptual (5)

1. Explain why Theorem 3.3.1 (stability) only requires $\dot V\le0$, while Theorem 3.3.2
   (asymptotic stability) requires $\dot V$ to be negative DEFINITE, not just non-positive.
2. In the damped oscillator example, explain in your own words why $\dot V=-cv^2$ being zero
   whenever $v=0$ (not just at the origin) means Theorem 3.3.2 cannot be directly applied, even
   though the origin actually IS asymptotically stable.
3. What is an "omega-limit set," in plain language? Why must it be an invariant set?
4. Explain the role of "positively invariant" in Corollary 3.3.5's global stability conclusion —
   what could go wrong if $G$ were NOT positively invariant?
5. Theorem 3.3.3 (instability) requires finding points $x_n\to0$ where $V(x_n)<0$. Why is this a
   fundamentally different kind of condition than "V is negative definite everywhere"?

## Computational (5)

6. For the system $x'=-x$, $y'=-2y$, verify that $V(x,y)=x^2+y^2$ is positive definite, and
   compute $\dot V$. Is the origin asymptotically stable by Theorem 3.3.2?
7. For the system $x'=y$, $y'=-x$ (a pure rotation, no damping), compute $\dot V$ for
   $V(x,y)=x^2+y^2$. What does this tell you about the origin's stability (stable? asymptotically
   stable? neither?)
8. For the damped oscillator $x'=v,\ v'=-x-cv$ with $c=0.3$, at the point $(x,v)=(2,0)$, is this
   point in the set $\{\dot V=0\}$? Is it in the largest invariant subset $K$ of that set?
   (Hint: does the system stay at this point, or move away, if started there?)
9. Compute $\dot L$ for Sec. 2.3's Lyapunov function $L(S,I)=I$ directly from
   $I'=I(\beta S-\gamma-b)$, and confirm it matches $I(\beta S-\gamma-b)$ exactly (this is really
   just restating the given formula, but write out each substitution step).
10. In the Sec. 2.3 LaSalle argument, what ODE does $S(t)$ satisfy exactly ON the invariant
    segment $\{I=0\}$? Solve it explicitly (it's a simple linear ODE) and confirm $S(t)\to1$ as
    $t\to\infty$ for any $S_0\in[0,1]$.

## Coding Exercises (2)

11. Write a function `check_lyapunov_conditions(V_func, Vdot_func, region_points)` that, given a
    candidate Lyapunov function and its derivative (as Python functions) and a set of sample
    points, checks whether $V\ge0$ everywhere (with equality only at the origin) and $\dot V\le0$
    everywhere, reporting which condition (if any) fails. Test it on the damped oscillator's
    energy function.
12. Numerically verify the LaSalle "largest invariant subset" claim for Sec. 2.3's example
    differently: starting from several points ON the segment $\{I=0\}$ that are NOT exactly $P_0$,
    simulate forward and confirm they never leave the segment (confirming the segment IS
    invariant) while still converging to $S=1$ (confirming $K$, the largest invariant subset
    where trajectories can stay forever without further change, must be the single point $P_0$,
    not the whole segment).

## Visualization Exercise (1)

13. Create a contour plot of $V(x,v)=\frac12(x^2+v^2)$ (level curves, i.e. circles) overlaid with
    a single trajectory of the damped oscillator spiraling into the origin. Confirm visually that
    the trajectory crosses level curves of decreasing $V$ (moving to smaller and smaller circles)
    as time progresses, and never crosses back out to a larger circle.

## Challenge Problem (1)

14. Construct a Lyapunov function proving instability (Theorem 3.3.3) for the system
    $x'=x,\ y'=y$ (both coordinates grow without bound). Try $V(x,y)=x^2+y^2$: compute $\dot V$,
    and determine whether $V$ takes negative values near the origin (needed for hypothesis (1) of
    Theorem 3.3.3). If $V=x^2+y^2$ doesn't work for the instability theorem as stated (since it's
    never negative), what does that tell you about which theorem SHOULD apply instead, and what
    do you conclude about the origin's stability for this particular system using that theorem?

*Full worked solutions: `../solutions/section_3_3_solutions.md`.*
