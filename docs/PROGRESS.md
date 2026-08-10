# Progress Log

- 2026-07-30: Repository scaffold created (folder structure, README, requirements.txt,
  environment.yml, DESCRIPTION, renv setup, .gitignore, LICENSE, references).
- 2026-07-30: Chapter 1, Section 1.1 completed (Python notebook, R Markdown, exercises,
  solutions). See chapter_01/.
- 2026-07-31: Restructured repo per owner's request — top-level python/ and r/ folders, each
  organized by chapter_NN subfolders (was: chapter_NN/python, chapter_NN/r). Workflow changed
  from "pause after each subsection" to continuous chapter-by-chapter build (see WORKFLOW_NOTE.md).
- 2026-07-31: Chapter 1, Sections 1.2 and 1.3 completed (Python notebooks executed end-to-end,
  R Markdown mirrors, exercises + worked solutions with real computed numbers). Section 1.3
  includes the full Kermack-McKendrick derivation and epidemic-threshold proof, verified both
  symbolically/analytically and numerically (scipy solve_ivp / R deSolve).
- Next up: Section 1.4 (Important Concepts in Compartmental Epidemic Models) — the longest
  subsection in Chapter 1 (~24 pages: residence-time distributions, force of infection, standard
  vs. mass-action incidence, disease latency), then Chapter 2.
- 2026-07-31: Chapter 1, Section 1.4 completed (all four sub-subsections: transfer rates &
  residence-time distributions incl. a stable windowed-sum DDE simulation, incidence functional
  forms, demography/logistic growth, and an SEIR latency preview). CHAPTER 1 IS NOW COMPLETE
  (4/4 sections), fully executed in Python with R mirrors, all with exercises + worked solutions.
  Next up: Chapter 2 (Five Classic Epidemic Models and Their Analysis), sections 2.1-2.5.
- 2026-07-31: Chapter 2, Section 2.1 completed (detailed Kermack-McKendrick analysis: five
  well-posedness properties, first-integral phase portrait, exact final-size + peak formulas,
  and the historic Threshold Theorem with its accuracy verified/visualized numerically against
  the exact transcendental final-size equation). Executed Python notebook + R mirror + exercises
  + solutions. Next: Section 2.2 (SIS model - no immunity).
- 2026-08-01: Chapter 2, Section 2.2 completed (SIS model: dimension reduction via conservation
  law, phase-line analysis, disease-free/endemic equilibria, direct numerical SIR-vs-SIS
  comparison showing WHY SIS can't produce a peaked epidemic curve, and a full transcritical
  bifurcation diagram). Caught and fixed a floating-point-noise false positive in a monotonicity
  check before shipping. Executed Python notebook + R mirror + exercises + solutions.
  Next: Section 2.3 (model with demography).
- 2026-08-01: Chapter 2, Section 2.3 completed (model with demography: equilibria derived and
  cross-checked with sympy, local stability via Jacobian eigenvalues + Routh-Hurwitz verified
  numerically across a parameter sweep including complex-eigenvalue/spiraling cases, transcritical
  bifurcation diagram, and global stability via Lyapunov-LaSalle for P0 and Bendixson-Dulac +
  Poincare-Bendixson for P* -- confirmed by simulating many scattered initial conditions with no
  periodic orbits). Caught and fixed a sympy positive=True bug that silently dropped the P0=(1,0)
  solution branch before shipping. Executed Python notebook + R mirror + exercises + solutions.
  Next: Section 2.4 (SIR with varying total population).
- 2026-08-01: Chapter 2, Section 2.4 completed (homogeneous systems of degree 1, Euler Identity
  derived and symbolically verified, fractional-variable projection technique, and the R0/R1
  two-threshold analysis of raw population counts under b=d/b<d/b>d demographic regimes -- all
  confirmed by direct simulation, including a clean numerical confirmation that I(t)'s long-run
  growth rate crosses zero exactly at R1=1). Executed Python notebook + R mirror + exercises +
  solutions. Next: Section 2.5 (Ross-MacDonald malaria model, monotone systems) -- last section
  of Chapter 2.
- 2026-08-02: Chapter 2, Section 2.5 completed (Ross-MacDonald malaria model: cross-species
  transmission derivation, bite-conservation relation, homogeneous-system reduction reusing Sec.
  2.4's technique, a heuristic step-by-step biological derivation of R0 cross-checked against the
  algebraic threshold, and a brand-new global-stability technique -- monotone systems / Metzler
  matrices / strict sublinearity -- verified with zero violations across thousands of sampled
  points). Caught and fixed a multi-step sympy substitution bug (simultaneous vs sequential subs,
  and an incomplete Iv substitution) before shipping -- the symbolic reduction now matches the
  book's equations exactly. CHAPTER 2 IS NOW COMPLETE (5/5 sections). Next: Chapter 3 (Basic
  Mathematical Tools and Techniques) -- the stability/bifurcation toolkit, 8 subsections.
