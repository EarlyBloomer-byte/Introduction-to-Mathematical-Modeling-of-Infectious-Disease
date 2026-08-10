# Section 1.1 — Practice Problems

## Conceptual (5)

1. Explain, in your own words, the difference between an *outbreak* and an *epidemic*. Which one
   is always a subset of the other, and why?
2. Give an example (real or hypothetical) of a disease that is **endemic** but never becomes an
   **epidemic**. What must be true about the rate of new infections for a disease to stay endemic
   rather than dying out?
3. Name the four reasons the text gives for why traditional experimental/statistical methods can
   struggle with infectious disease questions. For each, give a one-sentence example.
4. The six-stage modeling process is described as a *loop*. Explain why it would be a mistake to
   treat it as a one-time, linear sequence instead.
5. A colleague says: "We should always use the most realistic, most detailed model available,
   since it's closest to the truth." Using the text's own reasoning, explain what is wrong with
   this statement.

## Computational (5)

6. Using the classification function from the notebook, what does
   `classify_disease_event(True, False, True, True)` return? Work it out by hand first, then
   check with code.
7. If a population has 50 people, how many potential person-to-person contact pairs are there
   (using the $n(n-1)/2$ formula from Block 2)? How many for 5,000 people? What is the ratio of
   the two answers, and is it equal to the ratio of the population sizes (50 vs. 5,000)? Explain
   why or why not.
8. In the realism/tractability demo (Block 4), the complex model uses a degree-6 polynomial (7
   parameters) fit to only 8 data points. What is special about fitting exactly 8 points with a
   degree-7 polynomial (8 parameters) instead? (Hint: think about how many points determine a
   polynomial of a given degree.)
9. In the deterministic-vs-stochastic demo (Block 5), the deterministic prediction is
   $N(t) = N_0 e^{rt}$. If $N_0 = 5$, $r = 0.3$, compute $N(15)$ by hand. Compare this to the
   `deterministic_final` value printed by the verification code cell.
10. Suppose you re-run the stochastic simulation with $N_0 = 5{,}000{,}000$ (five million) instead
    of 500. Without running the code, predict qualitatively what should happen to the coefficient
    of variation, and explain why, using the reasoning from Block 5's intuition section.

## Coding Exercises (2)

11. Modify `classify_disease_event` (Python or R) to also return `"regional epidemic"` for cases
    that are fast-spreading and *do* cross multiple countries but *not* multiple continents. You
    will need to add a new parameter, e.g. `crosses_countries`. Update the flow diagram to match.
12. Write a function `severity_summary(cases_over_time)` that takes a list/array of daily case
    counts and returns a dictionary/named list with: total cases (sum), peak daily cases (max),
    and the day index of the peak (`argmax`). Test it on a synthetic list you construct by hand,
    and explain which of the "five public health questions" from Block 2 each output answers.

## Visualization Exercise (1)

13. Build a new diagram (in Python with matplotlib or R with ggplot2) that shows the
    realism/tractability trade-off as a single line plot: x-axis = number of model parameters
    (2 through 7, i.e. polynomial degree 1 through 6), y-axis = average bootstrap prediction
    uncertainty (reuse the bootstrap logic from Block 4, looping over degree). Is the relationship
    monotonic? At what degree does uncertainty start increasing sharply?

## Challenge Problem (1)

14. The text says deterministic models are "not expected to be valid if population sizes are very
    small." Design (in words, then in code) a numerical experiment that finds the *approximate*
    population size $N_0$ at which the coefficient of variation of the stochastic birth process
    (Block 5) drops below 0.05 (i.e., stochastic fluctuations are within 5% of the mean, relative
    to the mean). Run your experiment and report the threshold you find for $r = 0.3$, $t_{max}=15$.
    Discuss whether this threshold would change for a different growth rate $r$, and why.

*Full worked solutions: `../solutions/section_1_1_solutions.md`.*
