# Phase I — Use Cases and Equipment Model

**Status:** Working use cases and conceptual model, not an implementation or an approved database schema. Detailed decisions and remaining questions are maintained in [equipment and training decisions](equipment-decisions.md). See also the [Phase I plan](plan.md) and [product specification](../product-specification.md).

## Scope and principles

The trainee can plan and record workouts offline, with no account, place, or equipment catalog. A private training place can describe a home setup or gym. Equipment information improves suggestions when sufficient; otherwise the user can enter a weight manually. Public gyms, verification, coach access, and synchronization are later work. Exercise identity, equipment identity, plan prescriptions, and performed sessions are separate concepts.

## Use cases

### UC1 — Start without a place

**Starting point:** A user launches the app for the first time, with no network connection.

1. The user completes or skips onboarding; selected settings or specified defaults are saved.
2. The user chooses or creates a plan and starts a workout without selecting a place.
3. The user enters exercises and weights manually, completes the session, and sees it in history.

**Result:** No gym or equipment data is required; the session persists after restart. If the starter plan was skipped or removed, the app offers plan creation.

### UC2 — Create a private training place

**Starting point:** A user wants equipment-aware suggestions for a home setup or gym.

1. The user creates a private place and names it.
2. The user adds the available equipment and the information they know, such as a bar's weight, plate denominations, or stack labels. Plate quantities are not recorded; calculations assume sufficient plates of each listed denomination.
3. The place and equipment persist locally; the user can return to edit them.

**Alternative:** The user does not know a value. The place can still be used, but any calculation requiring that value must not claim a verified result. Exact mandatory fields and validation remain open.

### UC3 — Prepare a plan with equipment

1. The user creates or chooses a program and plan, adds an exercise, and configures its working and warm-up sets.
2. The user may associate the unit with one private place, select suitable equipment for an exercise, and choose how weight is entered. A separate plan is used for another place.
3. The user saves the plan; the exercise remains conceptually distinct from the particular equipment.

**Alternatives:** No place or equipment is selected; manual entry remains possible. Editing a plan must not change existing session history.

### UC4 — Train with a barbell and warm-up suggestions

1. The user starts a planned workout, using the unit's place and barbell where applicable.
2. The planned first working-set weight is 100 kg total. The user has configured warm-up targets of 50%, 75%, and 90% and their repetitions.
3. The app calculates targets of 50, 75, and 90 kg. For each, if the required data is available, it finds a feasible total at or below the target and shows the suggested weight and plate arrangement per side.
4. The user may choose a different available weight or enter a value. Working sets are recorded and the session is saved automatically. Warm-up tasks can be checked and unchecked and are not saved to completed workout history; their visibility depends on the workout view. Recording the first working set automatically checks remaining warm-up tasks.

**Alternatives:** If the empty bar is already heavier than the target, show the situation and let the user decide. If plate denominations or bar mass are unknown, show the target without claiming a feasible arrangement. Suggestions assume sufficient plates; actual availability can vary. Changing the working weight does not automatically recalculate the current warm-up suggestions; the user can edit remaining sets manually.

### UC5 — Train using a stack or plate-loaded machine

1. The user selects a machine associated with the place and records its displayed stack setting or plate load, using a clearly labeled entry meaning.
2. If the available settings are sufficiently described, the app proposes feasible warm-up weights and later progression.
3. The user can override the suggestion or manually enter the recorded value.

**Alternatives:** Missing settings do not prevent training. A stack label or plate load is not presented as a measured resistance at the handle. Machine behavior, asymmetric loading, and unknown starting resistance require further design.

### UC6 — Resume and preserve history

1. The user records part of a workout; the app closes or the phone locks.
2. The user returns and resumes the locally saved session.
3. After completion, the user edits the plan or equipment catalog and opens the recorded session.

**Result:** Recorded set values and their meaning are preserved, even if the original plan or equipment changes or is removed. Renaming a shared exercise still updates its displayed historical name, as specified. The exact equipment snapshot and deletion behavior require design.

### UC7 — Progress and recover data

1. Based on prior working sets, the app presents editable progression suggestions and, where supported by equipment data, the next feasible weight and size of the increase.
2. The user exports a versioned JSON backup and can export CSV for viewing.
3. Before restoring a JSON backup that replaces current data, the app asks for confirmation and automatically creates a recoverable backup of the current data, including private places and equipment.

**Alternatives:** There may be no higher feasible weight or equipment data may be incomplete; exact progression behavior remains open. Invalid-file handling and recovery location must be designed before import is implemented.

## Conceptual equipment model

This is a domain sketch, not a table design. Fields described as **candidate** need review; missing values must not be silently treated as zero.

| Concept | Purpose and candidate information | Open questions |
|---|---|---|
| Private training place | Locally owned named place containing equipment. Multiple places are supported; selecting one is optional. | Selection for a session without a planned place and future connection to a public gym; copying is deferred. |
| Equipment item | Identifiable item belonging to a place; type, name and optional description. Distinct machines can have distinct settings. | Quantity versus separate item identity, availability, deletion, and whether an item can be shared between places. |
| Barbell | Bar mass is needed for total-weight calculations; unknown mass does not prevent manual entry. | Other properties such as capacity and compatibility. |
| Plate inventory | Available plate denominations in a shared inventory, without quantities. Symmetric bar arrangements assume sufficient plates and prefer fewer plates; collar mass is excluded. | Compatibility and handling adjustable dumbbell plates. |
| Fixed dumbbells / kettlebells | Available individual weights and quantities. Dumbbell entry can represent one or a combined load. | How the chosen equipment and entry convention are represented for each exercise. |
| Adjustable dumbbells / kettlebells | A way to describe the actual selectable weights or a compatible loading configuration. | Which configurations are required in Phase I and how quantities and increments are expressed. |
| Stack machine | A specific machine and its selectable labeled settings, entered using a range and increment and then editable. | Units, starting resistance and independent stacks. |
| Plate-loaded machine | A specific machine with plate-loading information where known. | Symmetric versus independent sides, starting load, compatible plates, and whether a total can be meaningfully calculated. |
| Exercise equipment selection | Associates a plan occurrence or performed exercise with equipment, without defining the exercise itself. | Selection scope and behavior when training elsewhere or equipment is unavailable. |
| Recorded equipment and weight meaning | Preserves what was actually recorded when equipment or settings later change. | What to snapshot, how corrections work, and handling deletion without rewriting history. |

## Weight semantics and calculation examples

The following barbell modes are agreed as user preferences: total including the bar and plates; plates on both sides combined; or plates on one side, assuming symmetric loading. A per-exercise choice is supported. For a 20 kg bar and 25 kg of plates per side, the respective entries are 70 kg, 50 kg, and 25 kg. A conversion is only valid when the required mass and loading assumptions are known.

For a 100 kg first working set and a 75% warm-up target, the target is 75 kg. If the feasible totals immediately surrounding it are 72.5 and 77.5 kg, suggest 72.5 kg by default and offer user choice. Actual feasible totals depend on the bar and individual plate inventory; the values here illustrate rounding rather than a specific inventory.

**Open:** Detailed weight meaning for each equipment type; rounding precision and unit conversion; plate compatibility and missing inventory interactions. The progression method selects either equipment settings or a custom increment. Keep a distinction between a target, a feasible suggestion, and what the user actually records.

## Remaining decisions

See [equipment and training decisions](equipment-decisions.md#questions-to-resolve) for the current list. Examples and candidate fields above do not establish additional product requirements.
