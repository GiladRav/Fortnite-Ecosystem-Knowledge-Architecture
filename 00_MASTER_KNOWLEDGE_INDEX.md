---
document_id: "00"
corpus_role: "master_routing_and_authority"
authority: "corpus_governance"
language: "English; Hebrew appears only in canonical terminology fields and exact source identifiers"
active_knowledge_files: 20
canonical_document_ids: "00-19"
creative_canonical_units: 222
community_secondary_file: "19_FORTNITE_COMMUNITY_WIKI_CREATIVE_DELTA_INDEX.md"
last_verified: "2026-08-03"
verified_release: "Fortnite Ecosystem v41.30"
verified_release_date: "2026-07-30"
status: "production-ready"
---

# Master Knowledge Index

## Document Purpose

This document is the governing entry point for the complete Fortnite development knowledge corpus.

Its job is to:

1. route every question to the correct authoring environment;
2. identify the primary owner document for each claim;
3. define the authority order between official documentation, local synthesis, glossaries, research, and adapted guidance;
4. prevent facts from one Device, workflow, environment, release, or evidence level from being transferred incorrectly to another;
5. define how answers should be composed, verified, labeled, and tested;
6. define which files belong to the active corpus and which files must remain outside it;
7. preserve the separation between Fortnite Creative, UEFN without Verse, and UEFN with Verse.

The active corpus contains **20 knowledge files**, identified by Document IDs `00` through `19`. File `19` is a secondary community discovery and metadata layer; it does not change the technical ownership of files `03` through `18`.

## Scope and Non-Goals

Use this document for environment resolution, corpus-wide routing, authority and freshness decisions, cross-file composition, feature-status interpretation, answer-quality requirements, and maintenance rules.

Do not use it as the detailed implementation source for a specific Device, editor procedure, API signature, publishing rule, gameplay recipe, or glossary definition. Those details belong to the relevant owner file and, when version-sensitive, to the current official Epic Games source.

## Core Operating Principle

A technically correct fact can still produce an incorrect answer when it is taken from the wrong environment, Device, release, evidence level, or lifecycle state.

Every answer must therefore resolve four questions:

1. **Environment:** Where is the user building?
2. **Owner:** Which file owns the requested behavior?
3. **Evidence:** What proves the claim?
4. **Lifecycle:** What happens at start, respawn, reset, player exit, and Join in Progress?

## Quick Topic Index

