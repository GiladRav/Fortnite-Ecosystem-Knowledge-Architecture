---
document_id: "16"
corpus_role: "active_agent_knowledge"
authority: "verified_uefn_glossary"
primary_environment: "UEFN"
last_verified: "2026-08-03"
verified_release: "Fortnite Ecosystem v41.30"
---

# Glossary: UEFN

## Document Metadata

| Field | Value |
|---|---|
| Document ID | `16` |
| Domain | Detailed UEFN terminology |
| Primary Environment | UEFN |
| Language | English definitions with canonical Hebrew names |
| Authority | Current Epic terminology and documentation; exact UI fields, Device Events, Functions, limits, and publishing rules remain in `08`-`10` and official owner pages |
| Last Verified | 2026-08-03 |
| Verified Release | Fortnite Ecosystem `v41.30`, released 2026-07-30 |
| Stability Status | Definitions are generally stable; feature status, UI labels, Devices, limits, and publishing requirements are version-sensitive |

## Purpose and Authority Boundary

Use this glossary to identify a UEFN term, understand its professional meaning, and preserve searchable English labels. Do not use it as authority for exact Device options, Events, Functions, API signatures, memory limits, UI paths, or publishing eligibility.

Implementation routing:

- editor, projects, assets, world building, lighting, streaming, HLOD, and Scene Graph → `08`;
- Devices, Direct Event Binding, non-Verse UI, NPCs, Conversations, cinematics, audio, and VFX → `09`;
- sessions, testing, validation, optimization, Lore, Private Versions, and publishing → `10`;
- Verse language/API terminology → `17` and `12`-`14`.

## Use Rules

1. Keep official English UI labels, Device names, Events, Functions, asset classes, and product names in English.
2. Never transfer an Event, Function, option, or limit from one Device/system to another.
3. Treat Beta, Experimental, publishing, policy, and release-specific terms as version-sensitive.
4. When Epic does not document an edge case, label it undocumented and require a controlled launched-session test.
5. `Lore Version Control` is the current product name; `Unreal Revision Control` and some uses of `Revision Control` are legacy/contextual labels.
6. Scene Graph remains a Beta-gated production choice in the verified release.
7. Authored Conversations are no longer Experimental as of v41.30, but individual Conversation, LLM, Persona, UI, and publishing capabilities remain version-sensitive.

## A-Z Index

