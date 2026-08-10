# Chapter 5 — Special Topics

Source: Li (2018), pp. 125–150. **Status: COMPLETE (2/2 sections) — FINAL CHAPTER OF THE BOOK.**

| Section | Title | Status |
|---|---|---|
| 5.1 | Higher Dimensional Models: SEIR Models | ✅ Done |
| 5.2 | In-host Models and Backward Bifurcation | ✅ Done |

Notebooks: `../python/chapter_05/`. R Markdown: `../r/chapter_05/`.

Section 5.1 shows every Chapter 2–3 technique (dimension reduction, well-posedness, R0,
full 3×3 Routh-Hurwitz, Lyapunov/LaSalle, uniform persistence) scaling unchanged to a
4-compartment model. Section 5.2 is the deliberate capstone contrast: the SAME toolkit, applied to
a model with one extra nonlinear ingredient (shared logistic growth for both cell populations),
produces a genuinely NEW phenomenon — backward bifurcation and bistability — that never occurs in
any earlier model in the book. Its numerics required real, from-scratch verification rather than
trusting the book's compact closed-form bifurcation-threshold formulas (easy to mistranscribe from
a scanned PDF); every claim there is instead checked by directly solving the equilibrium
equations, computing Jacobian eigenvalues, and simulating — see that notebook's opening note and
`docs/PROGRESS.md` for what was caught and fixed along the way.
