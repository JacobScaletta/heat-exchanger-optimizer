# Heat Exchanger Optimizer

A shell-and-tube heat exchanger design tool that sizes the unit from first
principles, optimizes it for minimum cost, and adds a fouling prediction
and cleaning-schedule layer. Built as a self-directed project to go deeper
than my coursework, I had to teach myself most of the heat transfer and ML
along the way.

## What it does

- Sizes a shell-and-tube exchanger (hot oil cooled by water) from an energy
  balance and validates the result by hand
- Calculates the overall heat transfer coefficient U from the fluid physics
  rather than assuming it, using real fluid properties from CoolProp
- Models the capital vs pumping-cost tradeoff and finds the minimum-cost
  design velocity
- Models fouling over time and how it degrades performance
- Trains a machine learning model (Random Forest) to predict fouling from
  operating conditions
- Optimizes the cleaning schedule to minimize lifecycle cost

## Key results

- Overall U calculated at 286 W/m2K, within 5% of the initial estimate
- The oil-side film controls 87% of the total resistance
- Optimal water velocity of 1.58 m/s, within the standard 1-3 m/s design range
- ML fouling predictor reaches R2 = 0.937 on held-out test data
- Optimal cleaning interval of 29 days

## Why ML is used (and where it isn't)

I only used machine learning where there's no governing equation. U is
calculated directly from heat transfer correlations. Fouling, by contrast,
depends on several interacting operating conditions (temperature, flow,
water hardness, time) with no single closed-form equation, which is exactly
the kind of problem ML is suited to. The feature importances from the
trained model match the physics I built into the data, which is a check
that it learned real structure rather than noise.

## Limitations

This is a learning project, not production software. Real exchanger design
uses validated commercial tools (HTRI, Aspen EDR) backed by proprietary
test data. Specific simplifications:

- Single-phase flow only (no boiling or condensation)
- Shell-side uses a simplified correlation, not the full Kern or
  Bell-Delaware method
- Oil properties are representative values for a light hydrocarbon, not a
  real assay
- The fouling ML model is trained on synthetic data generated from physics
  plus noise, since real plant data is proprietary
- Capital cost uses a simplified annualization (no discounting / interest rate)
- The optimization varies one design variable (water velocity) with others
  held fixed

## Tools

Python, NumPy, pandas, scikit-learn, CoolProp, Matplotlib.

## Author

Jacob Scaletta, Chemical Engineering & Management, McMaster University
