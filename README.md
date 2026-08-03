# Fortnite Ecosystem Knowledge Architecture

**A structured, source-aware knowledge system for Fortnite Creative, UEFN, Verse, and AI-assisted development.**

[**Open AMIE: Fortnite Development Agent**](https://chatgpt.com/g/g-6a5f259f370081919ce0a30502915ce5-g-vnsy-hkhbr-shlk-bpvrtnyyt)

---

## Overview

**Fortnite Ecosystem Knowledge Architecture** is the knowledge foundation behind **AMIE: Fortnite Development Agent**. It organizes Fortnite development knowledge into a governed, navigable system designed to support accurate technical guidance, practical implementation, troubleshooting, learning, and AI retrieval.

The repository covers three distinct development environments:

1. **Fortnite Creative**
2. **UEFN without Verse**
3. **UEFN with Verse**

This separation is fundamental. A technically correct instruction can still fail when it is taken from the wrong environment, Device, API, release, or lifecycle state. The corpus therefore routes questions by environment, assigns primary ownership to each type of claim, distinguishes official knowledge from professional synthesis, and requires version-sensitive information to be checked against current Epic Games sources.

The repository currently contains **20 interconnected Markdown documents (`00–19`)**. Its Fortnite Creative layer preserves **222 official canonical knowledge units**, while the wider corpus adds verified UEFN workflows, Verse language knowledge, terminology systems, game-design research, educational frameworks, troubleshooting patterns, and a governed community-reference layer.

> The corpus metadata currently records technical verification through **Fortnite Ecosystem v41.30**. Individual documents define their own verification dates, authority boundaries, and evidence status.

---

## AMIE: Fortnite Development Agent

AMIE turns this repository from a static collection of documents into a practical development assistant.

It is designed to help users:

- identify whether a solution belongs to Fortnite Creative, UEFN without Verse, or UEFN with Verse;
- select the correct Devices, editor workflows, systems, or Verse concepts;
- avoid mixing Creative-only, UEFN-only, and Verse-dependent instructions;
- build complete gameplay flows rather than isolated Device lists;
- reason about player, team, and global state;
- account for rounds, resets, respawns, player exits, and Join in Progress;
- troubleshoot using structured tests instead of unsupported assumptions;
- distinguish official platform behavior from professional practice, inference, and community observations;
- translate technical knowledge into clear implementation steps;
- connect game development with learning design, social mechanics, accessibility, and educational use.

AMIE is intended to provide practical, grounded guidance—not to replace the current Epic Games documentation, the active editor interface, compiler output, runtime tests, validation, or publishing checks.

### Why AMIE is useful

General-purpose AI systems may combine information from different Fortnite environments, reuse outdated interface labels, invent unsupported Device settings, or present experimental Verse features as currently available.

AMIE is supported by a knowledge architecture built specifically to reduce those errors. The corpus provides:

- explicit environment routing;
- source-authority rules;
- Device and API isolation;
- technical ownership by document;
- exact English product and interface terminology;
- lifecycle-aware implementation guidance;
- evidence labels and testing boundaries;
- cross-links between design, implementation, testing, and publishing.

[**Try AMIE: Fortnite Development Agent →**](https://chatgpt.com/g/g-6a5f259f370081919ce0a30502915ce5-g-vnsy-hkhbr-shlk-bpvrtnyyt)

---

## Who This Repository Is For

The repository is primarily designed for:

1. **Fortnite developers** building in Creative, UEFN, or Verse;
2. **AI agent developers** creating retrieval systems, specialized assistants, or development-support agents;
3. **Educators** using Fortnite for learning, game-based education, professional development, and collaborative design.

It can also support game designers, instructional designers, researchers, curriculum developers, technical writers, workshop facilitators, and teams building structured Fortnite learning programs.

---

## Start Here

| Your goal | Recommended starting point |
|---|---|
| Understand how the complete corpus is governed and routed | [`00_MASTER_KNOWLEDGE_INDEX.md`](./00_MASTER_KNOWLEDGE_INDEX.md) |
| Find current Epic Games documentation and source URLs | [`01_EPIC_GAMES_DOCUMENTATION_INDEX.md`](./01_EPIC_GAMES_DOCUMENTATION_INDEX.md) |
| Standardize English and Hebrew terminology | [`02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md`](./02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md) |
| Build only in Fortnite Creative | [`03`](./03_FORTNITE_CREATIVE_FOUNDATIONS_AND_INTERFACE.md) through [`07`](./07_FORTNITE_CREATIVE_GAMEPLAY_PATTERNS_COMPLETE_EXAMPLES_AND_PRODUCTION.md) |
| Build in UEFN without Verse | [`08`](./08_UEFN_EDITOR_PROJECTS_ASSETS_AND_WORLD_BUILDING.md) through [`10`](./10_UEFN_TESTING_OPTIMIZATION_COLLABORATION_AND_PUBLISHING.md) |
| Learn or implement Verse | [`12`](./12_BOOK_OF_VERSE_I_FOUNDATIONS.md) through [`14`](./14_BOOK_OF_VERSE_III_EFFECTS_CONCURRENCY_AND_EVOLUTION.md), plus [`09`](./09_UEFN_GAMEPLAY_VERSE_API_UI_NPC_AND_CINEMATICS.md) |
| Design educational, cooperative, or social mechanics | [`11_GAME_DESIGN_SOCIAL_MECHANICS_RESEARCH_AND_LEARNING.md`](./11_GAME_DESIGN_SOCIAL_MECHANICS_RESEARCH_AND_LEARNING.md) |
| Find a detailed definition | [`15`](./15_GLOSSARY_FORTNITE_CREATIVE.md), [`16`](./16_GLOSSARY_UEFN.md), or [`17`](./17_GLOSSARY_VERSE.md) |
| Use teaching sequences and troubleshooting templates | [`18_SUPPLEMENTAL_EDUCATIONAL_SOCIAL_TROUBLESHOOTING_AND_REFERENCE_KNOWLEDGE.md`](./18_SUPPLEMENTAL_EDUCATIONAL_SOCIAL_TROUBLESHOOTING_AND_REFERENCE_KNOWLEDGE.md) |
| Discover community metadata and Creative references | [`19_FORTNITE_COMMUNITY_WIKI_CREATIVE_DELTA_INDEX.md`](./19_FORTNITE_COMMUNITY_WIKI_CREATIVE_DELTA_INDEX.md) |

---

## What Makes This Knowledge Architecture Different

This repository is not organized as a flat collection of tutorials. It is designed as an operational knowledge system.

### Environment-first routing

Every implementation question begins by identifying the working environment. Fortnite Creative, UEFN without Verse, and UEFN with Verse are treated as related but technically distinct systems.

### One primary owner per claim

Different documents own different types of knowledge. Editor authoring, Device gameplay, Verse semantics, testing, publishing, terminology, research, and community discovery are routed separately to reduce duplication and contradiction.

### Source-authority hierarchy

Current Epic Games documentation, active product behavior, compiler output, validation, and runtime evidence take priority over older or secondary material. Professional synthesis and educational guidance may extend official knowledge, but they do not override it.

### Device and API isolation

An option, Event, Function, property, API member, limit, or workflow is not transferred from one Device or environment to another unless an authoritative source explicitly supports that transfer.

### Lifecycle-aware design

The corpus treats game start, round start, respawn, reset, player exit, reconnect, and Join in Progress as part of implementation—not as optional edge cases.

### AI-retrieval readiness

Documents include routing rules, authority boundaries, exact terminology, topic indexes, cross-references, and explicit limitations. This structure helps an AI system retrieve the right evidence before composing an answer.

### Integrated technical and educational design

The repository connects platform knowledge with game design, social mechanics, learning theory, workshop planning, accessibility, ethics, playtesting, and reflective practice.

---

## Knowledge Architecture

### Governance, Sources, and Terminology

These files govern the complete corpus, point to official sources, and standardize language:

- [`00_MASTER_KNOWLEDGE_INDEX.md`](./00_MASTER_KNOWLEDGE_INDEX.md)
- [`01_EPIC_GAMES_DOCUMENTATION_INDEX.md`](./01_EPIC_GAMES_DOCUMENTATION_INDEX.md)
- [`02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md`](./02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md)

### Fortnite Creative

The Creative layer covers interface foundations, world building, Island Settings, player flow, Devices, feedback, examples, testing, and publishing:

- [`03_FORTNITE_CREATIVE_FOUNDATIONS_AND_INTERFACE.md`](./03_FORTNITE_CREATIVE_FOUNDATIONS_AND_INTERFACE.md)
- [`04_FORTNITE_CREATIVE_WORLD_BUILDING_AND_LEVEL_DESIGN.md`](./04_FORTNITE_CREATIVE_WORLD_BUILDING_AND_LEVEL_DESIGN.md)
- [`05_FORTNITE_CREATIVE_ISLAND_RULES_PLAYERS_AND_GAME_FLOW.md`](./05_FORTNITE_CREATIVE_ISLAND_RULES_PLAYERS_AND_GAME_FLOW.md)
- [`06_FORTNITE_CREATIVE_DEVICES_SYSTEMS_NPC_AI_AND_FEEDBACK.md`](./06_FORTNITE_CREATIVE_DEVICES_SYSTEMS_NPC_AI_AND_FEEDBACK.md)
- [`07_FORTNITE_CREATIVE_GAMEPLAY_PATTERNS_COMPLETE_EXAMPLES_AND_PRODUCTION.md`](./07_FORTNITE_CREATIVE_GAMEPLAY_PATTERNS_COMPLETE_EXAMPLES_AND_PRODUCTION.md)

### UEFN

The UEFN layer covers editor authoring, assets, world building, Device gameplay, UI, NPCs, conversations, cinematics, testing, optimization, collaboration, and publishing:

- [`08_UEFN_EDITOR_PROJECTS_ASSETS_AND_WORLD_BUILDING.md`](./08_UEFN_EDITOR_PROJECTS_ASSETS_AND_WORLD_BUILDING.md)
- [`09_UEFN_GAMEPLAY_VERSE_API_UI_NPC_AND_CINEMATICS.md`](./09_UEFN_GAMEPLAY_VERSE_API_UI_NPC_AND_CINEMATICS.md)
- [`10_UEFN_TESTING_OPTIMIZATION_COLLABORATION_AND_PUBLISHING.md`](./10_UEFN_TESTING_OPTIMIZATION_COLLABORATION_AND_PUBLISHING.md)

### Game Design, Learning, and Social Mechanics

- [`11_GAME_DESIGN_SOCIAL_MECHANICS_RESEARCH_AND_LEARNING.md`](./11_GAME_DESIGN_SOCIAL_MECHANICS_RESEARCH_AND_LEARNING.md)

### Verse

The Verse layer covers language foundations, program structure, types, effects, concurrency, persistence, and language evolution. Current Epic documentation and the active UEFN compiler remain authoritative for shipping availability.

- [`12_BOOK_OF_VERSE_I_FOUNDATIONS.md`](./12_BOOK_OF_VERSE_I_FOUNDATIONS.md)
- [`13_BOOK_OF_VERSE_II_PROGRAM_STRUCTURE_AND_TYPES.md`](./13_BOOK_OF_VERSE_II_PROGRAM_STRUCTURE_AND_TYPES.md)
- [`14_BOOK_OF_VERSE_III_EFFECTS_CONCURRENCY_AND_EVOLUTION.md`](./14_BOOK_OF_VERSE_III_EFFECTS_CONCURRENCY_AND_EVOLUTION.md)

### Glossaries

- [`15_GLOSSARY_FORTNITE_CREATIVE.md`](./15_GLOSSARY_FORTNITE_CREATIVE.md)
- [`16_GLOSSARY_UEFN.md`](./16_GLOSSARY_UEFN.md)
- [`17_GLOSSARY_VERSE.md`](./17_GLOSSARY_VERSE.md)

### Supplemental and Community Knowledge

- [`18_SUPPLEMENTAL_EDUCATIONAL_SOCIAL_TROUBLESHOOTING_AND_REFERENCE_KNOWLEDGE.md`](./18_SUPPLEMENTAL_EDUCATIONAL_SOCIAL_TROUBLESHOOTING_AND_REFERENCE_KNOWLEDGE.md)
- [`19_FORTNITE_COMMUNITY_WIKI_CREATIVE_DELTA_INDEX.md`](./19_FORTNITE_COMMUNITY_WIKI_CREATIVE_DELTA_INDEX.md)

---

## Complete Document Index

| ID | Document | Primary scope | Knowledge role |
|---:|---|---|---|
| `00` | [`Master Knowledge Index`](./00_MASTER_KNOWLEDGE_INDEX.md) | All environments | Governing entry point, routing, authority, ownership, and answer-construction rules |
| `01` | [`Epic Games Documentation Index`](./01_EPIC_GAMES_DOCUMENTATION_INDEX.md) | Creative, UEFN, Verse, Creator Portal | Searchable official-source and URL registry |
| `02` | [`English–Hebrew Terminology Index`](./02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md) | All environments | Canonical bilingual terminology and naming |
| `03` | [`Fortnite Creative: Foundations and Interface`](./03_FORTNITE_CREATIVE_FOUNDATIONS_AND_INTERFACE.md) | Fortnite Creative | Entry, interface, Create/Play modes, Phone Tool, controls, and first workflows |
| `04` | [`Fortnite Creative: World Building and Level Design`](./04_FORTNITE_CREATIVE_WORLD_BUILDING_AND_LEVEL_DESIGN.md) | Fortnite Creative | Spatial design, building, Prefabs, Galleries, paths, boundaries, lighting, and construction |
| `05` | [`Fortnite Creative: Island Rules, Players, and Game Flow`](./05_FORTNITE_CREATIVE_ISLAND_RULES_PLAYERS_AND_GAME_FLOW.md) | Fortnite Creative | Island Settings, spawning, teams, classes, rounds, objectives, scoring, inventory, and lifecycle |
| `06` | [`Fortnite Creative: Devices, Systems, NPC, AI, and Feedback`](./06_FORTNITE_CREATIVE_DEVICES_SYSTEMS_NPC_AI_AND_FEEDBACK.md) | Fortnite Creative | Devices, Events, Functions, NPC/AI, HUD, audio, video, VFX, and feedback systems |
| `07` | [`Fortnite Creative: Gameplay Patterns, Complete Examples, and Production`](./07_FORTNITE_CREATIVE_GAMEPLAY_PATTERNS_COMPLETE_EXAMPLES_AND_PRODUCTION.md) | Fortnite Creative | Complete recipes, prototypes, debugging, playtesting, optimization, production, and publishing |
| `08` | [`UEFN: Editor, Projects, Assets, and World Building`](./08_UEFN_EDITOR_PROJECTS_ASSETS_AND_WORLD_BUILDING.md) | UEFN without Verse | Editor workflows, project structure, assets, materials, Landscape, lighting, streaming, HLOD, and world authoring |
| `09` | [`UEFN: Gameplay, Verse API, UI, NPC, and Cinematics`](./09_UEFN_GAMEPLAY_VERSE_API_UI_NPC_AND_CINEMATICS.md) | UEFN with and without Verse | Device gameplay, Direct Event Binding, UI, NPCs, Conversations, animation, cinematics, audio, VFX, and Verse integration |
| `10` | [`UEFN: Testing, Optimization, Collaboration, and Publishing`](./10_UEFN_TESTING_OPTIMIZATION_COLLABORATION_AND_PUBLISHING.md) | UEFN with and without Verse | Sessions, testing, validation, memory, profiling, Lore Version Control, Private Versions, and publishing |
| `11` | [`Game Design, Social Mechanics, Research, and Learning`](./11_GAME_DESIGN_SOCIAL_MECHANICS_RESEARCH_AND_LEARNING.md) | Environment-neutral | Game-design reasoning, cooperation, belonging, ethics, playtesting research, and learning |
| `12` | [`Book of Verse I: Foundations`](./12_BOOK_OF_VERSE_I_FOUNDATIONS.md) | UEFN with Verse | Expressions, values, containers, operators, mutability, functions, control flow, and failure |
| `13` | [`Book of Verse II: Program Structure and Types`](./13_BOOK_OF_VERSE_II_PROGRAM_STRUCTURE_AND_TYPES.md) | UEFN with Verse | Structs, enums, classes, interfaces, type systems, access, modules, and paths |
| `14` | [`Book of Verse III: Effects, Concurrency, and Evolution`](./14_BOOK_OF_VERSE_III_EFFECTS_CONCURRENCY_AND_EVOLUTION.md) | UEFN with Verse | Effects, async and concurrency, persistence, compatibility, and language evolution |
| `15` | [`Glossary: Fortnite Creative`](./15_GLOSSARY_FORTNITE_CREATIVE.md) | Fortnite Creative | Detailed Creative definitions and canonical Hebrew terminology |
| `16` | [`Glossary: UEFN`](./16_GLOSSARY_UEFN.md) | UEFN | Detailed editor, production, Device, asset, collaboration, and publishing terminology |
| `17` | [`Glossary: Verse`](./17_GLOSSARY_VERSE.md) | UEFN with Verse | Verse language and UEFN API terminology |
| `18` | [`Supplemental Educational, Social, Troubleshooting, and Reference Knowledge`](./18_SUPPLEMENTAL_EDUCATIONAL_SOCIAL_TROUBLESHOOTING_AND_REFERENCE_KNOWLEDGE.md) | Primarily Fortnite Creative | Teaching sequences, planning templates, nonviolent examples, playtesting, and troubleshooting support |
| `19` | [`Fortnite Creative Community Wiki Delta Index`](./19_FORTNITE_COMMUNITY_WIKI_CREATIVE_DELTA_INDEX.md) | Primarily Fortnite Creative | Governed community discovery, metadata, aliases, visual references, reported behaviors, and delta knowledge |

---

## Using the Repository Independently

Although this repository is the knowledge foundation for AMIE, it is also designed to function independently.

### For Fortnite developers

Use the Master Knowledge Index to identify the correct environment and owner document, then follow the technical cross-references to implementation, testing, and production guidance.

### For AI agent developers

The corpus can support retrieval-augmented generation, specialized assistants, evaluation systems, or domain-specific search. Its metadata, authority boundaries, routing rules, document ownership, and exact terminology are designed to improve retrieval precision.

An AI implementation should preserve the hierarchy defined in `00_MASTER_KNOWLEDGE_INDEX.md` rather than treating every passage as equally authoritative.

### For educators

Use the technical documents for platform accuracy and the design and supplemental layers for lesson planning, workshop sequencing, nonviolent game concepts, cooperative systems, social design, assessment, and reflective playtesting.

### Recommended retrieval sequence

1. Resolve the environment.
2. Classify the user's intent.
3. Select the primary owner document.
4. Retrieve the exact Device, workflow, language rule, or concept.
5. Check current official documentation for version-sensitive claims.
6. Separate documented behavior from professional guidance or inference.
7. Include lifecycle, reset, multiplayer, and testing considerations.

---

## Sources and Authority

This repository integrates several knowledge types. They are not interchangeable.

### Official Epic Games documentation

Current Epic Games documentation is the primary external authority for Fortnite Creative, UEFN, Verse APIs, Creator Portal workflows, publishing rules, platform limits, and release-sensitive behavior.

- [Epic Games Fortnite Documentation](https://dev.epicgames.com/documentation/en-us/fortnite/fortnite-documentation)

Where current documentation, the active editor, compiler output, validation, or runtime behavior conflicts with repository wording, the current official product evidence takes priority.

### Book of Verse

The Book of Verse is an important external learning and language-design resource:

- [Book of Verse](https://verselang.github.io/book/)

Some Book of Verse material may describe language-main, planned, experimental, or not-yet-shipping functionality. Its presence in the book does not by itself prove availability in the current UEFN release. Executable guidance should be checked against current Epic Games Verse documentation and the active compiler.

### Fortnite Wiki on Fandom

The Fortnite Wiki provides useful community-maintained discovery, historical context, aliases, visual identification, asset metadata, and reported behavior:

- [Fortnite: Creative — Fortnite Wiki](https://fortnite.fandom.com/wiki/Fortnite:_Creative)

The Fortnite Wiki is a **community-maintained reference**, not official Epic Games documentation. Community claims must not override current Epic documentation, product behavior, validation, or controlled testing.

---

## Rights, Attribution, and Reuse

Epic Games-authored documentation, Fortnite content, product names, interfaces, systems, assets, and associated intellectual property remain the property of Epic Games and their respective rights holders.

**Epic Games**, **Fortnite**, **Unreal Editor for Fortnite (UEFN)**, **Verse**, and related names and marks are trademarks or intellectual property of their respective owners.

This repository is an independent knowledge-architecture project. It is **not an official Epic Games product** and is not endorsed, sponsored, maintained, or approved by Epic Games.

The repository combines and integrates:

- references to official Epic Games material;
- verified technical synthesis;
- original information architecture and routing systems;
- original educational and game-design frameworks;
- terminology mapping;
- annotations, editorial organization, and implementation guidance;
- community-reference metadata.

Original repository materials created by **Gilad Ravid** may be reused with appropriate attribution. This permission does not grant rights to Epic Games content or to third-party material. Any reuse, redistribution, modification, or publication must also comply with the applicable terms, policies, licenses, attribution requirements, and intellectual-property rights of Epic Games and all relevant third-party sources.

The repository should not be treated as a single open license covering every included or referenced source.

Suggested attribution:

> Fortnite Ecosystem Knowledge Architecture, developed and maintained by Gilad Ravid  
> https://github.com/GiladRav/Fortnite-Ecosystem-Knowledge-Architecture

---

## Maintenance and Suggested Improvements

This repository is maintained by **Gilad Ravid**. It is not operated as an open community-maintained project, and unsolicited Pull Requests are not the default contribution workflow.

Corrections, source updates, broken-link reports, technical conflicts, and improvement proposals are welcome. Please contact the maintainer with:

- the affected document;
- the relevant environment;
- the source supporting the proposed change;
- a concise explanation of the issue or improvement.

Contact: [gilad84@gmail.com](mailto:gilad84@gmail.com)

---

## About Gilad Ravid

**Gilad Ravid — Educational Technology, Game-Based Learning & AI Specialist**

Gilad Ravid is an educational technology and game-based learning specialist who connects AI, interactive systems, pedagogy, and knowledge architecture to create practical learning and development environments. He is an **Epic MegaGrant recipient** recognized for his educational work with Fortnite and brings extensive experience translating complex technical ecosystems into structured tools, professional learning programs, and usable experiences for developers and educators.

- **GitHub:** [github.com/GiladRav](https://github.com/GiladRav)
- **LinkedIn:** [linkedin.com/in/gilad-ravid](https://www.linkedin.com/in/gilad-ravid)
- **Email:** [gilad84@gmail.com](mailto:gilad84@gmail.com)
- **AMIE:** [Fortnite Development Agent](https://chatgpt.com/g/g-6a5f259f370081919ce0a30502915ce5-g-vnsy-hkhbr-shlk-bpvrtnyyt)
- **Repository:** [Fortnite Ecosystem Knowledge Architecture](https://github.com/GiladRav/Fortnite-Ecosystem-Knowledge-Architecture)

---

## Final Note

Fortnite development changes continuously. Use this repository to understand the system, route questions, plan implementations, and retrieve the right evidence—but use current Epic Games documentation, active product behavior, compiler output, launched-session tests, validation, and publishing checks as the final authority for release-sensitive decisions.