- [Mandatory First Decision: Resolve the Environment](#mandatory-first-decision-resolve-the-environment)
- [Environment Resolution Table](#environment-resolution-table)
- [Source Authority Hierarchy](#source-authority-hierarchy)
- [Evidence and Verification Ladder](#evidence-and-verification-ladder)
- [Canonical Retrieval Sequence](#canonical-retrieval-sequence)
- [Common Question Router](#common-question-router)
- [Knowledge Base Map](#knowledge-base-map)
- [Primary Ownership Matrix](#primary-ownership-matrix)
- [File 09 Split-Ownership Boundary](#file-09-split-ownership-boundary)
- [Environment Boundaries](#environment-boundaries)
- [Device and API Isolation Rules](#device-and-api-isolation-rules)
- [Verse Code and Feature Policy](#verse-code-and-feature-policy)
- [Lifecycle and Multiplayer Requirements](#lifecycle-and-multiplayer-requirements)
- [Terminology and Naming Policy](#terminology-and-naming-policy)
- [Design, Education, and Recommendation Policy](#design-education-and-recommendation-policy)
- [Community Evidence and Delta Policy](#community-evidence-and-delta-policy)
- [Conflict Resolution Rules](#conflict-resolution-rules)
- [Answer Construction Standards](#answer-construction-standards)
- [Active Upload Set](#active-upload-set)
- [Excluded and Delivery-Only Files](#excluded-and-delivery-only-files)
- [Maintenance and Regression Rules](#maintenance-and-regression-rules)
- [Final Corpus Replacement Gate](#final-corpus-replacement-gate)

---

## Mandatory First Decision: Resolve the Environment

Do not provide technical implementation steps until the working environment is known.

The three supported environments are:

1. **Fortnite Creative only**
2. **UEFN without Verse**
3. **UEFN with Verse**

When the environment is genuinely ambiguous and the implementation would materially differ, ask one short question:

> Are you working in Fortnite Creative, UEFN without Verse, or UEFN with Verse?

Do not ask this question when the environment is already clear from the request or previous context.

## Environment Resolution Table

| Signal in the request | Resolve as | Primary route |
|---|---|---|
| Phone Tool, Create Mode, Content menu, golden rift, in-game Creative inventory | Fortnite Creative | `03`-`07`, glossary `15` |
| Island Settings or Devices used inside Fortnite without UEFN editor tools | Fortnite Creative | `05`-`07` |
| Viewport, Outliner, Details panel, Content Browser, Landscape, imported assets, Sequencer, UMG | UEFN | `08`-`10`, glossary `16` |
| UEFN explicitly requested with no code | UEFN without Verse | `08`-`10`, glossary `16` |
| `creative_device`, Verse Explorer, `.verse`, `@editable`, Verse API, `Build Verse Code` | UEFN with Verse | `09`, `12`-`14`, glossary `17` |
| NPC Character Definition or NPC Spawner | UEFN; determine whether custom Verse behavior is requested | `09` |
| Scene Graph | UEFN; determine whether editor-only or Verse integration is requested | `08` for authoring, `09` for gameplay, `14` for feature gate |
| Creator Portal, Private Version, moderation, IARC, publishing | Preserve source environment | `07` for Creative, `10` for UEFN |
| Definition or translation only | Any environment | `02` for naming; `15`-`17` for definitions |
| Social mechanic, pedagogy, ethics, research, playtest design | Environment-neutral design first | `11`; implementation routes afterward |
| Adapted worksheet, workshop sequence, troubleshooting card, nonviolent Creative pattern | Fortnite Creative support | `18`, with technical verification in `03`-`07` |
| Fortnite Wiki, Fandom page, community alias, visual reference, Prefab or Gallery contents | Preserve the page's claimed environment; verify actual availability | `19` for discovery and metadata, then the relevant technical owner |
| Community-reported bug, workaround, undocumented behavior, or Memory value | Resolve Creative versus UEFN before use | Relevant technical owner first; `19` as provisional community evidence |

### Mixed Requests

A request may contain several layers. Route each layer independently.

Example:

- world building in UEFN → `08`;
- Device gameplay → non-Verse section of `09`;
- custom Verse state → `12`-`14`;
- multiplayer testing → `10`;
- terminology → `02` or `17`.

Do not collapse these layers into one source.

---

## Source Authority Hierarchy

Use the following hierarchy from highest to lowest authority.

### Level 1 — Current Official Runtime and Product Evidence

1. current Epic Games documentation;
2. current Verse Language Reference;
3. current Verse API Reference;
4. current release notes;
5. current creator rules and publishing documentation;
6. current UEFN or Fortnite UI;
7. active UEFN compiler output;
8. validation, cook, launched-session, Private Version, and platform results.

Current product evidence outranks older corpus wording.

### Level 2 — Official Epic Canonical Units in the Corpus

Files `03`-`07` preserve **222 official Creative canonical units**:

- `03` = 7 units;
- `04` = 36 units;
- `05` = 59 units;
- `06` = 63 units;
- `07` = 57 units.

Each unit is authoritative only for the page or Device it preserves.

### Level 3 — Verified UEFN Technical Owners

Files `08`, `09`, and `10` contain current verified UEFN technical synthesis:

- `08` owns editor and world authoring;
- `09` owns gameplay integration and has separate non-Verse and Verse layers;
- `10` owns sessions, production, optimization, collaboration, and publishing.

Professional practice must not be presented as an undocumented platform guarantee.

### Level 4 — Verified Shipping Verse Owners

Files `12`, `13`, and `14` are the active Verse language and architecture layer.

Their filenames retain “Book of Verse” for compatibility, but their active content is **not a raw Book of Verse dump**.

- current official Verse documentation is authoritative;
- current API pages are authoritative for signatures and imports;
- the active UEFN compiler is authoritative for compilation;
- Book of Verse and language-main content require a shipping-availability gate.

### Level 5 — Terminology and Glossaries

- `02` owns canonical English-Hebrew naming;
- `15` owns Creative definitions;
- `16` owns UEFN definitions;
- `17` owns Verse definitions and routing.

Glossaries do not prove exact option values, UI paths, API signatures, publishing requirements, or runtime behavior.

### Level 6 — Design, Research, Education, and Adapted Guidance

- `11` owns game design, social mechanics, research, learning, ethics, and playtest methodology;
- `18` provides adapted teaching sequences, planning cards, nonviolent Creative patterns, and troubleshooting support.

These files do not define platform capability.

### Level 7 — Community Discovery and Delta Evidence

File `19` is the secondary owner for Fortnite Wiki source discovery, normalized community metadata, visual identification, Prefab and Gallery composition, community aliases, thematic tags, genre patterns, reported Device combinations, community Memory reports, reported bugs, edge cases, and undocumented-behavior claims.

Community evidence may improve discovery or provide a hypothesis. It must not define or override:

- Device option names or option values;
- Events or Functions;
- Direct Event Binding behavior;
- Island Settings or UI paths;
- Creative or UEFN feature availability;
- publishing, moderation, eligibility, or platform limits;
- Verse syntax or API signatures;
- current Memory validation rules;
- runtime guarantees.

Every material community claim must retain its source, environment classification, official owner, confidence level, verification status, and permitted agent use. Epic documentation, the current product, the relevant technical owner, validation, compiler output, and recorded tests always outrank it.

### Level 8 — Inference

Inference is permitted only when necessary, supported, clearly labeled, and unable to override an official or verified source. A controlled test is required for undocumented behavior.

A lower authority level must never override a higher one.

---

## Evidence and Verification Ladder

| Evidence level | What it proves | What it does not prove |
|---|---|---|
| Official documentation found | The feature or concept is documented | Exact runtime result in the user's project |
| Official API signature found | The identifier, import, and signature are documented | The user's complete code compiles |
| `SHIPPING — OFFICIAL-DOCUMENTATION-VERIFIED` | The example uses current official syntax/API evidence | Local compilation or runtime behavior |
| `SHIPPING — COMPILED` | The exact code compiled in the recorded release | Runtime, multiplayer, reset, or publishing correctness |
| Editor validation passed | Content passed that validation layer | Cook, runtime, memory, moderation, or platform behavior |
| Launched-session test passed | The tested runtime path worked | Other player counts, resets, platforms, or Private Versions |
| Multiplayer test passed | The tested player-count and ordering scenario worked | Join in Progress, reconnect, next round, or maximum load |
| Memory calculation passed | The publication-memory workflow passed | Runtime performance, project size, or networking |
| Private Version test passed | The cooked uploaded version passed the tested flow | Moderation approval or public-release behavior |
| Moderation approved | The submitted release was approved | Future compatibility or absence of runtime defects |
| Community page or structured field found | A community-maintained source reports the claim or metadata | Official accuracy, current availability, or runtime behavior |
| Community claim normalized in `19` | The source, classification, confidence, verification status, and allowed use were recorded | Technical correctness or current product support |
| Community claim tested or officially matched | The recorded claim passed the named test or matches a current Epic source | Broader behavior outside the tested conditions or source scope |

Never upgrade an evidence label without recording the new evidence.

---

## Canonical Retrieval Sequence

### Step 1 — Resolve the Environment

Choose Creative, UEFN without Verse, or UEFN with Verse.

### Step 2 — Classify the Intent

Classify the request as one or more of: definition, terminology, interface, world building, game rules, Device configuration, gameplay architecture, UI, NPC, Conversation, cinematics, audio, VFX, Verse language, Verse architecture, concurrency, persistence, testing, optimization, collaboration, publishing, design, education, ethics, research, community source discovery, visual identification, asset composition, community alias, genre pattern, reported bug, workaround, or undocumented-behavior claim.

### Step 3 — Select One Primary Owner per Claim

Use the ownership tables below. Secondary files may add context but must not replace the owner.

### Step 4 — Retrieve the Exact Unit or API

For a Device:

1. retrieve the named Device's unit;
2. verify exact options;
3. verify exact Events;
4. verify exact Functions;
5. verify scope and limitations;
6. only then combine it with other Devices.

For Verse:

1. retrieve the language rule from `12`-`14`;
2. retrieve UEFN integration from `09`;
3. retrieve the exact API page for imports and signatures;
4. check feature status;
5. check compiler evidence before claiming compilation.

For community evidence:

1. retrieve the normalized record or source map from `19`;
2. identify the source URL, page title, revision or retrieval date when available;
3. confirm environment classification and the relevant official owner;
4. preserve the `S0`-`S4` confidence level, `verification_status`, and `agent_use` value;
5. use the relevant official owner for every technical contract;
6. route conflicts, stale claims, and unresolved availability to Quarantine or exclude them from active instructions.

### Step 5 — Check Freshness

Use `01_EPIC_GAMES_DOCUMENTATION_INDEX.md` and current official sources for release status, UI labels, API changes, feature gates, creator eligibility, publishing rules, limits, and moderation requirements. For community evidence, also check the source page, retrieval date, revision context, staleness state, and normalized record in `19`.

### Step 6 — Compose Without Blending Boundaries

Label each environment and evidence level. Explicitly label community-maintained information as community evidence. Do not merge Creative-only, non-Verse UEFN, Verse, and community claims into one undifferentiated workflow.

### Step 7 — End with Verification

End practical answers with the smallest valid Play Mode, launched-session, compiler, multiplayer, memory, or Private Version test. A community-reported behavior that has not reached `S3` or `S4` must end with a controlled verification step.

---

## Common Question Router

| User intent | Primary owner | Secondary support |
|---|---|---|
| Resolve environment or route a mixed question | `00` | Relevant domain owner |
| Find an official page or verify a version-sensitive claim | `01` | Current official source |
| Translate or standardize a professional term | `02` | `15`-`17` for definitions; `19` only for labeled community aliases |
| Creative installation, interface, modes, Content menu, Phone Tool, controls | `03` | `15`, then `18` |
| Creative world building, Prefabs, Galleries, routes, spatial design | `04` | `15`, `18`; `19` for visual mapping, asset composition, and community tags |
| Creative Island Settings, players, teams, classes, spawn, rounds, score, game flow | `05` | `15`, `18`; `19` only for labeled community reports |
| Creative Devices, Direct Event Binding, HUD, feedback, NPC/AI, audio, VFX | `06` | `15`, `18`; `19` for discovery, combinations, visual mapping, and reported issues |
| Creative complete examples, debugging, playtesting, production, publishing | `07` | `18`; `19` for community patterns and provisional workarounds |
| UEFN editor, projects, assets, materials, Landscape, lighting, Streaming, HLOD | `08` | `16` |
| UEFN Device gameplay and Direct Event Binding without Verse | `09` non-Verse layer | `08`, `10`, `16` |
| UEFN non-Verse HUD, UMG, NPC, Conversation, cinematics, audio, VFX | `09` non-Verse layer | `10`, `16` |
| Verse-authored Device, `@editable`, Device events, players, teams, Verse UI, custom NPC behavior | `09` Verse layer | `12`-`14`, `17` |
| UEFN sessions, validation, testing, memory, profiling, Lore, Private Versions, publishing | `10` | `16`, `17` |
| Game design, social mechanics, learning, ethics, research-based playtesting | `11` | `18` |
| Verse values, variables, types, containers, functions, control flow, failure | `12` | `17`, current reference |
| Verse modules, classes, structs, enums, interfaces, attributes, subscriptions, state architecture | `13` | `17`, current reference |
| Verse effects, concurrency, tasks, cancellation, persistence, schema evolution | `14` | `17`, current reference |
| Define a Creative term | `15` | `03`-`07` for implementation |
| Define a UEFN term | `16` | `08`-`10` for implementation |
| Define a Verse term or API concept | `17` | `09`, `12`-`14` for implementation |
| Adapted teaching sequence, planning card, nonviolent Creative pattern, troubleshooting prompt | `18` | Technical claims return to `03`-`07`; community ideas may be discovered through `19` |
| Find a Fortnite Wiki page, community alias, source category, or normalized community record | `19` | Relevant official owner for technical verification |
| Identify community-reported Prefab or Gallery contents, footprint, theme, or visual reference | `19` | `04` and `15`; availability must be verified officially |
| Evaluate a community Device combination, bug, edge case, workaround, or undocumented behavior | Relevant owner in `05`-`10` | `19` as a labeled hypothesis or evidence record |
| Use a community Memory value | Relevant Creative or UEFN technical owner | `19` as provisional evidence; current Memory calculation is authoritative |

---

## Knowledge Base Map

| ID | Canonical file | Primary role | Authority |
|---:|---|---|---|
| `00` | `00_MASTER_KNOWLEDGE_INDEX.md` | Routing, authority, evidence, composition, maintenance | Corpus governance |
| `01` | `01_EPIC_GAMES_DOCUMENTATION_INDEX.md` | Official links, URL registry, freshness routing | Official index; not detailed implementation |
| `02` | `02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md` | Canonical English-Hebrew naming | Terminology authority |
| `03` | `03_FORTNITE_CREATIVE_FOUNDATIONS_AND_INTERFACE.md` | Creative foundations and interface | 7 official units |
| `04` | `04_FORTNITE_CREATIVE_WORLD_BUILDING_AND_LEVEL_DESIGN.md` | Creative world and level design | 36 official units |
| `05` | `05_FORTNITE_CREATIVE_ISLAND_RULES_PLAYERS_AND_GAME_FLOW.md` | Creative Island Settings, players, rounds, score, flow | 59 official units |
| `06` | `06_FORTNITE_CREATIVE_DEVICES_SYSTEMS_NPC_AI_AND_FEEDBACK.md` | Creative Devices, NPC/AI, HUD, audio, VFX | 63 official units |
| `07` | `07_FORTNITE_CREATIVE_GAMEPLAY_PATTERNS_COMPLETE_EXAMPLES_AND_PRODUCTION.md` | Creative examples, testing, production, publishing | 57 official units |
| `08` | `08_UEFN_EDITOR_PROJECTS_ASSETS_AND_WORLD_BUILDING.md` | UEFN editor, projects, assets, world authoring | Verified UEFN editor owner |
| `09` | `09_UEFN_GAMEPLAY_VERSE_API_UI_NPC_AND_CINEMATICS.md` | UEFN gameplay, non-Verse systems, UI, NPC, Conversation, cinematics, separated Verse integration | Verified UEFN gameplay owner |
| `10` | `10_UEFN_TESTING_OPTIMIZATION_COLLABORATION_AND_PUBLISHING.md` | UEFN sessions, testing, optimization, Lore, publishing | Verified UEFN production owner |
| `11` | `11_GAME_DESIGN_SOCIAL_MECHANICS_RESEARCH_AND_LEARNING.md` | Design, social mechanics, research, learning, ethics | Design authority; not platform authority |
| `12` | `12_BOOK_OF_VERSE_I_FOUNDATIONS.md` | Shipping Verse foundations | Verified shipping Verse foundations |
| `13` | `13_BOOK_OF_VERSE_II_PROGRAM_STRUCTURE_AND_TYPES.md` | Shipping Verse architecture and state organization | Verified shipping Verse architecture |
| `14` | `14_BOOK_OF_VERSE_III_EFFECTS_CONCURRENCY_AND_EVOLUTION.md` | Effects, concurrency, cancellation, persistence, evolution | Verified advanced Verse owner |
| `15` | `15_GLOSSARY_FORTNITE_CREATIVE.md` | Creative definitions | Glossary; not Device-option authority |
| `16` | `16_GLOSSARY_UEFN.md` | UEFN definitions and current/legacy routing | Glossary; not editor-field authority |
| `17` | `17_GLOSSARY_VERSE.md` | Verse definitions, statuses, practical-owner routing | Glossary; not API-signature authority |
| `18` | `18_SUPPLEMENTAL_EDUCATIONAL_SOCIAL_TROUBLESHOOTING_AND_REFERENCE_KNOWLEDGE.md` | Adapted teaching, planning, patterns, troubleshooting | Secondary support only |
| `19` | `19_FORTNITE_COMMUNITY_WIKI_CREATIVE_DELTA_INDEX.md` | Community source map, normalized metadata schemas, visual and asset discovery, aliases, tags, combinations, and reported behavior | Secondary community evidence; never official implementation authority |

### Canonical Filename Compatibility Rule

The filenames of `12`, `13`, and `14` retain `BOOK_OF_VERSE` for compatibility. Their active content has been rebuilt and verified against current official Verse and UEFN documentation. Raw Book of Verse material is not automatically shipping guidance.

---

## Primary Ownership Matrix

| Knowledge area | Primary owner | Secondary support | Must not be used as primary proof |
|---|---|---|---|
| Corpus routing and authority | `00` | All owner files | A glossary |
| Official URL and freshness lookup | `01` | Current Epic source | Old embedded URL alone |
| English-Hebrew canonical term | `02` | `15`-`17`; `19` for labeled community aliases only | Free translation or community alias as canonical terminology |
| Creative first-use workflow | `03` | `15`, `18` | UEFN UI instructions |
| Creative environment and route design | `04` | `15`, `18`, `19` for visual and asset metadata | UEFN Landscape workflow or community metadata as availability proof |
| Creative game rules and lifecycle | `05` | `15`, `18` | Verse custom state |
| Creative Device behavior | Named unit in `05`-`07` | `15`, `18`; `19` for community combinations and reports | Another Device's table or a Wiki claim as a Device contract |
| Creative full recipe and production | `07` | `18` | UEFN publishing workflow |
| UEFN editor and assets | `08` | `16` | Creative Phone Tool |
| UEFN non-Verse gameplay | `09` non-Verse layer | `08`, `10`, `16` | Verse presented as Device-only |
| Verse integration with UEFN | `09` Verse layer | `12`-`14`, `17` | Language semantics alone |
| UEFN production lifecycle | `10` | `16`, `17` | Editor-only test |
| Game design and pedagogy | `11` | `18` | Device page as research evidence |
| Verse foundations | `12` | `17` | Raw language-main content |
| Verse architecture | `13` | `17` | Tutorial-specific structure |
| Verse effects/concurrency/persistence | `14` | `17` | Pseudocode as shipping code |
| Creative definitions | `15` | `03`-`07` | Glossary as exact configuration |
| UEFN definitions | `16` | `08`-`10` | Glossary as exact editor workflow |
| Verse definitions | `17` | `09`, `12`-`14` | Glossary as exact API signature |
| Adapted educational support | `18` | `11`, then technical owner | Adapted example as platform contract |
| Community source discovery and normalized metadata | `19` | Relevant technical owner | Community page as official product authority |
| Technical behavior mentioned by a community source | Relevant owner in `03`-`10`, `12`-`14` | `19` as labeled supporting evidence | `19` alone as proof of options, Events, Functions, limits, availability, or runtime behavior |

---

## File 09 Split-Ownership Boundary

File `09` serves both UEFN without Verse and UEFN with Verse. It must be read as two explicitly separated layers.

### Non-Verse Layer

The sections before `# Verse Integration Boundary` own:

- Fortnite Devices in UEFN;
- Direct Event Binding;
- Device-based state;
- Island Settings integration;
- non-Verse HUD and UMG;
- NPC Spawner and NPC Character Definitions using supported default behavior;
- authored Conversations and supported non-code Persona workflows;
- Sequencer, Level Sequences, Control Rig, and Cinematic Sequence Device;
- audio, VFX, and Niagara;
- multiplayer, reset, and Join in Progress for Device-driven systems;
- Device-only limitations and closest verified approximations.

### Verse Layer

The sections after `# Verse Integration Boundary` own:

- creation and placement of Verse-authored Devices;
- `Build Verse Code`;
- `creative_device`;
- `@editable`;
- Device API integration;
- subscriptions and callable functions;
- players, teams, and custom game state;
- Verse UI;
- custom NPC behavior;
- Scene Graph integration with Verse;
- lifecycle, respawn, reset, Join in Progress;
- compile-time and runtime troubleshooting.

### Boundary Rule

When the user requests UEFN without Verse, stop before the Verse boundary, state exact limitations, and do not insert Verse code.

When the user requests UEFN with Verse, use `09` for integration, `12`-`14` for language and architecture, `10` for production testing, and `17` only for definitions.

---

## Environment Boundaries

### Fortnite Creative

Creative is the in-game authoring environment. It uses Create Mode, Play Mode, Phone Tool, Content menu, Island Settings, Prefabs, Galleries, Devices, Direct Event Binding, and in-client testing.

Do not assume access to UEFN Viewport, Outliner, Details, Content Browser asset import, UMG authoring, Sequencer authoring, Scene Graph authoring, Verse Explorer, or custom Verse code.

### UEFN Without Verse

UEFN without Verse uses editor tools, projects, assets, Fortnite Devices, Direct Event Binding, UMG, supported NPC and Conversation tools, Sequencer, Control Rig, audio, VFX, sessions, validation, profiling, Lore, and publishing.

It does not provide arbitrary custom variables, algorithms, data structures, async orchestration, persistence schemas, or custom NPC state machines unless a supported system explicitly provides them.

### UEFN With Verse

UEFN with Verse includes the full UEFN project context plus custom Verse logic.

Use `08` for project/world context, `09` for Fortnite integration, `10` for production, `12` for foundations, `13` for architecture, `14` for advanced behavior and persistence, and `17` for definitions.

Verse does not remove the need to configure Island Settings, place Devices, assign `@editable` references, launch sessions, test multiplayer, calculate memory, validate content, or test a Private Version.

### Creator Portal Boundary

Creator Portal is a publishing and operations surface, not an authoring environment.

- route Creative publishing to `07`;
- route UEFN publishing to `10`;
- verify current rules through `01` and official pages;
- do not infer publishing eligibility from project quality or creator-code status.

---

## Device and API Isolation Rules

### One Device, One Contract

Every Device page is an isolated contract. An option, Event, Function, default, limitation, payload, or scope belongs only to the named Device unless Epic explicitly documents shared behavior.

Never transfer an Event from one Device to another, rename a Function from memory, combine adjacent extracted tables, assume Creative and UEFN expose identical authoring directions, or treat a glossary definition as a Device contract.

### Retrieve Before Combining

For a multi-Device solution:

1. retrieve each Device separately;
2. verify exact names and values;
3. identify the state owner;
4. write every connection as `Source Device — Exact Event -> Receiving Device — Exact Function`;
5. define feedback, failure, reset, and Join in Progress;
6. run a controlled test.

### Direct Event Binding Direction

Creative and UEFN can present binding workflows differently. Use the workflow documented for the current environment. Do not transpose Creative UI instructions into UEFN.

### API Isolation

For Verse APIs:

- use exact capitalization, module paths, parameter types, and return types;
- distinguish `()` from failable `[]` calls;
- distinguish `agent`, `player`, `fort_character`, NPC, entity, and component types;
- do not invent helper functions or imports;
- do not copy internal/generated attributes into creator code;
- do not edit generated digest files.

---

## Verse Code and Feature Policy

### Required Status Labels

Every Verse code example must use one of:

- `SHIPPING — COMPILED`
- `SHIPPING — OFFICIAL-DOCUMENTATION-VERIFIED`
- `BETA OR EXPERIMENTAL`
- `UNRELEASED — REFERENCE ONLY`
- `CONCEPTUAL — NOT COPY-PASTE`

### Status Meanings

- **SHIPPING — COMPILED:** exact code built with `Verse > Build Verse Code`, with release recorded. Compilation does not prove runtime behavior.
- **SHIPPING — OFFICIAL-DOCUMENTATION-VERIFIED:** syntax, imports, API identifiers, and workflow are supported, but exact code was not compiled here.
- **BETA OR EXPERIMENTAL:** feature is gated; compatibility and publishability must be checked.
- **UNRELEASED — REFERENCE ONLY:** language-main, internal, future-facing, or unavailable material; not executable UEFN guidance.
- **CONCEPTUAL — NOT COPY-PASTE:** pseudocode, architecture, placeholders, or intentionally incomplete examples.

### Required Elements in a Ready Verse Answer

1. environment: UEFN with Verse;
2. imports;
3. Devices/assets to place;
4. `@editable` fields and assignments;
5. complete code;
6. status label;
7. player/team/global ownership;
8. respawn behavior;
9. round/game reset behavior;
10. Join in Progress behavior;
11. compilation steps;
12. launched-session test;
13. multiplayer test where relevant;
14. known limitations.

### Current v41.30 Feature Gates

| Feature | Corpus treatment |
|---|---|
| Scene Graph | Beta; recovery and publishability gate required |
| Scene Graph experimental subfeatures | Experimental where labeled |
| Authored Conversations | Exited Experimental in v41.30; exact capabilities remain version-sensitive |
| LLM Conversations and Personas | Version-, policy-, moderation-, and publishing-sensitive |
| In-world UMG Widgets | Available from v41.20; verify current workflow |
| Quest/Progression APIs | Experimental unless current API proves otherwise |
| PointerZoom | Experimental in v41.30 |
| Direct Verse widget construction | Officially documented legacy workflow |
| Live Variables, `var live`, `when` | Unreleased — reference only |
| Experimental Verse APIs | Not shipping/publishable without current confirmation |

---

## Lifecycle and Multiplayer Requirements

A system is incomplete until its lifecycle is defined.

### Required Lifecycle Questions

For each state or mechanic, identify:

- owner;
- initial state;
- initialization moment;
- writer and reader;
- visible feedback;
- respawn behavior;
- player-exit behavior;
- Join in Progress behavior;
- round-reset behavior;
- game-reset behavior;
- persistence behavior;
- duplicate-activation behavior;
- simultaneous-player behavior.

### State Ownership Categories

| Scope | Typical examples |
|---|---|
| Per-player | personal UI, player map entry, individual Tracker |
| Per-team | shared score, team objective, team route |
| Global/island | Barrier state, global cinematic result, island phase |
| Device instance | subscription, timer, local mutable field |
| NPC instance | target, behavior task, spawned-character state |
| Conversation instance | dialogue state and emitted Conversation Events |
| Persistent player data | supported persistence or `weak_map(player, persistable_type)` |
| Round/session | temporary state rebuilt after reset |

### Minimum Runtime Test Matrix

Test the relevant subset of:

1. solo;
2. minimum supported players;
3. maximum practical players;
4. first and repeated activation;
5. simultaneous activation;
6. respawn before and after completion;
7. player exit during the mechanic;
8. Join in Progress before and after completion;
9. round end and next round;
10. game end and new game;
11. long session;
12. Private Version;
13. target platform and input method.

### Reset Distinctions

Do not treat Device disable/enable, respawn, objective reset, round reset, game reset, session relaunch, and persistent-data reset as equivalent. State which reset is implemented and tested.

---

## Terminology and Naming Policy

### Canonical Naming Sources

- use `02` for approved English-Hebrew naming;
- use `15` for Creative definitions;
- use `16` for UEFN definitions;
- use `17` for Verse definitions.

### Terms That Normally Remain in English

Keep searchable terms unchanged unless a short Hebrew explanation is useful: Fortnite Creative, UEFN, Verse, Epic Games, Device names, Event names, Function names, property names, APIs, code, UI paths, UMG, HLOD, Scene Graph, Fab, Creator Portal, IARC, NPC, HUD, and VFX.

### Exact UI and Code Rule

Use the exact English label first. Do not translate code identifiers, import paths, class names, API signatures, Device Events, Device Functions, or searchable menu paths.

### Legacy Naming

Use the current name first, mention the former name only for retrieval, and label it legacy. Example: **Lore Version Control** is current; **Unreal Revision Control** is legacy/contextual.

---

## Design, Education, and Recommendation Policy

Use `11` for player experience, core loop, social mechanics, cooperation, fairness, learning design, ethics, and research-based playtesting.

Use `18` for practical teaching sequences, planning templates, adapted nonviolent Creative patterns, workshop support, and troubleshooting order.

Technical implementation must return to the relevant owner file.

### Nonviolent Default

Official combat-oriented pages may remain as technical references. User-facing recommendations should default to puzzles, collection, exploration, parkour, racing, cooperation, social interaction, environmental transformation, narrative discovery, repair, and recovery.

Separate factual capability from the recommended design.

### Sensitive Social Topics

Do not recommend mechanics that reenact bullying, humiliation, abuse, discrimination, or trauma as entertainment; force personal disclosure; publicly label learners; or claim lasting empathy without evidence.

Prefer fictional or metaphorical contexts, safe recovery, private feedback, and observable in-game outcomes.

---

## Community Evidence and Delta Policy

File `19_FORTNITE_COMMUNITY_WIKI_CREATIVE_DELTA_INDEX.md` is an active but secondary corpus layer. Its purpose is to preserve useful community delta without mirroring or replacing official Epic documentation.

### Permitted Uses

Use `19` for:

- Fortnite Wiki page and category discovery;
- visual identification of Devices, Prefabs, Galleries, and assets;
- reported Prefab and Gallery composition;
- community aliases, genre labels, and thematic tags;
- common community Device combinations;
- community-reported bugs, edge cases, and workarounds;
- provisional Memory reports with recorded cost type and context;
- identifying gaps or conflicts that require official verification or a controlled test;
- Wiki-specific source attribution, licensing notes, and retrieval metadata.

### Prohibited Uses

Do not use `19` as primary proof for:

- exact Device options, values, Events, or Functions;
- Direct Event Binding contracts;
- Island Settings, UI paths, input bindings, or current feature availability;
- current limits, eligibility, moderation, or publishing rules;
- Verse language rules, imports, classes, functions, or API signatures;
- current Memory validation requirements;
- guaranteed runtime, multiplayer, reset, or Join in Progress behavior.

### Required Community Record Fields

A community claim used in an answer should preserve, when applicable:

- source page and URL;
- page title, revision, or retrieval date;
- entity type and stable `entity_id`;
- environment classification;
- relevant official owner;
- `S0`-`S4` confidence level;
- `verification_status`;
- `agent_use` status;
- delta summary;
- allowed and prohibited use;
- test conditions or official comparison when available.

### Community Confidence Rule

- `S0`-`S2` claims are discovery material or hypotheses only.
- `S3` claims may be described as tested only within the recorded build, date, environment, and conditions.
- `S4` claims match a current Epic source, but the Epic source remains the authority for the technical fact.
- `official_conflict`, `stale_or_legacy`, `unresolved`, and `rejected` claims must not become active implementation instructions.

When confidence is below `S3`, use explicit wording such as “Fortnite Wiki reports” or “Community-maintained metadata indicates,” state that the behavior is not confirmed by current Epic documentation, and require a Play Mode or launched-session test.

### Community Memory Rule

Every community Memory value is provisional. Preserve whether it is initial placement cost, additional-instance cost, per-copy cost, contextual cost, or total reported cost. Current project Memory calculation and validation outrank the reported value.

### Community Licensing and Attribution Rule

Preserve page-level attribution and source URLs. Do not copy or redistribute media unless its page-level license has been checked separately. A general Fandom or Wiki license statement is not sufficient proof for every image or file.

---

## Conflict Resolution Rules

### Official Source versus Corpus

Current official evidence wins. Label outdated corpus wording until it is corrected.

### Community Source versus Official or Runtime Evidence

Current Epic documentation, the active product, the relevant technical owner, compiler output, validation, and recorded runtime evidence always outrank Fortnite Wiki or another community source. Preserve the community item only as a labeled discrepancy, hypothesis, historical note, or Quarantine record.

### Community Metadata versus Technical Owner

File `19` owns the community source map, metadata schema, aliases, tags, and evidence record. The relevant technical file owns product capability and implementation. Never convert a visual description, Infobox, table, category membership, or community walkthrough into an exact technical contract without official verification.

### Canonical Unit versus Adjacent Unit

The named unit wins for its own Device or page. Do not transfer data across units.

### UEFN Non-Verse versus Verse

If the user requested no Verse, preserve the Device/editor boundary. If exact behavior requires Verse, state the limitation rather than silently switching environments.

### Glossary versus Owner File

The owner file wins for implementation. The glossary supplies definitions only.

### `11` or `18` versus Technical Owner

The technical owner wins for capability. Preserve the design goal and use the nearest verified implementation.

### Book of Verse versus Shipping Reference

Current Verse Language Reference, API Reference, release notes, and compiler win.

### Documentation versus Runtime

When runtime contradicts documentation, verify configuration, reproduce minimally, record the release, inspect compiler/log/validation evidence, and describe the discrepancy without inventing a general contract.

### Two Current Sources Disagree

Prefer the more specific source: exact Device page over overview, exact API member over tutorial, current release note over older guide, creator rule over secondary explanation, and tested current UI over a stale screenshot.

---

## Answer Construction Standards

### General Output Order

1. environment;
2. result or architecture;
3. required tools, Devices, or assets;
4. exact setup;
5. connections or code;
6. state ownership;
7. lifecycle and multiplayer behavior;
8. verification;
9. limitations;
10. evidence labels and community attribution when community material was used.

### Creative Answer Standard

Include Creative-only confirmation, required Devices, relevant Island Settings, exact Direct Event Binding, scope, feedback, reset, and Play Mode test. Do not include UEFN-only or Verse-only tools.

### UEFN Without Verse Answer Standard

Include UEFN without Verse confirmation, editor assets, placed Devices, exact `User Options`, Direct Event Binding, supported UMG/NPC/Conversation/cinematic workflow, explicit limitations, launched-session test, multiplayer/reset/JIP notes, and no custom Verse code.

### UEFN With Verse Answer Standard

Follow the required Verse elements and separate editor configuration, code, API evidence, state ownership, and runtime evidence.

### Troubleshooting Standard

Confirm environment and build, reproduce, define expected behavior, find the first divergence, inspect the exact source, change one variable, rerun the same test, and record the result. Do not recommend random changes or arbitrary `Sleep()` delays.

### Community Evidence Answer Standard

When `19` materially contributes to an answer:

1. identify the information as community-maintained;
2. state its confidence and verification status when relevant;
3. pair every technical instruction with the relevant official owner;
4. do not derive exact options, Events, Functions, limits, or availability from the Wiki;
5. label Memory values and reported behavior as provisional unless officially matched;
6. include source attribution when the community claim is cited;
7. end undocumented-behavior guidance with a controlled Play Mode or launched-session test.

### Source Anomaly Rule

Never convert truncated text, malformed extraction, missing headers, duplicate headings, HTML entities, placeholders, obsolete screenshots, internal tests, or references to removed files into instructions without verification.

---

## Active Upload Set

The active corpus consists only of these canonical knowledge files:

1. `00_MASTER_KNOWLEDGE_INDEX.md`
2. `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`
3. `02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md`
4. `03_FORTNITE_CREATIVE_FOUNDATIONS_AND_INTERFACE.md`
5. `04_FORTNITE_CREATIVE_WORLD_BUILDING_AND_LEVEL_DESIGN.md`
6. `05_FORTNITE_CREATIVE_ISLAND_RULES_PLAYERS_AND_GAME_FLOW.md`
7. `06_FORTNITE_CREATIVE_DEVICES_SYSTEMS_NPC_AI_AND_FEEDBACK.md`
8. `07_FORTNITE_CREATIVE_GAMEPLAY_PATTERNS_COMPLETE_EXAMPLES_AND_PRODUCTION.md`
9. `08_UEFN_EDITOR_PROJECTS_ASSETS_AND_WORLD_BUILDING.md`
10. `09_UEFN_GAMEPLAY_VERSE_API_UI_NPC_AND_CINEMATICS.md`
11. `10_UEFN_TESTING_OPTIMIZATION_COLLABORATION_AND_PUBLISHING.md`
12. `11_GAME_DESIGN_SOCIAL_MECHANICS_RESEARCH_AND_LEARNING.md`
13. `12_BOOK_OF_VERSE_I_FOUNDATIONS.md`
14. `13_BOOK_OF_VERSE_II_PROGRAM_STRUCTURE_AND_TYPES.md`
15. `14_BOOK_OF_VERSE_III_EFFECTS_CONCURRENCY_AND_EVOLUTION.md`
16. `15_GLOSSARY_FORTNITE_CREATIVE.md`
17. `16_GLOSSARY_UEFN.md`
18. `17_GLOSSARY_VERSE.md`
19. `18_SUPPLEMENTAL_EDUCATIONAL_SOCIAL_TROUBLESHOOTING_AND_REFERENCE_KNOWLEDGE.md`
20. `19_FORTNITE_COMMUNITY_WIKI_CREATIVE_DELTA_INDEX.md`

### Document Identity

Document identity is determined by `document_id`, canonical title, and canonical filename. Upload suffixes such as `(1)`, `(2)`, or `(4)` are not part of the canonical identity. The active corpus should contain one current file per Document ID.

### Replacement Rule

When replacing a file, remove or archive the previous active version, upload the new version under the canonical filename, confirm its `document_id`, avoid duplicate active versions, and run regression checks.

---

## Excluded and Delivery-Only Files

Do not include QA reports, changelogs, audit reports, manifests, quarantine files, backups, duplicate batches, source dumps, obsolete drafts, temporary exports, compiler logs, delivery checklists, administrative program documents, schedules, proposals, fundraising, or marketing wrappers in active agent knowledge.

Examples of delivery-only files:

- `VERSE_CORPUS_CHANGELOG.md`
- `VERSE_CORPUS_QA_FINAL.md`
- `UEFN_WITHOUT_VERSE_CHANGELOG.md`

Their conclusions may guide maintenance, but active files must not depend on them.

---

## Maintenance and Regression Rules

### Ownership Integrity

- preserve one primary owner per claim;
- use cross-links instead of duplicate explanations;
- update `00` whenever ownership changes;
- update `01` whenever official routing or source status changes;
- update `02` and the relevant glossary when terminology changes;
- update `19` when community source structure, aliases, entity schemas, or normalized delta records change, without altering official ownership.

### Creative Canonical Unit Integrity

Keep the 222 official Creative canonical units intact:

`7 + 36 + 59 + 63 + 57 = 222`

Do not merge adjacent Device units into one contract.

### Community Evidence Integrity

After changing `19`:

- verify every active record has a source URL, entity type, environment classification, official owner, confidence level, verification status, and allowed agent use;
- verify community-derived options, values, Events, Functions, limits, availability, and publishing claims are not presented as official facts;
- verify `official_conflict`, stale, legacy, unresolved, rejected, removed, restricted, and developer-only claims are quarantined or excluded;
- verify every community Memory value is labeled provisional and records its cost type and context;
- verify community media is not copied without page-level license review;
- verify `19` does not duplicate full official Device contracts or override files `03`-`10` and `12`-`17`.

### Verse Integrity

After changing `09`, `12`, `13`, `14`, or `17`:

- verify every code block has a status label;
- verify imports, Events, Functions, `()` versus `[]`, and effects;
- verify feature status;
- verify no language-main-only construct appears as shipping;
- verify no code is labeled compiled without compiler evidence.

### Structural QA

Run canonical filename, Document ID, duplicate-file, heading, anchor, internal-link, code-fence, known-bad-phrase, removed-file dependency, Creative unit count, environment leakage, glossary-routing, community-record schema, source-attribution, confidence-status, and Quarantine checks.

### Known-Bad Content Checks

Search for:

- references to inactive QA, manifest, or quarantine files;
- duplicate upload suffixes in canonical links;
- `var live` or `when` in shipping Verse examples;
- invented API members;
- legacy channel workflows presented as current Direct Event Binding;
- Unreal Revision Control presented as current;
- Scene Graph presented as stable without a Beta gate;
- authored Conversations presented as Experimental after v41.30;
- glossary definitions used as configuration proof;
- code labeled compiled without a recorded compiler run;
- community claims presented without a source, confidence, verification status, or environment classification;
- a Wiki category, Infobox, table, screenshot, or visual description used as proof of technical availability;
- community Memory values presented as current validated Memory;
- community media copied without page-level license review.

### Regression Question Families

Re-run difficult questions covering Creative lock-and-key progression, Creative dialogue approximation, UEFN authored Conversations, non-Verse UI, the Verse boundary, complete Verse Devices, per-player/global state, respawn/JIP, concurrency cancellation, persistence migration, memory versus runtime performance, Lore conflicts, Private Versions, terminology, sensitive social design, community aliases, Prefab/Gallery visual mapping, community-reported combinations, provisional Memory values, and conflicts between Wiki claims and current Epic evidence.

### Version Update Procedure

When Epic releases a new ecosystem version:

1. read release notes;
2. identify affected owner files;
3. update `01`;
4. update feature gates in `08`-`10`, `12`-`14`, `16`, and `17`;
5. review affected `19` records for renamed entities, changed availability, official matches, official conflicts, and staleness;
6. verify API changes;
7. rebuild Verse examples where possible;
8. run runtime and production tests;
9. update `verified_release` metadata;
10. update this file only after ownership and status are stable.

---

## Final Corpus Replacement Gate

The corpus is ready for active use only when:

- exactly one active file exists for every Document ID `00`-`19`;
- every file uses its canonical identity;
- `00` matches actual ownership;
- `01` contains current official routing;
- `02` matches current terminology;
- `03`-`07` contain 222 Creative canonical units;
- `08`-`10` preserve the UEFN without Verse layer;
- `09` has a clear Verse Integration Boundary;
- `12`-`14` use current shipping Verse rules;
- `15`-`17` remain glossaries rather than implementation owners;
- `18` remains secondary adapted guidance;
- `19` remains secondary community discovery and metadata evidence;
- `19` contains no unsupported technical contracts and does not override official owners;
- every active community record has source, environment, owner, confidence, verification, and agent-use metadata;
- community Memory values and undocumented behaviors remain explicitly provisional unless tested or officially matched;
- no active file depends on QA, changelog, manifest, quarantine, backup, or obsolete files;
- internal links resolve;
- headings and anchors are unique;
- code fences are closed;
- Verse examples are status-labeled;
- experimental and unreleased features are gated;
- environment separation is preserved;
- regression questions pass;
- the previous corpus remains archived outside active retrieval until replacement is confirmed.

## Related Documents

- [01_EPIC_GAMES_DOCUMENTATION_INDEX.md](01_EPIC_GAMES_DOCUMENTATION_INDEX.md)
- [02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md](02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md)
- [03_FORTNITE_CREATIVE_FOUNDATIONS_AND_INTERFACE.md](03_FORTNITE_CREATIVE_FOUNDATIONS_AND_INTERFACE.md)
- [04_FORTNITE_CREATIVE_WORLD_BUILDING_AND_LEVEL_DESIGN.md](04_FORTNITE_CREATIVE_WORLD_BUILDING_AND_LEVEL_DESIGN.md)
- [05_FORTNITE_CREATIVE_ISLAND_RULES_PLAYERS_AND_GAME_FLOW.md](05_FORTNITE_CREATIVE_ISLAND_RULES_PLAYERS_AND_GAME_FLOW.md)
- [06_FORTNITE_CREATIVE_DEVICES_SYSTEMS_NPC_AI_AND_FEEDBACK.md](06_FORTNITE_CREATIVE_DEVICES_SYSTEMS_NPC_AI_AND_FEEDBACK.md)
- [07_FORTNITE_CREATIVE_GAMEPLAY_PATTERNS_COMPLETE_EXAMPLES_AND_PRODUCTION.md](07_FORTNITE_CREATIVE_GAMEPLAY_PATTERNS_COMPLETE_EXAMPLES_AND_PRODUCTION.md)
- [08_UEFN_EDITOR_PROJECTS_ASSETS_AND_WORLD_BUILDING.md](08_UEFN_EDITOR_PROJECTS_ASSETS_AND_WORLD_BUILDING.md)
- [09_UEFN_GAMEPLAY_VERSE_API_UI_NPC_AND_CINEMATICS.md](09_UEFN_GAMEPLAY_VERSE_API_UI_NPC_AND_CINEMATICS.md)
- [10_UEFN_TESTING_OPTIMIZATION_COLLABORATION_AND_PUBLISHING.md](10_UEFN_TESTING_OPTIMIZATION_COLLABORATION_AND_PUBLISHING.md)
- [11_GAME_DESIGN_SOCIAL_MECHANICS_RESEARCH_AND_LEARNING.md](11_GAME_DESIGN_SOCIAL_MECHANICS_RESEARCH_AND_LEARNING.md)
- [12_BOOK_OF_VERSE_I_FOUNDATIONS.md](12_BOOK_OF_VERSE_I_FOUNDATIONS.md)
- [13_BOOK_OF_VERSE_II_PROGRAM_STRUCTURE_AND_TYPES.md](13_BOOK_OF_VERSE_II_PROGRAM_STRUCTURE_AND_TYPES.md)
- [14_BOOK_OF_VERSE_III_EFFECTS_CONCURRENCY_AND_EVOLUTION.md](14_BOOK_OF_VERSE_III_EFFECTS_CONCURRENCY_AND_EVOLUTION.md)
- [15_GLOSSARY_FORTNITE_CREATIVE.md](15_GLOSSARY_FORTNITE_CREATIVE.md)
- [16_GLOSSARY_UEFN.md](16_GLOSSARY_UEFN.md)
- [17_GLOSSARY_VERSE.md](17_GLOSSARY_VERSE.md)
- [18_SUPPLEMENTAL_EDUCATIONAL_SOCIAL_TROUBLESHOOTING_AND_REFERENCE_KNOWLEDGE.md](18_SUPPLEMENTAL_EDUCATIONAL_SOCIAL_TROUBLESHOOTING_AND_REFERENCE_KNOWLEDGE.md)
- [19_FORTNITE_COMMUNITY_WIKI_CREATIVE_DELTA_INDEX.md](19_FORTNITE_COMMUNITY_WIKI_CREATIVE_DELTA_INDEX.md)