- 2026-08-02: Chapter 3, Sections 3.1-3.2 completed (formal epsilon-delta stability definitions,
  illustrated with an undamped-vs-damped oscillator contrast; the linearization theorem; and the
  Routh-Hurwitz criteria for 2x2 AND 3x3 matrices, validated against direct eigenvalue computation
  across 10,000+ random matrices with 100% agreement, plus a direct callback check against Sec.
  2.3's specific Jacobian). This formally closes the loop on stability shortcuts used without
  proof in Chapter 2. Executed Python notebook + R mirror + exercises + solutions.
  Next: Section 3.3 (Lyapunov functions, formalizing Sec. 2.3's global stability argument).
- 2026-08-02: Chapter 3, Section 3.3 completed (Lyapunov's direct method: Theorems 3.3.1-3.3.3 for
  local stability/asymptotic stability/instability, illustrated with the damped oscillator's
  energy function -- including the subtlety that its Lyapunov derivative is only negative
  SEMI-definite, not enough for Theorem 3.3.2 alone; then LaSalle's Invariance Principle, applied
  step-by-step to re-derive Sec. 2.3's P0 global stability result from first principles, verified
  by simulating both the invariant segment and full scattered convergence). Executed Python
  notebook + R mirror + exercises + solutions. Next: Section 3.4 (Floquet theory for periodic
  solutions) -- genuinely new material, not previewed in Chapter 2.
- 2026-08-03: Chapter 3, Section 3.4 completed (Floquet theory: orbital stability vs ordinary
  stability, demonstrated via the nonlinear pendulum's amplitude-dependent period causing phase
  drift; Floquet multipliers computed numerically via the monodromy matrix for the exact
  r'=r(1-r^2) limit cycle, confirming one multiplier is always exactly 1 and the other matches the
  predicted e^{-4pi} to machine precision, with direct confirmation via trajectories spiraling
  onto the limit cycle from multiple starting points). Executed Python notebook + R mirror +
  exercises + solutions. Next: Sections 3.5-3.6 (phase-line and phase-plane analysis --
  formalizing techniques already used in Sec. 2.2 and 2.1/2.3).
- 2026-08-03: Chapter 3, Sections 3.5-3.6 completed (phase-line analysis formalized via the
  logistic equation; limit sets and the Poincare-Bendixson Theorem demonstrated concretely on the
  van der Pol oscillator, whose periodic orbit was confirmed via Routh-Hurwitz instability of the
  origin plus trajectory convergence from multiple starting points; Poincare's stability condition
  proven, via Liouville's formula, to be exactly equivalent to Sec 3.4's Floquet multiplier test --
  cross-checked to full numerical precision on the r'=r(1-r^2) example and independently to ~4 sig
  figs on van der Pol's numerically-found limit cycle; and Bendixson's Negative Criterion directly
  contrasted between van der Pol (sign-changing divergence, periodic orbit permitted) and the
  damped linear oscillator (sign-definite divergence, correctly ruled out), closing the loop on
  why Sec 2.3 needed a nontrivial Dulac multiplier). Executed Python notebook + R mirror +
  exercises + solutions. Next: Sections 3.7-3.8 (uniform persistence; Metzler matrices/monotone
  systems -- the latter formalizing Sec 2.5's technique) -- final two sections of Chapter 3.
- 2026-08-03: Chapter 3, Sections 3.7-3.8 completed (uniform persistence formalized and proven for
  Sec 2.3's model via Theorem 3.7.2, reusing that section's own Lyapunov calculation, confirmed
  numerically for both R0>1 and R0<1 regimes; Metzler matrices, Perron-Frobenius theorem, and
  Theorem 3.8.3's equivalent stability characterizations verified across 1000 random matrices;
  Theorem 3.8.5 -- the general theorem Sec 2.5 anticipated -- confirmed by directly re-simulating
  that section's regimes). CHAPTER 3 IS NOW COMPLETE (8/8 sections).
  IMPORTANT FIX: while cross-checking Theorem 3.8.5 against Sec. 2.5, discovered that section's
  "below threshold" example (m=2.0) actually had R0=5.4 (well ABOVE 1) -- a mislabeling that
  existed in the original Sec. 2.5 notebook/Rmd since they were written. Fixed retroactively:
  Sec. 2.5's python notebook, R markdown, and solutions doc were all corrected and Sec 2.5's
  notebook was re-executed and its figure regenerated with a genuine R0<1 case (m=0.3, R0=0.81).
  Next: Chapter 4 (Parameter Estimation and Nonlinear Least-Squares Methods, 3 subsections).
- 2026-08-04: Chapter 4, Section 4.1 completed (linear least-squares: normal equations derived
  from orthogonal-projection geometry, verified via explicit dot-product orthogonality checks;
  all three of the book's worked examples -- linear fit, quadratic fit, and exponential fit via
  log-linearization -- reproduced exactly and cross-checked against numpy.linalg.lstsq and
  numpy.polyfit, plus a synthetic-noisy-data recovery test for the exponential case). Executed
  Python notebook + R mirror + exercises + solutions. Next: Section 4.2 (nonlinear least squares
  -- Gauss-Newton / gradient-based iterative methods for models that can't be linearized).
- 2026-08-04: Chapter 4, Section 4.2 completed (nonlinear least squares / Gauss-Newton method,
  implemented entirely from scratch and applied to the book's Example 4, reproducing its
  iteration-by-iteration values essentially exactly; verified symbolically that Gauss-Newton
  reduces to Sec. 4.1's linear least squares when f is linear in theta; cross-checked against
  scipy.optimize.curve_fit; and confirmed numerically that the log-linearized and direct
  Gauss-Newton solutions minimize genuinely different objectives -- direct GN achieves strictly
  lower original-space SSE, even though the two fitted curves are visually indistinguishable,
  exactly matching the book's own remark about Figure 4.4). Executed Python notebook + R mirror
  (using minpack.lm for the professional cross-check) + exercises + solutions.
  Next: Section 4.3 (parameter estimation for epidemic models) -- final section of Chapter 4.
- 2026-08-06: Chapter 4, Section 4.3 completed (epidemic model parameter estimation: fully
  reproduced the book's SIR fitting example end to end). Two real, non-cosmetic issues caught and
  fixed during development, both documented in the shipped notebook rather than papered over:
  (1) the default RK45 solver made the optimizer effectively hang once the search visited
  large-lambda territory (stiff ODE regime) -- fixed by switching to LSODA; (2) a single run from
  the book's own suggested starting guess [8, 0.02] landed in a genuine spurious local minimum
  (confirmed via direct SSE comparison against the true parameters, and confirmed ROBUST across
  7 different random seeds via the Challenge Problem) -- fixed via the multi-start search the book
  itself recommends. Also discovered and correctly explained why I(t)-only and I(t)+N(t) fits are
  identical for this model (dN/dt=0 identically, proven back in Sec 1.3/2.1) rather than reporting
  a superficial "more data helps" conclusion. CHAPTER 4 IS NOW COMPLETE (3/3 sections). Executed
  Python notebook + R mirror + exercises + solutions.
  Next: Chapter 5 (Special Topics: SEIR models, in-host models/backward bifurcation) -- FINAL
  chapter of the book, 2 subsections.
- 2026-08-06: Chapter 5, Sections 5.1 and 5.2 completed — THE FINAL TWO SECTIONS OF THE BOOK.
  Section 5.1 (SEIR models): every Chapter 2-3 technique (dimension reduction, well-posedness,
  R0, full 3x3 Routh-Hurwitz verified across random parameters, Lyapunov/LaSalle global
  stability, uniform persistence) confirmed to scale unchanged to a 4-compartment model. Caught
  and fixed the same class of mislabeling bug found earlier in Sec 2.5 (a "R0<1" demo case that
  actually had R0=1.14).
  Section 5.2 (in-host models, backward bifurcation): the book's most mathematically dramatic
  result. Initial attempt to reproduce the book's exact closed-form bifurcation-threshold formulas
  (5.19)-(5.20) produced negative/nonsensical threshold values -- rather than ship a plausible-
  looking but wrong result, rebuilt the entire numerical demonstration from self-derived,
  independently-checked equilibrium equations: located the bifurcation by direct root-counting,
  confirmed the stable-node/saddle/stable-node topology via Jacobian eigenvalues, and demonstrated
  genuine bistability (R0=0.873<1, 8 different starting points splitting between disease-free and
  chronic outcomes) by direct simulation. As a bonus finding (Challenge Problem), located a second
  parameter regime (nu2 closer to mu2) that recovers the book's own cleaner 4-panel bifurcation
  picture with a genuine lower threshold sigma_0, and explained why the first parameter set didn't
  show one (infected-cell self-proliferation sustaining a chronic state independent of new
  transmission). Executed Python notebooks + R mirrors + exercises + solutions for both sections.

  ============================================================
  CHAPTER 5 COMPLETE (2/2 sections). THE ENTIRE BOOK IS COMPLETE: 19/19 sections across all
  5 chapters, from Sec 1.1 through Sec 5.2.
  ============================================================
