---
document_id: "18"
corpus_role: "active_agent_knowledge"
authority: "supplemental_adapted_educational_and_troubleshooting"
primary_environment: "Fortnite Creative"
status: "production-ready"
must_not_override_official: true
source_basis: "Consolidated and editorially rewritten from the 88-unit supplemental draft"
last_updated: "2026-08-03"
---

# Supplemental Educational, Social, Troubleshooting, and Reference Knowledge

## Document Purpose

Provide practical teaching sequences, build-planning templates, nonviolent Creative examples, playtest methods, and troubleshooting patterns that supplement the official Fortnite Creative units in files `03`–`07`.

This document is **adapted guidance**, not an official Device reference.

## Authority Boundary

Use this document for:

- lesson and workshop sequences;
- planning cards and worksheets;
- adapted nonviolent examples;
- social-design implementation prompts;
- playtest and observation templates;
- troubleshooting order and test discipline.

This document must **not** define or override:

- Device option names or option values;
- Events or Functions;
- UI paths or keyboard bindings;
- platform limits or undocumented runtime behavior;
- environment availability;
- publishing requirements;
- Verse syntax or API signatures.

For technical claims, retrieve the relevant official unit in `03`–`07`, use `01_EPIC_GAMES_DOCUMENTATION_INDEX.md` for the official source, and use `15_GLOSSARY_FORTNITE_CREATIVE.md` only for definitions. File `11` remains the primary owner for research, ethics, learning theory, and social-mechanic design.

## How to Use This Document

1. Resolve the environment first. The examples below are for **Fortnite Creative** unless a section explicitly says otherwise.
2. Select a practical pattern or template.
3. Identify every Device involved.
4. Verify each Device separately in `06` or the relevant official unit.
5. Write every connection as `Source Device — Event → Receiving Device — Function`.
6. Test in Play Mode, change one variable, and record the result.

## Quick Topic Index

