# Phase I — Local Training App: Requirements and Schedule

**Status:** Planning draft incorporating the newly discussed private equipment scope. Estimates are proposals, not completion status. See the [product specification](product-specification.md) and [implementation roadmap](implementation-roadmap.md) for related requirements and order.

## Goal and boundaries

Deliver an Android Flutter training app that works offline, without an account or backend, and retains data locally. One device has one local trainee profile. The MVP supports Polish and English, training plans and sessions, private training places and equipment, equipment-aware weight suggestions, progress tracking, and data export and recovery. Selecting a place is optional. Accounts, synchronization, coaching, public gym profiles and their verification, geolocation, partner matching, and offers are later phases; Phase I prepares data for synchronization but does not implement it.

## Functional requirements

### Application and settings

- **F1.1** Provide Training, Statistics, and Settings navigation. Support Polish and English, kg/lb, system theme selection, workout view, and rest settings. Persist settings locally.
- **F1.2** Offer skippable onboarding for language, units, theme, rest time, and starter plan. If skipped, use supported device language or Polish, kg, system theme, 90 seconds, and an active starter-plan copy.
- **F1.3** Without a plan, offer plan creation and a quick workout. The starter template remains unchanged when its editable copy is modified or removed.

### Exercises and plans

- **F1.4** Provide an offline exercise catalog with translated names, search, filters, categories, and favorites. Allow custom exercises with a name and one primary muscle group; secondary groups are optional and drawn from the fixed list in the specification.
- **F1.5** Allow exercises to be shared across plans or copied with independent future history. Renaming a shared exercise updates its displayed historical name without changing recorded set values; plan-specific configuration remains separate.
- **F1.6** Allow multiple programs containing ordered workout plans, one main program, and optional weekday assignments. A weekday does not restrict when a plan can be started.
- **F1.7** Configure working sets, repetition ranges and per-set overrides, progression increment, warm-up sets and suggestions, RIR, persistent notes, and an optional rest override as described in the specification. Initial working weight is not required.

### Private places and equipment

- **F1.19** Create and edit private training places, including a home setup, with equipment stored locally. Training and planning remain possible without selecting a place or entering equipment. Design for multiple places and a future public gym profile; public discovery and management are outside this phase.
- **F1.20** Represent barbells and their mass; plates by mass and available quantity; fixed or adjustable dumbbells and kettlebells; stack machines with their available weight steps; and plate-loaded machines. The exact fields, adjustable-equipment behavior and minimum supported configurations must be agreed in design. A machine's labeled weight is not necessarily its actual resistance.
- **F1.21** Allow a plan exercise or workout to use appropriate equipment from a selected place, while permitting manual entries if a place, equipment, or necessary data is missing. Preserve the equipment and recorded weight meaning in session history when the catalog later changes; the representation is a design decision.
- **F1.22** Support individual weight-entry preferences, including barbell total weight, plates on both sides combined, or plates on one side. Make the selected meaning visible and allow an exercise-specific choice. Only convert when the required equipment data and assumptions are known; avoid claiming a plate-loaded machine's unknown resistance.
- **F1.23** For supported symmetrical barbell configurations, suggest a feasible plate arrangement using bar mass, available plate quantities and weights. Show the arrangement per side and total; handle an impossible target and insufficient or missing data explicitly. Other equipment configurations require design before implementation.

### Equipment-aware warm-up and progression

- **F1.24** Allow configurable warm-up sets with repetitions and percentages of the planned first working-set weight. Calculate targets from that weight; when equipment data is sufficient, propose the closest feasible weight at or below the target and allow another available choice. Show the target and proposed actual weight. If no feasible weight is at or below the target, explain the situation and allow a user decision; if equipment data is missing, show the unrounded target without claiming availability.
- **F1.25** Preserve completed warm-up entries when the first working weight changes; allow recalculation of uncompleted suggestions and preserve user control over manual values. Exact recalculation interaction is open.
- **F1.26** Combine the existing per-set progression rules with available equipment weights. Show the proposed next feasible higher weight and size of the increase; the user can change the suggestion. Design how to handle an unavailable next weight, large jumps, and missing equipment data before implementation.

### Workouts and history

- **F1.8** Start a workout from a plan or as a quick workout. Record working-set weight and repetitions and optional per-set RIR; keep warm-up sets separate. Allow workout and exercise notes.
- **F1.9** Support list and focused views, navigation back to prior exercises, and correction of entries. During a session allow exercises to be added, removed, and reordered and working sets to be added or removed.
- **F1.10** Start the rest timer automatically after a working set, not a warm-up set. Automatically save an active session and allow resumption after closing the app or locking the phone.
- **F1.11** Confirm saving on completion. Ask whether session changes should be saved to a personal plan; optionally save a completed quick workout's structure as a new plan. Allow completed sessions to be viewed, edited, and deleted.
- **F1.12** Keep performed sessions independent of plan changes and deletion. Removing a plan or an exercise from a plan must not remove recorded history.

### Progress and statistics

- **F1.13** Suggest progression independently for each working set according to the repetition-range and weight-increment rules in the specification. Suggestions are editable; RIR does not affect them. Decide how sets correspond across sessions before implementation.
- **F1.14** Store one body-weight measurement per day, replacing an existing measurement for that date. Provide history, selectable moving-average period, independent raw/average visibility, optional target, and estimates based on a smoothed trend. Forecasts require at least seven measurements in the last 14 days and must be labeled as estimates.
- **F1.15** Provide configurable body-weight and exercise charts with metric and date-range selection. Exercise metrics include maximum working weight, tonnage, estimated 1RM, and manually entered actual 1RM. Exclude warm-up sets from working-set calculations; attribute sets and tonnage to the primary muscle group only.

