---
document_id: "10"
corpus_role: "active_agent_knowledge"
authority: "verified_uefn_production_testing_optimization_collaboration_publishing"
primary_environment: "UEFN with or without Verse; procedures default to no-Verse projects"
last_verified: "2026-08-03"
verified_release: "Fortnite Ecosystem v41.30"
---

# UEFN: Testing, Optimization, Collaboration, and Publishing

## Document Metadata

| Field | Value |
|---|---|
| Document ID | `10` |
| Domain | UEFN sessions, testing, validation, optimization, collaboration, and publishing |
| Primary Environment | UEFN with or without Verse; examples default to UEFN without Verse |
| Language | English; exact UI labels remain in English |
| Authority | Current Epic Games documentation, release notes, Creator Portal documentation, creator rules, and production guidance labeled as professional practice |
| Last Verified | 2026-08-03 |
| Verified Release | Fortnite Ecosystem `v41.30`, released 2026-07-30 |
| Stability Status | Stable production principles plus version-sensitive session, validation, memory, Lore, moderation, eligibility, and publishing behavior |

## Document Purpose

Provide an implementation-ready production lifecycle for UEFN projects from the first launched session through validation, memory calculation, runtime profiling, team collaboration, Private Versions, Creator Portal submission, release, and maintenance.

This file owns:

- Launch Session, Live Edit, Push Changes, Refresh Session, and cook behavior;
- solo, multiplayer, Join in Progress, lifecycle, reset, and regression testing;
- validation, cook, upload, runtime, and publishing troubleshooting;
- memory calculation, streaming/HLOD verification, Spatial Profiler, and optimization;
- cross-platform and controller testing;
- Lore Version Control and team workflows;
- backup, recovery, migration, and release records;
- Private Versions, Creator Portal, IARC, moderation, visibility, and post-release maintenance.

Editor authoring belongs to `08`. Device gameplay, non-Verse UI, NPCs, Conversations, cinematics, audio, and VFX belong to `09`. Verse language semantics belong to `12`-`14`.

## Scope Boundary

Included:

- UEFN projects that use no Verse;
- shared production procedures that also apply to Verse projects;
- exact separation between editor validation, cooking, memory, runtime performance, and publishing;
- evidence-based testing and release gates.

Excluded:

- Verse code and Verse API instruction;
- Blueprint gameplay scripting;
- Fortnite Creative Phone Tool workflows;
- unsupported methods for bypassing validation, moderation, eligibility, or creator rules.

## Authority and Evidence Labels

- **Documented:** Supported by current official Epic documentation.
- **Professional practice:** A recommended production method, not a platform guarantee.
- **Version-sensitive:** Confirm exact labels, thresholds, eligibility, feature status, and publishing requirements in the current release.
- **Session-dependent:** Must be verified in a launched Fortnite session.
- **Private-Version-dependent:** Must be checked on a generated Private Version, not only through Live Edit.
- **Platform-dependent:** Results can differ by hardware, input, rendering path, and network conditions.

## Quick Topic Index

