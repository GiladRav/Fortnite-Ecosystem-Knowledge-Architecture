---
document_id: "09"
corpus_role: "active_agent_knowledge"
authority: "verified_uefn_gameplay_ui_npc_cinematics"
primary_environment: "UEFN without Verse and UEFN with Verse"
non_verse_layer_last_verified: "2026-08-03"
verified_release: "Fortnite Ecosystem v41.30"
verse_sections_policy: "Preserved from the previous canonical file without expansion in this task"
---

# UEFN: Gameplay, Verse API, UI, NPC, and Cinematics

## Document Metadata

| Field | Value |
|---|---|
| Document ID | `09` |
| Domain | UEFN gameplay integration, UI, NPC, Conversation, animation, cinematics, audio, and VFX |
| Primary Environment | UEFN without Verse by default; Verse sections are explicitly separated |
| Language | English; exact UI labels, Device names, Events, Functions, asset types, and code identifiers remain in English |
| Authority | Current Epic documentation and release notes; professional inference is labeled |
| Non-Verse Layer Last Verified | 2026-08-03 |
| Verified Release | Fortnite Ecosystem `v41.30`, released 2026-07-30 |
| Verse Section Status | Existing Verse-specific sections preserved without expansion |
| Stability | Devices and core workflows are stable; UI, NPC, Conversation, LLM Conversations, Scene Graph, mobile, and feature status are version-sensitive |

## Document Purpose

Provide practical implementation knowledge for gameplay in UEFN, with a fully developed **UEFN-without-Verse** layer built from:

- Fortnite Devices;
- Direct Event Binding;
- Island Settings;
- Device-based state;
- non-Verse HUD and UMG workflows;
- NPC Spawner and NPC Character Definition workflows that use default behavior;
- authored Conversation Device graphs;
- current non-code LLM Conversation/Persona workflows;
- Sequencer, Level Sequences, Control Rig, and Cinematic Sequence Device;
- imported audio, Audio Player, audio mixing, MetaSounds where supported;
- VFX Devices and Niagara;
- multiplayer, lifecycle, reset, Join in Progress, and launched-session verification.

Verse code, Verse UI, custom Verse NPC behavior, custom Verse state, and Verse-authored Devices are not developed in this task.

## Mandatory Boundary

When the requested behavior cannot be expressed reliably with current Devices, Direct Event Binding, authored Conversation graphs, UMG Device integration, NPC default behavior, Sequencer, or other non-Verse systems:

1. state that the exact behavior is not reliably available without Verse;
2. identify the closest verified Device-only approximation;
3. describe the limitation;
4. do not provide Verse code in the non-Verse answer;
5. route the user to the preserved Verse sections only when the environment changes.

## Evidence Labels

- **Documented:** supported by current Epic documentation.
- **Professional practice:** recommended production method; not an undocumented platform guarantee.
- **Session-dependent:** verify in a launched Fortnite session.
- **Version-sensitive:** exact status, label, or behavior may change.
- **Beta / Experimental:** follow current release notes before shipping.
- **Not reliably available without Verse:** do not imply that a Device graph can perform custom logic it cannot express.

## Quick Topic Index

