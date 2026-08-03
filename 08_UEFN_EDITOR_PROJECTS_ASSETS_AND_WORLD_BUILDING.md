---
document_id: "08"
corpus_role: "active_agent_knowledge"
authority: "verified_uefn_editor_and_world_authoring"
primary_environment: "UEFN without Verse"
last_verified: "2026-08-03"
verified_release: "Fortnite Ecosystem v41.30"
---

# UEFN: Editor, Projects, Assets, and World Building

## Document Metadata

| Field | Value |
|---|---|
| Document ID | `08` |
| Domain | UEFN editor, projects, assets, and world authoring |
| Primary Environment | UEFN without Verse |
| Language | English; exact UI labels remain in English |
| Authority | Current Epic Games documentation and release notes; professional workflow guidance is labeled as such |
| Last Verified | 2026-08-03 |
| Verified Release | Fortnite Ecosystem `v41.30`, released 2026-07-30 |
| Stability Status | Stable foundations plus version-sensitive UI, validation, Scene Graph, lighting, import, and publishing-adjacent behavior |

## Document Purpose

Provide implementation-ready knowledge for creating, organizing, importing, building, lighting, streaming, and troubleshooting a UEFN project without writing Verse.

This document owns the **editor and world-authoring layer**. Device gameplay, non-Verse UI, NPC, Conversation, animation, cinematics, audio, and VFX belong to `09`. Sessions, testing, optimization, collaboration, validation, and publishing belong to `10`.

## Scope Boundary

Included:

- UEFN installation and Project Browser;
- editor layout and navigation;
- project, level, Actor, asset, and folder organization;
- transforms, pivots, snapping, grouping, and attachment;
- Fortnite content, Fab, imported content, and licensing evidence;
- Static Mesh, Skeletal Mesh, texture, material, decal, collision, and LOD workflows;
- Modeling Mode;
- Landscape, foliage, water, modular construction, and environmental layout;
- lighting systems and platform-aware exposure;
- World Partition, Streaming, HLOD, Data Layers, and editor loading regions;
- Scene Graph environment authoring with a Beta gate;
- practical blockout, production, and troubleshooting procedures.

Excluded:

- Verse code or Verse API instruction;
- Blueprint gameplay scripting;
- Verse-authored Devices;
- Fortnite Creative Phone Tool workflows;
- gameplay implementation owned by `09`;
- production and publishing operations owned by `10`.

## Mandatory Environment Rule

Use this file only when the user is working in **UEFN**. Do not translate these procedures into Fortnite Creative instructions. UEFN uses the Viewport, Outliner, Details panel, Content Browser/Content Drawer, asset editors, Build tools, Message Log, Output Log, and launched Fortnite sessions.

## Authority and Evidence Labels

- **Documented:** Directly supported by current official Epic documentation.
- **Professional practice:** A production recommendation inferred from Unreal/UEFN workflows; it must not override current validation or product behavior.
- **Version-sensitive:** Confirm in the active UEFN release before relying on the exact label or availability.
- **Session-dependent:** Must be verified in a launched Fortnite session; the editor viewport is not sufficient evidence.
- **Beta / Experimental:** Follow the current Epic feature-status page and release notes before shipping.

## Quick Topic Index