### Data and backups

- **F1.16** Store data in SQLite with a domain model independent of storage details. Generate stable UUIDs on the client; retain timestamps and an owner/profile, keep synchronization metadata separate, and design deletions for later synchronization.
- **F1.17** Export a versioned full JSON backup and a CSV for spreadsheet viewing. Import JSON by replacing current data only after confirmation and an automatic backup of the existing state. JSON does not require a password; CSV import is not planned.
- **F1.18** Design migration and recovery for local data, including invalid backup handling. The bundled exercise catalog is application data, distinct from user data for future account linking.

## Design deliverables

Before implementation, prepare and review: MVP use cases and main/alternative flows; screen map and basic wireframes; domain entities and relationships; plan-versus-session lifecycle and exercise identity rules; private-place, equipment and weight-entry models; calculation rules and worked examples for barbell, stack machine, plate-loaded machine and dumbbell; SQLite schema and migration strategy; and assumptions for identifiers, ownership, timestamps, deletions, and future synchronization. Validate the designs against starting offline without a place, unavailable or incomplete equipment, resuming a session, changing a plan or equipment after training, and later linking a local profile or sharing gym information with a coach.

## Proposed schedule

Estimates are **working days for one developer**, assuming full-time availability and a usable Flutter development environment. They are initial planning estimates, not commitments or elapsed calendar dates; review them after the foundation stage. Work is sequential where dependencies require it, with testing and documentation throughout.

| Stage | Estimate | Work and deliverable | Exit criteria |
|---|---:|---|---|
| 1. Foundations | 8–12 days | Design use cases, screens, equipment and data models, calculation examples, SQLite and synchronization assumptions. | Core flows and weight semantics reviewed; unresolved decisions recorded with an owner and deadline. |
| 2. Mobile foundation | 5–8 days | Organize Flutter and persistence; navigation, localization, settings, onboarding. | Android app starts offline; both languages and settings work and persist after restart; onboarding can be skipped with specified defaults. |
| 3. Equipment, exercises and plans | 17–25 days | Private places and equipment, exercise catalog, programs, plans, equipment selection and starter copy. | A local place and its supported equipment can be configured; plans work with or without a place and persist after restart. |
| 4. Workout and history | 14–21 days | Planned/quick workouts, weight entry, sets, views, notes, timer, autosave, resumption and history. | Offline workout resumes; entry meaning is clear; later plan or equipment changes preserve recorded history. |
| 5. Calculations, progression and statistics | 16–24 days | Feasible weights and plate arrangements, warm-up suggestions, progression, measurements, charts and records. | Worked examples and edge cases match agreed rules; suggestions remain editable and forecasts respect eligibility. |
| 6. Backups and MVP completion | 8–12 days | JSON/CSV export, JSON import and recovery including equipment, end-to-end verification and documentation. | A full backup restores data; replacement requires confirmation and creates a recoverable backup; acceptance scenarios pass. |
| **Total** | **68–102 days** | Initial estimate, excluding external review delays and major scope changes. | Phase I acceptance criteria below. |

Dependencies: stage 2 follows design; stage 3 uses persistence and navigation; stage 4 uses plans and equipment; stage 5 uses recorded history and equipment data; stage 6 verifies recovery and the complete flow. Agree core equipment and weight semantics before stages 3–4, calculation details before stage 5, and backup recovery before stage 6. Exact timing depends on availability and decision turnaround.

## Phase I acceptance

1. A user can install and use the app offline without registering, choose Polish or English, and retain settings across restarts.
2. A user can create a plan, perform and resume a workout, complete it, and view or edit its history. Plan changes and deletion preserve performed history.
3. A user can create a private place, configure supported equipment, and train with or without selecting it. Equipment changes do not rewrite recorded session values.
4. A user can see and edit equipment-aware warm-up and progression suggestions and a feasible barbell plate arrangement; missing data and impossible targets are communicated. Entry modes have clear meanings.
5. A user can perform a quick workout and use body-weight tracking, exercise charts, and records under the documented rules.
6. A user can export JSON and CSV and restore JSON including private places and equipment, with confirmation and an automatic recoverable backup before replacement. Invalid input handling follows the agreed design.
7. Relevant checks cover core flows, persistence, calculation examples and edge cases, localization, and backup/recovery on Android. Known limitations and setup and operating instructions are documented.

## Decisions to resolve

| Decision | Needed by |
|---|---|
| Equipment fields, adjustable weights, asymmetric machines, and minimum supported configurations | Stage 1, before stage 3 |
| Session snapshots, per-exercise entry preferences and changing or unavailable equipment | Stage 1, before stages 3–4 |
| Plate arrangement selection, rounding ties, no feasible lower weight, and units/precision | Stage 1, before stage 5 |
| Warm-up recalculation after a change; handling manually edited suggestions | Stage 1, before stage 5 |
| Progression with large jumps, no higher weight or missing equipment data; matching sets across sessions | Before stage 5 |
| Bundled exercise catalog and starter-plan contents | Stage 3 |
| Estimated 1RM formula and valid repetition range | Stage 5 |
| Body-weight trend and forecast method and data horizon | Stage 5 |
| Automatic pre-import backup location, recovery, and invalid-file handling | Stage 6 |
| Detailed synchronization API and conflict resolution | Later phase; preserve the Phase I architectural assumptions |

These items remain open until agreed. Estimates and acceptance criteria should be updated if the scope or decisions change.
