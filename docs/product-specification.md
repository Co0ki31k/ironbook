# Ironbook — Product Specification

**Status:** Working specification based on current decisions.

**Product:** A configurable strength-training journal for trainees, later extended with coaching and social features.

This document defines the agreed product scope and behavior. The [roadmap](implementation-roadmap.md) summarizes the order of work. Section 15 lists unresolved decisions. Features described here are planned, not necessarily implemented.

## 1. Product goal

Ironbook supports fast workout logging and progress tracking. Users can start locally without an account. Later phases add accounts, synchronization, a coaching dashboard, workout partner matching, and a coaching services directory.

## 2. First release: local MVP

The first release is a Flutter application for Android that works without an account or backend connection. Each device has one local trainee profile. The interface supports Polish and English from the start and has three tabs:

1. **Training:** programs, workout plans, and active workouts.
2. **Statistics:** body-weight measurements and configurable exercise charts.
3. **Settings:** language, units, theme, workout view, rest periods, backups, and other settings.

The short onboarding screen can be skipped. Sections 3–11 define the local MVP. Accounts, synchronization, the coaching dashboard, location features, and offers are subsequent phases. A separate mobile app for coaches and a marketplace are outside the current implementation plan.

Phase I also includes optional private training places and equipment-aware weight suggestions, as detailed in the [Phase I plan](phase-one/plan.md). Public gym profiles and their management are later work. Equipment-specific design details remain open in that plan.

The [equipment decisions](phase-one/equipment-decisions.md) provide further detail on private equipment, warm-up behavior, progression methods, and future equipment sharing with a coach.

The [statistics decisions](phase-one/statistics-decisions.md) refine the requirements below, including repetition-only exercises and the accepted initial 1RM, trend and forecast designs; remaining thresholds and eligibility details require validation.

## 3. Onboarding and defaults

Onboarding allows users to choose language, weight unit, theme, default rest time, and whether to add a starter plan. If skipped, defaults are the device language when supported or Polish otherwise, kilograms, the system theme, a 90-second rest period, and an added and active starter plan.

Without a plan, Training offers **Create plan**. A workout starts from a plan; a separate quick-workout flow is outside Phase I. The starter plan is a shared template; each user works on an editable and removable copy, leaving the original intact. An optional beginner plan wizard may be added in a later MVP iteration.

## 4. Training programs and plans

The model has three levels:

1. **Training program (`TrainingProgram`):** a named collection such as Push/Pull/Legs. Users can have several programs and designate one as their main program.
2. **Workout plan (`WorkoutTemplate`):** a tile within a program, optionally assigned a weekday and ordered among other tiles.
3. **Plan exercise:** an ordered reference to an exercise in the user's collection. Exercise settings and history are shared when the same exercise is used in multiple plans.

A weekday is an organizational hint and does not prevent starting a different plan or training on another day. Workout reminders and geofencing are outside the local MVP.

Users can create programs and plans, select a main program for suggestions, assign weekdays and reorder tiles, and start a workout from a plan.

## 5. Exercise library and model

The bundled exercise library works offline and provides translated names, search, filtering, categories, and favorites. Filters may use muscle groups and equipment from the bundled catalog. Equipment is not required for user-created exercises.

A custom exercise requires a name and one primary muscle group; secondary groups are optional. The fixed list is: chest, latissimus dorsi, trapezius, spinal erectors/lower back, shoulders, biceps, triceps, forearms, quadriceps, hamstrings, glutes, calves, abdominals, and other/unassigned. Users cannot create groups. Secondary groups are descriptive; sets and tonnage are attributed only to the primary group to avoid counting the same work more than once.

An exercise is shared between plans by default, including its name, settings, note, and history. Users can copy an exercise, including its name, settings, and note, while starting an independent empty history. Later edits to a copy do not affect the original. Renaming a shared exercise changes the displayed name in older history without changing recorded set values. A different exercise variation should be a separate copy. The same exercise cannot occur twice in one plan; an independent copy can be used instead.

Exercise recording supports weight and repetitions, repetitions only, and assistance load and repetitions. An exercise without additional weight and a weighted version are independent exercises. Assistance is distinct from lifted weight: greater assistance does not automatically mean greater progress. Specific eligibility for load-based calculations depends on the meaning of the recorded load; no actual resistance is inferred from a machine label.

