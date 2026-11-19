# OLS Diagnostics: Robust and Clustered Standard Errors

**ECON 5200 · Causal Machine Learning & Applied Analytics · Lab 14**

## Objective

I measured how often a nominal 95% interval actually contains the true slope when the errors break OLS's assumptions, and tested which standard errors fix which problem.

## Methodology

- Simulated 500 samples where the error's spread grows with x, and counted how often classical, HC1 and HC3 intervals contained the true slope of 2.0.
- Simulated 300 panels of 40 firms with 25 employees each, where every employee in a firm shares one shock, and ran the same coverage count for classical, HC3 and firm-clustered intervals.
- Computed every standard error statsmodels offers, classical, HC0 to HC3 and clustered, on one regression with 960 usable rows, and compared how clustering moved each coefficient's.
- Planted three rows in the panel and found them with leverage, studentised residuals and Cook's distance, then counted how many ordinary rows the usual cutoffs also flag.
- Wrote my own prompt asking an AI for a function that builds the table of standard errors, and checked every number it gave against statsmodels.
- Used a `diagnostics.py` module that picks the covariance type from the design and gives the reason.

## Key Findings

**Robust errors fix unequal spread.** With heteroskedastic errors, classical intervals covered the truth 92.4% of the time, against 94.2% for HC1 and 94.4% for HC3.

**Robust errors do not fix errors shared within a firm.** On the firm panel, HC3 intervals covered only 50.3% of the time, and classical 49.0%. Clustering by firm brought coverage back to 93.7%. The clustered standard error on x was 0.0879, more than three times HC3's 0.0264.

**Clustering only matters for a regressor shared within the firm.** In the analyst's regression, clustering raised x's standard error from 0.02576 to 0.08644, but left tenure's almost unchanged (0.00643 to 0.00629), because tenure has no part shared within a firm.

**One planted row moved the slope.** The row far out in x and far off the line moved the slope from 1.4927 to 1.4468. The row with the most leverage sat on the true line, and the row with a large residual sat at the centre of x, so neither moved it much. The usual cutoffs flagged 44 rows by Cook's distance and 28 by leverage, almost all of them rows nobody planted.

**The AI's function matched statsmodels in one round.** All 18 standard errors matched, and the function gave the same 18 again with every column renamed.

## Repository Contents

| File | Description |
|---|---|
| `lab-ch14-diagnostic.ipynb` | Full lab notebook with rendered output |
| `src/diagnostics.py` | Recommends a covariance type from the design (HC3, or clustered with a warning below 40 clusters) and fits OLS with it |

## Data

- All data are simulated with fixed seeds: the firm panel uses `RANDOM_STATE = 42`, and the coverage simulations use one seed per sample.
