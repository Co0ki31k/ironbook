# Phase I — Equipment and Training Decisions

**Status:** Decisions recorded from the product discussion; implementation details and the open questions below still need design. This document refines the [Phase I plan](plan.md) and [working use cases](use-cases-and-equipment.md).

## Places and equipment

- A user can create multiple private places and later select multiple gyms. Places form a list. A workout plan/unit is associated with one gym when a place is selected; using the same plan at a different gym means creating a separate plan. The place for a session is known before it starts and does not change during the session. Using a place remains optional.
- A private place has a name and may have an address and description. Copying places is deferred. Private users add as much equipment as they want; later, gyms maintain their own public equipment catalogs. Private data can be incomplete and entered gradually.
- Equipment has a name and description. Include equipment photos in Phase I for testing, including private places. Similar machines are separate items, distinguishable by description and photos. Start with an empty inventory and add items by type. Photo storage, backups, limits and optionality still need design.
- Suggest common bar presets such as 20 kg and 10 kg, with a custom option. Store plate denominations and counts of individual plates in a shared place inventory. Assume symmetric bar loading for automatic calculations, prefer an arrangement with fewer plates, and exclude collar mass.
- Record fixed dumbbell and kettlebell weights and quantities. Let users choose whether a dumbbell entry refers to one dumbbell or a combined load. Adjustable dumbbells may be modeled like small bars with a known handle mass and compatible plates; additional plates can be entered. Adjustable kettlebell settings can be entered as a list. Exact compatibility and loading rules remain open.
- Stack settings may be generated from a range and increment and then edited for irregular values. Plate-loaded machines distinguish a shared loading point from separate sides. Unknown starting load remains unknown; record plate load without inventing total resistance. For independent stacks or sides, manual recording is acceptable initially; automatic support can be considered later.

## Exercises and plans

- Machines are distinct equipment items. An exercise may reference a specific machine and its settings, but this is optional. Multiple exercises can refer to the same machine; independent exercises can have the same name while referring to different machines. Exercise-specific and equipment-specific settings must be distinguished during design.
- Support different equipment configurations for the same exercise across separate place-specific plans in Phase I. The exact relationship between these configurations and independent exercise histories needs clarification.
- No special occupied-machine or wrong-gym workflow is required by the current decision. Users may save workout changes to a personal plan when they choose; the exact interaction with equipment substitution needs clarification.
- A user has a default weight-entry convention and can choose a convention for an exercise. Total, both sides of plates, and one side are supported for barbells. Showing a calculated equivalent is desirable where reliable, but its placement is not a priority. Manual entries remain allowed even when inventory does not confirm the load.

## Warm-up and progression

- Users can manually add and configure warm-up sets per exercise. The app can suggest settings based on the number of sets, while users retain control. Warm-up sets are displayed as tasks and are not saved to workout history. Completed sets can be unchecked; whether they remain visible or disappear depends on the selected workout view (list or interactive). If the first working set is recorded while configured warm-up sets remain unchecked, those warm-ups are automatically marked complete, with visibility determined by the view. They remain separate from working-set progression and statistics. Defaults and persistence during an interrupted session need design.
- Warm-up targets use the planned first working-set weight. A later change does not retroactively affect completed warm-ups; the user's described behavior is that it affects the next workout. Exact behavior for unfinished warm-ups and first-time planning must be clarified.
- When equipment data is available, use the next available weight for progression; do not introduce a separate large-jump threshold. If data is incomplete, provide a visual progression indication and encourage users choosing progression to enter equipment information. Exact manual fallback and relation to the configured increment remain open.
- If no higher weight exists, allow switching to manual entries. A future profile badge or other gamification is an idea for a later phase, not an MVP requirement. Progression remains independent per working set. Set matching when the number or order changes is open.
- Treat same-named exercises configured for different machines as independently created exercises where the user chooses that arrangement; do not assume machine loads are equivalent.

## History and future coaching

- Show the selected place and equipment in history when available. Keep the historical place and equipment names when those items are renamed or deleted. The separate rule for shared exercise renaming remains in the product specification.
- In the future coaching phase, agreeing to connect with a coach shares the trainee's equipment inventory. This decision concerns equipment; existing restrictions on workout history, body weight and personal plans continue to apply. Exact scope, withdrawal and relationship termination behavior require design.

## Questions to resolve

1. When a plan has no associated place, how is the optional place chosen before a session?
2. Are photos and descriptions optional, and how are photos stored, backed up and limited?
3. For adjustable dumbbells, are plates a separate compatible inventory or shared with other equipment, and must the two dumbbells load symmetrically?
4. How do configurations in separate place-specific plans relate to independent same-named exercises and progression history?
5. For incomplete equipment data, what is the exact visual/manual progression fallback, and how does the configured progression increment relate to the next available weight?
6. Do unfinished warm-up suggestions remain unchanged when the working weight changes during a workout? How are warm-up completion and undo retained while an interrupted session is resumed, without saving them to completed history?
7. Which warm-up settings should be suggested for each number of sets, and how are sets matched between sessions when their count or order changes?
8. What exactly is shared with a coach when a relationship is accepted, and can equipment sharing be separately withdrawn?
