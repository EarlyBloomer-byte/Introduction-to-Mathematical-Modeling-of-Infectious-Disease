# Chapter 2 — Five Classic Epidemic Models and Their Analysis

Source: Li (2018), pp. 35–78. **Status: COMPLETE (5/5 sections).**

| Section | Title | Status |
|---|---|---|
| 2.1 | Kermack–McKendrick Model (detailed: well-posedness, phase portrait, final size, Threshold Theorem) | ✅ Done |
| 2.2 | A Model for Diseases with No Immunity (SIS) | ✅ Done |
| 2.3 | A Model with Demography | ✅ Done |
| 2.4 | An SIR Model with Varying Total Population: Homogeneous Systems | ✅ Done |
| 2.5 | Ross–MacDonald Model for Malaria: A Monotone System | ✅ Done |

Notebooks: `../python/chapter_02/`. R Markdown: `../r/chapter_02/`.

This chapter progressively builds up the field's core toolkit: well-posedness proofs, phase-plane
analysis, first integrals, final-size relations, linearization/Jacobian eigenvalues, Routh-Hurwitz
criteria, Lyapunov-LaSalle theory, Bendixson-Dulac/Poincare-Bendixson, homogeneous systems and the
Euler Identity, and monotone systems/Metzler matrices — all formally systematized in Chapter 3.

**Correction (2026-08-03):** Section 2.5's original "below threshold" example (m=2.0) was
mislabeled — it actually gives R0=5.4 (above 1), not below. Caught while cross-checking Theorem
3.8.5 in Chapter 3 against this section. Fixed: the notebook, R Markdown, and solutions doc now
use m=0.3 (R0=0.81, genuinely below threshold) for that panel.
