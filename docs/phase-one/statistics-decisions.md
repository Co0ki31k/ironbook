# Phase I — Statistics Decisions

**Status:** Product decisions and open questions for design. This defines what the statistics mean, not their screen layout or calculation implementation. See the [product specification](../product-specification.md) and [Phase I plan](plan.md).

## Exercise progress

- Progress can include more repetitions at the same weight as well as a higher weight. Independent exercises, including copies with the same name or different machines, have independent histories and statistics.
- Support exercises performed in repetitions only, without a weight entry, throughout the app. Below the repetition-range upper limit suggest one more repetition; at the limit inform the user rather than automatically adding weight. Offer maximum repetitions in one working set and total working-set repetitions per workout as separate metrics. Do not assume that an omitted load means zero resistance or automatically calculate load-based tonnage or 1RM. A bodyweight-only exercise and its additionally weighted version are separate independent exercises.
- Assisted exercises record assistance load and repetitions, with assistance represented distinctly: a larger assistance value is not automatically progress. Below the upper repetition limit suggest the same assistance and one more repetition; on reaching it suggest the next available lower assistance and the lower repetition limit. Show assistance and repetitions; do not calculate ordinary tonnage or estimated 1RM. Edge cases such as unavailable lower assistance remain to be designed.
- For weight-based exercise charts, a point represents a workout: maximum weight is the highest working-set weight; tonnage is the sum of weight multiplied by repetitions across working sets; estimated 1RM is the highest estimate from an eligible working set. Warm-up tasks are excluded. Eligibility and formula are not yet decided.
- A manually entered actual 1RM and an estimated 1RM are separate values and do not automatically replace each other. An actual record may be entered independently of a workout, with exercise, date and weight. Exercise renaming updates the record's displayed exercise name. Label estimates clearly and identify the source set; calculate only where the recorded weight is a meaningful input, excluding repetition-only and assisted exercises. The estimate formula and eligible repetition range remain open; limit estimates to that agreed range.
- Initial statistics compare a user's own results, without user rankings. Present independent exercises separately rather than comparing them on a single chart. Avoid automatic good/bad assessments or claims of imbalance until rules and supporting data are designed.

## Body weight

- Record daily measurements when the user enters them, with at most one value per date; another entry for that date replaces it. Do not assume a measurement exists every day or invent missing values.
- Allow the user to choose a 3–7 day moving-average period and whether to view the average, trend, or both. The existing specification also supports showing or hiding raw measurements independently. Average available measurements within the chosen calendar window without inventing missing values, and show the number of measurements and window length (e.g. 3 of 7 days). A window with no measurements has no average value. Trend is a separate line describing the general direction over a selected recent period, with 30, 60 and 90 days as proposed choices; its calculation method remains open.
- An optional target and a forecast of when the trend may reach it remain in scope. Label forecasts as estimates and require at least seven measurements in the last 14 days, as specified; explain missing data when insufficient. Do not give a target date when the trend is flat or moving away; explain that the date cannot be estimated from the current trend. Define the exact method and thresholds later.

## Volume and data changes

- Working-set count and tonnage are distinct metrics. Count completed working sets by primary muscle group for the selected period, excluding warm-ups, and do not label the count too high or too low. Tonnage can also be shown, but is not a straightforward comparison of effort across machines or exercise types. Attribute sets and tonnage only to the exercise's primary muscle group; secondary groups are descriptive. Changing the primary group of an existing exercise reassigns its historical results to the current group. Use an independent exercise for a different movement variant.
- Statistics reflect the current retained history: corrections change future calculations and deleting results removes them from calculations. Removing exercise history follows the user's explicit preserve-or-delete choice. Independently entered 1RM records follow the same choice: keep them with retained history, or explicitly warn and confirm that they will also be deleted.

## Accepted calculation design

### Estimated 1RM

Use Epley's estimate `weight × (1 + repetitions / 30)` for eligible working sets of 2–10 repetitions, and show the source set. A single repetition displays the lifted load as the set result, not automatically as a true maximum. Use the maximum eligible estimate per workout. Do not add RIR to repetitions in Phase I; explain that an estimate can be low when the set is stopped well short of failure. Do not encourage training to failure to obtain an estimate. Eligibility must reflect equipment/load semantics: no repetition-only sets or assistance values, and no machine label represented as measured resistance. Actual manually entered 1RM remains separate. The 2–10 limit is a product choice, not a guarantee of accuracy. [Prediction study](https://pubmed.ncbi.nlm.nih.gov/16937972/); [equation reference](https://vbn.aau.dk/ws/portalfiles/portal/799845937/Open_Access_Article.pdf); [RIR estimation study](https://pubmed.ncbi.nlm.nih.gov/37036795/).

### Moving average, trend and forecast

For each day and selected 3–7 calendar-day window, use the arithmetic mean of available measurements in that window and show the measurement count; no measurements means no average. Do not fabricate missing values. For trend, fit an ordinary least-squares line to actual dated measurements in the selected recent 30, 60 or 90 calendar days, separately from the moving average; show its slope per week and fitted value at the current date. Initially require at least seven measurements spanning at least 14 days in the selected period and a latest measurement no older than seven days. These are product thresholds to validate on representative data, not established clinical cutoffs. [Daily body mass variability](https://pmc.ncbi.nlm.nih.gov/articles/PMC10653631/); [example of regression for weight trajectories](https://pmc.ncbi.nlm.nih.gov/articles/PMC6578494/).

Forecast weight at a selected future date and time to goal by extrapolating the same fitted trend, clearly labeled an approximate scenario rather than a promise. Additionally require at least seven measurements in the last 14 days. Limit extrapolation to 90 days; communicate when a requested date or target crossing lies beyond the horizon. Do not show a target date if the slope is effectively flat, points away from the goal, or data are insufficient; explain why. If the goal is already reached by the fitted current value, show that state rather than a past date. Define and test the near-zero slope threshold rather than presenting a guessed cutoff as scientific fact. No formal prediction interval is promised without a validated method. Linear extrapolation ignores future changes in behavior and other factors. [Forecasting limitations](https://otexts.com/fpp3/intro.html).

### Assistance boundary

On reaching the upper repetition limit, select the next lower listed assistance setting and reset to the lower repetition limit. If no lower setting exists, tell the user and do not invent one; allow manual entry or creation of a separate exercise without assistance. Do not automatically merge exercise histories.

## Remaining design and validation

1. Repetition-only exercise configuration and assisted-exercise input/edge cases.
2. Validate trend thresholds, define the near-zero slope rule, and test missing days, stale data, forecast horizon and goal-already-reached behavior.
3. Define eligible equipment and load meanings for estimated 1RM and tonnage and handle invalid or missing input without presenting a machine label as measured resistance.
4. The detailed presentation of primary-muscle-group summaries and any future comparison or imbalance detection.

These decisions must be resolved before the relevant calculations are implemented; screen design can proceed with the distinction between agreed metrics and open formulas visible.