A persistent technical note is sufficient for machine settings; separate seat-height fields and similar fields are not required. Notes can belong to a workout or exercise.

## 6. Exercise configuration

An exercise can specify a fixed planned number of working sets; a default repetition range and optional per-set overrides; a weight progression increment; the number of warm-up sets; a suggested warm-up weight as a percentage of the first working-set weight and suggested repetitions for each warm-up set; whether RIR is enabled; and an optional persistent note. A user finds the initial working weight through trial and error; it is not required in the plan.

## 7. Active workout logging

For each working set, users record repetitions and, where applicable, weight or assistance; RIR is optional and stored separately per set. Repetition-only exercises do not require a weight. RIR can be enabled per exercise; a global setting can hide it everywhere. It is informational and does not affect progression.

Warm-up sets are separate and excluded from progression and calculations based on working sets. The app may suggest their weights and repetitions from exercise settings and the first working-set weight; users can change each suggestion.

Warm-up targets are percentages of the planned first working-set weight. Where sufficient equipment data exists, the default suggestion is the closest feasible weight at or below the target; users may choose another. Missing data and cases with no feasible lower weight must be made clear. Equipment-aware progression and plate arrangements are described in the Phase I plan.

Two globally selected views are available: **List view**, showing exercises and sets in a list, and **Focused view**, showing one exercise at a time and moving to the next after its sets are complete. Users can return to earlier exercises and correct entries.

A rest timer starts automatically after a working set, never after a warm-up set. Its global default is 90 seconds, with an optional per-exercise override. Active sessions save locally automatically and can be resumed after closing the app or locking the phone. Completing a workout shows a save confirmation; a summary screen is not mandatory.

During a session users can add, remove, and reorder exercises and add or remove working sets. Afterward, the app asks whether to save changes to a personal plan. Trainee changes to a coach's plan apply only to the performed session. Completed workouts can be edited or deleted; deleting a plan or an exercise from a plan does not delete historical sessions.

## 8. Progression rules

Progression is independent for each working set:

1. Below the upper repetition limit, suggest the same weight and one additional repetition next time.
2. On reaching or exceeding the upper limit, suggest the next load according to the selected plate, machine-setting, or custom-increment method. For assistance, suggest the next lower available assistance; for repetition-only exercises, indicate that the limit is reached without automatically adding weight.
3. After increasing load or reducing assistance, reset the suggested repetitions to that set's lower limit.
4. Pre-fill suggestions for the user to accept or change. RIR does not affect them.

Match working sets by their order between sessions. A new set without a previous counterpart has no imposed suggestion. Corrections and deletions of historical results affect future suggestions. See [equipment decisions](phase-one/equipment-decisions.md) for missing equipment and method-specific behavior.

## 9. Body-weight statistics

Users can record one measurement per day; entering another for the same date replaces it. A single global kg/lb setting applies throughout the app. Users can view measurement history, select a 3–7 calendar-day moving-average period, independently show or hide raw measurements, the average and the trend, and set an optional target weight without a deadline. Average available measurements within the window and show the count; do not invent missing measurements.

The initial trend design fits a linear regression to dated measurements in a selected recent 30-, 60- or 90-day period, subject to minimum data requirements. Users can choose a future date for an estimated weight and see an estimated date when the trend may cross their target. Forecasts are labeled estimates, require at least seven measurements in the last 14 days, and are limited to 90 days. Do not give a target date for a flat or contrary trend. Thresholds and edge cases require validation; see [statistics decisions](phase-one/statistics-decisions.md).

## 10. Exercise statistics and records

Statistics is configurable: users can add a body-weight chart and exercise-history charts with an exercise, metric, and time range. Weight-based metrics include maximum working weight, tonnage (sum of `weight × repetitions`), estimated 1RM, and actual 1RM where meaningful. Repetition-only exercises support maximum repetitions in a working set and total working-set repetitions per workout; assisted exercises show assistance and repetitions without ordinary tonnage or estimated 1RM. A chart point represents one workout. Independent exercises are shown separately. Include completed working-set counts by primary muscle group, excluding warm-ups; group-level tonnage, if displayed, must not imply comparable effort across equipment types. Changing the primary group reassigns the exercise's historical results to its current group.