- [Gameplay Architecture Without Verse](#gameplay-architecture-without-verse)
- [Device-Only Capability Boundary](#device-only-capability-boundary)
- [Finding and Configuring Devices](#finding-and-configuring-devices)
- [Direct Event Binding](#direct-event-binding)
- [Device Naming and System Documentation](#device-naming-and-system-documentation)
- [State Scope and Lifecycle](#state-scope-and-lifecycle)
- [Island Settings in UEFN](#island-settings-in-uefn)
- [Non-Verse Gameplay Patterns](#non-verse-gameplay-patterns)
- [Non-Verse UI and HUD](#non-verse-ui-and-hud)
- [UMG Without Verse](#umg-without-verse)
- [NPC Spawner and NPC Character Definitions](#npc-spawner-and-npc-character-definitions)
- [Navigation and Default NPC Behavior](#navigation-and-default-npc-behavior)
- [Authored Conversations](#authored-conversations)
- [LLM Conversations and Personas](#llm-conversations-and-personas)
- [Sequencer, Level Sequences, and Cinematics](#sequencer-level-sequences-and-cinematics)
- [Control Rig and NPC Animation](#control-rig-and-npc-animation)
- [Audio](#audio)
- [VFX and Niagara](#vfx-and-niagara)
- [Multiplayer and Join in Progress](#multiplayer-and-join-in-progress)
- [Reset and Repeated Activation](#reset-and-repeated-activation)
- [Debugging Device-Only Gameplay](#debugging-device-only-gameplay)
- [Complete Non-Verse Project Example](#complete-non-verse-project-example)
- [Preserved Verse-Specific Sections](#preserved-verse-specific-sections)
- [Official Source Registry](#official-source-registry)

## Common Question Router

- World building, assets, materials, Landscape, lighting, Streaming, HLOD, Scene Graph environment authoring -> `08`.
- Sessions, memory, profiling, Lore, validation, Private Versions, publishing -> `10`.
- UEFN definitions -> `16`.
- Verse language semantics -> `12`-`14`.
- Current official URL or release-sensitive status -> `01`.

---

# Gameplay Architecture Without Verse

A Device-only UEFN system should be designed as an explicit event graph.

Use this chain:

`Player or system action -> Source Device Event -> Receiving Device Function -> visible state change -> feedback -> reset`

Example:

`Character Device — On Interacted With -> Conversation Device — Initiate Conversation`

Then a Conversation Event can trigger additional Device functions.

## Device-First Rule

Use Devices when:

- the required behavior already exists as a Device option;
- state scope can be expressed as player, team, class, or global through documented settings;
- events and functions can be inspected in the editor;
- reset behavior is available through phases, rounds, timers, or Device functions;
- the graph remains understandable.

A Device-only solution becomes unsafe when it requires:

- arbitrary custom variables;
- dynamic collections;
- custom algorithms;
- procedural decision trees beyond Device options;
- per-player UI data not exposed by Devices/Viewmodels;
- persistence schema logic;
- robust custom NPC state machines;
- synchronization that Devices cannot represent;
- custom event filtering by data not exposed in Device settings.

## One Owner Per State

For each gameplay state, write:

| Field | Required answer |
|---|---|
| State | What changed? |
| Owner | Player, team, class, NPC, Device, or island |
| Initial state | What is true at game/round start? |
| Writer | Which Device function changes it? |
| Reader | Which systems depend on it? |
| Feedback | How does the player see or hear it? |
| Reset | What restores it? |
| Join in Progress | What should a late player observe? |
| Failure path | What happens if the event never occurs? |

Examples:

- a Barrier's enabled state is usually global;
- a HUD Message can target an instigating player or broader audience according to Device settings;
- a Tracker can be individual, team-shared, or global depending on configuration;
- a Conversation is initiated for a player/agent, but its Conversation Events can trigger global Devices;
- a Level Sequence can be visible to everyone, an instigator, or a team according to Cinematic Sequence Device settings.

---

# Device-Only Capability Boundary

## Behaviors Commonly Achievable Without Verse

- item collection;
- lock-and-key progression;
- timers and timed objectives;
- score, stats, and trackers;
- team/class routing;
- buttons, triggers, volumes, switches, and conditional checks;
- barriers, locks, teleporters, prop visibility, and prop movement;
- HUD messages, pop-up dialogs, trackers, map indicators, and custom UMG overrides;
- authored branching conversations;
- default Guard/Wildlife NPC behavior and lifecycle;
- cinematics and environmental state changes;
- audio and VFX feedback;
- round and game ending;
- persistence offered by supported persistence Devices.

## Behaviors That Usually Require Verse or a Compromise

- arbitrary custom inventory logic outside supported systems;
- complex branching based on many custom variables;
- deterministic per-player quest state with custom data;
- custom NPC navigation/decision logic;
- dynamic UI generated from custom gameplay data;
- custom lists, maps, queues, procedural content selection, or weighted algorithms;
- reliable orchestration of many concurrent tasks;
- custom save schema and migration;
- custom anti-duplication logic where no Device exposes the required state;
- dynamically spawning or referencing arbitrary object sets beyond exposed Device functions.

## Closest-Solution Rule

When exact behavior is unavailable:

- use a Tracker, Stat, Switch, Timer, or Class/Team Device as the nearest supported state carrier;
- use separate Device copies for distinct player/team paths only when scope and reset remain testable;
- reduce the design to a finite set of explicit branches;
- make the limitation visible in the documentation;
- run a multiplayer lifecycle test before calling the workaround reliable.

---

# Finding and Configuring Devices

## Location

Devices are found in the UEFN Content Browser under the Fortnite content hierarchy. The exact category can vary. Use search by the exact Device name.

For the Conversation Device, current official documentation places it under:

`Fortnite > Devices > UI`

The NPC Spawner is UEFN-only and its location/status labels can change; current documentation has historically shown a Beta folder. Always search by exact name and verify the current feature status.

## Configuration Workflow

1. drag the Device into the Viewport;
2. rename it immediately in the Outliner;
3. select it;
4. edit `User Options` in Details;
5. configure gameplay scope and phase;
6. configure receiving functions through Direct Event Binding;
7. inspect read-only event relationships;
8. save;
9. launch a session;
10. test the smallest expected result.

## Contextual Filtering

Some options appear, disappear, or become disabled based on other values. Do not conclude that a Device lacks an option until the controlling setting is checked.

## Device Instance versus Asset

Most Fortnite Devices are placed Actors configured per instance. UEFN-only systems can also reference assets such as:

- Conversation Bank;
- NPC Character Definition;
- Level Sequence;
- Widget Blueprint;
- imported sound assets;
- Niagara systems;
- materials and textures.

Document both the placed Device and the asset it references.

---

# Direct Event Binding

## Core Model

UEFN Devices use Direct Event Binding by default. It binds a source Device event to a receiving Device function.

Use this notation:

`Source Device — Event -> Receiving Device — Function`

Example:

`Character_Archivist — On Interacted With -> Conversation_Archivist — Initiate Conversation`

## UEFN Authoring Direction

Current official UEFN documentation instructs creators to configure the **receiving function**:

1. select the receiving Device;
2. expand `User Options - Functions`;
3. add an array element for the required function;
4. select the source Device;
5. select the source Event.

`User Options - Events` is populated from bindings and is treated as read-only in the current UEFN workflow.

Do not tell a UEFN user to author from the read-only Events section unless the active release explicitly changes this workflow.

## Why Naming Matters

Direct Event Binding identifies Devices by name rather than by channel numbers. Rename Devices before binding.

Bad:

- `Trigger`
- `Trigger2`
- `HUD Message`
- `Barrier`

Good:

- `TRG_ArchiveEntered`
- `HUD_ArchiveObjective`
- `BAR_SealedGallery`
- `CINE_ArchiveRestore`

## Copy and Paste

Epic documents that duplicated Device systems receive unique bindings rather than sharing the original binding identity. Still verify:

- copied source references;
- copied receiving references;
- unique Device names;
- team/class scope;
- reset;
- multiplayer behavior.

## Binding Table Template

| # | Source Device | Exact Event | Receiving Device | Exact Function | Scope | Expected result |
|---:|---|---|---|---|---|---|
| 1 |  |  |  |  |  |  |

Never fill exact names from memory when the Device page is available.

## Event Browser

Use Event Browser to inspect Device relationships where available. It helps answer:

- what is bound to this Device;
- which Device emits the source event;
- which functions are called;
- whether a duplicate or stale binding exists.

Event Browser does not prove the runtime instigator, state scope, or reset behavior. Launch a session.

---

# Device Naming and System Documentation

## Prefix Convention

This is professional practice.

| Type | Example prefix |
|---|---|
| Trigger | `TRG_` |
| Button | `BTN_` |
| Barrier | `BAR_` |
| HUD Message | `HUD_` |
| Pop-Up Dialog | `POP_` |
| Tracker | `TRK_` |
| Audio Player | `AUD_` |
| VFX Spawner | `VFX_` |
| Cinematic Sequence | `CINE_` |
| Conversation | `CONV_` |
| Character Device | `CHAR_` |
| NPC Spawner | `NPCSP_` |
| End Game | `END_` |

## System Card

For every Device system:

```text
System:
Player goal:
Supported players:
State owner:
Start condition:
Binding list:
Feedback:
Failure path:
Reset:
Join in Progress:
Minimal test:
Known limitation:
```

## Instigator Chain

Many Device functions act on the instigating player. When an event passes through several Devices, test whether the instigator remains available.

Do not assume every Device preserves or forwards the same player context. If the official page does not document it, label the edge case and test with two players.

---

# State Scope and Lifecycle

## Scope Categories

- **Per-player:** only the triggering/assigned player.
- **Per-team:** shared by a team.
- **Per-class:** filtered by class.
- **Global:** island-wide Device state.
- **Spawner/NPC instance:** attached to an individual spawned NPC or a Spawner's lifecycle.
- **Conversation instance:** active for a player, but events can affect broader systems.

## Lifecycle Moments

Test every relevant system at:

1. pre-game;
2. game start;
3. first player spawn;
4. first activation;
5. repeated activation;
6. player elimination/respawn;
7. player leaves;
8. Join in Progress;
9. round end;
10. next round;
11. game end;
12. new session.

## Join in Progress

For a late player, define:

- spawn location;
- current objective;
- UI shown;
- open/closed routes;
- conversation availability;
- cinematic state;
- NPC state;
- inventory;
- team/class;
- whether completed global events should replay.

Devices do not automatically reconstruct a custom narrative history for late joiners. Use existing persistent/global Device state where supported, or design a late-join-safe entry path.

---

# Island Settings in UEFN

Every UEFN island includes Island Settings. The settings are largely shared with Creative, but presented in UEFN through Details.

Use Island Settings for:

- maximum players;
- teams and team size;
- spawn and respawn;
- rounds;
- game start;
- Join in Progress;
- score and end conditions;
- building and movement;
- HUD and UI;
- debug visualization;
- Sidekick and current feature options where applicable.

## Configuration Rule

Before adding Devices, set the minimum Island Settings that define:

- player count;
- teams;
- rounds;
- spawn;
- game start;
- Join in Progress;
- win/end condition ownership.

Then verify Device overrides do not contradict Island Settings.

## Conflict Example

If Island Settings end the round based on score while an End Game Device is configured to end the game, document which event should win and test both simultaneous and delayed completion.

---

# Non-Verse Gameplay Patterns

## Interaction Opens a Route

**Devices:** Character or Button, Barrier or Lock, HUD Message.

**Binding:**

`Source interaction Event -> Barrier — Disable`

Add feedback:

`Same source Event -> HUD Message — Show`

**Test:** Two players; only one interacts. Confirm whether the route opens globally and whether the message audience is correct.

## Item Opens a Route

**Devices:** Item Spawner or Capture Item Spawner, Conditional Button, Barrier/Lock.

Two patterns:

1. item pickup directly changes state;
2. player must carry/present the item to a Conditional Button.

The second pattern is safer when the design requires proof that the player still possesses the item.

## Tracker-Based Objective

**Devices:** source objective Devices, Tracker, completion feedback, Barrier/End Game.

Verified Tracker functions include:

- `Assign`;
- `Assign to All`;
- `Increment Progress`;
- `Decrement Progress`;
- `Reset Progress`;
- `Complete`;
- persistence-related functions when enabled.

Use the exact labels shown in the active UI.

## Finite Branching Choice

Use:

- Conversation Device choices;
- Pop-Up Dialog buttons;
- Voting Devices;
- several Buttons or Triggers;
- Switch/Attribute Evaluator for supported conditions.

Without Verse, keep branch count finite and state explicit. Prevent duplicate completion through Device disablement, Tracker completion, Switch state, or a global route change.

## Timed Sequence

Use Timer or Timed Objective Devices to:

- start;
- pause;
- resume;
- complete;
- fail;
- reset;
- trigger feedback.

Test game start, manual start, repeated start, round reset, player removal, and shared versus individual timer scope.

## Global Environmental Transformation

Use:

- Cinematic Sequence Device;
- Data Layer track;
- Prop Manipulator/Prop Mover;
- lighting Devices;
- Audio Player;
- VFX Spawner.

Because these are often global, do not replay the transformation independently for each late player unless the design supports it.

---

# Non-Verse UI and HUD

## UI Layers

1. **Fortnite native HUD** controlled by Island Settings and HUD Controller.
2. **HUD Devices** such as HUD Message, Tracker, Map Indicator, Objective, Message Feed, and Pop-Up Dialog.
3. **UMG Widget Blueprint overrides** shown through supported Devices.
4. **Conversation UI** using radial, box, or custom presentation.
5. **In-world UMG** placed in the level as of v41.20.
6. **Verse-created UI** excluded from the non-Verse layer.

## HUD Message Device

Use for:

- objective prompts;
- confirmation;
- warnings;
- short narrative text;
- custom UMG User Widget display.

Current documented receiving functions include:

- `Show`;
- `Hide`;
- `Clear Layer`.

Check:

- audience;
- display time;
- layer;
- priority;
- queue behavior;
- placement;
- widget override;
- repeated activation;
- respawn;
- Join in Progress.

## Pop-Up Dialog Device

Use when the player must choose from a finite set of buttons.

For custom UMG, use a **Modal Dialog Variant** Widget Blueprint. Current official UI documentation states that the Pop-Up Dialog Device works with Modal Dialog Variant, while HUD Message can use supported User Widget workflows.

Test:

- keyboard;
- controller;
- touch;
- focus;
- button labels;
- cancel/back behavior;
- UI blocking gameplay input;
- repeated display;
- multiple players opening simultaneously.

## HUD Controller

Use to hide or replace selected Fortnite HUD elements. Do not hide information that the experience still depends on.

Test the transition into and out of:

- pre-game UI;
- cinematic;
- gameplay;
- conversation;
- respawn;
- game-end screen.

## Tracker UI

Tracker can provide objective state without custom code. Define whether progress is individual, shared, or assigned to all. Test assignment before progress events; otherwise progress can be missed or shown to the wrong audience.

## Accessibility

Critical information should use at least two compatible cues when practical:

- text + sound;
- text + environmental change;
- icon + text;
- VFX + HUD confirmation.

Avoid color-only, sound-only, or rapidly flashing feedback.

---

# UMG Without Verse

## Supported Non-Verse Uses

- custom HUD Message appearance;
- custom Pop-Up Dialog appearance;
- Conversation UI;
- Viewmodel-bound supported Fortnite HUD data;
- UI animations;
- materials and textures;
- Action Widget for input representation;
- in-world UMG display.

## Widget Types

Current official UEFN UI documentation distinguishes:

- **User Widget:** general custom UI; supported by HUD Message workflows and in-world placement.
- **Modal Dialog Variant:** clickable modal UI; used with Pop-Up Dialog.

## Creation Workflow

1. right-click in Content Browser;
2. choose `User Interface > Widget Blueprint`;
3. choose the required widget type;
4. name the asset;
5. open Widget Editor;
6. build hierarchy;
7. set anchors, alignment, size, and safe-zone behavior;
8. add text, image, material, or animation;
9. assign to the supported Device field;
10. launch a session and test all inputs.

## Viewmodels

Viewmodels can bind supported Fortnite/Device data to UMG. Use current official Viewmodel documentation because available fields and bindings are version-sensitive.

A Viewmodel is not arbitrary custom game state. If the needed data is not exposed by a supported Viewmodel or Device, dynamic updates may require Verse.

## In-World UMG

As of v41.20, a UMG User Widget can be dragged from the Content Browser into the Viewport to display UI in the world. It can also be placed in a Scene Graph prefab.

Non-Verse uses include:

- signs;
- animated displays;
- museum labels;
- maps;
- decorative screens;
- static or viewmodel-driven information.

Test:

- readability distance;
- occlusion;
- orientation;
- scale;
- lighting-independent appearance;
- multiplayer visibility;
- collision;
- performance;
- mobile.

## Non-Verse UI Limitation

Do not claim that a Widget Blueprint alone can run arbitrary gameplay logic. Blueprint gameplay scripting is excluded, and custom dynamic UI data generally requires supported Viewmodels, Devices, or Verse.

---

# NPC Spawner and NPC Character Definitions

## NPC Ownership

- **NPC Character Definition asset:** reusable character type, behavior selection, and modifiers.
- **NPC Spawner Device:** spawn timing, active count, total limit, spawn radius, enable/disable, despawn policy, and per-Spawner overrides.
- **Navigation mesh:** pathfinding environment.
- **Conversation Device or Persona Modifier:** conversation system.
- **Cinematic Sequence:** staged animation.

## Creating a Character Definition

Two documented methods:

### From Content Browser

1. right-click in project content;
2. choose `Artificial Intelligence > NPC Character Definition`;
3. name the asset;
4. open it;
5. choose Character Type, Behavior, and Modifiers;
6. save;
7. assign to an NPC Spawner or drag the definition into the level when supported.

### From NPC Spawner

1. place NPC Spawner;
2. in `User Options`, open `NPC Character Definition`;
3. create a new NPC Character Definition asset;
4. name and open it;
5. configure and save.

## Character Types Without Verse

Current official documentation includes:

- **Guard:** default Guard behavior with configurable modifiers.
- **Wildlife:** supported animal subtypes with default behavior.
- **Participant:** version-sensitive; verify current options and behavior.
- **Custom:** custom behavior is defined in Verse; do not choose it for a non-Verse gameplay NPC unless using Empty Behavior solely for Sequencer or a currently documented non-code feature such as Persona that explicitly supports it.

Brand-specific types can appear only in the relevant IP project.

## Behavior Without Verse

Use:

- `Default Behavior` for Guard or Wildlife;
- `Empty Behavior` for a stationary/reference-pose custom character intended to be animated only by Sequencer, when current documentation supports it.

Do not select `Verse Behavior` in a non-Verse project.

## Modifiers

Modifiers can control available aspects such as:

- cosmetic;
- team;
- health;
- movement;
- inventory;
- Guard-specific behavior;
- Wildlife-specific behavior;
- animation/Sequencer identification;
- Persona where current feature access allows.

Modifier availability depends on Character Type and current release. Do not transfer modifiers between types.

## NPC Spawner Core Options

Current official documentation includes options such as:

- `Spawn Count`;
- `Spawn Through Walls`;
- `Spawn Character at Game Start`;
- `Additional NPC Character Modifiers`;
- `Allow Infinite Spawn`;
- `Total Spawn Limit`;
- `Spawn On Timer`;
- `Spawn Timer`;
- `Show Spawn Radius`;
- `Spawn Radius`;
- `Despawn AIs When Disabled`.

Exact defaults and contextual fields are version-sensitive.

## Non-Verse NPC Limit

Default Guard/Wildlife behavior can be configured, but custom goals, custom navigation decisions, memory, schedules, and complex role behavior require Verse or a constrained Device approximation.

---

# Navigation and Default NPC Behavior

NPCs use NavMesh for pathfinding decisions.

## NavMesh Visualization in UEFN

Current documented workflow:

1. select Island Settings in the Outliner;
2. under `User Options - Debug`, enable Debug and Navigation;
3. place at least one supported AI Spawner;
4. inspect the NavMesh in the Fortnite/Live Edit session.

The NavMesh visualization is not necessarily shown directly in the editor viewport.

## Navigation Checklist

- walkable surface exists;
- slope is acceptable;
- stairs are wide enough;
- door/barrier opening updates path access as expected;
- no invisible collision blocks the path;
- spawn radius contains valid NavMesh;
- NPC is not spawning behind invalid geometry;
- streaming does not remove required pathing;
- Scene Graph entities are included correctly in the current release;
- dynamic obstacles recover.

## Minimal NPC Test

1. place one NPC Spawner;
2. assign one Guard definition with Default Behavior;
3. spawn at game start;
4. launch;
5. observe spawn;
6. enable NavMesh debug;
7. move around the NPC;
8. disable and re-enable the Spawner;
9. start a new round.

Expected:

- NPC spawns inside the valid radius;
- NavMesh exists;
- disable/despawn behavior matches configuration;
- round reset produces the intended count.

## Multiplayer NPC Test

Use at least two players:

- approach from opposite sides;
- one player leaves;
- one joins late;
- one respawns;
- trigger a cinematic or conversation while the other player remains active;
- verify NPC target/attention behavior does not block the objective.

---

# Authored Conversations

## Current Status

The authored Conversation Device workflow is a UEFN system for manually created conversation trees. It is distinct from LLM Conversations/Personas.

## Components

- Conversation Device;
- Conversation Bank asset;
- Conversation Editor graph;
- Character Device or NPC Spawner;
- interaction source;
- Conversation UI type;
- optional Conversation Events connected to Devices.

## Finding the Device

Current official path:

`Fortnite > Devices > UI`

Search the exact Device name if folders differ.

## Create a Conversation Bank

1. place Conversation Device;
2. in its Conversation option, create a new Conversation Bank asset; or
3. right-click Content Browser and choose `Gameplay > Conversation Bank`;
4. open the asset;
5. add a `Default Entry Point`;
6. add speech, choice, event, and exit nodes as required;
7. connect every branch;
8. save;
9. assign the bank to the Conversation Device.

## Initiating a Conversation

Verified Device-only pattern:

`Character Device — On Interacted With -> Conversation Device — Initiate Conversation`

A Trigger or Volume can also initiate when supported by the active Device event/function list.

## Conversation UI Types

Current documentation describes:

- **Radial:** wheel-style choices;
- **Box:** Standard, Single Speaker, or Two Speakers;
- **Custom:** Widget Editor-authored custom appearance.

Keep one consistent conversation modality unless there is a tested design reason to change.

## Conversation Events

Conversation graph Event nodes can trigger Device events. Current documentation states that a single conversation can connect to up to 10 events.

Use explicit numbering:

- Conversation Event 1;
- Conversation Event 2;
- and so on.

Document each graph node and bound Device consequence.

Example:

`Conversation Device — Conversation Event 1 -> Cinematic Sequence Device — Play`

`Conversation Device — Conversation Event 1 -> Barrier — Disable`

## Conversation Scope

The UI is shown to the initiating player. A Conversation Event can call a global Device function.

Therefore a single player's choice may open a route for everyone. Document whether that is intended.

## Authored Conversation Test

Test:

- first interaction;
- repeated interaction;
- two players interact simultaneously;
- one player walks away;
- UI cancel;
- every branch;
- Conversation Event numbering;
- global consequences;
- round reset;
- Join in Progress;
- controller/touch input;
- localization and text overflow.

## Limitation Without Verse

Device-only conversations are finite authored graphs. Complex state-dependent dialogue may require duplicating Conversation Banks/Devices or using Device state. If the required conditions cannot be represented by supported Devices, state that Verse is required for reliable custom logic.

---

# LLM Conversations and Personas

## Current Status in v41.30

LLM Conversations exited Experimental in v41.30. Epic states that creators can publish islands with LLM-powered NPCs and characters. A subset of Fortnite IP characters has locked voices and personas that cannot be edited.

This status is version-sensitive and subject to current Developer Rules, AI/voice/persona rules, moderation, and feature availability.

## Distinction from Authored Conversations

| Authored Conversation Device | LLM Conversation / Persona |
|---|---|
| creator writes the graph and choices | model produces conversational responses |
| deterministic finite branches | adaptive natural-language exchange |
| explicit Conversation Events | behavior depends on persona system and supported outputs |
| easier to test exhaustively | requires safety, prompt, latency, and moderation testing |
| works as a scripted narrative tool | works as an AI character interaction |

## Persona Setup

Current official workflow includes:

1. create an NPC Character Definition;
2. use the supported Custom character path;
3. add a `Persona Modifier`;
4. define character facts, personality, voice, and interruption handling through current tools;
5. save;
6. assign to NPC Spawner;
7. use Prompt Editor and current Persona documentation;
8. launch a session;
9. test safe and unsafe prompts;
10. verify publishing rules.

## Production Cautions

- responses are not fully deterministic;
- voice/persona availability can be locked for IP characters;
- moderation and content rules apply;
- latency and service availability must be tested;
- do not use the NPC as a real-world authority;
- do not request or expose sensitive personal data;
- provide a non-LLM fallback for critical progression where possible;
- do not make essential game completion depend on a single unpredictable free-form response.

## Non-Verse Progression Rule

If an LLM NPC must unlock gameplay, use supported deterministic Device outputs or a parallel explicit interaction. Do not assume arbitrary conversation content can be reliably converted into custom gameplay state without documented support.

---

# Sequencer, Level Sequences, and Cinematics

## Cinematic Ownership

- **Level Sequence asset:** timeline content.
- **Sequencer editor:** tracks, keys, cameras, animation, audio, events, Data Layers.
- **Cinematic Sequence Device:** runtime playback and multiplayer visibility.
- **Control Rig:** authored animation for supported characters/objects.
- **NPC Sequencer Modifier:** required for certain spawned NPC bindings.

## Create a Level Sequence

1. create a Level Sequence asset in project content;
2. name it;
3. open Sequencer;
4. add Actors or spawnables;
5. add Camera Cuts where needed;
6. add transform, animation, audio, visibility, or Data Layer tracks;
7. set sequence length;
8. preview;
9. save;
10. assign to Cinematic Sequence Device.

## Cinematic Sequence Device Fields

Current documented fields include:

| Field | Purpose |
|---|---|
| `Sequence` | Level Sequence to play |
| `Loop Playback` | repeats until stopped |
| `Finish Completion State Override` | no override, keep state, or restore state |
| `Auto Play` | automatic playback |
| `Enabled on Phase` | phase for automatic playback |
| `Visibility` | everyone, instigator, or instigator's team according to current options |
| `Level Sequence Actor Always Relevant` | keeps the sequence network-relevant where distance would otherwise matter |

Exact option values must be checked in the active release.

## Documented Functions and Event

Functions:

- `Play`;
- `Pause`;
- `Stop`;
- `Toggle Pause`.

Event:

- `On Stopped Event`.

Do not transfer these names to another Device.

## Completion State

Choose deliberately:

- **Restore State:** return animated properties to pre-sequence values.
- **Keep State:** leave final animated state.
- **No Override:** follow sequence behavior.

For a gameplay door or environmental reveal, test whether final collision and Device state match the visual final state. Sequencer animation alone may not produce the gameplay state required by other systems.

## Multiplayer Visibility

Test:

- visibility setting;
- instigator ownership;
- a second player outside the area;
- a player who joins during playback;
- a player who joins after completion;
- interruption;
- player elimination during sequence;
- camera return.

## Skip and Recovery

Without Verse, skip handling is limited to Device functions and available input/interaction Devices.

Provide:

- a Stop/Pause binding if appropriate;
- a fallback route after interruption;
- a clear completion event;
- a final gameplay state independent of camera position;
- no soft lock if a player misses the sequence.

## Data Layers in Sequencer

A Level Sequence can load, activate, and unload Data Layers. Use preroll to allow loading before activation. This is a global environment transition unless otherwise documented.

---

# Control Rig and NPC Animation

## Control Rig

Use Control Rig to pose and animate supported characters or objects in UEFN.

Workflow:

1. confirm a supported Skeletal Mesh or character;
2. create/open the Control Rig;
3. create a Level Sequence;
4. add the character;
5. animate controls;
6. preview;
7. play through Cinematic Sequence Device;
8. test on target platforms.

## Spawned NPC Binding

Current Epic documentation for NPC animation requires a Sequencer Modifier on the NPC Character Definition for runtime binding in specific workflows. The modifier includes a Unique Identifier used to find the spawned NPC.

If the modifier is missing, validation can fail.

Test:

- one NPC;
- multiple NPCs with unique identifiers;
- spawn before sequence;
- spawn triggered with sequence;
- replaceable versus spawnable binding;
- NPC despawn;
- round reset.

A documented pattern is:

`NPC Spawner — On Spawned -> Cinematic Sequence Device — Play`

Use the exact active event label displayed by the NPC Spawner.

## Sidekick Control Rig

v41.20 introduced Control Rig support for selected Sidekick archetypes. Sidekick publishing and asset rules are version-sensitive. Use only the rigs and project context currently permitted.

---

# Audio

## Audio Layers

Separate:

- ambience;
- music;
- dialogue;
- objective feedback;
- interaction feedback;
- failure/success accents;
- spatial environmental sound;
- UI sound.

## Imported Audio

Current UEFN import documentation supports AIF, FLAC, OGG, and WAV. Import into a project-owned folder and retain rights evidence.

Check:

- duration;
- channels;
- sample quality;
- loop;
- loudness;
- compression;
- licensing;
- localization;
- content rules.

## Audio Player Device

Use to play selected audio in response to Device events.

Verified receiving function commonly used:

- `Play`.

Current Device pages also expose additional functions such as stop, enable, disable, register, and unregister according to Device configuration; retrieve the exact active list before documenting a build.

## Spatial Audio

Decide:

- spatialized or non-spatial;
- attenuation range;
- location;
- audience;
- sync;
- looping;
- fade in/out.

Critical feedback should not be audio-only.

## Audio Mixer

Use Audio Mixer Device for dynamic control of groups of sounds. Define music, SFX, dialogue, and ambience priorities. Test voice/chat coexistence and platform volume settings.

## MetaSounds

MetaSounds are available in UEFN, but their exact supported feature set and validation constraints are version-sensitive. Use for procedural or designed audio only after a minimal import/validation/session test.

## Audio Test

- one player near source;
- one far away;
- two players in different areas;
- repeated playback;
- rapid retrigger;
- stop;
- round reset;
- late join;
- low-volume platform;
- voice chat enabled.

---

# VFX and Niagara

## VFX Device Layer

Use VFX Spawner or VFX Creator for configured feedback and environmental effects.

Typical functions include enable, disable, start, stop, or toggle according to the Device. Retrieve the exact Device page before writing a binding.

## Niagara

Use Niagara for custom particle systems.

Workflow:

1. create or import a supported Niagara system;
2. build a small emitter;
3. set bounds;
4. set spawn rate;
5. set lifetime;
6. add material;
7. place;
8. bind through a supported Device/sequence workflow;
9. launch and profile.

## Performance Risks

- excessive spawn rate;
- long particle lifetime;
- transparent overdraw;
- large bounds;
- particle lights;
- collisions;
- many unique systems;
- always-running ambient effects;
- full-island visibility;
- unsafe flashing.

## Feedback Rule

VFX should reinforce a state change, not substitute for state. A route should actually open; the VFX merely confirms it.

---

# Multiplayer and Join in Progress

## Test Matrix

| Scenario | Minimum check |
|---|---|
| Solo | core loop and reset |
| Two players | instigator versus observer |
| Maximum practical players | congestion and Device scope |
| Join before objective | onboarding and current state |
| Join during conversation | availability and global consequences |
| Join during cinematic | visibility and camera |
| Join after global unlock | route and objective state |
| Player leaves | Tracker/team/NPC recovery |
| Respawn | UI and inventory restoration |
| New round | all intended state reset |

## Global Choice Warning

If one player selects a Conversation choice that disables a Barrier globally, every player receives the route change. If each player needs an independent choice, a global Barrier is the wrong state owner.

Without Verse, per-player world geometry is limited to Device-supported ignore lists, teams/classes, visibility, or separate routes.

## Simultaneous Interaction

Test two players interacting with:

- same Character;
- same Conversation Device;
- same Pop-Up Dialog;
- same Conditional Button;
- same item;
- same Tracker;
- same NPC.

Document concurrency limits and current Device options.

---

# Reset and Repeated Activation

## Reset Categories

- Device automatically resets at round start;
- Device remains changed until explicitly reset;
- sequence restores state;
- sequence keeps state;
- item respawns;
- NPC Spawner produces new NPCs;
- Tracker resets;
- persistence reloads;
- UI clears;
- global Island Settings start a new round.

## Reset Checklist

For every system, answer:

- Does the Device reset itself?
- Is it enabled at game start?
- Is state global or per-player?
- Does a repeated event duplicate score/progress?
- Can the same conversation event fire twice?
- Does a sequence replay from frame zero?
- Does the NPC respawn?
- Does audio overlap?
- Do particles remain?
- Does the route close again?

## Anti-Duplication Without Verse

Use supported Device controls:

- disable source Device after success;
- consume required item;
- complete or remove Tracker;
- Switch state;
- one-time trigger;
- finite spawn limit;
- Barrier remains disabled;
- round reset.

If no Device exposes a reliable one-time state, state that the design needs Verse or simplification.

---

# Debugging Device-Only Gameplay

## Trace Format

For a failed mechanic, write:

```text
Expected:
Source Device:
Expected Event:
Receiving Device:
Expected Function:
Instigator:
Current state:
Visible result:
Reset state:
```

## Troubleshooting Order

1. confirm the correct project/session;
2. confirm latest changes were pushed;
3. verify source Device enabled and phase;
4. verify source Event can occur;
5. verify receiving Device name;
6. verify function binding in `User Options - Functions`;
7. verify scope/team/class;
8. verify instigator context;
9. verify receiving Device current state;
10. isolate the binding;
11. test solo;
12. test two players;
13. test reset;
14. inspect Event Browser and logs.

## Conversation Fails to Start

Check:

- Conversation Device assigned a Conversation Bank;
- graph has Default Entry Point;
- graph is saved;
- initiation binding uses the correct source;
- Device enabled;
- player can interact;
- UI type valid;
- simultaneous conversation limit;
- NPC/Character present;
- current release status.

## Conversation Event Does Nothing

Check:

- correct numbered Event node;
- graph path actually reaches it;
- receiving function configured;
- source selected as Conversation Device;
- exact Conversation Event number;
- global Device enabled;
- instigator required;
- repeated activation disabled.

## Cinematic Does Not Play

Check:

- Sequence assigned;
- Play function binding;
- active phase;
- visibility;
- sequence length;
- camera cut;
- spawned NPC binding;
- Sequencer Modifier and identifier;
- network relevance;
- session up to date.

## UI Not Visible

Check:

- Device audience;
- auto display;
- Show binding;
- HUD Controller not hiding it;
- widget type compatible;
- widget asset saved;
- anchors and opacity;
- safe zone;
- focus;
- another message layer replacing it.

---

# Complete Non-Verse Project Example

## Project: The Restored Archive

### Goal

Build a cooperative 1-4 player museum quest:

1. players explore a damaged archive;
2. they find a restoration key;
3. they speak with an Archivist;
4. a conversation choice restores the gallery;
5. a short cinematic changes the environment;
6. a Barrier opens;
7. audio, VFX, and HUD confirm success;
8. players enter the final exhibit;
9. an End Game Device ends the game after the final trigger.

No Verse is used.

## Systems Used

### World and Assets

- modular museum walls and floors;
- imported or Fortnite props;
- one custom sign texture;
- one Material Instance;
- a damaged and restored visual state;
- optional Data Layer for the restored state.

### Devices and Assets

- Player Spawn Pads;
- Capture Item Spawner or Item Spawner;
- Character Device for the Archivist, or NPC Spawner with default/empty behavior;
- Conversation Device;
- Conversation Bank;
- Barrier;
- HUD Message Devices;
- Cinematic Sequence Device;
- Level Sequence;
- Audio Player;
- VFX Spawner;
- Trigger;
- End Game Device;
- optional Tracker;
- Island Settings.

## Folder Structure

```text
Maps/Main
Gameplay/Devices/Archive
NPC/CharacterDefinitions
NPC/ConversationBanks
Cinematics/Archive
UI/Archive
Audio/Archive
VFX/Archive
Art/Museum
Tests
```

## World Build

1. create a start lobby;
2. create an exploration hall;
3. create an Archivist interaction area;
4. place a sealed gallery entrance;
5. place the final exhibit behind the Barrier;
6. create a recovery path so players cannot fall out of the level;
7. keep corridors wide enough for four players;
8. verify NavMesh if using NPC Spawner;
9. build lighting for both Lumen and non-Lumen readability.

## Item Setup

Use an item source with a documented pickup event.

Example binding:

`ItemSpawner_RestorationKey — On Item Pick Up Send Event To → HUD_KeyFound — Show`

Optional:

`ItemSpawner_RestorationKey — On Item Pick Up Send Event To → Tracker_Archive — Increment Progress`

If using a Conditional Button instead, require the player to present the key before the Archivist interaction is enabled or before the Barrier can open.

## Archivist Setup

### Option A: Character Device

1. place Character Device;
2. choose a Fortnite character;
3. set interaction text;
4. rename `CHAR_Archivist`;
5. place Conversation Device;
6. assign `CB_ArchivistRestoration`.

Binding:

`CHAR_Archivist — On Interacted With Send Event To → CONV_Archivist — Initiate Conversation`

This exact pattern is documented by Epic's Conversation Device design example.

### Option B: NPC Spawner Without Verse

1. create `NPCD_Archivist`;
2. use Guard Default Behavior only if movement/Guard behavior is desired; or use a currently supported Empty Behavior/cinematic-only path;
3. add cosmetic/team/modifier settings;
4. assign to `NPCSP_Archivist`;
5. verify NavMesh;
6. use a supported interaction source to initiate the Conversation.

Do not claim a custom friendly guide behavior without Verse.

## Conversation Graph

Create:

- Default Entry Point;
- Archivist greeting;
- choice 1: "Restore the archive";
- choice 2: "I need more time";
- `Event One` after choice 1;
- return/exit after choice 2.

Event meaning:

- `On Conversation Event One` = global restoration accepted.

## Direct Event Bindings

Configure receiving functions in each receiving Device.

| # | Source Device | Event | Receiving Device | Function | Result |
|---:|---|---|---|---|---|
| 1 | `CHAR_Archivist` | `On Interacted With Send Event To` | `CONV_Archivist` | `Initiate Conversation` | initiating player sees dialogue |
| 2 | `CONV_Archivist` | `On Conversation Event One` | `CINE_ArchiveRestore` | `Play` | restoration sequence begins |
| 3 | `CONV_Archivist` | `On Conversation Event One` | `BAR_FinalGallery` | `Disable` | route opens globally |
| 4 | `CONV_Archivist` | `On Conversation Event One` | `AUD_RestoreAccent` | `Play` | success accent plays |
| 5 | `CONV_Archivist` | `On Conversation Event One` | `VFX_RestoreBurst` | `Enable` | visual success cue |
| 6 | `CONV_Archivist` | `On Conversation Event One` | `HUD_Restored` | `Show` | objective update appears |
| 7 | `TRG_FinalExhibit` | `On Triggered Send Event To` | `END_ArchiveComplete` | `Activate` | game ends |

This table uses the documented `Enable` function for the VFX Spawner. If a different VFX Device is substituted, retrieve that Device's own documented function list.

## Level Sequence

The sequence can:

- animate damaged props away;
- animate restored props into place;
- change a Data Layer state;
- animate lights;
- play camera cuts;
- play audio.

Cinematic Sequence Device:

- assign `LS_ArchiveRestore`;
- `Loop Playback`: off;
- choose `Finish Completion State Override` intentionally;
- choose visibility;
- consider `Level Sequence Actor Always Relevant` if players can be far apart.

Use `On Stopped Event` for any consequence that must wait until playback ends.

Alternative binding:

`CINE_ArchiveRestore — On Stopped Event → BAR_FinalGallery — Disable`

This avoids opening the route before the visual restoration finishes.

## UI

- `HUD_KeyFound`: "Restoration key found. Speak with the Archivist."
- `HUD_Restored`: custom User Widget with icon and "Final Gallery Open".
- optional Pop-Up Dialog for pre-game instructions.
- Conversation uses Box or Custom UI.
- in-world UMG label at the final exhibit.

## Audio and VFX

- ambient archive room tone;
- key pickup sound;
- restoration accent;
- looping ambient VFX disabled by default;
- restoration burst activated once.

Ensure audio has visual/text backup.

## Multiplayer Ownership

This design uses a global restoration:

- one player's choice opens the Barrier for everyone;
- the cinematic should be visible to the intended audience;
- other players may be elsewhere;
- late joiners should see the restored gallery open.

If each player must independently restore the gallery, this global Device design is not appropriate without separate per-player routes or Verse.

## Reset

For a one-round game:

- item available at round start;
- Conversation available;
- Barrier enabled;
- VFX disabled;
- sequence at start state;
- End Game inactive until trigger;
- round/game restart resets everything.

Test a second round. If any prop remains restored because Keep State persists beyond the intended lifecycle, use Restore State or an explicit reset sequence/device path.

## Minimal Launched-Session Test

### Solo

1. spawn;
2. collect key;
3. verify key HUD;
4. interact with Archivist;
5. choose "more time";
6. verify route remains sealed;
7. interact again;
8. choose restore;
9. verify cinematic, audio, VFX, HUD, and Barrier;
10. enter final exhibit;
11. verify game end.

### Two Players

1. both spawn;
2. player A collects key;
3. inspect player B's HUD;
4. player A initiates conversation;
5. player B moves to sealed gallery;
6. player A chooses restore;
7. verify player B sees the route open;
8. verify cinematic visibility for both;
9. player B enters final area;
10. verify game end scope.

### Join in Progress

1. player A restores archive;
2. player B joins afterward;
3. verify spawn;
4. verify Barrier is open;
5. verify the objective message does not incorrectly tell B to find an already-used key;
6. verify final trigger remains available.

## Optimization and Publishing Preparation

- reuse museum meshes and Material Instances;
- exclude indoor small props from HLOD;
- measure imported textures;
- keep VFX bounded and short;
- profile the cinematic area;
- run validation;
- run memory calculation;
- test Private Version;
- document imported-asset licensing;
- complete publishing steps in `10`.

## Known Non-Verse Limitations

- no arbitrary per-player narrative memory;
- no custom dynamic conversation conditions beyond exposed systems;
- no custom NPC guide behavior;
- no custom reconstructed UI history for late joiners;
- no guaranteed one-time event beyond Device-supported state;
- no custom save schema.

---

# Preserved Verse-Specific Sections

The following sections are retained from the previous canonical file and were not expanded as part of the UEFN-without-Verse task.

## Verse Toolchain in UEFN

Create Verse files through Verse Explorer, build Verse code, inspect compiler diagnostics, place generated Verse-authored devices, assign editable references, launch a session, and test runtime behavior. A successful build does not prove correct gameplay; validate events, players joining late, rounds, reset, cancellation, and failure paths.

## Core Fortnite and Verse API Objects

- `agent` represents a participant abstraction and may refer to a player or NPC.
- `player` is a human participant and can be obtained only where conversion is valid.
- `fort_character` represents the Fortnite character associated with an agent when available.
- `fort_playspace` provides players and team context.
- Devices expose functions and listenable events through the Fortnite API.
- Transforms, vectors, rotations, and spatial units must use the correct namespace and coordinate conventions.

## Controlling Devices from Verse

Expose Device references with editable properties, subscribe to listenable events, store subscription handles when cancellation matters, and keep callbacks small. Validate reference assignment before the session. Avoid creating hidden bidirectional ownership where a Device graph and Verse both mutate the same state independently.

### Persistence Applications

Use persistence only for player data that must survive sessions. Define a versioned, persistable schema; provide defaults; update through safe copies; limit data size; and plan migration before changing published schemas. Language rules are in `14`; production maintenance is in `10`.

### Custom NPC Behavior with Verse

Use a clear behavior state machine: initialize, acquire context, navigate, wait/interact, recover, and terminate. Treat navigation results and missing targets as failable. Avoid unbounded loops and ensure tasks can end when the NPC despawns or gameplay phase changes.

# Official Source Registry

- [Devices in UEFN](https://dev.epicgames.com/documentation/fortnite/devices-in-unreal-editor-for-fortnite)
- [Direct Event Binding in UEFN](https://dev.epicgames.com/documentation/fortnite/direct-event-binding-in-unreal-editor-for-fortnite)
- [Island Settings in UEFN](https://dev.epicgames.com/documentation/fortnite/island-settings-in-unreal-editor-for-fortnite)
- [In-Game User Interfaces](https://dev.epicgames.com/documentation/fortnite/ingame-user-interfaces-in-unreal-editor-for-fortnite)
- [User Interface Devices](https://dev.epicgames.com/documentation/fortnite/user-interface-devices-in-unreal-editor-for-fortnite)
- [UI Widget Editor](https://dev.epicgames.com/documentation/fortnite/ui-widget-editor-in-unreal-editor-for-fortnite)
- [UI Pop-Ups](https://dev.epicgames.com/documentation/fortnite/ui-popups-in-unreal-editor-for-fortnite)
- [Using the Viewmodel in UMG](https://dev.epicgames.com/documentation/fortnite/using-the-viewmodel-in-umg-in-unreal-editor-for-fortnite)
- [Conversations](https://dev.epicgames.com/documentation/fortnite/conversations-in-unreal-editor-for-fortnite)
- [Using the Conversation Device](https://dev.epicgames.com/documentation/fortnite/using-the-conversation-device-in-unreal-editor-for-fortnite)
- [Creating Conversations](https://dev.epicgames.com/documentation/fortnite/creating-conversations-in-unreal-editor-for-fortnite)
- [Custom Conversation UI](https://dev.epicgames.com/documentation/fortnite/custom-conversation-ui-in-unreal-editor-for-fortnite)
- [LLM Conversations](https://dev.epicgames.com/documentation/fortnite/llm-conversations-in-unreal-editor-for-fortnite)
- [Developing Personas](https://dev.epicgames.com/documentation/fortnite/developing-personas-overview-in-unreal-editor-for-fortnite)
- [NPC Character Definitions](https://dev.epicgames.com/documentation/fortnite/using-npc-character-definitions-in-unreal-editor-for-fortnite)
- [NPC Spawner](https://dev.epicgames.com/documentation/fortnite/using-npc-spawner-devices-in-unreal-editor-for-fortnite)
- [Understanding NPC Behavior](https://dev.epicgames.com/documentation/fortnite/understanding-npc-behavior-in-unreal-editor-for-fortnite)
- [NPC Spawner with Animations](https://dev.epicgames.com/documentation/fortnite/using-the-npc-spawner-with-animations-in-unreal-editor-for-fortnite)
- [Animation and Cinematics](https://dev.epicgames.com/documentation/fortnite/animation-and-cinematics-in-unreal-editor-for-fortnite)
- [Cinematic Sequence Device](https://dev.epicgames.com/documentation/fortnite/using-cinematic-sequence-device-in-unreal-editor-for-fortnite)
- [Audio](https://dev.epicgames.com/documentation/fortnite/audio-in-unreal-editor-for-fortnite)
- [Visual Effects](https://dev.epicgames.com/documentation/fortnite/visual-effects-in-unreal-editor-for-fortnite)
- [v41.20 Release Notes](https://dev.epicgames.com/documentation/fortnite/41-20-fortnite-ecosystem-updates-and-release-notes-in-fortnite)
- [v41.30 Release Notes](https://dev.epicgames.com/documentation/fortnite/41-30-fortnite-ecosystem-updates-and-release-notes)

## Related Documents

- [00_MASTER_KNOWLEDGE_INDEX.md](00_MASTER_KNOWLEDGE_INDEX.md)
- [01_EPIC_GAMES_DOCUMENTATION_INDEX.md](01_EPIC_GAMES_DOCUMENTATION_INDEX.md)
- [02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md](02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md)
- [08_UEFN_EDITOR_PROJECTS_ASSETS_AND_WORLD_BUILDING.md](08_UEFN_EDITOR_PROJECTS_ASSETS_AND_WORLD_BUILDING.md)
- [10_UEFN_TESTING_OPTIMIZATION_COLLABORATION_AND_PUBLISHING.md](10_UEFN_TESTING_OPTIMIZATION_COLLABORATION_AND_PUBLISHING.md)
- [12_BOOK_OF_VERSE_I_FOUNDATIONS.md](12_BOOK_OF_VERSE_I_FOUNDATIONS.md)
- [13_BOOK_OF_VERSE_II_PROGRAM_STRUCTURE_AND_TYPES.md](13_BOOK_OF_VERSE_II_PROGRAM_STRUCTURE_AND_TYPES.md)
- [14_BOOK_OF_VERSE_III_EFFECTS_CONCURRENCY_AND_EVOLUTION.md](14_BOOK_OF_VERSE_III_EFFECTS_CONCURRENCY_AND_EVOLUTION.md)
- [16_GLOSSARY_UEFN.md](16_GLOSSARY_UEFN.md)
- [17_GLOSSARY_VERSE.md](17_GLOSSARY_VERSE.md)
