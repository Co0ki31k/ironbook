# Phase I — Weight Calculation Examples

**Status:** Working design, not implemented. The [equipment decisions](equipment-decisions.md) define agreed behavior; examples below illustrate it and identify unresolved details. All examples use kg.

## Principles

- Distinguish the target, a suggestion based on known equipment, and the result actually recorded. Users can change suggestions and manually enter a load.
- A weight's meaning must be visible: total barbell load, plates on both sides combined, or plates on one side. Convert only when the necessary bar mass and symmetrical-loading assumptions are known.
- Plate calculations use denominations selected for the exercise and assume enough plates; actual availability is not guaranteed. Automatic barbell suggestions assume symmetry and exclude collar mass. Prefer fewer plates among equivalent arrangements.
- Warm-up targets are percentages of the planned first working set; where feasible weights are known, suggest the closest at or below the target. If none is at or below the target, explain and let the user choose. Without sufficient data, show the target without asserting availability.
- Progression is selected per exercise: available plates, machine settings, or a required custom increment. Match working sets by position. Recorded results, not previous suggestions, determine later suggestions.

## Barbell and entry conventions

With a 20 kg bar and 20 kg of plates on each side, the same load is represented as:

| Entry convention | Current | After adding 1.25 kg per side |
|---|---:|---:|
| Bar and plates total | 60 kg | 62.5 kg |
| Plates on both sides combined | 40 kg | 42.5 kg |
| Plates on one side | 20 kg | 21.25 kg |

If the selected plate inventory includes 20 and 1.25 kg, the latter arrangement is 20 + 1.25 kg on each side. The suggestions are in the user's chosen convention; an optional equivalent may be shown if reliably calculable.

## Warm-up with a barbell

Assume a 20 kg bar, selected denominations 20, 10, 5 and 2.5 kg, symmetrical loading, a planned first working set of 100 kg total and warm-up targets of 50%, 75% and 90%.

| Target | Exact target | Suggested feasible total | Example plates per side |
|---|---:|---:|---|
| 50% | 50 kg | 50 kg | 10 + 5 kg |
| 75% | 75 kg | 75 kg | 20 + 5 + 2.5 kg |
| 90% | 90 kg | 90 kg | 20 + 10 + 5 kg |

If the 2.5 kg denomination is excluded, the 75 kg target is not feasible with these denominations: suggest 70 kg (20 + 5 kg per side), the highest feasible total below it; the user can choose another available weight. If the target is 15 kg and the empty bar is 20 kg, there is no feasible total at or below target: explain this and allow the user to decide. Missing bar mass or plate data must not produce a supposedly verified arrangement.

## Stack machine

A machine exposes the labeled settings 10, 15, 20, …, 100 kg. For a first working set of 60 kg and a 75% warm-up, the target is 45 kg, which is listed, so suggest 45 kg. For a 62% target, 37.2 kg, suggest 35 kg. The labels are settings, not a claim about actual resistance at the handle.

For a working set at 60 kg reaching its upper repetition limit, machine-based progression suggests the next setting, 65 kg, and the lower repetition limit. At 100 kg there is no listed higher setting; inform the user and permit manual entry or switching to a custom increment without claiming an unlisted value is a machine setting.

## Progression by set

For an 8–12 repetition range:

| Actual result | Next suggestion |
|---|---|
| 60 kg × 6 | 60 kg × 7 |
| 60 kg × 10 | 60 kg × 11 |
| 60 kg × 12 | Next weight by chosen method × 8 |

For the last row, with a symmetric bar and selected 1.25 kg plates, an available 62.5 kg total is the next feasible value if no smaller increase can be formed. With a custom increment of 5 kg, suggest 65 kg instead; custom increments do not depend on equipment inventory. Suggestions remain editable and use the actual result when determining the following workout.

For a manually recorded total of 67.5 kg that cannot be assembled using the selected inventory, preserve the result and suggest the nearest feasible higher total; the actual result depends on the inventory. If equipment data is incomplete, identify the missing information and offer to complete it, switch to a custom increment, or enter a weight manually.

## Open calculation decisions

1. Exact internal precision and display rounding for kg/lb; converting back and forth must not accumulate rounding error or change the underlying recorded value.
2. Detailed representation and calculation of adjustable dumbbell compatibility and independent sides; independent-side machines initially permit manual entry.
3. Search bounds and validation for plate weights. When equally few plates can make a load, prefer the arrangement with larger plates; define exact tie ordering if needed.
4. Invalid input handling and exercise types where a simple added weight of zero does not fully describe the load.
5. How the proposed warm-up guidance below becomes specific editable presets, including rounding, exercise classification and rest behavior.