Actual 1RM is entered manually under Records, separately from normal sets, linked to an exercise and date, and need not originate in a recorded workout. For eligible working sets of 2–10 repetitions, estimate 1RM using `weight × (1 + repetitions / 30)`; show the source set and label the result as an approximation. RIR is not automatically added. Renaming an exercise updates its displayed name in records. When deleting an exercise, the user's choice to retain or delete its history also applies to manual records; warn and confirm before deletion. See [statistics decisions](phase-one/statistics-decisions.md) for eligibility and limitations.

## 11. Local data and backups

Users can export a full versioned JSON backup, export CSV for viewing in a spreadsheet, and import JSON. The full backup includes private places, equipment and photos, workouts, body-weight measurements and goal, and manual records. Import replaces current data after confirmation and automatic creation of a backup of the existing state. JSON files do not require a password. JSON is for restoration; CSV import is not currently planned. Backup storage and recovery, photo representation, and handling invalid files require design before implementation.

## 12. Cloud-ready data architecture

Flutter stores local data in SQLite; a later phase synchronizes through a Spring Boot API with PostgreSQL. The domain model is independent of local database details.

- Stable UUIDs are generated on the client; records carry timestamps and an owner/profile. Synchronization metadata is separate from domain data.
- Deletions can be synchronized, and the JSON backup format is versioned.
- A local profile can later be linked to an account, migrating plans, workouts, measurements, notes, custom exercises, and settings. The bundled exercise library is part of the app and is not migrated as user data.
- If the same workout is changed on two devices, synchronization detects the conflict, presents both versions, and lets the user choose which to keep.
- After account creation the app remains usable offline, with mobile statistics and suggestions calculated locally. The backend supplies the same metric definitions to the web dashboard so results agree.

## 13. Future coaching module

After the local MVP, trainees use the mobile app and coaches use a responsive React dashboard designed primarily for desktop. A separate coach mobile app is deferred.

### Accounts and pairing

Trainee and coach roles are separate; each account has one role. A trainee can have one active coach; a coach can have many trainees. Either side can initiate a relationship using a private, single-use code or link valid for 24 hours, and the other side must accept. Either can end the relationship, immediately revoking coach access.

### Consent and data visibility

Accepting a coach relationship shares the trainee's equipment inventory; its exact scope and whether it can be withdrawn separately remain to be designed. Connecting does not automatically share all history. Coaches see explicitly shared workouts; completed workouts from an actively coached plan are shared automatically. Other workouts are private unless individually shared. Body-weight measurements and personal programs/plans require separate consent or sharing. Access and sharing can be withdrawn. Trainees can ask questions about any plan through comments on a plan or exercise, rather than a separate chat.

### Plans and coaching

- Coaches can send several plans, choosing whether each is actively coached or a copy/template for independent use. Only one plan can be actively monitored at a time; the trainee manages independent copies.
- Assigned or changed plans become available to the trainee, who chooses whether to make one the main plan. The app notifies the trainee of assignments and changes.
- Plan changes do not alter started or completed sessions. Trainees can deviate during a workout without changing the shared plan; the dashboard compares plan and execution.
- Coaches cannot edit trainees' recorded entries, but can view them and provide feedback. Notes on workouts or exercises may be one-time or persistent. A one-time note is marked/archived automatically after the next relevant workout; a persistent note remains until changed or removed.
- New coach notes notify the trainee. A new workout appears in the coach's list without a separate notification. Coach notifications are in the web dashboard; email and push notifications are not initially planned.

## 14. Later modules

Later modules include accounts and synchronization, coaching, location-based gym presence and workout partner matching, and a trainer directory with two-way postings and offers. See the [implementation roadmap](implementation-roadmap.md) for their planned order. Geofencing and matching details require design, including whether presence suggests or automatically starts a workout. A separate coach mobile app and marketplace are possible later extensions outside the current plan.

## 15. Decisions deferred

- Exact equipment/load eligibility for estimated 1RM and tonnage, and validation of trend and forecast thresholds.
- Exact bundled exercise catalog and starter-plan contents.
- Repetition-only configuration and assisted-exercise edge cases.
- Automatic pre-import backup storage and recovery, and invalid-file handling.
- Detailed synchronization API and conflict-resolution design.
- Geofencing, partner matching, the coach mobile app, and marketplace details.
