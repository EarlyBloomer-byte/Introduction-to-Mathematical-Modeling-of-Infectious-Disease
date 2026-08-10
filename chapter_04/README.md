# Chapter 4 — Parameter Estimation and Nonlinear Least-Squares Methods

Source: Li (2018), pp. 103–124. **Status: COMPLETE (3/3 sections).**

| Section | Title | Status |
|---|---|---|
| 4.1 | Curve-Fitting and Linear Least-Squares Problem | ✅ Done |
| 4.2 | Nonlinear Least-Squares Problem | ✅ Done |
| 4.3 | Parameter Estimation for Epidemic Models | ✅ Done |

Notebooks: `../python/chapter_04/`. R Markdown: `../r/chapter_04/`.

Section 4.3 reproduces the book's own SIR parameter-estimation example end to end, and turned up
two genuine, worth-knowing findings beyond just reproducing the book's numbers: (1) a single
optimizer run from the book's own suggested starting guess reliably lands in a spurious local
minimum — confirmed robust across 7 different random seeds, not a one-off fluke — fixed by the
multi-start safeguard the book itself recommends; (2) fitting with only I(t) vs. I(t)+N(t) gives
identical results for this specific (demography-free) model because N(t) is analytically constant
regardless of the parameters, not because more data is generally unhelpful. See
`python/chapter_04/section_4_3.ipynb` for the full analysis.
