# Ironbook — Implementation Roadmap

The [product specification](product-specification.md) defines product requirements and open decisions. This roadmap describes the agreed order of work, not its current status.

Work proceeds through four phases. Each design stage ends with agreed, verifiable artifacts; each implementation stage ends with a working, verifiable product increment. Testing and documentation continue throughout.

### Phase I — local training app

1. **Foundations:** agree the MVP scope, use cases, screen flows, equipment and domain models, weight calculation rules, SQLite approach, and future synchronization assumptions. Outcome: a coherent foundation for implementation.
2. **Mobile foundation:** organize Flutter and local storage; build navigation, Polish/English localization, settings, and skippable onboarding. Outcome: an Android app with persistent settings and main navigation.
3. **Equipment, exercises and plans:** build private places and equipment, the offline library, custom exercises, programs, plans, configuration, and editable starter-plan copy. Outcome: users can prepare a local plan with optional equipment selection.
4. **Workout and history:** implement planned and quick sessions, sets, RIR, notes, timer, workout views, autosave and resumption, history, editing, and deletion. Outcome: a complete offline plan-to-workout-to-history flow.
5. **Calculations, progression and statistics:** implement equipment-aware weight and plate suggestions, progression, body-weight measurements, exercise charts and records; decide and document formulas. Outcome: users can analyze history and receive suggestions using explicit rules.
6. **Backups and MVP completion:** add versioned JSON export/import, CSV export, confirmation and automatic backup before replacement; verify core flows and data persistence. Outcome: a standalone offline app with data recovery.

### Phase II — accounts and synchronization

7. **Backend and infrastructure:** design API contracts and set up Spring Boot, PostgreSQL, migrations, development configuration, authentication and authorization foundations. Outcome: an API service running with its database.
8. **Accounts and synchronization:** add registration, login, local profile linking, offline change queue, safe retries, deletion synchronization, conflict resolution, and status display. Outcome: local data can be associated with an account and synchronized while offline work remains possible.

### Phase III — remote coaching

9. **Coach dashboard and relationship:** build the responsive React dashboard, coach login, invitations, acceptance, termination, trainee list, consent, and access control. Outcome: accounts can be connected with limited, revocable access.
10. **Coaching plans, feedback, and reports:** implement plan delivery, shared session and progress views, notes, specified notifications, plan-versus-execution comparison, and reports. Outcome: a coach can send a plan and review and comment on a trainee's shared workout.

### Phase IV — social features and final release

11. **Gym presence and partner matching:** design and implement clubs, consent, presence visibility and expiry, recommendations, blocking and reporting. Manual presence can precede geofencing. Outcome: users control presence sharing and can receive partner suggestions.
12. **Coach directory and offers:** add instructor search and filters, trainee requests, and coach offers. Outcome: both parties can discover potential collaborations.
13. **Integration and final release:** verify end-to-end flows, offline synchronization, permissions and revocation; prepare integration and demonstration tests, deployment configuration, HTTPS, secrets outside the repository, backups, monitoring, operating instructions, and thesis materials. Outcome: the full system can be reproduced and demonstrated.

A separate coach mobile app and marketplace are future ideas, not requirements for completion of this plan.

See the [Phase I plan](phase-one/plan.md) for detailed scope, open decisions, acceptance criteria, and indicative estimates. Public gym profiles and their administration are later work.