- [Project Creation and Initial Decisions](#project-creation-and-initial-decisions)
- [Editor Interface and Navigation](#editor-interface-and-navigation)
- [Project Structure and Naming](#project-structure-and-naming)
- [Levels, Actors, Assets, Components, and Devices](#levels-actors-assets-components-and-devices)
- [Transform, Pivot, Snapping, Attachment, and Duplication](#transform-pivot-snapping-attachment-and-duplication)
- [Finding and Using Fortnite Content](#finding-and-using-fortnite-content)
- [Importing Custom Assets](#importing-custom-assets)
- [Static Mesh and Collision Workflow](#static-mesh-and-collision-workflow)
- [Skeletal Mesh and Animation Asset Gate](#skeletal-mesh-and-animation-asset-gate)
- [Textures, Materials, Material Instances, and Decals](#textures-materials-material-instances-and-decals)
- [Modeling Mode](#modeling-mode)
- [World Building and Modular Construction](#world-building-and-modular-construction)
- [Landscape](#landscape)
- [Foliage](#foliage)
- [Water and Traversal](#water-and-traversal)
- [Lighting and Environment](#lighting-and-environment)
- [World Partition and Streaming](#world-partition-and-streaming)
- [HLOD](#hlod)
- [Data Layers](#data-layers)
- [Scene Graph Environment Authoring](#scene-graph-environment-authoring)
- [Performance-Aware Authoring](#performance-aware-authoring)
- [Editor Troubleshooting](#editor-troubleshooting)
- [Minimal Editor Verification Tests](#minimal-editor-verification-tests)
- [Official Source Registry](#official-source-registry)

## Common Question Router

- Device gameplay, Direct Event Binding, UI, NPC, Conversation, Sequencer, audio, or VFX -> `09`.
- Launch Session, multiplayer tests, memory, profiling, Lore, validation, private versions, or publishing -> `10`.
- Definitions -> `16`.
- Current official page or release-sensitive claim -> `01`.
- Verse implementation -> `12`-`14` and the Verse sections of `09`; do not derive it from this file.

---

# Project Creation and Initial Decisions

## Installation and Launch

**Documented workflow:**

1. Install Fortnite and UEFN from the Epic Games Launcher.
2. Launch UEFN on a supported Windows PC.
3. Use the Project Browser to create a project from an available template or open an existing project.
4. Keep Fortnite installed because UEFN launches Fortnite for runtime playtests.
5. Sign in with the Epic account that owns or has team access to the project.

**Before creating the project, decide:**

| Decision | Why it matters |
|---|---|
| Maximum players | Affects layout, spawn capacity, Device state, UI, and test matrix |
| Teams or cooperative play | Affects Island Settings, navigation, objective scope, and late-join behavior |
| Target platforms | Affects texture, material, lighting, UI, memory, and input decisions |
| Compact or large world | Affects World Partition, Streaming, HLOD, and traversal tests |
| Imported content | Adds licensing, validation, project-size, and optimization work |
| Device-only or Verse | This document assumes Device-only; custom logic requiring Verse must be deferred |
| Publishing intent | Requires validation, memory calculation, metadata, content rules, and team ownership |

## Template Selection

Choose a template because it provides an appropriate starting configuration, not because its visual theme is attractive.

**Professional practice:**

- Start with the smallest template that supports the intended game.
- Avoid inheriting systems you do not understand.
- Inspect Island Settings, placed Devices, World Settings, Data Layers, and project folders before editing.
- Record which template and release produced the original project.

## New Project Gate

Before production begins:

- open the project;
- save the main level;
- launch one empty or near-empty session;
- confirm the Fortnite client connects;
- confirm the team can access the project;
- create the initial Lore checkpoint if collaboration is enabled;
- document the project ID, owner, target player count, and lowest target platform.

---

# Editor Interface and Navigation

## Core Surfaces

| Surface | Primary use | Common mistake |
|---|---|---|
| Viewport | Spatial placement and visual inspection | Treating editor appearance as proof of runtime behavior |
| Outliner | Finding and organizing placed Actors and Devices | Leaving default names and losing system traceability |
| Details panel | Editing the selected object's properties | Editing the wrong selected instance |
| Content Browser / Content Drawer | Creating, importing, organizing, and opening assets | Mixing source assets with gameplay-ready assets |
| Toolbar | Save, launch, session, build, and project operations | Assuming every change is live in the current session |
| Place Actors | Placing supported Actor types and tools | Using UE types that UEFN does not validate |
| Message Log | Validation, memory, and actionable asset messages | Ignoring the first causal error |
| Output Log | Runtime/editor diagnostics | Treating warnings as harmless without checking impact |
| World Settings | Streaming, World Partition, lighting, and level-level behavior | Changing project-wide systems without a recovery checkpoint |
| World Partition window | Loading/unloading editor regions | Believing editor-unloaded regions are removed from the project |

## Selecting the Correct Object

Before changing a property:

1. select the object in the Outliner when possible;
2. confirm its name and type at the top of Details;
3. confirm the transform matches the visible object;
4. check whether the selected item is an Actor, component, Device, entity, or asset;
5. verify whether the field is inherited, overridden, disabled, or context-filtered.

## Viewport Modes

Use editor visualization modes for diagnosis, but validate the final result in Fortnite.

Typical uses include:

- Lit view for final composition;
- Unlit or material-oriented modes for diagnosing lighting/material problems;
- collision visualization for geometry checks;
- game view for a cleaner approximation of player-facing visuals;
- World Partition grid/minimap for spatial loading work.

Exact menu labels can change. Route the user to the current Viewport toolbar rather than relying on a shortcut alone.

## Search Discipline

Use:

- Outliner search for placed objects;
- Content Browser filters for asset types;
- folder paths for ownership;
- meaningful prefixes or suffixes for systems;
- source-control status overlays to identify checked-out assets;
- Event Browser or Device references in `09` for gameplay relationships.

---

# Project Structure and Naming

## Recommended Folder Structure

The exact folder names are a professional convention, not an Epic requirement.

```text
ProjectContent/
  Maps/
    Main/
    Tests/
  Art/
    Meshes/
    Materials/
    Textures/
    Decals/
    Foliage/
  Audio/
  VFX/
  Animation/
  Cinematics/
  UI/
  NPC/
    CharacterDefinitions/
    ConversationBanks/
  Gameplay/
    Devices/
    Objectives/
  Data/
  Imported/
    SourceName/
  Documentation/
```

Do not create or import assets into Epic or Fortnite content folders. Imported and creator-authored assets belong under the project's own content folder.

## Naming Rules

Use names that reveal type, role, and scope.

Examples:

- `SM_Museum_Wall_A`
- `MI_Museum_Marble_Dark`
- `T_UI_KeyIcon`
- `LS_ArchiveRestored`
- `WBP_ObjectivePopup`
- `NPCD_Archivist`
- `CB_ArchivistIntro`
- `DEV_ArchiveDoor_Barrier`
- `TEST_NavmeshRoom`

**Professional practice:**

- Keep stable names once an asset is widely referenced.
- Use UEFN's move/rename operation inside the Content Browser.
- Fix redirectors or reference issues through supported editor workflows when exposed.
- Do not rename files outside UEFN.
- Avoid special characters in project and folder names when external pipelines are involved.

## One Owner Per Asset

For team work, record:

- feature owner;
- level/area owner;
- asset owner;
- expected submit window;
- dependent systems;
- test status.

This reduces Lore conflicts and accidental overwrites.

---

# Levels, Actors, Assets, Components, and Devices

## Definitions in Context

- A **project** contains levels, assets, settings, and collaboration history.
- A **level** stores placed world content.
- An **Actor** is a placeable object in the level.
- An **asset** is reusable project content stored in the Content Browser.
- A **component** adds data or functionality to an object.
- A **Device** is a Fortnite gameplay object configured through User Options and Direct Event Binding.
- A **Scene Graph entity** is part of the Beta Scene Graph system and must not be conflated with a conventional Actor.

## Reference Consequences

Deleting a placed Actor does not necessarily remove its asset from the project or cooked dependencies. Moving or renaming an asset can affect every Actor, Device, widget, sequence, material, or definition that references it.

Before deletion:

1. search for references;
2. identify whether the asset is used in another level, sequence, widget, NPC definition, or prefab;
3. create a source-control checkpoint;
4. delete only after a launched-session and validation check.

## Global Managers and Spatial Content

Keep global or always-relevant systems separate from room-specific content.

Examples:

- Island Settings;
- game-state Devices;
- global audio;
- global HUD control;
- end-game Devices;
- main cinematics;
- test-only debug Devices.

Spatial content should be organized by room, zone, or feature so it can be loaded, tested, and assigned independently.

---

# Transform, Pivot, Snapping, Attachment, and Duplication

## Transform Workflow

1. select the Actor;
2. choose translate, rotate, or scale;
3. confirm world or local coordinate space;
4. set snapping appropriate to the asset;
5. place the object;
6. inspect from player height;
7. launch a session to test collision and traversal.

## Scale and Units

UEFN follows Unreal-style centimeter units. Do not assume a model imported from another application has the correct scale.

Check:

- door height;
- stair step height;
- corridor width;
- player clearance;
- interaction reach;
- camera clipping;
- NPC navigation width;
- prop collision.

## Non-Uniform Scale

Use non-uniform scaling carefully.

Potential problems:

- distorted collision;
- stretched textures;
- incorrect normals;
- lighting artifacts;
- unexpected bounds;
- inconsistent HLOD generation.

Prefer authoring the correct mesh dimensions or using Modeling Mode for permanent geometry changes when practical.

## Pivots

A bad pivot makes modular placement and animation unreliable.

Check:

- rotation center;
- floor alignment;
- hinge position for doors;
- snap edge for modular pieces;
- sequence animation origin;
- duplicated instance alignment.

## Attachment

Parent-child attachment can simplify movement, but it also creates transform and HLOD consequences.

Before attaching:

- confirm the child should inherit translation, rotation, and scale;
- avoid attaching unrelated distant Actors;
- inspect HLOD grouping if distant objects are unexpectedly combined;
- detach with `Attach: none` when troubleshooting inherited behavior.

## Duplication and Reuse

Duplicating a placed instance is usually cheaper and safer than importing many unique variants. Direct Event Binding duplication behavior is covered in `09`; copied Device groups create unique bindings according to current Epic documentation, but the copied system must still be retested.

---

# Finding and Using Fortnite Content

## Fortnite Content Browser

Use the Fortnite folder and its categories to find supported Devices, props, galleries, environments, lighting tools, and other content.

Do not modify Epic-owned source assets directly. Create project-owned assets or instances where supported.

## Fab

Fab is a source of third-party and Epic content, not an automatic approval gate.

For every acquired asset, record:

- asset title and creator;
- license;
- acquisition date;
- original URL or listing identifier;
- attribution requirement;
- whether redistribution or commercial use is allowed;
- project(s) using it.

Then evaluate:

- validation compatibility;
- geometry and texture cost;
- material complexity;
- collision;
- style fit;
- content rules;
- target-platform behavior.

## Brand and IP Content

Brand-specific templates and assets can have project and publishing restrictions. Do not assume an asset can be moved into another project or used outside its permitted IP environment. Route current brand rules to `10` and the official source.

---

# Importing Custom Assets

## Supported Import Families

Current official documentation lists supported types including:

- 3D: FBX, OBJ, glTF, GLB;
- textures: JPG, PNG, JPEG, BMP, DDS, EXR, HDR, PCX, PSD, TGA, TIF, TIFF;
- audio: AIF, FLAC, OGG, WAV;
- Epic/project packages: `.island`, `.upack`;
- data: CSV, JSON.

Availability and restrictions can change. Confirm the current Importing Assets page when a format fails.

## Where to Import

Import into a project-owned folder under **Project Content**. UEFN does not allow creators to import into Epic or Fortnite folders.

## Import Methods

Officially documented methods:

1. drag a file from Windows Explorer into the Content Browser;
2. right-click in the target project folder and choose the import command;
3. use the Content Browser Import control.

FBX opens FBX Import Options. Other supported formats use the Interchange Asset Import framework.

## Pre-Import Checklist

| Area | Check |
|---|---|
| License | Permission and attribution retained |
| Units | Real-world scale confirmed |
| Axes | Orientation confirmed |
| Pivot | Intended placement/animation pivot |
| Geometry | No open surfaces, duplicate faces, or accidental internal geometry |
| UVs | Texture UV and lightmap/secondary UV requirements understood |
| Materials | Material slot count minimized |
| Textures | Dimensions, channels, alpha, and compression planned |
| Collision | Simple collision plan defined |
| LOD | Lower-fidelity meshes available or generation plan documented |
| Animation | Skeleton, bones, naming, and clips verified |
| Target platforms | Lowest platform budget considered |

## Import Execution

1. create the destination folder;
2. import the file;
3. review the import options;
4. decide whether to combine meshes;
5. decide whether to import materials and textures;
6. set collision-related options only when understood;
7. import;
8. read Message Log warnings;
9. open the imported asset;
10. inspect scale, bounds, normals, UVs, materials, collision, LODs, and validation.

## Import Errors

Do not dismiss an import warning as harmless.

Classify it:

- source file invalid;
- unsupported feature;
- missing material or texture;
- scale or transform issue;
- skeletal hierarchy issue;
- collision issue;
- texture dimension warning;
- validation restriction.

Fix the source asset when the error originates in the source. Reimport rather than applying repeated manual corrections to every placed instance.

## Reimport Discipline

When updating a source file:

- keep the same source location and naming when practical;
- checkpoint before reimport;
- reimport one asset first;
- inspect material slots, collision, sockets, animation, and references;
- launch a session;
- submit the updated asset and dependent changes together.

---

# Static Mesh and Collision Workflow

## Static Mesh Inspection

Open the Static Mesh asset and inspect:

- bounds;
- pivot;
- vertex and triangle complexity;
- normals and tangents;
- UV channels;
- material slots;
- LODs;
- collision;
- distance appearance;
- shadow behavior.

## Collision Choice

Use the simplest collision that supports gameplay.

Prefer:

- boxes, capsules, or convex hulls for simple props;
- simplified custom collision for architecture;
- no collision for purely decorative inaccessible objects;
- per-poly/complex collision only when justified and tested.

**Session-dependent rule:** Editor collision visualization is not proof of Fortnite runtime interaction. Test walking, jumping, mantling, item pickup, camera movement, vehicles where applicable, and NPC navigation.

## Collision Test Cases

- player approaches from every side;
- player jumps onto and off the mesh;
- player attempts to pass through thin edges;
- camera does not clip into the object;
- objective/item interaction is reachable;
- NPC path is not blocked unexpectedly;
- physics-enabled props do not become trapped.

## LOD

Lower LODs reduce distant geometry cost and influence HLOD generation. HLODs are generated from the asset's lowest-fidelity LOD according to current Epic documentation. A custom mesh without appropriate LODs can generate expensive HLOD assets.

---

# Skeletal Mesh and Animation Asset Gate

Use Skeletal Meshes only when deformation or bone-driven animation is needed.

Check:

- supported skeleton;
- bone hierarchy;
- skin weights;
- animation compatibility;
- material slots;
- LODs;
- bounds;
- collision/physics requirements;
- validation;
- runtime animation owner.

NPC and Sequencer workflows belong to `09`.

A successfully imported Skeletal Mesh is not automatically a valid NPC cosmetic or a publishable animation asset. Verify the required NPC Character Definition modifiers, retargeting support, and current validation rules.

---

# Textures, Materials, Material Instances, and Decals

## Texture Workflow

1. import into `Textures`;
2. confirm intended use: color, normal, mask, UI, decal, or HDR;
3. inspect dimensions and mip generation;
4. use power-of-two dimensions where streaming and mipmaps are required;
5. select appropriate compression and color-space behavior;
6. check alpha use;
7. test at target scalability and distance.

Non-streaming textures can remain at maximum memory cost. Texture warnings should be resolved through the Message Log or the current texture-fix workflow.

## Materials

Materials define surface rendering. Keep materials readable and performant.

Avoid unnecessary:

- translucency;
- layered material complexity;
- animated calculations;
- high-frequency noise;
- expensive world-position effects;
- many unique materials that increase shader and draw-call pressure.

## Material Instances

Use Material Instances for controlled variation without duplicating an entire material graph.

Recommended parameter groups:

- base color;
- roughness;
- metallic;
- normal intensity;
- emissive strength;
- texture selection where supported;
- tint or team color;
- tiling.

## UI Materials

UI materials belong to UMG and Conversation presentation workflows in `09`. Do not assume a world material works correctly in UI.

## Decals

Use the UEFN Decal device or supported decal materials for markings, clues, directions, and environmental storytelling.

Check:

- projection direction;
- affected surfaces;
- opacity;
- distance;
- overlapping decals;
- mobile and low-end appearance;
- overdraw;
- collision-independent placement.

Critical guidance should not rely on a decal alone.

---

# Modeling Mode

## Appropriate Uses

- blockout geometry;
- simple custom meshes;
- cutting or joining simple forms;
- fixing pivots;
- reducing geometry;
- creating collision helpers;
- converting controlled dynamic geometry to Static Mesh assets;
- quick prototyping before external modeling.

## Destructive Change Rule

Before a destructive modeling operation:

1. duplicate the source asset;
2. rename the working copy;
3. checkpoint in Lore;
4. perform the operation;
5. inspect UVs, normals, materials, collision, LODs, and bounds;
6. launch a session;
7. replace instances gradually.

Do not use Modeling Mode as a substitute for a clean external source pipeline when an asset will be repeatedly revised.

---

# World Building and Modular Construction

## Blockout First

A blockout must prove:

- start and finish;
- main route;
- optional route;
- recovery route;
- player scale;
- interaction distances;
- sightlines;
- objective visibility;
- multiplayer circulation;
- NPC navigation;
- camera/cinematic space.

Use simple geometry and repeated modules before decoration.

## Modular Kit Rules

A modular set should have:

- consistent dimensions;
- compatible pivots;
- predictable snap points;
- limited material set;
- interior and exterior variants;
- corner and transition pieces;
- collision appropriate to gameplay;
- LODs;
- naming that preserves adjacency.

## Spatial Readability

Use:

- landmarks;
- framing;
- contrast;
- light;
- path width;
- repeated symbols;
- vertical hierarchy;
- environmental response.

Avoid relying on text alone. Test with a player who has not seen the editor.

## Multiplayer Scale

Check:

- two players can pass each other;
- four or more players can gather at an objective;
- spawn areas do not overlap;
- a cutscene camera is not blocked by other players;
- one player cannot physically prevent all progress unless intended;
- late joiners can reach the active area.

---

# Landscape

## When to Use Landscape

Use Landscape for large editable terrain. Use Static Meshes for authored structures, modular architecture, cliffs requiring exact silhouettes, and reusable set pieces.

## Landscape Workflow

1. define world size and playable boundary;
2. create or select the Landscape;
3. establish a scale reference;
4. sculpt large forms first;
5. add paths and playable slopes;
6. paint materials;
7. add collision-sensitive details;
8. place foliage after route validation;
9. test streaming and memory;
10. test traversal in a launched session.

## Landscape Risks

- terrain too steep for players or NPCs;
- thin spikes and pits causing collision traps;
- excessive material layers;
- large components increasing edit and runtime cost;
- holes exposing invalid spaces;
- distant landscape proxies always loaded;
- grass or foliage obscuring objectives.

## Landscape Test

Walk the fastest route, the edge boundary, every intended slope, and every transition between Landscape and mesh geometry. Test respawn and teleport destinations on stable surfaces.

---

# Foliage

## Use

Foliage efficiently distributes repeated vegetation and similar meshes, but density, collision, shadowing, material complexity, and draw distance can still create major cost.

## Workflow

1. choose a small reusable set of foliage meshes;
2. verify collision policy;
3. set scale and rotation variation;
4. paint a limited test region;
5. inspect from player height;
6. profile draw calls, primitives, memory, and frame time;
7. expand only after evidence is acceptable.

## Common Mistakes

- collision on every grass or small plant;
- too many unique meshes;
- full-resolution textures;
- dynamic shadows on dense foliage;
- foliage hiding navigation markers;
- painting before Landscape routes are final;
- generating unnecessary HLODs for tiny foliage.

---

# Water and Traversal

Water is both a visual system and a gameplay surface.

Check:

- entry and exit points;
- swimming behavior;
- depth;
- collision;
- camera behavior;
- NPC interaction;
- vehicle behavior if applicable;
- audio and VFX;
- streaming seams;
- recovery from falling in.

Do not use water as a boundary without testing how players can escape, mantle, swim, teleport, or respawn.

---

# Lighting and Environment

## Lighting Strategy

Choose one intentional model:

- default Time of Day system;
- Day Sequence device control;
- fully manual lighting with Time of Day Managers disabled;
- Environment Light Rig;
- platform-aware exposure through Lumen Exposure Manager.

Do not stack multiple global lighting systems without understanding which one owns sun, sky, fog, exposure, and reflections.

## Lumen and Non-Lumen

UEFN uses Lumen and Virtual Shadow Maps on supported high-end configurations, but lower-end platforms can render differently. Current Epic documentation provides the Lumen Exposure Manager with separate `LumenPostProcess` and `NonLumenPostProcess` control when all Time of Day Managers are disabled.

## Lighting Workflow

1. set gameplay readability target;
2. choose global lighting owner;
3. set sun/sky/time;
4. set exposure;
5. add local lights only where needed;
6. check shadow cost;
7. test dark adaptation and bright transitions;
8. test Lumen and non-Lumen appearance;
9. test color-blind-safe guidance;
10. launch on target platforms.

## Lighting Scalability

Use the current Lighting Scalability Manager where appropriate to show or hide lights and post-process volumes based on scalability. Treat exact options as version-sensitive and verify the official page.

## Common Lighting Failures

| Symptom | Likely cause | First check |
|---|---|---|
| Scene too dark on low-end | Non-Lumen exposure mismatch | Lumen Exposure Manager / target preview |
| Flickering shadows | overlapping lights or distance behavior | shadow settings and light count |
| Objective invisible | contrast and exposure | player-height launched session |
| Emissive blown out | material emissive and exposure | material instance and post process |
| Different session look | stale pushed changes or system ownership | push/refresh session and global lighting owner |

---

# World Partition and Streaming

## Core Model

World Partition divides the world into cells. Streaming loads cells around the player or another streaming source and unloads distant cells. It does not automatically make every referenced object spatially streamable.

## Enable and Inspect

Current official path:

1. open `Window > World Settings`;
2. inspect `Enable Streaming`;
3. confirm when first enabling;
4. optionally enable `Preview Grids`;
5. inspect cell size and loading range only when there is a measured reason to change defaults.

Epic currently prompts creators to enable Streaming when an island exceeds approximately 1 km on its widest axis, including cases where an Actor is more than 1 km from the level origin.

## Is Spatially Loaded

An Actor with `Is Spatially Loaded` disabled can remain in the main level package and stay loaded. Use this only for objects that genuinely need to remain available.

Examples that may require always-loaded behavior:

- global visual landmark;
- global Device or manager;
- large background object excluded from HLOD;
- system referenced across the island.

Measure the cost rather than assuming.

## Editor Loading Regions

For large worlds, editor loading regions improve editor responsiveness without deleting content.

Official workflow includes:

1. enable Streaming;
2. open Editor Preferences;
3. adjust the World Partition editor-loading setting;
4. reload the project;
5. select regions in the World Partition window;
6. unload or reload selected regions.

Landscape, Devices, and Actors with `Is Spatially Loaded` disabled are not affected in the same way.

## Streaming Test

Run at the fastest practical player speed through:

- start area;
- dense hub;
- long corridor;
- teleporter destination;
- elevated vista;
- underground area;
- return route.

Observe late textures, missing collision, disappearing meshes, audio activation, NPC spawn timing, and HLOD swaps.

---

# HLOD

## HLOD Purpose

HLOD groups nearby Actors into simplified distant representations. It can preserve distant silhouettes while source Actors stream out.

## Cost Model

- Streaming off: all assets stay loaded; HLODs are not used.
- Streaming on with HLODs: distant representations remain visible; HLOD assets add project/memory cost.
- Streaming on without HLODs: distant content may disappear; memory can be lower.

HLOD is not a universal optimization switch.

## Build Workflow

1. enable Streaming;
2. prepare asset LODs;
3. exclude Actors that should not generate HLODs;
4. use `Build > Build HLODs`;
5. inspect generated results;
6. launch a traversal session;
7. run memory and runtime profiling in `10`.

Subsequent builds can be incremental for changed regions.

## Exclusion Candidates

Current Epic guidance identifies likely exclusions such as:

- indoor props;
- underground content never visible from afar;
- small screen-space Actors;
- certain very large background Actors kept always loaded.

Use `Include Actor in HLODs` or `Include Component in HLODs` as appropriate.

## HLOD Troubleshooting

- HLOD not visible in editor -> search Outliner and pin HLOD Actors.
- unrelated distant objects grouped -> inspect attachment hierarchy.
- important object disappears -> inspect HLOD inclusion and spatial loading.
- HLOD expensive -> improve source LODs and exclude irrelevant objects.
- large prefab always loaded -> inspect far-away child Actors in its hierarchy.

---

# Data Layers

## Data Layer Purpose

Data Layers organize Actors into logical sets that can support editing, variants, and runtime state changes through supported systems.

Examples:

- summer/winter set dressing;
- intact/damaged environment;
- open/closed route variants;
- tutorial/advanced layout;
- cinematic reveal.

Data Layers do not automatically remove all referenced memory or guarantee a clean gameplay reset.

## Sequencer Control

Current official documentation supports Data Layer tracks in Sequencer:

1. add a Data Layer track;
2. add the target Data Layer asset;
3. choose desired state;
4. configure preroll and postroll states;
5. provide enough preroll frames for loading;
6. preview with the playhead;
7. play through a Cinematic Sequence Device.

Test with multiple players and late joiners because a global Data Layer transition may not match an individual player's cinematic timing.

---

# Scene Graph Environment Authoring

## Current Status

**Beta as of v41.30.** Epic explicitly says to use caution when shipping with Scene Graph.

Scene Graph uses an entity-and-component model and can integrate Verse components, but environment composition does not inherently require custom Verse code. This task permits non-code authoring with entities, components, and prefabs while excluding custom Verse components and Scene Graph gameplay code.

## Appropriate Non-Verse Uses

- hierarchical environment composition;
- reusable entity prefabs;
- prefab instances;
- transform inheritance;
- in-world UMG entity placement where supported;
- One File Per Entity collaboration benefits;
- prefab editing and controlled overrides.

## Production Gate

Use Scene Graph as a production foundation only when:

- the current feature status permits publishing;
- known issues are reviewed;
- the team understands prefab/entity recovery;
- a Lore checkpoint exists;
- the project has a conventional fallback for critical content;
- save/reopen, duplication, overrides, validation, session, and publishing tests pass.

## One File Per Entity

One File Per Entity can reduce collaboration contention by storing entities separately, but it does not eliminate conflicts in shared assets, prefab classes, levels, or dependencies.

## Prefab Test

For every entity prefab:

1. create;
2. save;
3. place two instances;
4. override one instance;
5. reopen the project;
6. sync on a second workstation if collaborating;
7. launch a session;
8. validate;
9. duplicate;
10. confirm no overrides or child entities disappear.

---

# Performance-Aware Authoring

Performance work begins during world construction.

## Authoring Budget Categories

- unique meshes;
- total placed Actor count;
- material slots;
- unique materials and shaders;
- texture dimensions and streaming;
- lights and shadows;
- foliage density;
- collision complexity;
- audio emitters;
- VFX emitters;
- Devices;
- always-loaded references;
- HLOD generated assets;
- UI textures and materials.

## Reuse Strategy

Reuse:

- mesh assets;
- Material Instances;
- texture atlases where appropriate;
- modular sets;
- repeated foliage variations;
- Device patterns;
- tested prefabs.

Do not create unique assets solely to avoid visible repetition when a small controlled variation set is sufficient.

## Build Stages

1. spatial blockout;
2. traversal and collision;
3. Device gameplay;
4. multiplayer lifecycle;
5. lighting and audio;
6. art pass;
7. streaming and HLOD;
8. memory and runtime optimization;
9. release candidate.

Do not polish a route that has not passed a blind playtest.

---

# Editor Troubleshooting

## Universal Order

1. confirm environment and project;
2. confirm selected object;
3. check save state;
4. check Outliner visibility and folder;
5. check transform and parent;
6. check Data Layer and World Partition loading;
7. check asset references;
8. check collision/material/LOD;
9. read Message Log and Output Log;
10. push or refresh the session;
11. launch the smallest reproduction;
12. validate the project.

## Object Missing in Viewport

Check:

- hidden state;
- Outliner eye state;
- Data Layer;
- unloaded region;
- transform far from origin;
- tiny or extreme scale;
- parent transform;
- material opacity;
- asset failed to load;
- Game View;
- editor-only state.

## Object Visible but Missing in Session

Check:

- changes pushed;
- current client connected to the expected session;
- Actor is allowed/validated;
- Data Layer runtime state;
- streaming/loading range;
- `Is Spatially Loaded`;
- visibility or enabled phase;
- session started before the asset existed;
- stale cook.

## Collision Wrong

Check:

- collision asset;
- complex versus simple;
- scale;
- blocking profile;
- hidden duplicate mesh;
- Landscape seam;
- session using latest changes;
- player versus NPC/physics behavior separately.

## Material Wrong

Check:

- correct material slot;
- instance parent;
- texture references;
- color space/compression;
- opacity blend;
- two-sided requirement;
- scalability;
- lighting/exposure;
- validation warning;
- stale session.

## Import Fails

Check source integrity, supported type, path, filename, size, FBX/Interchange options, skeleton, material count, texture format, and Message Log. Re-export a minimal source asset to separate UEFN restrictions from source-file corruption.

---

# Minimal Editor Verification Tests

## Imported Static Mesh

**Setup:** One imported mesh with one material and simple collision.

**Test:**

1. place it at player scale;
2. launch a session;
3. walk around and on it;
4. verify material and shadows;
5. run validation.

**Expected:** Correct scale, no collision traps, correct material, no blocking validation error.

## Lighting

**Setup:** One test room, objective prop, dark corner, bright exterior.

**Test:**

1. inspect high-end preview;
2. inspect non-Lumen/low-end path;
3. launch a session;
4. approach from dark to bright and bright to dark.

**Expected:** Objective remains readable, exposure transition is not disorienting, critical route is visible.

## Streaming

**Setup:** Two distant zones with a fast traversal path.

**Test:**

1. enable Streaming;
2. build HLODs if needed;
3. launch;
4. travel quickly in both directions;
5. repeat with a late-joining player.

**Expected:** No missing collision, acceptable visual swaps, objectives and Devices available when needed.

## Scene Graph Prefab

**Setup:** One Beta entity prefab with nested child entities.

**Test:** save, duplicate, override, reopen, sync, validate, launch.

**Expected:** hierarchy and overrides remain intact; no blocking validation or missing child entity.

---

# Official Source Registry

- [Unreal Editor for Fortnite](https://dev.epicgames.com/documentation/fortnite/unreal-editor-for-fortnite)
- [User Interface Reference for UEFN](https://dev.epicgames.com/documentation/fortnite/user-interface-reference-for-unreal-editor-for-fortnite)
- [UEFN versus Unreal Engine](https://dev.epicgames.com/documentation/fortnite/uefn-vs-ue-in-unreal-editor-for-fortnite)
- [Importing Assets](https://dev.epicgames.com/documentation/fortnite/importing-assets-in-unreal-editor-for-fortnite)
- [Materials](https://dev.epicgames.com/documentation/fortnite/materials-in-unreal-editor-for-fortnite)
- [Landscape](https://dev.epicgames.com/documentation/fortnite/landscape-in-unreal-editor-for-fortnite)
- [Lighting](https://dev.epicgames.com/documentation/fortnite/lighting-in-unreal-editor-for-fortnite)
- [Lumen Exposure Manager](https://dev.epicgames.com/documentation/fortnite/using-the-lumen-exposure-manager-in-unreal-editor-for-fortnite)
- [Streaming and HLODs](https://dev.epicgames.com/documentation/fortnite/streaming-and-hlods-in-unreal-editor-for-fortnite)
- [Scene Graph](https://dev.epicgames.com/documentation/fortnite/scene-graph-in-unreal-editor-for-fortnite)
- [Validation and Fix-Up Tool](https://dev.epicgames.com/documentation/fortnite/validation-and-fixup-tool-in-unreal-editor-for-fortnite)
- [v41.30 Release Notes](https://dev.epicgames.com/documentation/fortnite/41-30-fortnite-ecosystem-updates-and-release-notes)

## Related Documents

- [00_MASTER_KNOWLEDGE_INDEX.md](00_MASTER_KNOWLEDGE_INDEX.md)
- [01_EPIC_GAMES_DOCUMENTATION_INDEX.md](01_EPIC_GAMES_DOCUMENTATION_INDEX.md)
- [02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md](02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md)
- [09_UEFN_GAMEPLAY_VERSE_API_UI_NPC_AND_CINEMATICS.md](09_UEFN_GAMEPLAY_VERSE_API_UI_NPC_AND_CINEMATICS.md)
- [10_UEFN_TESTING_OPTIMIZATION_COLLABORATION_AND_PUBLISHING.md](10_UEFN_TESTING_OPTIMIZATION_COLLABORATION_AND_PUBLISHING.md)
- [16_GLOSSARY_UEFN.md](16_GLOSSARY_UEFN.md)