- [Actor](#actor)
- [Asset](#asset)
- [Asset Dependency](#asset-dependency)
- [Audio Mixer Device](#audio-mixer-device)
- [Audio Player Device](#audio-player-device)
- [Auto Checkout](#auto-checkout)
- [Barrier Device](#barrier-device)
- [Beta](#beta)
- [Branch Explorer](#branch-explorer)
- [Build HLODs](#build-hlods)
- [Changelist](#changelist)
- [Character Device](#character-device)
- [Check In](#check-in)
- [Checkout](#checkout)
- [Cinematic Sequence Device](#cinematic-sequence-device)
- [Collision](#collision)
- [Component](#component)
- [Content Browser](#content-browser)
- [Content Drawer](#content-drawer)
- [Control Rig](#control-rig)
- [Conversation Bank](#conversation-bank)
- [Conversation Device](#conversation-device)
- [Conversation Editor](#conversation-editor)
- [Conversation Entry Point](#conversation-entry-point)
- [Conversation Event](#conversation-event)
- [Cook](#cook)
- [Creator Portal](#creator-portal)
- [Data Layer](#data-layer)
- [Day Sequence Device](#day-sequence-device)
- [Decal](#decal)
- [Decal Device](#decal-device)
- [Details Panel](#details-panel)
- [Device](#device)
- [Device Event](#device-event)
- [Device Function](#device-function)
- [Direct Event Binding](#direct-event-binding)
- [Draw Call](#draw-call)
- [Edit List](#edit-list)
- [Entity](#entity)
- [Environment Light Rig](#environment-light-rig)
- [Event Browser](#event-browser)
- [Experimental](#experimental)
- [Fab](#fab)
- [Fix-Up Tool](#fix-up-tool)
- [Foliage](#foliage)
- [Full Recook](#full-recook)
- [Game Phase](#game-phase)
- [Gameplay Tag](#gameplay-tag)
- [HLOD](#hlod)
- [HUD](#hud)
- [HUD Message Device](#hud-message-device)
- [IARC](#iarc)
- [Imported Asset](#imported-asset)
- [In-World UMG Widget](#in-world-umg-widget)
- [Island Settings](#island-settings)
- [Join in Progress](#join-in-progress)
- [Landscape](#landscape)
- [Launch Session](#launch-session)
- [Level](#level)
- [Level Instance](#level-instance)
- [Level Sequence](#level-sequence)
- [Live Edit](#live-edit)
- [LLM Conversation](#llm-conversation)
- [Lore Version Control](#lore-version-control)
- [Lumen](#lumen)
- [Lumen Exposure Manager](#lumen-exposure-manager)
- [Material](#material)
- [Material Instance](#material-instance)
- [Memory Calculation](#memory-calculation)
- [Memory Snapshot](#memory-snapshot)
- [Memory Test Results](#memory-test-results)
- [Message Log](#message-log)
- [MetaSound](#metasound)
- [Mobile Preview](#mobile-preview)
- [Modal Dialog Variant](#modal-dialog-variant)
- [Modeling Mode](#modeling-mode)
- [Moderation](#moderation)
- [Monitor Performance](#monitor-performance)
- [Nanite](#nanite)
- [Navigation Mesh](#navigation-mesh)
- [Niagara](#niagara)
- [NPC](#npc)
- [NPC Character Definition](#npc-character-definition)
- [NPC Character Modifier](#npc-character-modifier)
- [NPC Spawner](#npc-spawner)
- [One File Per Entity](#one-file-per-entity)
- [Outliner](#outliner)
- [Output Log](#output-log)
- [Performance Panel](#performance-panel)
- [Persona](#persona)
- [Persona Modifier](#persona-modifier)
- [Pivot](#pivot)
- [Pop-Up Dialog Device](#pop-up-dialog-device)
- [Private Version](#private-version)
- [Project Browser](#project-browser)
- [Project Size](#project-size)
- [Project Size Tool](#project-size-tool)
- [Publish Project](#publish-project)
- [Publishing Report](#publishing-report)
- [Push Changes](#push-changes)
- [Refresh Session](#refresh-session)
- [Release](#release)
- [Revision](#revision)
- [Revision Control](#revision-control)
- [Round Reset](#round-reset)
- [Safe Zone](#safe-zone)
- [Scene Graph](#scene-graph)
- [Sequencer](#sequencer)
- [Sequencer Modifier](#sequencer-modifier)
- [Session](#session)
- [Session Inspector](#session-inspector)
- [Skeletal Mesh](#skeletal-mesh)
- [Spatial Profiler](#spatial-profiler)
- [Static Mesh](#static-mesh)
- [Streaming](#streaming)
- [Sync Latest](#sync-latest)
- [Texture](#texture)
- [Texture Streaming Memory](#texture-streaming-memory)
- [Time of Day Manager](#time-of-day-manager)
- [Transform](#transform)
- [UEFN](#uefn)
- [UMG](#umg)
- [Unreal Revision Control](#unreal-revision-control)
- [User Widget](#user-widget)
- [Validation](#validation)
- [Verse-Authored Device](#verse-authored-device)
- [VFX Spawner Device](#vfx-spawner-device)
- [Viewmodel](#viewmodel)
- [Viewport](#viewport)
- [Widget Blueprint](#widget-blueprint)
- [World](#world)
- [World Partition](#world-partition)

## Entries

<a id="actor"></a>
### Actor

- **Canonical Hebrew:** אקטור
- **Definition:** Any object that can be placed in a UEFN level and transformed in 3D space. Devices, meshes, lights, cameras, and many other placed objects are Actors.
- **Environment:** UEFN
- **Implementation source:** `08-10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="asset"></a>
### Asset

- **Canonical Hebrew:** נכס
- **Definition:** A reusable content resource stored in the project, such as a mesh, texture, material, animation, sound, widget, Level Sequence, Conversation Bank, or NPC Character Definition.
- **Environment:** UEFN
- **Implementation source:** `08-10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="asset-dependency"></a>
### Asset Dependency

- **Canonical Hebrew:** תלות בין נכסים
- **Definition:** A reference that causes one asset or placed object to require another asset. Deleting a visible Actor does not necessarily remove its dependencies from the project.
- **Environment:** UEFN
- **Implementation source:** `08, 10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="audio-mixer-device"></a>
### Audio Mixer Device

- **Canonical Hebrew:** התקן Audio Mixer
- **Definition:** A Fortnite Device used to control supported audio mix behavior and categories. Exact options and runtime scope must be verified on the current Device page.
- **Environment:** UEFN / Fortnite Devices
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="audio-player-device"></a>
### Audio Player Device

- **Canonical Hebrew:** נגן שמע
- **Definition:** A Device that plays selected audio in response to configured gameplay or event-binding inputs and controls supported audience and spatial behavior.
- **Environment:** UEFN / Fortnite Devices
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="auto-checkout"></a>
### Auto Checkout

- **Canonical Hebrew:** הוצאה אוטומטית לעריכה
- **Definition:** A Lore Version Control feature that attempts to check out an asset when a contributor begins editing it, helping prevent conflicting simultaneous edits.
- **Environment:** UEFN collaboration
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="barrier-device"></a>
### Barrier Device

- **Canonical Hebrew:** התקן מחסום
- **Definition:** A Device that creates a configurable blocking volume and can be enabled or disabled through supported Device functions.
- **Environment:** UEFN / Fortnite Devices
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="beta"></a>
### Beta

- **Canonical Hebrew:** בטא
- **Definition:** A feature-status label indicating that a system is available for use but still evolving and should be used cautiously in production. Current status must be checked in Epic documentation and release notes.
- **Environment:** UEFN
- **Implementation source:** `08-10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="branch-explorer"></a>
### Branch Explorer

- **Canonical Hebrew:** Branch Explorer
- **Definition:** A Lore Version Control interface for inspecting revision/branch history and related project state where supported by the current release.
- **Environment:** UEFN collaboration
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="build-hlods"></a>
### Build HLODs

- **Canonical Hebrew:** בניית HLOD
- **Definition:** The editor build operation that generates Hierarchical Level of Detail assets for eligible world content.
- **Environment:** UEFN
- **Implementation source:** `08, 10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="changelist"></a>
### Changelist

- **Canonical Hebrew:** רשימת שינויים
- **Definition:** A grouped set of local project changes prepared for check-in through Lore Version Control.
- **Environment:** UEFN collaboration
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="character-device"></a>
### Character Device

- **Canonical Hebrew:** התקן דמות
- **Definition:** A Fortnite Device that displays and controls a character presentation. It is different from the UEFN NPC Spawner and does not provide custom autonomous NPC behavior.
- **Environment:** UEFN / Fortnite Devices
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="check-in"></a>
### Check In

- **Canonical Hebrew:** הכנסה לבקרת גרסאות
- **Definition:** The Lore action that submits a coherent set of changed assets as a shared project revision. Saving locally is not the same as checking in.
- **Environment:** UEFN collaboration
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="checkout"></a>
### Checkout

- **Canonical Hebrew:** הוצאה לעריכה
- **Definition:** The Lore ownership action that reserves an asset for editing when the asset type and workflow require exclusive ownership.
- **Environment:** UEFN collaboration
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="cinematic-sequence-device"></a>
### Cinematic Sequence Device

- **Canonical Hebrew:** התקן רצף קולנועי
- **Definition:** A Device that plays a selected Level Sequence and exposes documented playback and completion controls for gameplay integration.
- **Environment:** UEFN
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="collision"></a>
### Collision

- **Canonical Hebrew:** התנגשות
- **Definition:** The rules and geometry that determine whether players, NPCs, vehicles, traces, or other objects block, overlap, or pass through an object.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="component"></a>
### Component

- **Canonical Hebrew:** רכיב
- **Definition:** A reusable unit attached to a larger object that supplies data or capability. The exact meaning differs between conventional Actors and Scene Graph entities.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="content-browser"></a>
### Content Browser

- **Canonical Hebrew:** דפדפן התוכן
- **Definition:** The editor interface for creating, importing, finding, organizing, and opening project assets.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="content-drawer"></a>
### Content Drawer

- **Canonical Hebrew:** מגירת התוכן
- **Definition:** An auto-minimizing Content Browser instance that opens from the bottom of the editor and can be docked as a persistent browser.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="control-rig"></a>
### Control Rig

- **Canonical Hebrew:** Control Rig
- **Definition:** A rigging and animation toolset for posing and animating supported characters or objects directly in UEFN and Sequencer.
- **Environment:** UEFN
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="conversation-bank"></a>
### Conversation Bank

- **Canonical Hebrew:** מאגר שיחה
- **Definition:** A UEFN asset that stores an authored conversation graph, entry points, nodes, choices, and related conversation data.
- **Environment:** UEFN without Verse
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="conversation-device"></a>
### Conversation Device

- **Canonical Hebrew:** התקן שיחה
- **Definition:** A UEFN Device that initiates an authored Conversation Bank and can connect conversation outcomes to other Devices. Conversations exited Experimental in ecosystem v41.30; exact feature availability remains version-sensitive.
- **Environment:** UEFN without Verse
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="conversation-editor"></a>
### Conversation Editor

- **Canonical Hebrew:** עורך שיחות
- **Definition:** The graph-authoring editor used to create and connect conversation nodes, player choices, conditions supported by the system, events, and exits in a Conversation Bank.
- **Environment:** UEFN without Verse
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="conversation-entry-point"></a>
### Conversation Entry Point

- **Canonical Hebrew:** נקודת כניסה לשיחה
- **Definition:** A named or default starting location in an authored Conversation Bank used when a Conversation Device initiates dialogue.
- **Environment:** UEFN without Verse
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="conversation-event"></a>
### Conversation Event

- **Canonical Hebrew:** אירוע שיחה
- **Definition:** An authored event emitted from a conversation node or choice so the Conversation system can trigger supported gameplay responses through Device integration.
- **Environment:** UEFN without Verse
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="cook"></a>
### Cook

- **Canonical Hebrew:** Cook
- **Definition:** The platform preparation process that converts project content into runtime-ready data used by launched sessions, Private Versions, memory calculation, and publishing workflows.
- **Environment:** UEFN production
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="creator-portal"></a>
### Creator Portal

- **Canonical Hebrew:** Creator Portal
- **Definition:** Epic's web portal for managing Fortnite projects, teams, releases, metadata, IARC, moderation, publishing, and analytics.
- **Environment:** Fortnite creator ecosystem
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="data-layer"></a>
### Data Layer

- **Canonical Hebrew:** שכבת נתונים
- **Definition:** A system for organizing groups of world content and controlling supported editor or runtime states. Data Layers do not automatically remove all referenced content from memory.
- **Environment:** UEFN
- **Implementation source:** `08, 10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="day-sequence-device"></a>
### Day Sequence Device

- **Canonical Hebrew:** התקן Day Sequence
- **Definition:** A Device used to control supported time-of-day and environmental presentation settings in projects using the compatible lighting system.
- **Environment:** UEFN / Fortnite Devices
- **Implementation source:** `08, 09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="decal"></a>
### Decal

- **Canonical Hebrew:** מדבקת הקרנה
- **Definition:** A projected material used to add surface detail such as signs, stains, markings, or guidance without adding separate modeled geometry.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="decal-device"></a>
### Decal Device

- **Canonical Hebrew:** התקן Decal
- **Definition:** A Device-oriented workflow for displaying supported decal content with gameplay configuration. It is distinct from a conventional Unreal decal Actor/material workflow.
- **Environment:** UEFN
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="details-panel"></a>
### Details Panel

- **Canonical Hebrew:** חלונית Details
- **Definition:** The panel that displays and edits properties for the selected Actor, Device, asset, component, or entity.
- **Environment:** UEFN
- **Implementation source:** `08-09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="device"></a>
### Device

- **Canonical Hebrew:** התקן
- **Definition:** A configurable Fortnite gameplay object that supplies built-in behavior and can communicate with other Devices through Direct Event Binding.
- **Environment:** UEFN without Verse
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="device-event"></a>
### Device Event

- **Canonical Hebrew:** אירוע של התקן
- **Definition:** A documented signal emitted by a Device when a specific supported condition occurs. Event names belong only to the named Device.
- **Environment:** UEFN without Verse
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="device-function"></a>
### Device Function

- **Canonical Hebrew:** פונקציה של התקן
- **Definition:** A documented action a Device can perform when invoked by Direct Event Binding. Function names belong only to the named Device.
- **Environment:** UEFN without Verse
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="direct-event-binding"></a>
### Direct Event Binding

- **Canonical Hebrew:** חיבור ישיר בין אירועים
- **Definition:** The Device communication system that connects a source Device Event to a receiving Device Function without channels or Verse.
- **Environment:** UEFN without Verse
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="draw-call"></a>
### Draw Call

- **Canonical Hebrew:** קריאת ציור
- **Definition:** A rendering submission used to draw geometry/material work. Large numbers of visible objects and material sections can increase draw-call cost.
- **Environment:** UEFN performance
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="edit-list"></a>
### Edit List

- **Canonical Hebrew:** רשימת עריכות
- **Definition:** A session interface that identifies project changes that may need to be pushed to the connected Fortnite client.
- **Environment:** UEFN production
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="entity"></a>
### Entity

- **Canonical Hebrew:** ישות
- **Definition:** A Scene Graph object that can contain components and child entities. Scene Graph status is version-sensitive and currently requires a Beta production gate.
- **Environment:** UEFN Scene Graph
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="environment-light-rig"></a>
### Environment Light Rig

- **Canonical Hebrew:** Environment Light Rig
- **Definition:** A UEFN lighting system that groups controls for environment lighting, sky, exposure, and related presentation in supported projects.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="event-browser"></a>
### Event Browser

- **Canonical Hebrew:** דפדפן אירועים
- **Definition:** An interface for inspecting Device event-binding relationships in a project and tracing which Devices are connected.
- **Environment:** UEFN without Verse
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="experimental"></a>
### Experimental

- **Canonical Hebrew:** ניסיוני
- **Definition:** A feature-status label indicating that a system is not production-stable and may have limited publishing support or breaking changes. Always confirm current status before use.
- **Environment:** UEFN
- **Implementation source:** `08-10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="fab"></a>
### Fab

- **Canonical Hebrew:** Fab
- **Definition:** Epic's content marketplace and library integration. Assets acquired through Fab still require license, attribution, validation, compatibility, and performance review.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="fix-up-tool"></a>
### Fix-Up Tool

- **Canonical Hebrew:** כלי תיקון
- **Definition:** An official validation-related tool that can correct supported project issues. Automatic fixes must be reviewed and retested.
- **Environment:** UEFN production
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="foliage"></a>
### Foliage

- **Canonical Hebrew:** צמחייה
- **Definition:** The system for painting or distributing repeated vegetation and similar instances across surfaces with density, collision, culling, and performance controls.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="full-recook"></a>
### Full Recook

- **Canonical Hebrew:** בישול מלא מחדש
- **Definition:** The current Session menu operation that rebuilds the complete cooked project when incremental cooking is not operating correctly. It is a slow last-resort iteration action.
- **Environment:** UEFN production
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="game-phase"></a>
### Game Phase

- **Canonical Hebrew:** שלב משחק
- **Definition:** A lifecycle state such as pre-game, gameplay, round end, or game end that can affect Device availability and Island Settings behavior.
- **Environment:** UEFN / Fortnite runtime
- **Implementation source:** `09-10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="gameplay-tag"></a>
### Gameplay Tag

- **Canonical Hebrew:** תג משחקיות
- **Definition:** A named tag used by supported systems to categorize or identify gameplay data. Exact availability and behavior depend on the owning system.
- **Environment:** UEFN
- **Implementation source:** `08-09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="hlod"></a>
### HLOD

- **Canonical Hebrew:** HLOD
- **Definition:** Hierarchical Level of Detail generated content that represents groups of distant Actors more efficiently. HLOD can improve distant rendering but adds generated assets and must be measured.
- **Environment:** UEFN
- **Implementation source:** `08, 10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="hud"></a>
### HUD

- **Canonical Hebrew:** תצוגה עילית
- **Definition:** The player-facing on-screen interface layer used for objectives, messages, status, score, timers, and other gameplay information.
- **Environment:** UEFN / Fortnite runtime
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="hud-message-device"></a>
### HUD Message Device

- **Canonical Hebrew:** התקן הודעת HUD
- **Definition:** A Device that displays configured text or supported widget content to an intended audience and exposes documented show/hide/layer behavior.
- **Environment:** UEFN without Verse
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="iarc"></a>
### IARC

- **Canonical Hebrew:** IARC
- **Definition:** The International Age Rating Coalition questionnaire system used in Creator Portal to generate regional age ratings for a release.
- **Environment:** Publishing
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="imported-asset"></a>
### Imported Asset

- **Canonical Hebrew:** נכס מיובא
- **Definition:** Content brought into the project from an external source. It must use a supported format and pass licensing, validation, compatibility, and performance checks.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="in-world-umg-widget"></a>
### In-World UMG Widget

- **Canonical Hebrew:** וידג'ט UMG בעולם
- **Definition:** A UMG-based interface rendered in the 3D world rather than only on the HUD. Current in-world UMG workflows became available in the v41.20 ecosystem release and remain version-sensitive.
- **Environment:** UEFN without Verse
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="island-settings"></a>
### Island Settings

- **Canonical Hebrew:** הגדרות האי
- **Definition:** Project-level gameplay settings that define players, teams, spawning, rounds, scoring, UI, world rules, and other Fortnite runtime behavior.
- **Environment:** UEFN / Fortnite Creative
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="join-in-progress"></a>
### Join in Progress

- **Canonical Hebrew:** הצטרפות במהלך משחק
- **Definition:** A player entering an already-running match. The late player must receive a valid spawn, current world state, understandable objective, and usable recovery path.
- **Environment:** UEFN production
- **Implementation source:** `09-10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="landscape"></a>
### Landscape

- **Canonical Hebrew:** תוואי שטח
- **Definition:** UEFN's terrain-authoring system for sculpting and painting large outdoor surfaces.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="launch-session"></a>
### Launch Session

- **Canonical Hebrew:** הפעלת סשן
- **Definition:** The UEFN action that cooks and starts the project in Fortnite for runtime testing.
- **Environment:** UEFN production
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="level"></a>
### Level

- **Canonical Hebrew:** שלב / Level
- **Definition:** A saved world/map area containing placed Actors and project gameplay content.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="level-instance"></a>
### Level Instance

- **Canonical Hebrew:** מופע Level
- **Definition:** A reusable placed instance of level content used to organize or repeat authored areas where supported.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="level-sequence"></a>
### Level Sequence

- **Canonical Hebrew:** רצף שלב
- **Definition:** A Sequencer asset that stores time-based tracks for cameras, transforms, animation, audio, events, and other cinematic presentation.
- **Environment:** UEFN
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="live-edit"></a>
### Live Edit

- **Canonical Hebrew:** עריכה חיה
- **Definition:** The connected-session workflow that synchronizes supported editor changes to Fortnite for faster iteration. It does not replace a current cook or Private Version test.
- **Environment:** UEFN production
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="llm-conversation"></a>
### LLM Conversation

- **Canonical Hebrew:** שיחת LLM
- **Definition:** A current UEFN conversation capability using supported persona and language-model systems. It became publishable with the v41.30 ecosystem release and is subject to feature, policy, persona, and moderation restrictions.
- **Environment:** UEFN without custom Verse
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="lore-version-control"></a>
### Lore Version Control

- **Canonical Hebrew:** Lore Version Control
- **Definition:** Epic's integrated UEFN version-control system for syncing, ownership, checkout, changelists, check-in, history, conflicts, and collaboration. It was renamed from Unreal Revision Control in v41.10.
- **Environment:** UEFN collaboration
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="lumen"></a>
### Lumen

- **Canonical Hebrew:** Lumen
- **Definition:** Unreal's dynamic global illumination and reflection system used by supported UEFN rendering paths. Lower-end platforms can use different presentation paths.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="lumen-exposure-manager"></a>
### Lumen Exposure Manager

- **Canonical Hebrew:** Lumen Exposure Manager
- **Definition:** A UEFN tool for managing exposure presentation across Lumen and non-Lumen scalability paths.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="material"></a>
### Material

- **Canonical Hebrew:** חומר
- **Definition:** An asset that defines how a surface is rendered, including color, texture, roughness, emissive, opacity, and other shader behavior.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="material-instance"></a>
### Material Instance

- **Canonical Hebrew:** מופע חומר
- **Definition:** A parameterized child of a parent Material used to create variations without duplicating the full shader graph.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="memory-calculation"></a>
### Memory Calculation

- **Canonical Hebrew:** חישוב זיכרון
- **Definition:** The cooked-content analysis used to evaluate island memory for publishing. It is separate from project size and live runtime profiling.
- **Environment:** UEFN production
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="memory-snapshot"></a>
### Memory Snapshot

- **Canonical Hebrew:** תמונת מצב של זיכרון
- **Definition:** A runtime-connected performance tool that estimates memory used by currently loaded assets, shows impact and reference relationships, and can navigate to referencing Actors. It is separate from publication memory calculation.
- **Environment:** UEFN production
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="memory-test-results"></a>
### Memory Test Results

- **Canonical Hebrew:** תוצאות בדיקת זיכרון
- **Definition:** The Message Log result view that reports memory findings and high-cost project content after a current memory calculation.
- **Environment:** UEFN production
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="message-log"></a>
### Message Log

- **Canonical Hebrew:** יומן הודעות
- **Definition:** An editor window that presents structured validation, memory, build, and other diagnostic messages.
- **Environment:** UEFN
- **Implementation source:** `08-10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="metasound"></a>
### MetaSound

- **Canonical Hebrew:** MetaSound
- **Definition:** A node-based audio asset system used for procedural or parameterized audio in supported UEFN workflows. Availability and publishing support must be checked for the current release.
- **Environment:** UEFN
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="mobile-preview"></a>
### Mobile Preview

- **Canonical Hebrew:** תצוגה מקדימה לנייד
- **Definition:** A UEFN preview workflow for approximating lower-end/mobile rendering. It does not replace testing on actual target hardware.
- **Environment:** UEFN production
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="modal-dialog-variant"></a>
### Modal Dialog Variant

- **Canonical Hebrew:** וריאנט חלון מודאלי
- **Definition:** A UMG widget type intended for Device-driven modal UI, including supported Pop-Up Dialog workflows and input focus.
- **Environment:** UEFN without Verse
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="modeling-mode"></a>
### Modeling Mode

- **Canonical Hebrew:** מצב מידול
- **Definition:** UEFN's mesh-editing toolset for creating or modifying geometry, blockouts, UVs, collision helpers, and other modeled content.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="moderation"></a>
### Moderation

- **Canonical Hebrew:** בדיקת תוכן
- **Definition:** Epic's review process for a submitted release, including content, metadata, media, policy, IP, safety, and rating consistency.
- **Environment:** Publishing
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="monitor-performance"></a>
### Monitor Performance

- **Canonical Hebrew:** ניטור ביצועים
- **Definition:** A Launch Session option that enables current performance monitoring workflows, including Spatial Profiler and Memory Snapshot.
- **Environment:** UEFN production
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="nanite"></a>
### Nanite

- **Canonical Hebrew:** Nanite
- **Definition:** Unreal's virtualized geometry technology. UEFN support, validation, target-platform behavior, and performance must be confirmed for the current release and asset.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="navigation-mesh"></a>
### Navigation Mesh

- **Canonical Hebrew:** רשת ניווט
- **Definition:** The generated traversable area used by supported NPCs and AI for pathfinding. It must be inspected in a launched session where current debug visualization supports it.
- **Environment:** UEFN
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="niagara"></a>
### Niagara

- **Canonical Hebrew:** Niagara
- **Definition:** Unreal's node-based visual-effects system used to author supported particle and simulation effects in UEFN.
- **Environment:** UEFN
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="npc"></a>
### NPC

- **Canonical Hebrew:** NPC
- **Definition:** A non-player character controlled by configured Fortnite behavior, an NPC Character Definition, a Spawner, cinematic animation, or custom Verse behavior where the environment permits it.
- **Environment:** UEFN
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="npc-character-definition"></a>
### NPC Character Definition

- **Canonical Hebrew:** הגדרת דמות NPC
- **Definition:** A reusable UEFN asset that defines an NPC's character type, appearance, behavior choice, and supported modifiers.
- **Environment:** UEFN
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="npc-character-modifier"></a>
### NPC Character Modifier

- **Canonical Hebrew:** משנה דמות NPC
- **Definition:** A modifier attached to an NPC Character Definition or Spawner configuration to override a supported category such as appearance, navigation, team, health, or persona.
- **Environment:** UEFN
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="npc-spawner"></a>
### NPC Spawner

- **Canonical Hebrew:** מחולל NPC
- **Definition:** A UEFN Device that creates NPCs from an NPC Character Definition and controls supported spawn count, timing, radius, lifecycle, and despawn behavior.
- **Environment:** UEFN
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="one-file-per-entity"></a>
### One File Per Entity

- **Canonical Hebrew:** קובץ לכל ישות
- **Definition:** A Scene Graph storage/collaboration approach that separates entity data to reduce contention. Its status and migration implications are version-sensitive.
- **Environment:** UEFN Scene Graph
- **Implementation source:** `08, 10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="outliner"></a>
### Outliner

- **Canonical Hebrew:** Outliner
- **Definition:** The hierarchical list of placed Actors, Devices, folders, and other level objects used to find, select, name, and organize world content.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="output-log"></a>
### Output Log

- **Canonical Hebrew:** יומן פלט
- **Definition:** The broad editor diagnostic log used for messages that may not appear in the structured Message Log.
- **Environment:** UEFN
- **Implementation source:** `08-10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="performance-panel"></a>
### Performance Panel

- **Canonical Hebrew:** חלונית ביצועים
- **Definition:** The bottom UEFN panel used to access supported performance tools such as Memory Snapshot during a connected session.
- **Environment:** UEFN production
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="persona"></a>
### Persona

- **Canonical Hebrew:** פרסונה
- **Definition:** A configured identity and behavioral instruction set used by supported LLM Conversation/NPC systems. Availability, permitted voices/IP, safety, and publishing status are version-sensitive.
- **Environment:** UEFN
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="persona-modifier"></a>
### Persona Modifier

- **Canonical Hebrew:** משנה פרסונה
- **Definition:** An NPC modifier that associates supported persona behavior with an NPC Character Definition or spawned NPC workflow.
- **Environment:** UEFN
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="pivot"></a>
### Pivot

- **Canonical Hebrew:** נקודת ציר
- **Definition:** The point used as the origin for an object's movement, rotation, scaling, snapping, and placement.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="pop-up-dialog-device"></a>
### Pop-Up Dialog Device

- **Canonical Hebrew:** התקן חלון קופץ
- **Definition:** A Device that presents configured choices or modal UI and can use supported UMG Modal Dialog Variant assets.
- **Environment:** UEFN without Verse
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="private-version"></a>
### Private Version

- **Canonical Hebrew:** גרסה פרטית
- **Definition:** A cooked, uploaded project version used for testing, memory calculation, and as the basis for a Creator Portal release. It is not a public release.
- **Environment:** Publishing
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="project-browser"></a>
### Project Browser

- **Canonical Hebrew:** דפדפן פרויקטים
- **Definition:** The startup interface used to create a UEFN project from a template, open an existing project, or select project content.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="project-size"></a>
### Project Size

- **Canonical Hebrew:** גודל פרויקט
- **Definition:** The stored/uploaded content and dependency footprint of a project. It is different from memory calculation and runtime performance.
- **Environment:** UEFN production
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="project-size-tool"></a>
### Project Size Tool

- **Canonical Hebrew:** כלי גודל פרויקט
- **Definition:** An Epic tool/workflow for inspecting project content size and major contributors.
- **Environment:** UEFN production
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="publish-project"></a>
### Publish Project

- **Canonical Hebrew:** פרסום פרויקט
- **Definition:** The UEFN command/workflow that creates a release-ready Private Version and transitions publishing work to Creator Portal. It does not itself complete public publication.
- **Environment:** Publishing
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="publishing-report"></a>
### Publishing Report

- **Canonical Hebrew:** דוח פרסום
- **Definition:** A current UEFN panel that consolidates project publish-readiness checks and surfaces issues before the Creator Portal release workflow.
- **Environment:** UEFN production
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="push-changes"></a>
### Push Changes

- **Canonical Hebrew:** דחיפת שינויים
- **Definition:** The session action that sends pending project changes to the connected Fortnite client and recooks content when required.
- **Environment:** UEFN production
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="refresh-session"></a>
### Refresh Session

- **Canonical Hebrew:** רענון סשן
- **Definition:** The action used to refresh a connected Fortnite session when incremental changes or state have become stale.
- **Environment:** UEFN production
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="release"></a>
### Release

- **Canonical Hebrew:** מהדורה
- **Definition:** A Creator Portal record that selects a Private Version and adds metadata, ratings, disclosures, visibility, and moderation state for publication.
- **Environment:** Publishing
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="revision"></a>
### Revision

- **Canonical Hebrew:** גרסה בבקרת גרסאות
- **Definition:** A submitted Lore project state that can be synced, inspected, compared, or used as a recovery point.
- **Environment:** UEFN collaboration
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="revision-control"></a>
### Revision Control

- **Canonical Hebrew:** בקרת גרסאות
- **Definition:** A generic or legacy label for the integrated UEFN source-control workflow. Current Epic documentation uses Lore Version Control.
- **Environment:** UEFN collaboration
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="round-reset"></a>
### Round Reset

- **Canonical Hebrew:** איפוס סיבוב
- **Definition:** The restoration of intended Device, player, objective, UI, NPC, cinematic, audio, and VFX state when a new round begins.
- **Environment:** UEFN runtime
- **Implementation source:** `09-10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="safe-zone"></a>
### Safe Zone

- **Canonical Hebrew:** אזור בטוח בממשק
- **Definition:** The screen area in which critical UI should remain visible across displays, overscan, aspect ratios, and platforms.
- **Environment:** UEFN UI
- **Implementation source:** `09-10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="scene-graph"></a>
### Scene Graph

- **Canonical Hebrew:** Scene Graph
- **Definition:** UEFN's entity-and-component world-authoring system for hierarchical content and reusable prefabs. Current Epic documentation marks it Beta, so production use requires a version and recovery gate.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="sequencer"></a>
### Sequencer

- **Canonical Hebrew:** Sequencer
- **Definition:** The timeline editor used to author Level Sequences containing camera, animation, transform, audio, event, and other time-based tracks.
- **Environment:** UEFN
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="sequencer-modifier"></a>
### Sequencer Modifier

- **Canonical Hebrew:** משנה Sequencer
- **Definition:** An NPC Character Definition modifier used to identify and bind a spawned NPC for supported Sequencer animation workflows.
- **Environment:** UEFN
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="session"></a>
### Session

- **Canonical Hebrew:** סשן
- **Definition:** A connected Fortnite runtime instance launched from UEFN for testing and Live Edit.
- **Environment:** UEFN production
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="session-inspector"></a>
### Session Inspector

- **Canonical Hebrew:** מפקח סשן
- **Definition:** A UEFN diagnostic tool for Launch Session and Live Edit that displays session status, cook modules, child artifacts, cache use, wall time, and failure state.
- **Environment:** UEFN production
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="skeletal-mesh"></a>
### Skeletal Mesh

- **Canonical Hebrew:** רשת שלדית
- **Definition:** A mesh bound to a skeleton so it can deform and play skeletal animation.
- **Environment:** UEFN
- **Implementation source:** `08-09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="spatial-profiler"></a>
### Spatial Profiler

- **Canonical Hebrew:** פרופיילר מרחבי
- **Definition:** A UEFN runtime profiling tool that records spatial metrics such as object counts, memory-related values, rendering work, and frame/game/GPU timing where available.
- **Environment:** UEFN production
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="static-mesh"></a>
### Static Mesh

- **Canonical Hebrew:** רשת סטטית
- **Definition:** A non-deforming geometry asset used for architecture, props, terrain pieces, and other rigid world objects.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="streaming"></a>
### Streaming

- **Canonical Hebrew:** טעינה מדורגת
- **Definition:** The runtime loading and unloading of World Partition cells/content around streaming sources such as players.
- **Environment:** UEFN
- **Implementation source:** `08, 10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="sync-latest"></a>
### Sync Latest

- **Canonical Hebrew:** סנכרון לגרסה האחרונה
- **Definition:** The Lore action that updates a contributor's working copy to the newest shared project revision before editing or integrating work.
- **Environment:** UEFN collaboration
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="texture"></a>
### Texture

- **Canonical Hebrew:** מרקם
- **Definition:** An image or data asset used by materials, UI, VFX, and other rendering systems.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="texture-streaming-memory"></a>
### Texture Streaming Memory

- **Canonical Hebrew:** זיכרון טעינת מרקמים
- **Definition:** Runtime memory associated with streamed texture mip levels, available as a profiling concern/metric in supported tools.
- **Environment:** UEFN performance
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="time-of-day-manager"></a>
### Time of Day Manager

- **Canonical Hebrew:** מנהל זמן ביום
- **Definition:** The project lighting/environment system that owns supported sky and time-of-day behavior. Exact manager compatibility determines which environment Devices can be used.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="transform"></a>
### Transform

- **Canonical Hebrew:** טרנספורם
- **Definition:** An object's location, rotation, and scale, evaluated in world or local space.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="uefn"></a>
### UEFN

- **Canonical Hebrew:** UEFN
- **Definition:** Unreal Editor for Fortnite, the PC editor used to create Fortnite experiences with Unreal-style world, asset, Device, UI, animation, profiling, collaboration, and publishing tools.
- **Environment:** UEFN
- **Implementation source:** `08-10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="umg"></a>
### UMG

- **Canonical Hebrew:** UMG
- **Definition:** Unreal Motion Graphics, the widget-authoring framework used to create supported HUD, modal, and in-world UI assets in UEFN.
- **Environment:** UEFN
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="unreal-revision-control"></a>
### Unreal Revision Control

- **Canonical Hebrew:** Unreal Revision Control
- **Definition:** The former product name for UEFN's integrated revision-control system. Current Epic documentation uses Lore Version Control.
- **Environment:** UEFN collaboration
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="user-widget"></a>
### User Widget

- **Canonical Hebrew:** וידג'ט משתמש
- **Definition:** A UMG widget type used for authored HUD or supported in-world interface presentation. It differs from Modal Dialog Variant behavior.
- **Environment:** UEFN without Verse
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="validation"></a>
### Validation

- **Canonical Hebrew:** אימות
- **Definition:** UEFN checks that project assets, properties, references, and content are supported by the Fortnite runtime and publishing pipeline.
- **Environment:** UEFN production
- **Implementation source:** `10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="verse-authored-device"></a>
### Verse-Authored Device

- **Canonical Hebrew:** התקן שנכתב ב־Verse
- **Definition:** A custom Device class created with Verse and placed/configured in UEFN. It is outside the UEFN-without-Verse layer.
- **Environment:** UEFN with Verse
- **Implementation source:** `09, 12-14` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="vfx-spawner-device"></a>
### VFX Spawner Device

- **Canonical Hebrew:** התקן מחולל VFX
- **Definition:** A Device that plays a selected built-in visual effect and exposes supported enable, disable, and spawn-related controls.
- **Environment:** UEFN without Verse
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="viewmodel"></a>
### Viewmodel

- **Canonical Hebrew:** Viewmodel
- **Definition:** A supported data-binding layer that exposes Fortnite or Device data to UMG widgets without requiring the widget to own gameplay state.
- **Environment:** UEFN without Verse
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="viewport"></a>
### Viewport

- **Canonical Hebrew:** Viewport
- **Definition:** The primary 3D editor surface used to view, select, place, transform, and inspect world content.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="widget-blueprint"></a>
### Widget Blueprint

- **Canonical Hebrew:** תבנית וידג'ט
- **Definition:** A UMG asset that defines the hierarchy, layout, style, animation, and supported bindings of a user-interface widget.
- **Environment:** UEFN
- **Implementation source:** `09` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="world"></a>
### World

- **Canonical Hebrew:** עולם
- **Definition:** The complete UEFN spatial environment containing levels, Actors, gameplay systems, lighting, streaming, and player-accessible areas.
- **Environment:** UEFN
- **Implementation source:** `08` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

<a id="world-partition"></a>
### World Partition

- **Canonical Hebrew:** חלוקת עולם
- **Definition:** The large-world system that divides a world into spatial cells and supports editor/runtime streaming workflows.
- **Environment:** UEFN
- **Implementation source:** `08, 10` and the matching page in `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
- **Search rule:** Use the exact English term in UEFN and official Epic documentation.

## Status and Legacy Notes

### Current Naming

- Use **Lore Version Control** for the current integrated collaboration product.
- Treat **Unreal Revision Control** as the former product name.
- Use **Direct Event Binding** rather than legacy channel-based instructions for current UEFN Device connections.

### Current Feature Status Gate

- **Scene Graph:** Beta in the verified release; use a recovery copy and confirm current publishing support.
- **Authored Conversations:** exited Experimental in v41.30; verify the exact current Conversation Device/UI capabilities.
- **LLM Conversations and Personas:** publishable from v41.30 under current feature, policy, persona, and moderation restrictions.
- **In-World UMG Widgets:** available from v41.20; verify the current widget/component workflow.
- **MetaSounds, Nanite, custom materials, imported assets, and experimental editor systems:** confirm validation and target-platform support per project.

## Related Documents

- [00_MASTER_KNOWLEDGE_INDEX.md](00_MASTER_KNOWLEDGE_INDEX.md)
- [01_EPIC_GAMES_DOCUMENTATION_INDEX.md](01_EPIC_GAMES_DOCUMENTATION_INDEX.md)
- [02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md](02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md)
- [08_UEFN_EDITOR_PROJECTS_ASSETS_AND_WORLD_BUILDING.md](08_UEFN_EDITOR_PROJECTS_ASSETS_AND_WORLD_BUILDING.md)
- [09_UEFN_GAMEPLAY_VERSE_API_UI_NPC_AND_CINEMATICS.md](09_UEFN_GAMEPLAY_VERSE_API_UI_NPC_AND_CINEMATICS.md)
- [10_UEFN_TESTING_OPTIMIZATION_COLLABORATION_AND_PUBLISHING.md](10_UEFN_TESTING_OPTIMIZATION_COLLABORATION_AND_PUBLISHING.md)
- [17_GLOSSARY_VERSE.md](17_GLOSSARY_VERSE.md)