## Further agreed calculation behavior

- A custom increment uses the selected weight-entry convention; for a one-side entry, a 2.5 kg increment means 2.5 kg per side. Label the convention clearly.
- Changing kg/lb changes display units without progressively rounding the stored value. Display precision and conversion implementation remain open.
- Meeting **or exceeding** the upper repetition limit triggers the selected weight progression and reset to the lower repetition limit.
- A working set requires both weight and repetitions to be marked complete. Zero added weight can be entered; it does not represent body weight. More detailed exercise-specific representations remain open.
- For a plate-loaded machine with unknown starting resistance, warm-up percentages can apply to the entered plate load, explicitly labeled as excluding the unknown resistance; manual entry remains possible.
- For adjustable dumbbells, propose the arrangement for one dumbbell using known handle mass and selected compatible plates, assuming the same arrangement for a pair. Missing data allows manual entry; detailed compatibility and loading rules remain open.

## User-proposed warm-up guidance — design input

The following is the user's starting guidance for editable suggestions, not an implemented algorithm or a validated universal training prescription:

- For a first compound exercise for a muscle group, propose 3–4 warm-up sets; for very heavy work of 1–5 repetitions, 4–5; for lighter work of 12 or more, 2. For later exercises for the same group and isolation exercises, propose 0–1.
- For multiple sets, begin around 40–50% of the working weight and end around 85–90%, with 1–2 intermediate steps where applicable. For one set, use around 65–70%.
- Begin around half the target working repetitions, capped around 6–8; repetitions decrease as the load increases. The final set at 85–90% uses 1–2 repetitions.
- Warm-up effort should remain light and dynamic, away from failure. The user proposes brief rest during light sets and 2–3 minutes before the final warm-up and working set.

### Proposed initial editable presets

Use the exercise's primary muscle group and its position in the unit: if an earlier exercise has the same primary group, suggest fewer warm-up sets; otherwise use the applicable first-exercise template. Isolation exercises also receive the reduced suggestion. This is a convenience heuristic, not a determination of readiness. Users can change or omit all sets.

The user supplied the following **proposed count ranges** for the first compound exercise for a primary muscle group:

| Planned working repetitions | Suggested warm-up count |
|---|---|
| 1–5 | 4–5 |
| 6–8 | 3–4 |
| 8–12 | 2–3 |
| 12–15+ | 1–2 |

For deterministic defaults, use the planned target repetitions of the first working set and select the lower count in each range: 1–5 → 4 sets; 6–8 → 3; 9–11 → 2; 12 or more → 1. Thus 8 belongs to the 6–8 range and 12 to 12+. For later exercises with the same primary group, and for isolation exercises, suggest 1 set regardless of repetitions; the user may remove it or otherwise change the count. The way to classify compound and isolation exercises remains open. The user's rationale is recorded as design context, not as a guarantee that a particular person is warmed up or that load implies a particular percentage of their maximum.

| Number of warm-up sets | Percentages of first working-set weight |
|---:|---|
| 1 | 70% |
| 2 | 50%, 90% |
| 3 | 50%, 70%, 90% |
| 4 | 50%, 65%, 80%, 90% |
| 5 | 50%, 60%, 70%, 80%, 90% |

These are proposed fixed, rounded values within the discussed ranges; one set at 70% is an exception to the 90% final-set value. Actual suggested equipment loads use the established rounding-to-feasible rule.

For a deterministic starting proposal, take half the planned working repetitions, round up, use at least 1 and cap at 8 for the first warm-up set. With two or more sets, set the final repetitions to the smaller of 2 and that first value; interpolate intermediate repetition counts evenly, rounding up and never below 1. For a single set use the first value. This avoids suggesting more repetitions in the last set than the first when the target is very low; the user can change all values. Example for 10 working reps and three sets: 50% × 5, 70% × 4, 90% × 2. This exact interpolation is a proposal for review, not an agreed training rule.

Display a suggested rest time for warm-ups rather than automatically starting a warm-up timer. The existing automatic timer remains limited to working sets. The proposed 2–3 minute guidance before the last warm-up and working set is informational; the exact display and whether the user can configure it remain open.

Examples should later become calculation test cases, with additional kg/lb and edge-case tests once the open decisions are resolved.