- [Official Reference Additions](#official-reference-additions)
- [Teaching and Workshop Sequences](#teaching-and-workshop-sequences)
- [Planning Templates](#planning-templates)
- [Social-Design Implementation Cards](#social-design-implementation-cards)
- [Adapted Creative Build Patterns](#adapted-creative-build-patterns)
- [Playtesting and Troubleshooting](#playtesting-and-troubleshooting)
- [Accessibility and Safety](#accessibility-and-safety)
- [Delivery and Documentation](#delivery-and-documentation)

---

# Official Reference Additions

## Fortnite Documentation Hub

- **Source type:** Official Epic reference hub
- **Official URL:** https://dev.epicgames.com/documentation/fortnite/fortnite-documentation
- **Use:** Locate the current documentation hierarchy, new pages, release-specific material, and environment-specific documentation.
- **Boundary:** A hub page is a navigation source. It does not replace the detailed owner page for a Device, workflow, API, or publishing rule.

## Fortnite Creative Glossary

- **Source type:** Official Epic glossary
- **Official URL:** https://dev.epicgames.com/documentation/fortnite/fortnite-creative-glossary
- **Use:** Confirm official terminology and locate related concepts.
- **Boundary:** A glossary definition does not prove that a specific Device exposes a named option, Event, or Function.

---

# Teaching and Workshop Sequences

## First Creative Session

**Goal:** Enable a learner to enter an island, recognize Create Mode and Play Mode, place one object, place one Device, and verify one visible result.

**Sequence:**

1. Confirm that the learner is in Fortnite Creative rather than UEFN.
2. Identify the current mode and the on-screen controls.
3. Open the current content interface and locate one Prop, one building asset, and one Device.
4. Place, move, rotate, copy, and delete a test object.
5. Place one simple Device and configure only the minimum required behavior.
6. Start the game, observe the result, end the game, and return to Create Mode.
7. Record one control or interface action that was difficult to find.

**Success evidence:** The learner can repeat the workflow without step-by-step prompting.

## From Idea to Testable Mechanic

Convert an idea into this chain:

`Experience goal → player action → rule → state change → feedback → reset → test evidence`

Example:

- Experience goal: Players notice when someone needs help.
- Player action: A player activates a help request.
- Rule: Another player must respond before progress continues.
- State change: A route or shared objective becomes available.
- Feedback: The requesting player and helper receive clear confirmation.
- Reset: The request and route return to their starting state for the next round.
- Test evidence: Players recognize and respond to the request without instructor explanation.

## World Blockout Before Decoration

1. Define the start, objective, critical route, optional route, recovery route, and finish.
2. Build with simple shapes or repeated assets before detailed decoration.
3. Walk the route at player speed.
4. Check scale, visibility, collision, route choice, and dead ends.
5. Add guidance using spatial composition first; add text only where needed.
6. Decorate after the route and gameplay loop pass a blind playtest.

## Device-System Planning Session

Before placing Devices, complete a system card:

| Field | Required answer |
|---|---|
| Triggering action | What does the player or system do? |
| Source Device | Which Device detects it? |
| Source Event | Which verified Event is emitted? |
| Receiving Device | Which Device changes state? |
| Receiving Function | Which verified Function is called? |
| Scope | Per-player, per-team, or global? |
| Feedback | How does the player know it worked? |
| Reset | What restores the initial state? |
| Failure path | What happens when the condition is not met? |
| Test | What exact result should appear in Play Mode? |

Do not fill Event or Function names from memory. Retrieve them from the official Device unit.

## Playtest-as-Research Sequence

1. Write one question.
2. Predict what players will do.
3. Define one observation that would support or challenge the prediction.
4. Run the test without teaching the solution during the first attempt.
5. Record actions and delays separately from interpretation.
6. Ask short neutral questions.
7. Change one variable.
8. Repeat the same scenario and compare.

---

# Planning Templates

## Minimal Prototype Card

| Field | Entry |
|---|---|
| Player goal | One sentence |
| Supported player count | Minimum and maximum |
| Core action | The repeated player action |
| Success condition | Observable condition |
| Failure or blocked state | What stops progress |
| Feedback | Visual, text, audio, or environmental |
| Reset | Manual, round-based, timed, or event-driven |
| Devices | Verified candidate list |
| First Play Mode test | One expected result |

## State Ownership Card

For every value or condition, record:

- **Owner:** player, team, or island;
- **Initial value:** state at game or round start;
- **Writers:** actions or Devices allowed to change it;
- **Readers:** systems that depend on it;
- **Reset moment:** respawn, round start, round end, game end, or explicit event;
- **Join-in-progress behavior:** what a late player should see;
- **Visible evidence:** how the state is communicated.

## Event-to-Function Connection Log

| # | Source Device | Verified Event | Receiving Device | Verified Function | Expected result | Test status |
|---:|---|---|---|---|---|---|
| 1 |  |  |  |  |  |  |
| 2 |  |  |  |  |  |  |
| 3 |  |  |  |  |  |  |

## Change Log

| Build | One change made | Reason | Expected result | Actual result | Keep or revert |
|---|---|---|---|---|---|
|  |  |  |  |  |  |

---

# Social-Design Implementation Cards

## Belonging and Participation

Use this card after selecting the social goal in file `11`.

- **Access:** Can every intended player reach and understand the activity?
- **Opportunity:** Does each player have an actual action?
- **Contribution:** Does that action change the shared state?
- **Recognition:** Is the contribution visible without public embarrassment?
- **Recovery:** Can a delayed player rejoin meaningfully?

Technical implementation must be verified in `05` and `06`.

## Cooperation and Roles

A role is meaningful only when it has information, action, consequence, and recovery.

| Role question | Check |
|---|---|
| What information does this role receive? |  |
| What repeatable action does it perform? |  |
| How does the action affect the shared objective? |  |
| Can another player dominate or replace the role? |  |
| What happens if the role misses an action? |  |
| How does the system reset or rotate roles? |  |

## Choice and Consequence

For each choice, define:

- information available before selection;
- immediate response;
- delayed consequence;
- whether the choice is reversible;
- whether all active players can influence it;
- how the result is communicated;
- what happens when nobody chooses.

Avoid presenting two visually different options that produce the same gameplay result.

## Fairness and Responsibility

Select the intended rule explicitly: equality, need, contribution, rotation, random assignment, or strategic allocation. Do not treat the rule as self-explanatory.

Check whether one player can:

- control all inputs;
- consume all resources;
- block the team indefinitely;
- receive all visible credit;
- publicly expose another player's difficulty.

Use rotation, distributed information, confirmation, parallel tasks, or recovery routes where needed.

## Perspective and Sensitive Topics

Use fictional or metaphorical situations. Do not require personal disclosure or simulate humiliation, harassment, discrimination, abuse, or trauma as entertainment. Measure in-game actions and observations rather than claiming that the experience created lasting empathy or behavior change.

---

# Adapted Creative Build Patterns

The following are system patterns, not verified option lists. Retrieve every named Device independently before implementation.

## Item Opens a Door

**Player loop:** Find an item → return to the locked route → present the item → open progress.

**Candidate Devices:** Item Spawner or Item Granter, Conditional Button, Lock, HUD Message or environmental feedback.

**System checks:**

- Is the required item registered correctly?
- Is the requirement consumed or retained as intended?
- Does the result affect the correct player, team, or everyone?
- Does the door return to the intended state on round reset?
- Is failure feedback clear when the item is missing?

## Cooperative Two-Point Activation

**Player loop:** Players occupy or activate separated points at the same time to open a shared route.

**Candidate Devices:** Triggers, Player Counter or other presence detection, Barrier or Lock, HUD feedback.

**System checks:**

- Does each activation belong to the correct team?
- Is simultaneous presence required or is sequence sufficient?
- What happens when one player leaves?
- Can one player activate both points unintentionally?
- Does the system reset after success and between rounds?

## Character Gives a Clue

**Player loop:** Reach a character → interact → receive a clue → unlock or reveal the next task.

**Candidate Creative tools:** Character Device, Pop-up Dialog or other supported message system, Trigger, objective or route-control Device.

**Boundary:** Do not substitute UEFN NPC Spawner, NPC Character Definition, Conversation Graph, or Verse unless the environment changes to UEFN.

## Branching Question with Three Choices

**Player loop:** Read a question → choose one of three responses → receive immediate consequence.

**Candidate Devices:** Buttons or interaction Devices, Triggers, HUD or Pop-up feedback, Teleporter, Barrier, Lock, objective, or environmental response.

**System checks:**

- Only one answer should resolve the current question.
- Repeated activation should not award duplicate progress.
- Incorrect answers need a defined retry, recovery, or alternate path.
- Team and individual consequences must not be mixed accidentally.

## Collection Progression

**Player loop:** Discover and collect a set of objects → update visible progress → unlock the next area.

**Candidate Devices:** Collectible or item systems, Tracker, objective feedback, Barrier or Lock, HUD.

**System checks:**

- Is progress personal, team-shared, or island-wide?
- What happens when a player joins late?
- Can an item be collected more than once?
- Does the count reset at the correct lifecycle point?
- Is progress visible without relying on color alone?

## Nonviolent Obstacle Course

**Player loop:** Navigate movement challenges → reach checkpoints → recover quickly after failure → finish.

Prioritize readable scale, clear boundaries, forgiving recovery, checkpoint logic, and accessible cues. Difficulty should come from the designed movement challenge, not from hidden controls or unclear collision.

## Short Escape Room

Use a compact dependency chain:

`Inspect → discover clue → satisfy condition → open route → receive confirmation → continue`

Limit the first prototype to one room, one clue, one condition, one route change, and one reset path. Add narrative and decoration after the chain works reliably.

## Shared Resource Decision

Give the group a limited resource that can be used in more than one place. Make remaining quantity and consequence visible. Ensure no single player can silently spend the resource without the intended level of group influence.

## Ask-for-Help Mechanic

A blocked player can send a clear request. Another player can recognize it, respond, and confirm that help arrived. The request should be noticeable but not stigmatizing. Provide a fallback when no helper is available.

## Repair After Error

A mistake changes the state and opens a recovery task instead of forcing an unexplained full reset. Players should be able to identify what failed, what can be repaired, and when the system is restored.

---

# Playtesting and Troubleshooting

## One-Change-at-a-Time Rule

For every defect:

1. Reproduce it consistently.
2. Write the expected result.
3. Identify the earliest point where actual behavior differs.
4. Change one setting or one connection.
5. Run the same test again.
6. Record the result before making another change.

Changing several Devices at once destroys the evidence needed to identify the cause.

## Event-to-Function Trace

Trace the system in this order:

1. Is the source Device enabled?
2. Does the player action satisfy the source condition?
3. Does the verified Event occur?
4. Is the connection bound to the intended receiving Device?
5. Is the verified Function configured on that receiver?
6. Is the receiver enabled and allowed to affect the instigator or team?
7. Is another connection immediately reversing the result?
8. Does reset logic run at the wrong time?

## Wrong Team or Wrong Player Receives the Result

Check, one at a time:

- source-team restrictions;
- receiving-Device team or class filters;
- whether the instigator is preserved through the connection;
- whether feedback is configured for instigator, team, or everyone;
- duplicate Devices with similar names;
- copied connections still pointing to another team's receiver;
- global state being used where per-team state was intended.

## System Starts in the Wrong State

Check:

- enabled state at game start;
- initial lock, barrier, visibility, or objective state;
- round-start and game-start connections;
- completion behavior from the previous round;
- persistence or saved state where applicable;
- copied Devices with inherited configuration.

## Repeated Activation or Double Counting

Check:

- whether more than one source Event is connected to the same progress action;
- whether the source can trigger repeatedly before reset;
- whether both a Device and another system own the same state;
- whether success disables the source or marks completion;
- whether round reset creates duplicate active connections.

## HUD Message Does Not Disappear or Appears to the Wrong Audience

Verify:

- audience scope;
- display duration and clearing behavior in the official unit;
- whether a second message replaces or extends the first;
- whether leaving a zone emits the expected Event;
- whether team filters are consistent;
- whether the message is triggered globally rather than by the instigator.

## Item Condition Does Not Work

Verify:

- the exact registered item;
- required quantity;
- whether the item is in inventory or placed in the world;
- whether the item was granted to the intended player;
- whether the condition checks the instigator;
- consume/retain behavior;
- reset and respawn behavior.

## Door, Lock, or Barrier Does Not Match the Intended State

Check the route object and control Device separately. Confirm that the Device is attached to or affects the intended object, that the receiving Function is correct, and that another connection is not immediately closing, locking, enabling, or disabling it again.

## Spawn or Checkpoint Problem

Check Island Settings and the relevant spawn or checkpoint Device separately. Test initial spawn, respawn, team assignment, round transition, and join in progress as distinct cases.

## Character or Interaction Problem

Distinguish:

- a static character display;
- a Creative character interaction;
- a spawned AI character;
- a UEFN NPC workflow.

Do not troubleshoot one category using another environment's tools. Verify interaction distance, visibility, enablement, player scope, and the downstream message or progression connection.

## Memory, Backup, and Performance

Keep these questions separate:

- **Memory/budget:** Does the island pass the current Creative memory workflow?
- **Runtime performance:** Does play remain responsive at the supported player count?
- **Backup/recovery:** Can the project or island be restored after a destructive change?

Measure before and after a change. Removing a visible object does not necessarily remove every dependency or cost.

## Regression Test Matrix

| Test | Expected result |
|---|---|
| Start first game | All systems use the documented initial state. |
| Activate once | One intended result occurs. |
| Activate repeatedly | No unintended duplicate progress. |
| Leave and return | Presence and feedback update correctly. |
| Minimum players | The core loop remains solvable. |
| Maximum practical players | State and feedback remain correct. |
| Team A and Team B in parallel | No cross-team activation. |
| Respawn | Personal state and checkpoint behavior are correct. |
| End round and start next round | Only intended values persist. |
| Join in progress | The new player sees the correct current state. |

---

# Accessibility and Safety

- Explain required controls before the challenge; do not hide a keyboard action as puzzle difficulty.
- Use more than one cue for critical state changes where practical.
- Do not rely on color alone.
- Provide readable text, sufficient display time, and clear contrast.
- Provide recovery after failure and a lower-pressure role where appropriate.
- Avoid public ranking or messaging that embarrasses a delayed or learning player.
- Use fictional contexts for sensitive social topics.
- Follow current Epic content, privacy, publishing, and age-rating rules through files `01` and `07`.

---

# Delivery and Documentation

## Final Build Checklist

- The environment is correctly identified.
- Every Device has a clear purpose and descriptive name.
- Every Event → Function connection is recorded.
- Player, team, and global state are distinguished.
- Initial state and reset behavior are tested.
- Failure and recovery are visible.
- Critical feedback is accessible.
- The build works in repeated Play Mode tests.
- Known limitations and undocumented edge cases are recorded.
- Publishing and memory claims are checked against current official sources.

## Student or Team Handoff Card

Record:

- experience title;
- player goal;
- supported player count;
- core loop;
- required Devices;
- connection log;
- reset method;
- known issues;
- first three tests to run;
- one next improvement.

## Maintenance Note

This supplement should remain concise and adapted. When an official Epic page is added or updated, add the technical material to its primary owner file or official index rather than copying a second full Device reference into this document.