- [Production Lifecycle](#production-lifecycle)
- [Build Identity and Evidence](#build-identity-and-evidence)
- [Sessions, Live Edit, Push Changes, and Refresh](#sessions-live-edit-push-changes-and-refresh)
- [Testing Strategy and Playtest Matrix](#testing-strategy-and-playtest-matrix)
- [Multiplayer Lifecycle, Join in Progress, and Reset](#multiplayer-lifecycle-join-in-progress-and-reset)
- [Debugging and Validation](#debugging-and-validation)
- [Memory, Project Size, and Runtime Performance](#memory-project-size-and-runtime-performance)
- [Memory Calculation Workflow](#memory-calculation-workflow)
- [Streaming, World Partition, HLOD, and Data Layers](#streaming-world-partition-hlod-and-data-layers)
- [Spatial Profiler and Runtime Profiling](#spatial-profiler-and-runtime-profiling)
- [Optimization Playbooks](#optimization-playbooks)
- [Cross-Platform, Mobile, Controller, and Accessibility](#cross-platform-mobile-controller-and-accessibility)
- [Project Size and Dependency Cleanup](#project-size-and-dependency-cleanup)
- [Lore Version Control and Collaboration](#lore-version-control-and-collaboration)
- [Conflict Prevention and Resolution](#conflict-prevention-and-resolution)
- [Backup, Recovery, and Migration](#backup-recovery-and-migration)
- [Localization and Accessibility Operations](#localization-and-accessibility-operations)
- [Private Versions and Publishing](#private-versions-and-publishing)
- [Creator Portal, IARC, Moderation, and Release](#creator-portal-iarc-moderation-and-release)
- [Content Rules, IP, Attribution, and Safety](#content-rules-ip-attribution-and-safety)
- [Release Maintenance and Regression](#release-maintenance-and-regression)
- [Complete Production Example: The Restored Archive](#complete-production-example-the-restored-archive)
- [Production Checklists](#production-checklists)
- [Official Source Registry](#official-source-registry)

---

# Production Lifecycle

Use a staged lifecycle. Each stage must end with observable evidence, not a statement that the project “looks finished.”

| Stage | Primary goal | Required evidence before exit |
|---|---|---|
| Project setup | Establish scope, player count, platforms, ownership, naming, and risk | Project opens, syncs, launches, and has a recorded baseline |
| Blockout | Prove scale, route, spawn, objective, and reset | A player can complete the shortest loop in a launched session |
| Vertical slice | Prove one polished end-to-end experience | World, Devices, UI, feedback, reset, and multiplayer work together |
| Gameplay-complete | All intended mechanics exist | Full loop works for supported player states and failure paths |
| Content-complete | Required art, UI, audio, NPC, and narrative are present | No placeholder required for release; source licenses recorded |
| Optimization | Meet memory and runtime targets | Current memory calculation plus representative runtime profile |
| Release candidate | Freeze scope and remove blockers | Validation, Private Version, regression matrix, metadata, and rollback candidate |
| Moderation | Submit accurate release | Creator Portal checks and moderation submission completed |
| Launch | Publish approved version | Public or scheduled release verified from player-facing discovery flow |
| Maintenance | Observe, fix, and update safely | Analytics/feedback reviewed; regressions and ecosystem updates tracked |

## Stage Exit Rule

Do not advance because a calendar date arrived. Advance when the required evidence exists. A project can be visually content-complete while still failing multiplayer, memory, validation, moderation, or reset tests.

## Scope Freeze

Before optimization and publishing:

1. freeze major feature additions;
2. list only release-blocking fixes;
3. identify the source revision and Private Version under test;
4. avoid simultaneous art, gameplay, and publishing changes;
5. create a rollback point.

**Professional practice:** A release candidate should change only when a recorded defect is fixed. Every change invalidates at least part of the previous test evidence.

---

# Build Identity and Evidence

A test result is useful only when it identifies what was tested.

## Minimum Build Record

Record:

- project name;
- Lore revision/changelist or equivalent source identifier;
- UEFN/Fortnite ecosystem version;
- date and tester;
- test environment: editor session, pushed session, Private Version, or published release;
- platform and input method;
- player count;
- Island Settings relevant to the test;
- expected result;
- actual result;
- screenshots/video/log excerpts where needed.

## Evidence Layers

Keep these separate:

1. editor save completed;
2. local editor validation passed;
3. project cooked/uploaded;
4. launched-session runtime behavior passed;
5. current memory calculation passed;
6. representative runtime performance passed;
7. Private Version multiplayer test passed;
8. Creator Portal validation passed;
9. moderation approved;
10. public release verified.

Passing a lower layer does not prove the next layer.

## Version Label Template

Use a compact internal label such as:

`RC2 | Lore 184 | Private 7 | v41.30 | 2026-08-03`

This is a team convention, not an Epic-required format.

---

# Sessions, Live Edit, Push Changes, and Refresh

## Launch Session

`Launch Session` cooks and starts the current project in Fortnite for runtime testing. The session is authoritative for collision, Device behavior, UI, NPCs, networking, gameplay phases, and other runtime results.

### Minimal Workflow

1. save changed assets;
2. resolve blocking validation errors;
3. select `Launch Session`;
4. wait for Fortnite to connect to the project;
5. start the game when the test requires gameplay phase behavior;
6. run the written test;
7. end the game before changing lifecycle-sensitive settings;
8. return to UEFN and record the result.

## Live Edit

Live Edit accelerates iteration by synchronizing supported changes between UEFN and the connected Fortnite session. It does not guarantee that every asset, cooked dependency, Island Setting, UI asset, or structural change updates immediately.

Use Live Edit for fast iteration on changes that visibly update correctly. Do not treat a Live Edit result as proof that a Private Version contains the same cooked state.

## Push Changes

Use `Push Changes` when the session is connected but the current project state has not fully reached Fortnite.

Typical reasons:

- an asset or Device was added after session launch;
- a property requires recooking;
- an imported asset changed;
- a widget, material, sequence, or NPC definition did not refresh;
- memory-related cooked data is stale.

After pushing, repeat the exact test. Do not infer success from the progress indicator alone.

## Refresh Session

Use `Refresh Session` when the connected client is stale or the session state no longer reflects the intended build. Refreshing is a stronger reset than a small Live Edit update but may still be less expensive than a complete relaunch.

## Full Relaunch / Full Recook

Use a full relaunch when:

- Push Changes repeatedly fails;
- Island Settings or project-wide systems are stale;
- validation/cook output changed substantially;
- runtime behavior differs from the Private Version;
- a crash or disconnect left the session state uncertain;
- a release candidate is being tested.

**Professional practice:** Full-cook testing is the final authority before creating a Private Version. Fast iteration modes optimize development time, not evidence quality.

## Edit List

When UEFN marks changes as requiring a push, inspect the change/edit list where available. Group related changes and avoid pushing unrelated experimental content into a release-candidate session.

## Session Troubleshooting Order

1. confirm Fortnite is running on the expected account;
2. confirm the correct UEFN project is open;
3. save all relevant assets;
4. inspect validation, Message Log, and Output Log;
5. Push Changes;
6. Refresh Session;
7. end game and restart game;
8. close and relaunch the session;
9. restart UEFN/Fortnite if connection state remains invalid;
10. create a minimal reproduction if the issue persists.

## UEFN Session Inspector

The current **UEFN Session Inspector** exposes the launch and Live Edit pipeline rather than leaving creators with only a progress spinner.

Open it from:

`Tools > Live Edit Tools > Session Inspector`

It can also be configured to open automatically when launching a session.

Use it to inspect:

- open project;
- current session status;
- session wall time;
- module and child-artifact cook jobs;
- cache use;
- completion time;
- `In progress`, `Succeeded`, `Failed`, or `Unknown` cook state;
- links/context for Content Service and Output Log investigation.

### Session Inspector Troubleshooting

1. find the first failed child artifact rather than the parent module;
2. hover the session-status error for the full message;
3. inspect Output Log;
4. distinguish a long uncached cook from a failed cook;
5. correct the affected source asset or dependency;
6. Push Changes or relaunch;
7. verify that the same artifact now succeeds.

A long launch is not automatically a defect. Cached artifacts can complete quickly while changed or dependent modules cook again.

---

# Testing Strategy and Playtest Matrix

## Test Types

| Test type | Purpose | Typical build |
|---|---|---|
| Smoke test | Determine whether the project opens, launches, spawns, and completes one loop | Latest pushed session |
| Mechanic test | Verify one Device chain or system | Minimal test map/session |
| Lifecycle test | Verify start, respawn, round, game, and reset behavior | Full launched session |
| Multiplayer test | Verify global, team, and player-scoped results | Two or more clients |
| Join in Progress test | Verify late-player state reconstruction | Existing session plus late joiner |
| Regression test | Confirm old working features still work after changes | Release candidate |
| Performance test | Measure memory and runtime hotspots | Representative content and players |
| Private Version test | Verify cooked, uploaded release-like behavior | Current Private Version |
| Moderation readiness test | Verify metadata, media, ratings, and rules | Creator Portal draft |

## Minimum Playtest Matrix

Test at least:

- solo;
- minimum supported player count;
- maximum practical player count available to the team;
- cooperative players acting as intended;
- players acting out of order;
- repeated activation;
- two players activating the same objective simultaneously;
- respawn during each critical phase;
- player leaving during each critical phase;
- Join in Progress before and after each persistent/global transition;
- round end and next-round reset;
- game end and new-game reset;
- long session with repeated loops;
- controller and keyboard/mouse where supported;
- lowest target platform available.

## Test Case Format

| Field | Required content |
|---|---|
| ID | Stable test identifier |
| Build | Revision, Private Version, ecosystem version |
| Preconditions | Player count, state, inventory, phase |
| Steps | Exact numbered actions |
| Expected | Observable result |
| Actual | What occurred |
| Evidence | Screenshot, video, log, profiler capture |
| Status | Pass, fail, blocked, not tested |
| Owner | Person responsible for next action |

## Minimal Reproduction Map

For unstable behavior:

1. duplicate or create a small test level;
2. include only the required Devices/assets;
3. use default settings unless the defect depends on a changed setting;
4. create one Event → Function binding at a time;
5. launch a fresh session;
6. increase complexity until the defect returns.

This separates a platform issue from a project-specific dependency.

---

# Multiplayer Lifecycle, Join in Progress, and Reset

## State Ownership

For every gameplay state, record:

- owner: player, team, or island/global;
- initial value;
- Devices allowed to change it;
- visible feedback;
- reset moment;
- behavior after respawn;
- behavior after leaving;
- behavior for Join in Progress;
- behavior on round end and new game.

## Join in Progress

A late player does not experience the original event sequence. They enter the current world state. Test whether they receive enough information and access to continue.

Check:

- spawn point remains valid;
- barriers/doors match current global state;
- one-time collectibles do not create an impossible objective;
- HUD and tracker state reflect current progress;
- NPCs and Conversations are available or intentionally unavailable;
- cinematics are not replayed incorrectly;
- the player can reach the active group;
- game-end conditions do not immediately remove them without context.

## Respawn

Verify whether respawn should reset:

- inventory;
- class/team;
- tracker assignment;
- HUD messages;
- NPC interaction availability;
- local objective progress;
- checkpoint;
- cameras/controls;
- temporary visual effects.

Do not assume round reset and respawn reset are equivalent.

## Round Reset

At the next round, verify:

- Device enablement;
- item availability;
- barriers and locks;
- NPC spawn/despawn;
- Conversation entry state;
- cinematic completion state;
- moved props;
- Data Layer state;
- trackers, timers, score, and objectives;
- UI visibility and focus;
- audio and VFX loops.

## Full Game Reset

Stop and restart the game. Then repeat the opening path. A system that works only in the first playthrough is not release-ready.

## Simultaneous Events

With two clients, test:

- both interact at nearly the same time;
- both collect/activate the same objective;
- one leaves while the other completes;
- one is in a Conversation while another triggers the next phase;
- one is viewing a cinematic while another reaches a state transition.

Without Verse, Device-supported scope and conflict behavior are the limit. If exact arbitration is not exposed, document the result as session-dependent and redesign around a global state or separate routes.

---

# Debugging and Validation

## Defect Categories

Do not mix these categories:

1. editor layout/selection issue;
2. asset import or reference issue;
3. validation error;
4. cook/upload error;
5. session connection/staleness issue;
6. Device binding or configuration error;
7. multiplayer scope/state error;
8. lifecycle/reset error;
9. UI focus/visibility error;
10. NPC navigation/lifecycle error;
11. cinematic completion-state error;
12. runtime performance problem;
13. memory calculation failure;
14. Creator Portal or moderation problem.

## Validation

UEFN validation checks project content against Fortnite-supported assets, properties, references, and platform rules. Validation can run locally and again during upload/publishing. A project that passed in an earlier ecosystem version can later expose new warnings or errors when validation rules change.

### Validation Workflow

1. save the project;
2. run the available project validation/fix-up workflow;
3. open `Message Log` and inspect the first causal error;
4. identify the referenced asset/Actor/property;
5. correct or replace the unsupported content;
6. validate again;
7. launch a fresh session;
8. create a Private Version only after blocking errors are gone.

### Fix-Up Tool

Use the official Validation and Fix-Up Tool where the issue is supported. Do not apply automatic fixes without reviewing the affected assets and retesting references, materials, transforms, or runtime behavior.

### First-Causal-Error Rule

Logs often contain downstream errors. Fix the first specific, actionable error before chasing later symptoms.

## Common Validation Causes

- unsupported imported content;
- disallowed property values;
- references to editor-only or unsupported assets;
- invalid asset path or moved/deleted dependency;
- unsupported material/function usage;
- illegal object types or properties copied from Unreal Engine;
- stale or corrupted generated assets;
- feature-status change after an ecosystem update.

## Device Binding Debugging

For every connection, write:

`Source Device — Event → Receiving Device — Function`

Then verify:

1. correct receiving Device selected;
2. correct Function array entry exists;
3. correct source Device selected;
4. correct source Event selected;
5. both Devices enabled in the tested phase;
6. event can occur under current player/team conditions;
7. receiving Device is not already in the target state;
8. repeated activation behavior;
9. reset path;
10. session contains latest binding.

## Runtime State Trace

Use this sequence:

`Precondition → Player action → Source Event → Receiving Function → State change → Visible feedback → Reset`

When the visible feedback fails, inspect each boundary rather than changing multiple options at once.

## Logs and Messages

Use the appropriate evidence:

- `Message Log`: validation, memory, and editor-generated findings;
- `Output Log`: broader editor/runtime diagnostic output;
- session UI: connection and push state;
- Creator Portal: release validation and moderation feedback;
- Spatial Profiler: runtime spatial metrics.

Do not assume a missing log line proves a system did not run. Use visible runtime evidence and a minimal reproduction.

---

# Memory, Project Size, and Runtime Performance

These are different questions.

## Project Size

Project size describes stored/uploaded content and dependencies. It affects upload, iteration, patching, and download burden. A large project may still fit the in-island memory calculation, and a small project can still run poorly.

## Memory Calculation

Memory calculation evaluates cooked island content for publication. It is spatial where streaming applies and is not a live runtime profiler.

At the current documented workflow, publication requires every relevant area/cell to remain within the platform memory limit shown by the tool. Epic documentation currently describes a `100,000` memory-unit gate per area, but limits and reporting can change; always use the result shown by the active tool and current documentation.

## Runtime Performance

Runtime performance concerns frame time, game/server time, rendering, draw calls, shaders, primitives, AI, Devices, UI, VFX, audio, physics, streaming spikes, and networking while players are active.

A passing memory calculation does not prove acceptable runtime performance.

## Performance Budget Record

Record target budgets for:

- player count;
- target platforms;
- frame rate or acceptable responsiveness;
- worst-case NPC count;
- worst-case visible VFX;
- simultaneous UI/cinematic activity;
- fastest traversal speed;
- maximum active gameplay area;
- memory result and highest cell;
- top Spatial Profiler hotspots.

---

# Memory Calculation Workflow

## Preconditions

Before calculation:

- save all assets;
- Push Changes or create a fresh cooked state;
- remove known invalid references;
- run validation;
- build required HLODs;
- ensure streaming settings match the release configuration;
- decide which Private Version/revision will be measured.

## Run the Calculation

1. use the current UEFN project menu command for memory calculation;
2. allow the project to cook/upload as required;
3. create or identify the generated Private Version;
4. wait for calculation completion;
5. inspect the map/heat information and highest areas;
6. open `Window > Message Log > Memory Test Results` where available;
7. review the listed high-cost assets, including the documented top-heavy results;
8. record the Private Version, source revision, date, highest result, and major contributors.

## Snapshot Record

| Field | Value |
|---|---|
| Source revision |  |
| Private Version |  |
| Ecosystem version |  |
| Highest cell/area |  |
| Overall pass/fail |  |
| Top asset/device contributors |  |
| HLOD state |  |
| Streaming state |  |
| Change from previous snapshot |  |

## Memory Snapshot

**Memory Snapshot** is a runtime-connected asset analysis tool introduced in the current performance workflow. It is separate from the cooked publication memory calculation.

### Enable and Open

1. open the `Launch Session` menu;
2. enable `Monitor Performance`;
3. launch a local PC play session;
4. open the bottom `Performance` panel and select `Memory Snapshot`, or open it from Spatial Profiler;
5. capture `Detailed Insight` for the complete resource/reference analysis or `Quick Insight` for a faster, less complete capture.

Memory Snapshot currently supports locally launched PC clients. Spatial Profiler can collect from other connected clients, but Memory Snapshot insights are not available for Mobile, Remote PC, or Console sessions in the current documentation.

### What It Shows

- estimated system and video memory by loaded asset;
- asset impact percentage;
- reference counts;
- asset hierarchy and critical paths;
- `Referenced By` relationships;
- navigation to Actors that reference an asset;
- filters for external, low-memory, low-impact, or unreferenced objects.

Snapshots represent **currently loaded runtime assets**, not every project asset and not the publication memory calculation. Restart or refresh the session before comparison when stale/unused objects may remain loaded.

### Combined Investigation

1. use Spatial Profiler to locate the spatial/time hotspot;
2. take Memory Snapshot in the hotspot state;
3. identify high-impact loaded assets and unexpected references;
4. navigate to the referencing Actors;
5. change one asset/reference category;
6. restart/push as required;
7. capture the same state again.

## Live Edit Warning

Memory calculation uses cooked data. Live Edit changes do not necessarily refresh that cooked data. Push Changes or generate a new cooked/Private Version state before trusting a new result.

## Optimization Order for Memory

1. confirm the result is current;
2. identify the highest cell/area;
3. inspect top assets and repeated unique dependencies;
4. remove accidental duplicates and unused variants;
5. reduce texture/mesh/material cost where justified;
6. simplify HLOD inputs or exclusions;
7. reduce unique Device types where a repeated configured type can serve;
8. distribute dense content when streaming supports it;
9. retest after one category of change;
10. preserve visual/gameplay evidence so optimization does not break the experience.

## Device Instance Principle

Epic documentation notes that additional instances of an already-used Device type can cost less than introducing many unique Device types. This is not permission to duplicate Devices without design need. Compare measured results.

---

# Streaming, World Partition, HLOD, and Data Layers

## Streaming Verification

Test the release configuration, not only the editor-loaded region.

1. confirm `Enable Streaming` in `World Settings` where appropriate;
2. build HLODs when the project uses them;
3. launch a clean session;
4. travel through the fastest route in both directions;
5. observe geometry, collision, Devices, NPCs, audio, and objectives;
6. repeat with another client and Join in Progress;
7. inspect memory and runtime hotspots near cell boundaries.

## HLOD Verification

HLOD creates generated distant representations and can improve distant rendering/streaming behavior, but it also adds generated assets and memory.

Verify:

- correct Actors included;
- small interior props excluded when not useful at distance;
- distant silhouette acceptable;
- no missing collision when source content streams;
- no oversized HLOD generated from an inappropriate cluster;
- generated assets updated after major geometry changes;
- memory calculation measured before and after.

## Cross-Cell References

A reference from a persistent/global Actor or Device can keep content available longer than expected. Inspect:

- global Device references to distant Actors;
- sequences referencing far-away objects;
- shared materials/textures;
- parent/child relationships across areas;
- Data Layer dependencies;
- NPC/navigation needs.

Do not assume an empty visual cell has no memory cost.

## Data Layers

Use Data Layers for logical organization and controlled visibility/state where officially supported. Data Layers do not automatically remove every referenced asset from memory or runtime.

Test:

- initial state;
- activation/deactivation path;
- multiplayer visibility;
- sequence-driven changes;
- Join in Progress;
- round reset;
- memory result;
- session reload.

---

# Spatial Profiler and Runtime Profiling

## Spatial Profiler Purpose

Spatial Profiler captures spatially associated runtime metrics. Current documented metrics include categories such as Actor/object counts, memory-related values, texture streaming memory, draw calls, primitives, shaders, frame time, game time, GPU time, and render time. Metric availability can change by release.

## Representative Capture

1. define a test route and player count;
2. launch a current session;
3. reach the intended worst-case state;
4. open/start the Spatial Profiler using the current documented workflow;
5. traverse the route at gameplay speed;
6. trigger NPCs, UI, VFX, audio, and cinematics as players would;
7. capture hotspots;
8. correlate the hotspot time/location with visible content;
9. change one variable;
10. capture again and compare.

## Worst-Case States

Profile:

- all supported players grouped in one area;
- maximum NPC count active;
- largest VFX burst;
- cinematic plus UI plus audio overlap;
- fastest travel across streaming boundaries;
- dense foliage/transparent-material view;
- repeated Device activation;
- long session after multiple resets.

## Draw Call Investigation

High draw calls can arise from many visible meshes/material sections, lack of instancing, transparent layers, lights/shadows, or poorly grouped content.

Investigate:

- visible mesh count;
- material slots per mesh;
- repeated identical content versus unique variants;
- HLOD state;
- foliage setup;
- translucent decals/VFX;
- camera framing and occlusion;
- platform scalability.

## Before/After Rule

Never claim an optimization from intuition alone. Record the same route, build state, player count, platform, and profiler metric before and after.

---

# Optimization Playbooks

## Static Meshes

- prefer reusable modular meshes;
- reduce unnecessary material slots;
- provide appropriate LODs;
- use simple collision where possible;
- remove hidden duplicate geometry;
- verify bounds and scale;
- exclude tiny objects from HLOD where distant representation adds no value.

## Textures

- use appropriate dimensions and compression;
- avoid unique oversized textures for minor props;
- reuse atlases or shared textures where artistically appropriate;
- inspect texture streaming memory;
- avoid unnecessary alpha/transparency;
- test readability at target quality settings.

## Materials

- prefer Material Instances for variation;
- reduce expensive transparency, layered effects, and animation;
- limit unique shader complexity;
- verify mobile/low-end fallback;
- avoid using emissive intensity to compensate for poor lighting without profiling.

## Lighting

- design for the intended Time of Day/Environment Light Rig path;
- limit expensive shadow-casting lights;
- reduce overlapping dynamic lights;
- verify Lumen and non-Lumen/low-end presentation;
- profile exposure transitions and reflections;
- keep critical navigation readable without maximum effects.

## Landscape and Foliage

- control component/region complexity;
- reduce excessive foliage density;
- use appropriate cull/draw distance;
- limit collision to gameplay-relevant instances;
- review shadowing;
- profile dense views and traversal.

## Devices

- remove inactive duplicates and abandoned prototypes;
- reuse Device types where appropriate;
- disable systems outside intended phases;
- prevent repeated event storms;
- avoid many independent timers when one verified global phase system is sufficient;
- document every Event → Function path;
- test repeated activation and reset.

## NPCs

- cap active spawn count;
- define despawn/disable behavior;
- keep spawn radius and timer intentional;
- verify navigation coverage;
- avoid spawning through walls unless required;
- test many NPCs with many players;
- reduce simultaneous perception/pathing pressure through encounter design.

Without Verse, use only behavior and lifecycle exposed by NPC Character Definitions, modifiers, NPC Spawner, and Devices.

## UI

- show only necessary widgets;
- avoid excessive animation and frequent full-screen layers;
- keep Draw Size and in-world widget count appropriate;
- prevent overlapping Modal Dialog and gameplay input states;
- remove/hide UI at the correct lifecycle moment;
- test safe zones, localization expansion, and gamepad focus.

## Audio

- use attenuation and spatialization deliberately;
- stop loops when the phase ends;
- avoid many always-active emitters;
- group/mix categories rather than increasing every source;
- test simultaneous dialogue, music, feedback, and ambience;
- provide visual backup for required cues.

## VFX and Niagara

- bound effect duration and spawn rate;
- avoid large translucent overdraw across the screen;
- reduce simultaneous emitters;
- use Device enable/disable lifecycle;
- profile the actual camera/player distance;
- test photosensitivity-safe timing and intensity.

## Cinematics

- keep sequences only as long as necessary;
- limit simultaneously evaluated tracks;
- stop or restore state correctly;
- avoid hiding streaming or state problems behind a camera cut;
- test all intended viewers and late joiners;
- profile the sequence at its most complex frame.

## Networking and Multiplayer

Device-only projects still replicate Fortnite gameplay state. Reduce unnecessary global state changes and repeated broadcasts. Test with real remote clients where possible; local high-end testing does not reproduce every network condition.

---

# Cross-Platform, Mobile, Controller, and Accessibility

## Connect to Platform

UEFN provides a platform connection option from the session-launch control where supported. Use the current `Launch Session` menu/three-dot options and Epic instructions to connect a console or other supported platform for testing.

## Platform Matrix

Record:

- PC high settings;
- PC low settings;
- console available to the team;
- mobile/low-end preview where documented;
- keyboard/mouse;
- controller;
- touch where the experience targets touch devices.

## Mobile Preview

The current **Mobile Preview Session** launches the PC client with a selected mobile configuration.

### Workflow

1. open the vertical-ellipsis menu beside `Launch Session`;
2. under `Platform`, select `Mobile Preview (This PC)`;
3. choose a screen family such as common mobile, older mobile, or tablet;
4. select the target Device Profile;
5. launch the session;
6. test art, lighting, materials, VFX, HUD, aspect ratio, and emulated touch controls;
7. repeat on actual hardware or through the supported remote-input workflow where available.

Mobile Preview is a simulation. It does not reproduce every device's thermal, memory, GPU, network, or touch behavior.

### Current Mobile Feature Gate

The expanded touch controls, gesture support, HUD controls, and platform-control features introduced in v41.00 were still described as **Experimental** in the v41.20 release notes, with exit from Experimental planned for v42.00. In the verified v41.30 baseline, confirm live feature status and publishing support before making these systems mandatory.

## Controller

Test:

- all required interactions;
- UI focus order;
- Modal Dialog navigation;
- back/cancel behavior;
- camera/control changes;
- no keyboard-only instructions;
- readable prompts.

## Safe Zones and Readability

- keep critical UI inside safe zones;
- avoid small text;
- leave space for localization expansion;
- maintain contrast;
- do not rely only on color;
- pair audio cues with visual or text feedback;
- avoid rapid flashing;
- provide a recovery path after missed timed information.

---

# Project Size and Dependency Cleanup

## Dependency-Aware Deletion

Removing an Actor from a level does not necessarily remove its asset dependency. Before deleting:

1. find references;
2. determine whether another level, sequence, widget, NPC definition, material, or Device uses it;
3. archive source content if needed;
4. delete through the editor;
5. fix redirectors/references where supported;
6. validate and launch.

## Cleanup Targets

- abandoned prototype folders;
- duplicate imported textures;
- unused high-resolution variants;
- duplicate materials instead of Material Instances;
- unused sequences and animations;
- unused widgets;
- unused NPC Character Definitions;
- stale HLOD/generated assets after official rebuild workflow;
- disabled Devices left from experiments;
- third-party assets lacking license evidence.

## Project Size Tool

Use the current project-size/dependency tools documented by Epic to identify upload/download contributors. Treat project size, memory, and runtime separately.

## Safe Cleanup Practice

Create a recovery revision before bulk deletion. Delete one category, validate, launch, and compare project/memory/runtime evidence.

---

# Lore Version Control and Collaboration

## Current Name

Epic renamed the integrated UEFN revision-control product from `Unreal Revision Control` to **Lore Version Control** in the v41.10 ecosystem update. Older documentation, UI screenshots, logs, and team vocabulary may still say `Revision Control` or `Unreal Revision Control`. Treat them as naming generations of the integrated workflow, not separate UEFN systems.

## Core Concepts

- sync latest before editing;
- ownership/check-out controls who may edit an asset;
- Auto Checkout can reserve assets when edits begin;
- check-in/submit creates a shared revision;
- changelists group related changes;
- conflicts require intentional resolution;
- Branch Explorer provides history/branch context where available;
- saving locally is not the same as checking in.

## Start-of-Day Workflow

1. communicate the work area;
2. sync latest;
3. inspect incoming changes;
4. open the project and resolve upgrade/validation issues before editing;
5. check ownership of the assets/levels to be changed;
6. make a small working checkpoint early.

## Check-In Workflow

1. save all intended assets;
2. inspect the changelist;
3. remove accidental/unrelated files;
4. launch a smoke test;
5. write a meaningful description;
6. check in;
7. confirm teammates can sync and open the project.

## Changelist Description

Include:

- feature/fix;
- affected level/system;
- expected behavior;
- migration or reset note;
- test performed;
- known limitation.

Example:

`Archive finale: binds Conversation choice to Cinematic Sequence, Barrier, HUD, and VFX; tested solo/two-player/reset; JIP objective copy still under review.`

## Auto Checkout

Auto Checkout helps prevent simultaneous edits, but it does not replace communication. Verify that the intended asset was checked out and that hidden dependencies were not also modified.

## One File Per Entity

Scene Graph/One File Per Entity workflows can reduce contention by storing entities separately, but feature status and project compatibility are version-sensitive. Do not restructure a production project solely to avoid conflicts without a recovery copy and migration test.

---

# Conflict Prevention and Resolution

## Ownership Map

Assign ownership by:

- level/region;
- gameplay system;
- UI;
- NPC/Conversation;
- cinematics;
- audio/VFX;
- optimization;
- publishing metadata.

Avoid multiple people editing the same monolithic level or asset simultaneously.

## Conflict Prevention

- sync before starting;
- use small coherent check-ins;
- communicate renames and folder moves;
- avoid changing shared master materials casually;
- avoid simultaneous sequence edits;
- separate experimental assets;
- check in a working state before handoff;
- do not leave critical assets checked out unnecessarily.

## Conflict Resolution

1. stop editing the conflicted asset;
2. identify both contributors and intent;
3. inspect history/Branch Explorer where available;
4. determine which changes must survive;
5. resolve using the current Lore workflow;
6. reopen and validate references;
7. launch the affected feature;
8. check in a clear resolution description.

Do not resolve by blindly overwriting the other contributor's work.

## Level Conflict Strategy

For heavily shared maps:

- divide spatial ownership;
- use separate levels or logical content partitions where supported;
- place shared systems in dedicated assets;
- schedule integration windows;
- keep an integration owner.

---

# Backup, Recovery, and Migration

## Revision Control Is Not the Entire Backup Strategy

Lore provides project history, but production recovery also needs:

- milestone revisions;
- Private Versions;
- source files for imported assets;
- licensing and attribution records;
- exported documentation/settings records;
- release metadata and media;
- known-good rollback candidate.

## Recovery Drill

Periodically test:

1. restore/sync a known revision to a clean working copy;
2. open the project;
3. resolve expected version conversion prompts;
4. validate;
5. launch a smoke test;
6. confirm critical imported source assets are available.

A backup that has never been restored is unverified.

## Migration Gate

Before ecosystem upgrades, Scene Graph conversions, folder restructures, asset replacements, or major feature-status changes:

1. create a recovery revision;
2. duplicate or branch the project where supported;
3. test the conversion on the copy;
4. run validation;
5. launch and test critical paths;
6. recalculate memory;
7. profile representative areas;
8. merge/adopt only after evidence passes.

## Corruption or Missing Reference

- do not continue mass edits;
- record the first error;
- inspect recent changelists;
- restore the smallest affected asset where possible;
- validate dependencies;
- launch a minimal test;
- escalate with logs/reproduction if the issue appears platform-level.

---

# Localization and Accessibility Operations

## Localization

Player-facing text can appear in:

- Device messages;
- UMG widgets;
- Conversations;
- NPC Persona/LLM systems;
- metadata in Creator Portal;
- promotional media.

Keep text externalized in the system that owns it. Avoid embedding essential meaning only in textures.

Epic introduced automatic localization workflows for eligible published/private-version content in recent ecosystem releases; exact language coverage, eligibility, and workflow are version-sensitive. Verify current localization documentation and Creator Portal behavior before promising a language.

## Localization Test

- longest supported language/string expansion;
- right-to-left presentation where applicable;
- font glyph coverage;
- line wrapping;
- button size;
- gamepad focus;
- conversation response layout;
- in-world widget readability;
- title/description consistency.

## Accessibility Operational Gate

Before release:

- critical information has more than one compatible cue;
- color is not the only differentiator;
- text is readable and remains long enough;
- audio has visual/text backup;
- UI is operable by supported inputs;
- flashing and intense effects are reviewed;
- players can recover from missed instructions;
- no mechanic requires personal disclosure or public humiliation.

---

# Private Versions and Publishing

## Publishing Report

UEFN's current **Publishing Report** panel surfaces publish-readiness issues before the project reaches Creator Portal. It consolidates required checks into a project-level publishing checklist.

Use it before creating the release candidate to identify problems that can block upload or publication. Treat it as an additional evidence layer: a clean Publishing Report does not replace launched-session testing, memory calculation, Private Version testing, IARC, or moderation.

Recommended workflow:

1. open the current publishing/report workflow in UEFN;
2. inspect every required and warning item;
3. resolve validation, project configuration, memory, metadata-adjacent, and asset-health findings;
4. rerun the report after changes;
5. record the report state with the release candidate.

## Private Version Purpose

A Private Version is a cooked, uploaded version used for testing and as the basis for release creation. It is not a public published island.

Use it to verify:

- cooked asset state;
- multiplayer with invited testers;
- memory calculation;
- platform behavior;
- Join in Progress;
- metadata/release preparation;
- final regression before moderation.

## Create a Private Version

The exact button labels can change, but the documented flow begins in UEFN through the project publishing/private-version workflow.

1. save and sync the release candidate;
2. run validation;
3. run a clean launched-session smoke test;
4. calculate memory if required/current;
5. use `Publish Project` / Private Version workflow;
6. record the generated Private Version identifier/code;
7. test that exact version;
8. continue to Creator Portal to create a release.

## Private Version Test Gate

Test:

- full loop;
- minimum and maximum practical players;
- Join in Progress;
- respawn/reset;
- UI and Conversations;
- NPCs;
- cinematics and completion state;
- streaming;
- target platform/input;
- content and attribution visible as intended.

Do not submit a different Private Version than the one that passed the release-candidate matrix.

---

# Creator Portal, IARC, Moderation, and Release

## Creator Portal Release Workflow

1. open the project in Creator Portal;
2. choose the tested Private Version;
3. create a release;
4. enter accurate title, description, tags, and release notes;
5. upload compliant promotional media;
6. complete attribution and required disclosures;
7. complete the IARC questionnaire accurately;
8. configure visibility/timing options available to the account;
9. review automated checks;
10. submit for moderation;
11. respond to moderation findings by fixing the project or metadata;
12. after approval, publish or schedule as allowed;
13. verify the player-facing listing and island code.

## IARC

IARC ratings are generated from questionnaire responses. Answer based on actual content, not intended audience or educational purpose. Re-evaluate ratings when content changes materially.

## Metadata Accuracy

Metadata must match the experience. Avoid:

- promising unavailable mechanics;
- misleading player count or genre;
- unsupported claims;
- imagery not representative of gameplay;
- unlicensed brands/characters;
- text that conflicts with the actual age-rating content.

## Promotional Media

Verify current Epic media specifications. Capture media from a release-like build. Check:

- safe composition and readable subject;
- no hidden copyrighted material;
- no debug UI;
- no misleading post-processing;
- correct branding and text limits;
- consistency with title and description.

## Moderation Failure Workflow

1. read the exact reason;
2. distinguish project content from metadata/media failure;
3. locate the relevant current rule;
4. correct the source issue, not only the symptom;
5. generate/test a new Private Version if project content changed;
6. update release notes;
7. resubmit;
8. retain the moderation record for future releases.

Do not attempt to evade moderation by changing wording while retaining prohibited content.

## Visibility and Scheduling

Visibility options and scheduling availability can vary by current Creator Portal features and account/program status. Verify the live Portal. A successful approval does not automatically prove the release is publicly discoverable.

---

# Content Rules, IP, Attribution, and Safety

## Current Rules Gate

Before every submission, review the current:

- Fortnite Island Creator Rules;
- Creator Portal publishing documentation;
- intellectual-property and brand rules;
- promotional media requirements;
- privacy and personal-data rules;
- monetization/disclosure requirements;
- age-rating guidance;
- feature-specific restrictions.

Rules can change after a project begins.

## Third-Party Assets

For each imported/Fab asset, retain:

- source URL/marketplace record;
- license type;
- creator/publisher;
- acquisition date;
- proof of purchase/entitlement where relevant;
- attribution text if required;
- modifications made;
- responsible team member.

An asset being downloadable does not prove it can be published in every context.

## Brands and Fortnite IP

Use only content and personas permitted by current Epic documentation and license terms. Current LLM/persona releases include restrictions around some Fortnite IP voices/personas; verify the specific content available to the project.

## Privacy

Do not request or expose unnecessary personal information. Conversation/LLM systems, external links, user-generated text, and data collection require current policy review.

## Educational and Social Projects

Educational intent does not waive content, privacy, IP, age-rating, or moderation rules. Avoid requiring personal disclosure or simulating harassment/humiliation as entertainment.

---

# Release Maintenance and Regression

## Release Record

Store:

- public island code;
- Creator Portal project/release identifier;
- source revision;
- Private Version;
- publish date;
- ecosystem version;
- memory snapshot;
- target platform tests;
- known issues;
- moderation outcome;
- rollback candidate;
- analytics baseline.

## Post-Release Verification

After publishing:

1. launch the public version through the normal player path;
2. verify title, thumbnail, description, tags, and rating;
3. verify matchmaking/player count behavior;
4. complete the first objective;
5. test Join in Progress with another player;
6. monitor errors, feedback, and analytics;
7. compare to Private Version evidence.

## Ecosystem Update Regression

After major Fortnite ecosystem updates, prioritize:

- project opening and validation;
- Device option/event/function changes;
- UMG and input behavior;
- NPC/Conversation status;
- cinematics and animation;
- materials/lighting;
- memory calculation;
- Lore connectivity;
- Creator Portal and policy changes.

## Hotfix Rule

A hotfix still requires:

- source revision;
- focused regression;
- Private Version test;
- memory/validation review when affected;
- accurate release notes;
- rollback path.

---

# Complete Production Example: The Restored Archive

This section tests and prepares the complete non-Verse example defined in `09`.

## Project Summary

Players explore a museum/archive, collect a key, speak with an Archivist through an authored Conversation, choose whether to restore a sealed gallery, trigger a Level Sequence with audio/VFX, open a Barrier, and enter a final exhibit.

## Build Identity

Record:

- source revision;
- Private Version;
- UEFN release;
- player-count target;
- target platforms;
- memory snapshot;
- current known limitations.

## Smoke Test

1. launch a clean session;
2. start game;
3. spawn in intended lobby;
4. collect key;
5. receive correct HUD feedback;
6. initiate Conversation;
7. select restore branch;
8. verify Cinematic Sequence, audio, VFX, and Barrier;
9. enter final area;
10. verify game end/reset.

## Device Binding Audit

For each path, verify and record the exact documented names in the active project:

- Item Spawner `On Item Pick Up Send Event To` → HUD Message `Show`;
- Conversation Device `On Conversation Event One` → Cinematic Sequence `Play`;
- Conversation Device `On Conversation Event One` → Barrier `Disable`;
- Conversation Device `On Conversation Event One` → Audio Player `Play`;
- Conversation Device `On Conversation Event One` → VFX Spawner `Enable`;
- Trigger `On Triggered Send Event To` → End Game `Activate`.

Do not infer an Event or Function name from this architecture. Retrieve it from the named Device's current Details panel/documentation.

## Two-Player Test

1. player A collects the key;
2. player B remains at the sealed gallery;
3. player A speaks to the Archivist;
4. player A selects restore;
5. both observe intended feedback;
6. player B verifies Barrier opens and collision changes;
7. verify cinematic audience behavior;
8. verify either player can finish as designed.

## Join in Progress Test

1. player A completes restoration;
2. player B joins;
3. player B spawns in a valid location;
4. Barrier is already open;
5. obsolete key objective is not presented as mandatory;
6. player B can reach the final gallery;
7. game-end scope behaves as intended.

If the Device-only architecture cannot reconstruct a per-player objective state, use a global objective message or redesign the late-join route. Do not claim custom per-player narrative memory without Verse.

## Reset Test

Run two complete rounds/games. Verify:

- item respawns;
- Barrier returns to intended state;
- Conversation can be initiated;
- cinematic restores or keeps state as designed;
- VFX/audio stop;
- HUD layer clears;
- NPC respawns/remains available;
- final Trigger/End Game can activate again.

## Memory and Performance Test

- calculate memory on the current Private Version;
- inspect top contributors;
- profile the museum's densest view;
- trigger cinematic, VFX, audio, NPC, and UI together;
- profile two players in the same area;
- test fast entry into the final gallery;
- confirm HLOD/streaming settings do not remove collision or Devices.

## Publishing Preparation

- verify all imported museum assets and audio licenses;
- verify Conversation and Persona content complies with current rules;
- capture representative thumbnail/media;
- complete IARC from actual content;
- include accessibility notes in internal QA;
- submit the exact tested Private Version.

## Expected Release Evidence

- no blocking validation errors;
- memory calculation passes;
- solo, two-player, JIP, and reset tests pass or documented limitations accepted;
- target platform/UI test passes;
- Creator Portal metadata complete;
- rollback revision recorded.

---

# Production Checklists

## Daily Development

- [ ] Sync latest in Lore.
- [ ] Check ownership before editing.
- [ ] Save assets through the editor.
- [ ] Run one launched-session test of changed behavior.
- [ ] Record Event → Function bindings changed.
- [ ] Check in a coherent working change.
- [ ] Communicate unresolved blockers.

## Feature Complete

- [ ] Success, failure, retry, and reset paths exist.
- [ ] Solo and multiplayer scope are explicit.
- [ ] Join in Progress is defined.
- [ ] UI, audio, and visual feedback agree.
- [ ] Controller/input works.
- [ ] Imported assets have license evidence.
- [ ] Minimal reproduction exists for unstable behavior.

## Optimization

- [ ] Current cooked memory calculation recorded.
- [ ] Highest cells/areas reviewed.
- [ ] Top assets reviewed.
- [ ] Streaming and HLOD tested at gameplay speed.
- [ ] Spatial Profiler capture recorded.
- [ ] Worst-case NPC/VFX/UI/cinematic state profiled.
- [ ] Lowest target platform tested.
- [ ] Before/after evidence retained.

## Collaboration

- [ ] Ownership map current.
- [ ] No unintended checked-out assets.
- [ ] Changelists are small and described.
- [ ] Renames/moves communicated.
- [ ] Conflicts intentionally resolved.
- [ ] Recovery revision exists.
- [ ] Another contributor can sync and open.

## Release Candidate Gate

- [ ] Exact source revision recorded.
- [ ] Exact Private Version recorded.
- [ ] No blocking validation/cook/upload errors.
- [ ] Core loop passes.
- [ ] Multiplayer/JIP/respawn/reset passes.
- [ ] UI, Conversations, NPCs, cinematics, audio, and VFX pass.
- [ ] Current memory calculation passes.
- [ ] Runtime profile reviewed.
- [ ] Target platforms and inputs tested.
- [ ] Accessibility/localization reviewed.
- [ ] Licensing/attribution complete.
- [ ] Metadata, media, IARC, and rules reviewed.
- [ ] Rollback candidate available.

## Creator Portal Submission

- [ ] Tested Private Version selected.
- [ ] Title and description accurate.
- [ ] Tags accurate.
- [ ] Media compliant and representative.
- [ ] Attribution complete.
- [ ] IARC answers accurate.
- [ ] Privacy/disclosures reviewed.
- [ ] Visibility/schedule intentional.
- [ ] Release notes state meaningful changes.
- [ ] Moderation response owner assigned.

## Post-Release

- [ ] Public listing verified.
- [ ] Public island code tested.
- [ ] Matchmaking and JIP tested.
- [ ] Analytics baseline recorded.
- [ ] Feedback channel monitored.
- [ ] Known issues published internally.
- [ ] Ecosystem-update regression scheduled.

---

# Troubleshooting Decision Trees

## “My Latest Change Is Not in Fortnite”

1. Was the asset saved?
2. Is the correct session connected?
3. Does UEFN show pending changes?
4. Push Changes.
5. If still stale, Refresh Session.
6. If structural/cooked change, full relaunch.
7. If Private Version differs, create a new Private Version from the current revision.

## “Memory Did Not Change”

1. Was a current cooked/private version used?
2. Were changes pushed/cooked?
3. Did the optimization affect the highest cell?
4. Is the asset still referenced elsewhere?
5. Did HLOD generation add/retain assets?
6. Compare snapshots using the same configuration.

## “Works Solo, Fails Multiplayer”

1. Is state player, team, or global?
2. Does the triggering Event include the intended instigator?
3. Is the receiving Device global?
4. Can two players activate simultaneously?
5. What happens when one leaves/respawns?
6. Does Join in Progress receive current state?
7. Redesign to a clearly global Device state if per-player reconstruction is unavailable without Verse.

## “Works First Round Only”

1. Which Device/property retained state?
2. Did the cinematic use Keep State?
3. Did the Barrier/Lock re-enable?
4. Did the item respawn?
5. Did the NPC respawn?
6. Did HUD/UI clear?
7. Add explicit reset bindings or change lifecycle settings, then test two full cycles.

## “Private Version Fails but Live Edit Works”

1. Treat Private Version as authoritative.
2. run validation;
3. rebuild required generated assets/HLODs;
4. `Full Recook` or relaunch;
5. inspect unsupported references;
6. generate a new Private Version;
7. test again without relying on the old session.

## “Moderation Rejected the Release”

1. read the exact moderation reason;
2. map it to project, metadata, media, IP, rating, privacy, or rule issue;
3. fix the source issue;
4. create/test a new Private Version if content changed;
5. update metadata/IARC if necessary;
6. resubmit with accurate release notes.

---

# Official Source Registry

- [Playtesting Your Island](https://dev.epicgames.com/documentation/fortnite/playtesting-your-island-in-unreal-editor-for-fortnite)
- [UEFN Session Inspector](https://dev.epicgames.com/documentation/fortnite/uefn-session-inspector)
- [Live Edit and Iteration Improvements](https://dev.epicgames.com/documentation/fortnite/live-edit-and-iteration-improvements-in-fortnite)
- [Mobile Preview Session](https://dev.epicgames.com/documentation/fortnite/mobile-preview-session-in-unreal-editor-for-fortnite)
- [Validation and Fix-Up Tool](https://dev.epicgames.com/documentation/fortnite/validation-and-fixup-tool-in-unreal-editor-for-fortnite)
- [Memory Management](https://dev.epicgames.com/documentation/fortnite/memory-management-in-unreal-editor-for-fortnite)
- [Spatial Profiler](https://dev.epicgames.com/documentation/fortnite/spatial-profiler-in-unreal-editor-for-fortnite)
- [Memory Snapshot](https://dev.epicgames.com/documentation/fortnite/memory-snapshot-in-unreal-editor-for-fortnite)
- [Streaming and HLODs](https://dev.epicgames.com/documentation/fortnite/streaming-and-hlods-in-unreal-editor-for-fortnite)
- [Project Size Tool](https://dev.epicgames.com/documentation/fortnite/project-size-tool-in-unreal-editor-for-fortnite)
- [Collaborate and Publish](https://dev.epicgames.com/documentation/fortnite/collaborate-and-publish-in-unreal-editor-for-fortnite)
- [Lore Version Control](https://dev.epicgames.com/documentation/fortnite/lore-version-control-in-unreal-editor-for-fortnite)
- [Lore Version Control Best Practices](https://dev.epicgames.com/documentation/fortnite/lore-version-control-best-practices-in-unreal-editor-for-fortnite)
- [Conflicts in Lore Version Control](https://dev.epicgames.com/documentation/fortnite/conflicts-in-lore-version-control-in-unreal-editor-for-fortnite)
- [Branch Explorer](https://dev.epicgames.com/documentation/fortnite/branch-explorer-in-fortnite)
- [Publishing Projects](https://dev.epicgames.com/documentation/fortnite/publishing-projects-in-unreal-editor-for-fortnite)
- [Publishing from Creator Portal](https://dev.epicgames.com/documentation/fortnite/publishing-from-the-creator-portal-in-fortnite-creative)
- [IARC Overview and FAQs](https://dev.epicgames.com/documentation/fortnite/iarc-overview-and-faqs-in-fortnite-creative)
- [Fortnite Island Creator Rules](https://www.fortnite.com/news/fortnite-island-creator-rules)
- [v41.00 Release Notes](https://dev.epicgames.com/documentation/fortnite/41-00-fortnite-ecosystem-updates-and-release-notes)
- [v41.10 Release Notes](https://dev.epicgames.com/documentation/fortnite/41-10-fortnite-ecosystem-updates-and-release-notes)
- [v41.30 Release Notes](https://dev.epicgames.com/documentation/fortnite/41-30-fortnite-ecosystem-updates-and-release-notes)

## Related Documents

- [00_MASTER_KNOWLEDGE_INDEX.md](00_MASTER_KNOWLEDGE_INDEX.md)
- [01_EPIC_GAMES_DOCUMENTATION_INDEX.md](01_EPIC_GAMES_DOCUMENTATION_INDEX.md)
- [02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md](02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md)
- [08_UEFN_EDITOR_PROJECTS_ASSETS_AND_WORLD_BUILDING.md](08_UEFN_EDITOR_PROJECTS_ASSETS_AND_WORLD_BUILDING.md)
- [09_UEFN_GAMEPLAY_VERSE_API_UI_NPC_AND_CINEMATICS.md](09_UEFN_GAMEPLAY_VERSE_API_UI_NPC_AND_CINEMATICS.md)
- [16_GLOSSARY_UEFN.md](16_GLOSSARY_UEFN.md)
- [17_GLOSSARY_VERSE.md](17_GLOSSARY_VERSE.md)
