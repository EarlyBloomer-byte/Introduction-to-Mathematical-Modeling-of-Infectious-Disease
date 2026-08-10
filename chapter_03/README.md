# Chapter 3 — Basic Mathematical Tools and Techniques

Source: Li (2018), pp. 79–101. **Status: COMPLETE (8/8 sections).**

| Section | Title | Status |
|---|---|---|
| 3.1–3.2 | Stability of Equilibrium Solutions; Stability Analysis by Linearization (Routh–Hurwitz) | ✅ Done |
| 3.3 | Stability Analysis Using Lyapunov Functions | ✅ Done |
| 3.4 | Stability of Periodic Solutions: The Floquet Theory | ✅ Done |
| 3.5–3.6 | Phase-Line Analysis; Phase-Plane Analysis (Poincaré–Bendixson) | ✅ Done |
| 3.7 | Uniform Persistence | ✅ Done |
| 3.8 | Metzler Matrices and Monotone Systems | ✅ Done |

Notebooks: `../python/chapter_03/`. R Markdown: `../r/chapter_03/`.

This chapter formalizes every stability/analysis tool used informally throughout Chapter 2, and
cross-checks each general theorem numerically against the specific Chapter 2 example that first
motivated it (Sec. 2.1's phase portrait, 2.2's SIS bifurcation, 2.3's Lyapunov/Routh-Hurwitz
arguments, 2.5's monotone-systems check). A genuine mislabeling bug in Sec. 2.5's "below
threshold" example (m=2.0 actually gave R0=5.4, not <1) was caught while cross-checking Theorem
3.8.5 here and fixed retroactively in Chapter 2 — see docs/PROGRESS.md.
