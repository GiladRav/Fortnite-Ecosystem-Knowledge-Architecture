---
document_id: "19"
corpus_role: "community_wiki_delta_index"
authority: "secondary_community_discovery_and_metadata"
primary_environment: "Fortnite Creative"
secondary_environment: "UEFN discovery only; route implementation to 08-10"
language: "Hebrew; exact product names, UI labels, Device names, Events, Functions, identifiers, tags, and source titles remain in English"
status: "production-ready governance and indexing layer"
must_not_override_official: true
source_platform: "Fortnite Wiki on Fandom"
source_root: "https://fortnite.fandom.com/wiki/Fortnite:_Creative"
license_note: "Community text is generally presented by Fandom as CC BY-SA unless otherwise noted; preserve page-level attribution and verify media licenses separately"
last_updated: "2026-08-03"
---

# Fortnite Creative Community Wiki Delta Index

## מטרת המסמך

מסמך זה הוא שכבת ידע קהילתית משלימה עבור סוכן בינה מלאכותית העוסק ב־Fortnite Creative.

המסמך נועד לאפשר לסוכן:

- לאתר עמודי Fortnite Wiki רלוונטיים ליצירה ופיתוח;
- להבין איזה סוג מידע קהילתי ניתן להפיק מכל עמוד;
- למפות Devices, Prefabs, Galleries, נכסים, דפוסי משחק ומונחים קהילתיים;
- להשתמש במידע חזותי, במטא־דאטה, בתיאורי תכולה ובטענות קהילתיות כנקודת גילוי;
- להפריד בין עובדה רשמית, דיווח קהילתי, בדיקה מעשית והשערה;
- למנוע שכפול של התיעוד הרשמי של Epic Games;
- למנוע שימוש ב־Fortnite Wiki כמקור סמכות טכני במקום התיעוד הרשמי.

מסמך זה אינו Mirror מלא של Fortnite Wiki ואינו אמור לשמר כל משפט מכל עמוד. הוא מגדיר אינדקס ישויות, כללי חילוץ, שדות מנורמלים, רמות אמינות וכללי שימוש עבור מידע קהילתי שיש לו ערך מעשי מעבר לתיעוד הרשמי.

## גבול סמכות

### מה המסמך רשאי לספק

- קישורי גילוי לעמודים רלוונטיים ב־Fortnite Wiki;
- מיפוי שמות ותצוגות של Devices, Prefabs, Galleries ו־Creative assets;
- תיאור תכולת חבילות בנייה ונכסים;
- תגיות עיצוביות ותמטיות;
- ז'רגון קהילתי ודפוסי ז'אנר;
- Device combinations שהקהילה משתמשת בהם;
- דיווחי באגים, מגבלות, תקלות והתנהגויות לא מתועדות;
- נתוני Memory שמדווחים בעמודי הקהילה, כאשר מקורם והקשרם ברורים;
- סימון פערים, סתירות או חוסרים הדורשים בדיקה רשמית או Play Mode test;
- מטא־דאטה על עמודים, קטגוריות, Redirects, Infoboxes ורמת השלמות שלהם.

### מה המסמך אינו רשאי להגדיר

המסמך אינו מקור סמכות עבור:

- Device option names;
- option values;
- Events;
- Functions;
- Direct Event Binding behavior;
- Island Settings;
- UI paths;
- keyboard or controller bindings;
- Creative או UEFN feature availability;
- publishing eligibility;
- moderation requirements;
- current platform limits;
- current player limits;
- Verse syntax;
- Verse API signatures;
- current memory validation rules;
- runtime guarantees;
- current release status.

כל טענה מהסוגים האלה חייבת לעבור אל המקור הרשמי המתאים בקורפוס.

## היררכיית אמינות מחייבת

בעת יצירת תשובה, יש להשתמש בהיררכיה הבאה:

1. התיעוד הנוכחי של Epic Games, ה־UI הפעיל, release notes, creator rules, validation ותוצאות בדיקה במוצר.
2. היחידות הרשמיות שנשמרו בקבצים `03`–`07` עבור Fortnite Creative.
3. מסמכי UEFN המאומתים `08`–`10`, כאשר השאלה עוסקת ב־UEFN.
4. המונחים וההגדרות בקבצים `15`–`17`, ללא הרחבתם להוראות יישום.
5. מידע קהילתי מסמך זה, כאשר הוא מסומן במפורש כ־Community evidence.
6. מסקנה או השערה, רק כאשר היא מסומנת ככזו.

כלל מוחלט:

> במקרה של סתירה בין Fortnite Wiki לבין התיעוד הרשמי של Epic Games, המקור הרשמי גובר תמיד.

## כלל הדלתא

יש לשמור במסמך הפעיל רק מידע שמוסיף ערך שאינו כבר מיוצג באופן מספק בתיעוד הרשמי.

### דלתא קהילתית תקפה

מידע יכול להיכלל כאשר הוא מספק לפחות אחד מהבאים:

- visual identification;
- asset composition;
- gallery contents;
- prefab footprint or structural composition;
- community theme tags;
- community terminology;
- common Device combination;
- troubleshooting observation;
- undocumented behavior claim;
- measured community memory value;
- edge case;
- workflow workaround;
- discoverability improvement;
- cross-reference בין שמות ישנים, שמות קהילתיים ושמות נוכחיים;
- genre implementation pattern.

### דלתא שאינה תקפה

אין לכלול:

- העתקה מלאה של Device options שכבר קיימות ברשמי;
- העתקה מלאה של Events ו־Functions;
- מדריך בסיסי להצבת Device;
- הסבר בסיסי על Customize panel;
- הסבר כללי על Direct Event Binding;
- תיאור כללי של Fortnite Creative שכבר קיים בקבצים `03`–`07`;
- walkthrough שמעתיק tutorial רשמי;
- נתון שאינו מוסיף הקשר, מקור, מדידה או מיפוי שימושי.

## ניתוב לקבצי הקורפוס

| סוג שאלה | מקור ראשי | שימוש במסמך זה |
|---|---|---|
| בחירת סביבת עבודה | `00_MASTER_KNOWLEDGE_INDEX.md` | זיהוי Community page בלבד |
| איתור מקור Epic נוכחי | `01_EPIC_GAMES_DOCUMENTATION_INDEX.md` | קישור משלים ל־Wiki |
| Creative interface ו־Phone Tool | `03_FORTNITE_CREATIVE_FOUNDATIONS_AND_INTERFACE.md` | שמות קהילתיים או תצוגות בלבד |
| Prefabs, Galleries, structures ו־level design | `04_FORTNITE_CREATIVE_WORLD_BUILDING_AND_LEVEL_DESIGN.md` | תכולת חבילות, tags ומיפוי חזותי |
| Island Settings, players, items, scoring ו־flow | `05_FORTNITE_CREATIVE_ISLAND_RULES_PLAYERS_AND_GAME_FLOW.md` | דיווחים קהילתיים מסומנים בלבד |
| Devices, Events, Functions, NPC, HUD ו־feedback | `06_FORTNITE_CREATIVE_DEVICES_SYSTEMS_NPC_AI_AND_FEEDBACK.md` | גילוי, combos, visual mapping ותקלות קהילתיות |
| complete examples, testing ו־production | `07_FORTNITE_CREATIVE_GAMEPLAY_PATTERNS_COMPLETE_EXAMPLES_AND_PRODUCTION.md` | community tropes ו־workarounds |
| game design, social mechanics ו־learning | `11_GAME_DESIGN_SOCIAL_MECHANICS_RESEARCH_AND_LEARNING.md` | ז'אנרים קהילתיים, ללא טענות פדגוגיות |
| Creative definitions | `15_GLOSSARY_FORTNITE_CREATIVE.md` | alias ו־community terminology בלבד |
| adapted teaching ו־troubleshooting | `18_SUPPLEMENTAL_EDUCATIONAL_SOCIAL_TROUBLESHOOTING_AND_REFERENCE_KNOWLEDGE.md` | דוגמאות קהילתיות, לאחר אימות טכני |
| UEFN implementation | `08`–`10` | עמודי UEFN משמשים לגילוי בלבד |
| Verse | `09`, `12`–`14`, `17` | אין ללמוד Verse API מה־Wiki |

## כלל סביבת העבודה

לפני שימוש במידע יש לקבוע אחת מהסביבות הבאות:

- `Fortnite Creative`
- `UEFN without Verse`
- `UEFN with Verse`
- `Shared / cross-environment`
- `Unresolved`

חברות של עמוד ב־`Category:UEFN` אינה מוכיחה שהישות היא UEFN-only. חברות ב־`Category:Devices` אינה מוכיחה שה־Device זמין בכל סביבה. יש לאמת זמינות במקור הרשמי.

## עוגני השפה

- שמות Devices נשמרים באנגלית בדיוק כפי שהם מופיעים ב־UI או במקור.
- שמות Events נשמרים באנגלית.
- שמות Functions נשמרים באנגלית.
- שמות Prefabs ו־Galleries נשמרים באנגלית.
- שמות tags נשמרים באנגלית וב־`snake_case`.
- הסברים יכולים להיכתב בעברית.
- אין לתרגם מזהים, API names, UI paths או source titles.
- כאשר שם קהילתי שונה מהשם הרשמי, יש לשמור את שניהם בשדות נפרדים.

---

# אינדקס מקורות ראשי

## דף המקור הראשי

- **שם המקור:** `Fortnite: Creative`
- **כתובת המקור:** https://fortnite.fandom.com/wiki/Fortnite:_Creative
- **תפקיד:** נקודת כניסה קהילתית למצב Creative.
- **שימוש:** גילוי קטגוריות, עמודי־אב, מערכות ומונחים.
- **סמכות:** Community secondary.
- **אין להשתמש עבור:** current Device contract, current limits, publishing rules או feature availability.

## קטגוריות Seed

### Category:Creative

- **URL:** https://fortnite.fandom.com/wiki/Category:Creative
- **תפקיד:** קטגוריית־אב לגילוי תוכן הקשור ל־Creative.
- **תוכן צפוי:** subcategories, Devices, systems, assets, templates ותוכן היסטורי מעורב.
- **סיכון:** הקטגוריה רחבה ועלולה להחזיר תוכן שאינו מתאים לאינדקס הפעיל.
- **כלל:** להשתמש לגילוי בלבד, ולא להכניס אוטומטית את כל חברי הקטגוריה.

### Category:Devices

- **URL:** https://fortnite.fandom.com/wiki/Category:Devices
- **תפקיד:** גילוי Device pages.
- **תוכן צפוי:** Creative Devices, UEFN-related Devices, removed or restricted pages, utility pages ותתי־קטגוריות.
- **סיכון:** ערבוב של Devices פעילים, UEFN-only, removed, developer-only או incomplete pages.
- **כלל:** כל עמוד חייב לעבור environment classification, active-use filtering ו־official owner mapping.

### Category:Prefabs

- **URL:** https://fortnite.fandom.com/wiki/Category:Prefabs
- **תפקיד:** גילוי Prefab pages.
- **עדיפות חילוץ:** source location, building type, footprint, reported Memory, component description ו־theme tags.
- **כלל:** אין להניח שכל Prefab עדיין זמין או זהה בגרסה הנוכחית.

### Category:Galleries

- **URL:** https://fortnite.fandom.com/wiki/Category:Galleries
- **תפקיד:** גילוי Gallery pages.
- **עדיפות חילוץ:** prop families, building pieces, terrain assets, visual style, functional use ו־related Prefabs.
- **כלל:** Gallery membership אינה הוכחה לזמינות נוכחית.

### Category:UEFN

- **URL:** https://fortnite.fandom.com/wiki/Category:UEFN
- **תפקיד:** גילוי עמודים שהקהילה מקשרת ל־UEFN.
- **גבול:** מידע יישומי עובר ל־`08`–`10`; Verse עובר ל־`09`, `12`–`14`, `17`.
- **כלל:** אין להעביר UEFN workflow לתשובת Creative.

## עמודי־אב משלימים

### Devices (Creative)

- **URL:** https://fortnite.fandom.com/wiki/Devices_(Creative)
- **תפקיד:** רשימת גילוי ומיון קהילתית של Devices.
- **שימוש:** איתור שמות, families, aliases וקישורים לעמודים.
- **אין להשתמש עבור:** רשימה מלאה ועדכנית של options, Events או Functions.

### עמוד־האב Prefabs

- **URL:** https://fortnite.fandom.com/wiki/Prefabs
- **תפקיד:** עמוד־אב המסביר ומרכז Prefabs.
- **שימוש:** גילוי Prefab families ומבנים.

### Content Browser

- **URL:** https://fortnite.fandom.com/wiki/Content_Browser
- **תפקיד:** גילוי קטגוריות תוכן ושמות קהילתיים לממשק.
- **גבול:** UI paths ו־current interface behavior דורשים אימות ב־Epic.

### Characters (Creative)

- **URL:** https://fortnite.fandom.com/wiki/Characters_(Creative)
- **תפקיד:** מיפוי חזותי של Characters הזמינים דרך מערכות שונות.
- **שימוש:** visual discovery בלבד.
- **גבול:** אין להסיק זמינות ב־Character Device, Guard Spawner או NPC tool מסוים בלי אימות רשמי נפרד.

### Developer Features (Creative)

- **URL:** https://fortnite.fandom.com/wiki/Developer_Features_(Creative)
- **תפקיד:** זיהוי תכונות שייתכן שאינן זמינות ליוצר רגיל.
- **כלל:** כל ישות מסוג זה תסומן `restricted_or_nonpublic` ולא תופיע כהמלצת בנייה.

---

# מתודולוגיית איסוף

## עקרון דו־שלבי

אין להמיר עמוד Wiki ישירות לרשומת ידע פעילה.

התהליך המחייב הוא:

`Raw extraction → normalization → filtering → official comparison → delta selection → QA → active Markdown`

## שלב 1: גילוי

לגלות עמודים באמצעות:

- Seed categories;
- subcategories;
- linked lists;
- infobox types;
- redirects;
- page-title patterns;
- related pages;
- category membership.

## שלב 2: חילוץ גולמי

לכל עמוד יש לשמור, כאשר המידע זמין:

- `page_title`
- `page_url`
- `page_id`
- `revision_id`
- `retrieved_at`
- `last_modified`
- `redirect_target`
- `categories`
- `templates`
- `infobox_type`
- `infobox_fields`
- `section_titles`
- `tables`
- `outgoing_links`
- `media_files`
- `raw_claims`

## שלב 3: סיווג ישות

הערכים המותרים:

```yaml
entity_type:
  - device
  - prefab
  - gallery
  - building_piece_collection
  - prop_collection
  - terrain_collection
  - visual_effect_collection
  - island_template
  - creative_system
  - creative_constraint
  - community_combo
  - genre_pattern
  - community_term
  - troubleshooting_claim
  - undocumented_behavior_claim
  - restricted_or_nonpublic
  - excluded
  - unresolved
```

## שלב 4: בדיקת סף פונקציונליות

ישות תיכלל רק אם מתקיים לפחות תנאי אחד:

- היא משנה gameplay state;
- יש לה User Options;
- היא משדרת Event;
- היא מקבלת Function;
- היא משפיעה על player movement, state, visibility, scoring, inventory, team, class, objective, feedback או game flow;
- יש לה Memory cost שימושי לתכנון;
- היא מכילה assets שימושיים לבניית שלב;
- היא מספקת visual mapping שאינו נגיש בקלות ברשמי;
- היא מייצגת hard constraint;
- היא מתארת bug, workaround או undocumented behavior;
- היא משמשת כחלק מדפוס בנייה קהילתי מוכר;
- היא תורמת ל־asset recommendation.

## שלב 5: סינון שלילי

יש להחריג אוטומטית:

- Outfits;
- Back Blings;
- Pickaxes;
- Gliders;
- Emotes;
- Wraps;
- Item Shop history;
- cosmetic Sets;
- Battle Pass content;
- Fortnite story או lore;
- live events;
- Battle Royale season history;
- weapon damage;
- fire rate;
- magazine size;
- reload time;
- weapon rarity tables;
- vaulted או unvaulted tracking;
- update timelines;
- patch-history sections;
- removed-content nostalgia pages;
- unrelated locations;
- promotional pages;
- Talk pages כמקור עובדתי;
- user pages;
- image categories;
- template documentation שאינה נדרשת לחילוץ;
- trivia ללא ערך פונקציונלי;
- comments ללא בדיקה;
- duplicated mirrors;
- pages with no functional, asset, constraint, visual או community-pattern value.

## שלב 6: השוואה למקור רשמי

עבור Device:

1. לאתר את owner document ב־`05` או `06`.
2. לאתר את עמוד Epic דרך `01`.
3. להשוות שם, environment, options, Events, Functions והגבלות.
4. להסיר מהדלתא מידע שהוא שכפול רשמי.
5. לשמור רק מידע קהילתי בעל ערך נוסף.
6. להעביר סתירות ל־Quarantine.

עבור Prefab או Gallery:

1. להשוות למיפוי הרשמי ב־`04`.
2. לשמור composition, theme ו־visual discovery.
3. לא להציג availability או Memory כעובדה נוכחית בלי אימות.

## שלב 7: יצירת רשומה מנורמלת

כל רשומה חייבת לכלול:

- מזהה יציב;
- שם קנוני;
- סוג ישות;
- סביבת עבודה;
- מקור קהילתי;
- owner official;
- confidence;
- verification status;
- allowed agent use;
- prohibited agent use;
- delta fields בלבד.

---

# מודל אמינות

## רמת מקור

| קוד | הגדרה |
|---|---|
| `S0` | טקסט חופשי, ללא מבנה או ייחוס ברור |
| `S1` | Infobox, table או structured field בעמוד Wiki |
| `S2` | אותה טענה מופיעה בשני עמודים קהילתיים בלתי תלויים |
| `S3` | טענה שנבדקה ב־Creative או UEFN עם build, date ו־test conditions |
| `S4` | טענה שתואמת גם למקור Epic הנוכחי |

## סטטוס אימות

```yaml
verification_status:
  - unreviewed
  - community_report_only
  - cross_page_supported
  - tested_in_session
  - official_match
  - official_conflict
  - stale_or_legacy
  - unresolved
  - rejected
```

## סטטוס שימוש בסוכן

```yaml
agent_use:
  - active_discovery
  - active_visual_reference
  - active_asset_recommendation
  - active_community_pattern
  - cite_as_community_claim
  - troubleshooting_hypothesis_only
  - verification_required
  - quarantine
  - excluded
```

## כלל ניסוח

כאשר confidence נמוך מ־`S3`, יש לנסח:

- "Fortnite Wiki reports..."
- "Community-maintained metadata indicates..."
- "This behavior is not confirmed by current Epic documentation."
- "Test this in Play Mode before relying on it."

אין לנסח מידע כזה כעובדה מוחלטת.

---

# מודל ישות בסיסי

```yaml
entity_id: "creative.device.example"
canonical_name: "Example Device"
community_name: "Example Device"
entity_type: "device"
environment:
  creative_available: null
  uefn_available: null
  creative_only: null
  uefn_only: null
  classification_status: "unresolved"
source:
  page_title: "Example Device"
  page_url: "https://fortnite.fandom.com/wiki/..."
  page_id: null
  revision_id: null
  retrieved_at: "YYYY-MM-DD"
  source_level: "S1"
official_routing:
  owner_document: "06_FORTNITE_CREATIVE_DEVICES_SYSTEMS_NPC_AI_AND_FEEDBACK.md"
  official_url: null
  official_verification_required: true
community_delta:
  visual_mapping: []
  asset_contents: []
  reported_memory: null
  undocumented_behavior_claims: []
  workarounds: []
  common_combos: []
  thematic_tags: []
verification:
  status: "unreviewed"
  tested_release: null
  test_conditions: null
agent_policy:
  allowed_use: []
  prohibited_use: []
```

---

# Devices

## תפקיד החלק

חלק זה אינו רשימת Device documentation חלופית. הוא אינדקס קהילתי שמאפשר:

- למצוא עמוד Wiki של Device;
- לזהות aliases;
- לזהות visual variants;
- לאתר reported Memory;
- לזהות community combinations;
- לזהות claims של bugs או edge cases;
- להפנות למקור הרשמי הנכון.

## שדות Device

```yaml
entity_id: "creative.device.<slug>"
canonical_name: "Exact Device Name"
community_aliases: []
entity_type: "device"
environment: "Fortnite Creative | UEFN | Shared | Unresolved"
functional_family: []
source_url: ""
official_owner: "05 | 06 | 09"
official_url: ""
page_completeness: "complete | partial | stub | list_only"
legacy_terminology_detected: false
user_options_present_on_wiki: false
events_present_on_wiki: false
functions_present_on_wiki: false
community_delta:
  visual_variants: []
  placement_notes: []
  reported_memory: null
  common_combos: []
  known_failure_modes: []
  undocumented_claims: []
  workarounds: []
confidence: "S0-S4"
verification_status: ""
```

## משפחות פונקציונליות

### לוגיקה וקלט

תגיות אפשריות:

- `logic`
- `input`
- `condition_check`
- `randomization`
- `timing`
- `state_switch`
- `player_detection`
- `presence_detection`
- `voting`

Owner טיפוסי: `06`.

### שחקן, קבוצה, Class ומחזור חיים

תגיות אפשריות:

- `player_state`
- `team_state`
- `class_state`
- `spawn`
- `respawn`
- `round_flow`
- `game_end`
- `join_in_progress`

Owner טיפוסי: `05`.

### Items, Inventory, משאבים והתקדמות

תגיות אפשריות:

- `item_spawn`
- `item_grant`
- `item_remove`
- `inventory_check`
- `resource`
- `currency`
- `objective`
- `tracker`
- `score`
- `stat`
- `persistence`

Owner טיפוסי: `05`.

### תנועה ומעבר במרחב

תגיות אפשריות:

- `movement`
- `teleportation`
- `speed_change`
- `vertical_traversal`
- `rail_traversal`
- `water_traversal`
- `checkpoint`
- `route_control`

Owner טיפוסי: `05` או `06`, בהתאם ל־Device.

### מצב העולם ושליטה סביבתית

תגיות אפשריות:

- `barrier`
- `door_control`
- `prop_control`
- `visibility`
- `collision`
- `lighting`
- `time_of_day`
- `post_processing`
- `environmental_hazard`
- `volume`

Owner טיפוסי: `06`.

### HUD, טקסט, Audio ומשוב

תגיות אפשריות:

- `hud`
- `message`
- `dialog`
- `map_marker`
- `audio`
- `video`
- `vfx`
- `feedback`
- `instruction`

Owner טיפוסי: `06`.

### Character, NPC ו־AI

תגיות אפשריות:

- `character_display`
- `guard_ai`
- `wildlife`
- `patrol`
- `navigation_control`
- `crowd`
- `npc_feedback`

Owner טיפוסי ב־Creative: `06`.

UEFN NPC, Conversation Graph, NPC Character Definition או Verse behavior עוברים ל־`09`.

### מצלמה ושליטה

תגיות אפשריות:

- `camera`
- `first_person`
- `fixed_camera`
- `orbit_camera`
- `side_scroller`
- `control_scheme`

Owner טיפוסי: `05`.

## כללי Events ו־Functions

- ניתן לחלץ Events ו־Functions למאגר גולמי לצורך השוואה.
- אין להציג אותם כרשימה פעילה מתוך Fortnite Wiki.
- הרשימה הפעילה חייבת להגיע מהעמוד הרשמי של ה־Device.
- כאשר ה־Wiki משתמש ב־Channels, יש לסמן `legacy_terminology_detected: true`.
- אין להמיר Channel text ל־Direct Event Binding באמצעות ניחוש.
- כל connection בתשובת הסוכן חייב להיכתב כך:

`Source Device — verified Event → Receiving Device — verified Function`

## רשומת Device לדוגמה

> הדוגמה הבאה היא תבנית מבנית. ערכי options, Events, Functions ו־Memory אינם נחשבים מאומתים עד השלמת חילוץ ובדיקת מקור.

### Character Device

- **מזהה ישות:** `creative.device.character_device`
- **סוג ישות:** `device`
- **סביבת עבודה:** `Fortnite Creative / UEFN — verify current availability`
- **משפחה פונקציונלית:** `character_display`, `visual_mapping`, `feedback`
- **מקור קהילתי:** https://fortnite.fandom.com/wiki/Character_Device
- **בעל הסמכות הרשמי:** `06_FORTNITE_CREATIVE_DEVICES_SYSTEMS_NPC_AI_AND_FEEDBACK.md`
- **נדרש אימות רשמי:** `Yes`
- **שימוש קהילתי מותר:** visual discovery, aliases, character appearance mapping, community-reported observations.
- **שימוש אסור:** exact options, Events, Functions או current availability ללא אימות Epic.
- **בדיקת מינוח Legacy:** required.
- **רמת אמינות:** `Unassigned until extraction`.

---

# Prefabs

## מטרת חילוץ Prefab

רשומת Prefab צריכה לעזור לסוכן לענות על שאלות כגון:

- איזה מבנה מתאים לעיירה, טירה, חווה או אזור תעשייתי?
- אילו חלקים מרכזיים קיימים במבנה?
- האם המבנה מתאים כ־hub, puzzle room, landmark או traversal space?
- מה גודל המבנה המשוער?
- אילו Galleries קשורות אליו?
- מהו reported Memory, אם קיים?

## שדות Prefab

```yaml
entity_id: "creative.prefab.<slug>"
canonical_name: "Exact Prefab Name"
entity_type: "prefab"
source_url: ""
source_location: ""
environment: "Fortnite Creative | UEFN | Shared | Unresolved"
structure_type: ""
footprint:
  width_tiles: null
  depth_tiles: null
  height_tiles: null
reported_memory: null
component_summary: []
major_spaces: []
building_piece_types: []
prop_families: []
related_galleries: []
theme_tags: []
functional_use_tags: []
accessibility_notes: []
collision_or_navigation_claims: []
confidence: "S0-S4"
verification_status: ""
```

## Theme taxonomy

### Era

- `ancient`
- `medieval`
- `prehistoric`
- `western`
- `industrial_age`
- `contemporary`
- `near_future`
- `futuristic`
- `fantasy`
- `seasonal`

### Setting

- `urban`
- `suburban`
- `rural`
- `industrial`
- `commercial`
- `residential`
- `coastal`
- `seaside`
- `wilderness`
- `mountain`
- `desert`
- `forest`
- `farm`
- `sports`
- `underground`
- `space`

### Mood

- `welcoming`
- `playful`
- `formal`
- `mysterious`
- `abandoned`
- `ruined`
- `bright`
- `dark`
- `calm`
- `high_energy`

### Functional Use

- `hub`
- `spawn_area`
- `lobby`
- `landmark`
- `puzzle_room`
- `escape_room`
- `interior_route`
- `vertical_route`
- `obstacle_course`
- `social_space`
- `objective_location`
- `checkpoint_location`
- `boundary_structure`
- `background_scenery`

## כללי תכולה

- לתאר רק assets או spaces שנראים או מצוינים בבירור.
- אין להמציא רשימת props מדויקת כאשר העמוד אינו מספק אותה.
- אם נדרשת צפייה בתמונה, לסמן `visual_inspection_required: true`.
- אין להסיק collision behavior מהתמונה בלבד.
- אין להסיק שהמבנה optimized או מתאים לפלטפורמה חלשה ללא בדיקה.

## רשומת Prefab לדוגמה

### Noms

- **מזהה ישות:** `creative.prefab.noms`
- **סוג ישות:** `prefab`
- **מקור קהילתי:** `Noms (Prefab)` on Fortnite Wiki.
- **Theme ראשי:** `commercial`
- **Themes משניים:** `urban`, `restaurant`, `retail`
- **שימושים אפשריים:** `hub`, `objective_location`, `social_space`, `interior_route`
- **Memory מדווח:** extract only from a structured field; do not assume current accuracy.
- **בעל הסמכות הרשמי:** `04_FORTNITE_CREATIVE_WORLD_BUILDING_AND_LEVEL_DESIGN.md`
- **נדרשת בדיקה חזותית:** `Yes`
- **נדרש אימות זמינות:** `Yes`

---

# Galleries

## מטרת חילוץ Gallery

רשומת Gallery צריכה לאפשר לסוכן:

- להבין מה סוג הנכסים בחבילה;
- להמליץ על Gallery לפי theme;
- להבדיל בין building pieces, props, terrain, VFX או utility assets;
- לאתר Galleries משלימות;
- לזהות אילו Prefabs או locations קשורים לחבילה.

## שדות Gallery

```yaml
entity_id: "creative.gallery.<slug>"
canonical_name: "Exact Gallery Name"
entity_type: "gallery"
source_url: ""
environment: "Fortnite Creative | UEFN | Shared | Unresolved"
gallery_family: "building | props | terrain | nature | effects | devices | utility | mixed"
asset_summary: []
major_asset_types: []
building_piece_types: []
prop_types: []
terrain_types: []
visual_effect_types: []
related_prefabs: []
source_location: ""
theme_tags: []
functional_use_tags: []
reported_memory: null
visual_inspection_required: true
confidence: "S0-S4"
verification_status: ""
```

## כללי המלצה

כאשר הסוכן ממליץ על Gallery, עליו לציין:

1. שם Gallery באנגלית.
2. למה היא מתאימה מבחינה חזותית.
3. אילו סוגי assets צפויים להימצא בה.
4. האם ההמלצה מבוססת על Community metadata או Epic documentation.
5. שיש לאמת את הזמינות ב־Content menu הפעיל.

## רשומת Gallery לדוגמה

### Objective Device Gallery

- **מזהה ישות:** `creative.gallery.objective_device_gallery`
- **סוג ישות:** `gallery`
- **מקור קהילתי:** https://fortnite.fandom.com/wiki/Objective_Device_Gallery
- **משפחת Gallery:** `devices`, `objective_assets`
- **שימוש:** visual mapping of objective Device variants.
- **בעל הסמכות הרשמי:** `05` או `06`, בהתאם לישות שנבחרה בפועל.
- **כלל:** אין להניח שכל Variant מתנהג באופן זהה ללא בדיקת ה־Device הספציפי.

---

# מטא־דאטה של Memory

## מטרת מטא־דאטה של Memory

Memory data קהילתי יכול לסייע בהערכת כיוון ובזיהוי assets יקרים, אך אינו מחליף את Memory workflow הנוכחי של Epic.

## סוגי עלות מותרים

```yaml
cost_type:
  - base_memory
  - instance_memory
  - total_prefab_memory
  - per_asset_memory
  - variable_memory
  - reported_unspecified
  - unknown
```

## מבנה רשומת Memory

```yaml
memory:
  reported_value: null
  unit: "memory_units"
  cost_type: "unknown"
  measurement_context: ""
  tested_release: null
  source_type: "wiki_infobox | wiki_table | community_test | screenshot | unknown"
  source_url: ""
  source_revision_id: null
  exactness: "reported_not_verified"
  current_validity: "unknown"
```

## כללים מחייבים

- אין לשמור מספר ללא `cost_type`.
- אין לקרוא לכל מספר `Base Memory` אם העמוד אינו מציין זאת.
- אין לחשב `Instance Memory` באמצעות חיסור בין מספרים ממקורות שונים.
- אין להשוות בין מדידות מגרסאות שונות כאילו הן אותה בדיקה.
- אין להציג reported Memory כעובדה נוכחית ללא בדיקה בגרסה הפעילה.
- Prefab total אינו זהה לסכום קבוע של כל החלקים לאחר פירוק או שינוי.
- יש לשמור release, platform, island context ו־measurement method כאשר הם ידועים.
- כאשר מופיע `TBD`, `Unknown` או `Varies`, יש לשמור את הערך כבלתי ידוע ולא להשלים אותו.

## שימוש בתשובה

ניסוח מותר:

> Fortnite Wiki מדווח על ערך Memory עבור הנכס, אך את העלות הנוכחית יש למדוד באמצעות תהליך ה־Memory הפעיל ב־Creative.

ניסוח אסור:

> ה־Device הזה תמיד עולה בדיוק X יחידות Memory.

---

# מגבלות קשיחות

## מטרת חילוץ מגבלות

לאסוף claims קהילתיים לגבי גבולות מעשיים כגון:

- player count;
- object count;
- interaction range;
- rendering distance;
- collision limits;
- grid snapping behavior;
- prop scaling edge cases;
- device registration limits;
- AI navigation behavior;
- replication or multiplayer scope;
- HUD overlap;
- round reset behavior;
- join-in-progress behavior.

## כלל סמכות

כל Hard Constraint הוא version-sensitive. הוא אינו פעיל עד שמתקיים אחד מהבאים:

- source Epic נוכחי;
- current UI validation;
- controlled test in the active release.

## מבנה Claim

```yaml
claim_id: "creative.constraint.<slug>"
claim_text: ""
claim_category: "player_limit | rendering | grid | collision | navigation | scope | lifecycle | memory | other"
source_url: ""
source_level: "S0-S4"
reported_release: null
test_required: true
official_match: null
verification_status: "community_report_only"
agent_use: "verification_required"
```

---

# באגים והתנהגויות לא מתועדות

## מטרת תיעוד באגים

לשמור תצפיות קהילתיות שיכולות לסייע באבחון, בלי להפוך אותן לחוזה מוצר.

## תנאי הכללה

Bug או undocumented behavior ייכלל רק אם:

- ניתן לתאר את הבעיה באופן ניתן לשחזור;
- ידוע לפחות Device או system אחד שמעורב;
- יש expected result ו־actual result;
- יש תנאי בדיקה בסיסיים;
- קיים מקור;
- אין מדובר רק בתלונה כללית.

## מבנה רשומת Bug

```yaml
claim_id: "creative.bug.<slug>"
title: ""
environment: "Fortnite Creative | UEFN | Shared | Unresolved"
affected_entities: []
reported_release: null
preconditions: []
steps_to_reproduce: []
expected_result: ""
reported_result: ""
frequency: "always | frequent | intermittent | once | unknown"
player_scope: "single_player | team | global | multiplayer_only | unknown"
workaround: []
source_url: ""
evidence: []
verification_status: "community_report_only"
current_relevance: "unknown"
```

## כללי שימוש

- להציג כ־reported behavior.
- לבדוק אם הבעיה עדיין קיימת.
- לשנות משתנה אחד בכל בדיקה.
- לא להמליץ על workaround אם הוא עוקף validation, moderation או platform policy.
- כאשר workaround סותר את Epic, אין להשתמש בו.

---

# מעקפים ושילובי Devices

## עקרון מבני

Device combination הוא מערכת של מספר ישויות. אין לייחס את התוצאה ל־Device יחיד.

## מבנה Combo

```yaml
pattern_id: "creative.combo.<slug>"
pattern_name: ""
pattern_type: "device_combo"
environment: "Fortnite Creative"
player_goal: ""
system_goal: ""
devices: []
state_owner: "player | team | global | mixed | unresolved"
community_delta: []
known_failure_modes: []
reset_requirements: []
join_in_progress_risks: []
official_coverage: "full | partial | none | unresolved"
verification_status: ""
```

## פריט פותח דלת

- **Pattern ID:** `creative.combo.item_opens_door`
- **לולאת שחקן:** find item → present requirement → open route.
- **Devices טיפוסיים:** `Item Spawner` or `Item Granter`, `Conditional Button`, `Lock` or another verified route-control Device.
- **דלתא קהילתית:** common arrangement, visual layout ideas, failure patterns and reset observations.
- **אימות רשמי:** retrieve every Device independently from `05` או `06`.
- **אין לשמור כאן:** guessed Event names or Function names.
- **תחומי בדיקה ידועים:** item registration, required quantity, consume behavior, instigator scope, door association, round reset.

## הפעלה שיתופית בשתי נקודות

- **Pattern ID:** `creative.combo.cooperative_two_point_activation`
- **לולאת שחקן:** two players activate or occupy separated points → shared route changes.
- **מערכות טיפוסיות:** player detection, trigger logic, count or state aggregation, route control, feedback.
- **דלתא קהילתית:** spatial layout, anti-solo bypass patterns and coordination cues.
- **בדיקות נדרשות:** simultaneous activation, one player leaving, repeated activation, team scope, minimum player fallback.

## Character מוסר רמז

- **Pattern ID:** `creative.combo.character_gives_clue`
- **לולאת שחקן:** approach character → interact → receive clue → unlock next action.
- **גבול Creative:** do not substitute UEFN NPC Spawner, NPC Character Definition, Conversation Graph או Verse.
- **דלתא קהילתית:** character appearance mapping, environmental presentation and common clue-delivery patterns.

## שאלה עם הסתעפות

- **Pattern ID:** `creative.combo.branching_question`
- **לולאת שחקן:** read prompt → choose one of several responses → receive consequence.
- **דלתא קהילתית:** layout patterns, retry design, one-answer locking and duplicate-activation prevention.
- **אימות רשמי:** exact Devices, Events and Functions must come from official owner pages.

## התקדמות באמצעות איסוף

- **Pattern ID:** `creative.combo.collection_progression`
- **לולאת שחקן:** collect objects → update progress → unlock route or objective.
- **דלתא קהילתית:** common presentation, visibility, catch-up and duplicate collection issues.
- **בדיקות נדרשות:** personal versus shared progress, respawn, late join, round reset and repeated collection.

---

# דפוסי ז’אנר קהילתיים

## מטרת דפוסי הז’אנר

ז'אנר קהילתי נשמר כארכיטקטורת משחק כללית, לא כרשימת Devices קשיחה.

כל ז'אנר חייב לכלול:

- core loop;
- state model;
- common system families;
- common failure modes;
- player-scope risks;
- required tests;
- official verification boundary.

## חדר בריחה

- **Pattern ID:** `creative.genre.escape_room`
- **Core Loop:** inspect → discover clue → satisfy condition → open route → receive confirmation → continue.
- **מערכות טיפוסיות:** item condition, buttons, locks, triggers, HUD, trackers, timers, character presentation, environmental feedback.
- **סיכוני מצב:** one-time clue loss, blocked reset, global progress affecting wrong player, late joiner missing prior state.
- **ערך קהילתי:** puzzle tropes, room sequence patterns, visual clue conventions and common Device combinations.
- **גבול רשמי:** every Device and connection must be verified separately.

## Deathrun

- **Pattern ID:** `creative.genre.deathrun`
- **Core Loop:** navigate obstacle → avoid or recover from failure → reach checkpoint → continue.
- **מערכות טיפוסיות:** checkpoints, movement modifiers, hazards, timers, score or progression feedback.
- **ערך קהילתי:** course readability, checkpoint density, difficulty pacing and visual language.
- **התאמת בטיחות:** for educational or nonviolent contexts, frame as obstacle course or traversal challenge.

## Zone Wars

- **Pattern ID:** `creative.genre.zone_wars`
- **Core Loop:** survive or compete while the playable area changes.
- **ערך קהילתי:** community terminology and structural expectations.
- **גבול המלצה בקורפוס:** do not recommend combat as the default user-facing concept; preserve only technical or genre-recognition value.
- **אימות רשמי:** storm, scoring, teams, spawning and end conditions require official sources.

## Tycoon

- **Pattern ID:** `creative.genre.tycoon`
- **Core Loop:** collect → purchase → unlock → automate or expand → repeat.
- **מצב טיפוסי:** currency, ownership, unlock progression, production rate, saved or round-based progress.
- **משפחות מערכת טיפוסיות:** item or resource systems, Conditional Button, Tracker or Stat systems, route control, Save Point where supported.
- **ערך קהילתי:** progression structures, purchase pads, visual upgrade chains and common economy layouts.
- **סיכונים:** duplicate purchase, global state used instead of player state, persistence mismatch, join-in-progress inconsistency.

## Prop Hunt

- **Pattern ID:** `creative.genre.prop_hunt`
- **Core Loop:** disguise or hide → search → reveal or resolve round.
- **ערך קהילתי:** map readability, prop density, seeker route design and fairness conventions.
- **גבול:** exact disguise, team, camera, round and scoring behavior must be verified officially.

## Parkour / מסלול מכשולים

- **Pattern ID:** `creative.genre.parkour_obstacle_course`
- **Core Loop:** observe route → execute movement → recover or reach checkpoint → continue.
- **ערך קהילתי:** obstacle vocabulary, visual cues, difficulty pacing, shortcut prevention and checkpoint placement.
- **נגישות:** do not rely on hidden controls, color-only cues or excessively narrow timing without alternatives.

## מרכז חברתי

- **Pattern ID:** `creative.genre.social_hub`
- **Core Loop:** enter → navigate activities → interact → regroup or choose next activity.
- **מערכות טיפוסיות:** spawn, signage, teleportation, voting, activities, feedback and role-free participation.
- **ערך קהילתי:** spatial zoning, landmark conventions and activity discovery.

## מרוץ

- **Pattern ID:** `creative.genre.racing`
- **Core Loop:** start → follow route → pass checkpoints → finish → compare result or continue.
- **ערך קהילתי:** track readability, checkpoint placement, reset patterns and route theming.
- **גבול:** vehicle, checkpoint, lap, timer and scoring details require official sources.

---

# מינוח קהילתי

## מטרת המינוח הקהילתי

לשמור aliases ו־community terms שמסייעים לחיפוש, בלי להחליף מונחים רשמיים.

## מבנה מונח

```yaml
term_id: "creative.community_term.<slug>"
community_term: ""
official_or_canonical_term: ""
definition: ""
environment: ""
related_entities: []
search_aliases: []
legacy_status: "current | legacy | ambiguous | unresolved"
```

## כללי Alias

- Channel terminology יסומן Legacy כאשר הוא אינו מתאר את המערכת הנוכחית.
- Device names ישנים אינם מוחקים את השם הנוכחי.
- שם Category אינו בהכרח שם מוצר רשמי.
- שמות מקוצרים כגון `RNG`, `HUD`, `JIP` או `DBNO` צריכים להפנות למונח המלא.
- מונח קהילתי אינו מוכיח קיום של feature רשמי.

---

# גבול UEFN

## כלל הפרדה

עמוד Wiki יכול להתייחס ל־Creative וגם ל־UEFN. אין לערבב את ההוראות.

### מידע שניתן לשמור מעמוד UEFN

- page discovery;
- visual mapping;
- community naming;
- relationship to a Creative Device;
- community-reported differences, מסומנות לבדיקה;
- asset references.

### מידע שאסור ללמוד מהעמוד כתחליף לקורפוס UEFN

- editor workflow;
- Content Browser paths;
- asset import rules;
- Scene Graph behavior;
- NPC Character Definition workflow;
- Conversation authoring;
- UMG behavior;
- Sequencer workflow;
- validation;
- memory calculation;
- publishing;
- Verse API.

המידע הזה עובר ל־`08`–`10` ולמסמכי Verse המתאימים.

---

# חלוקה למקטעים ומבנה Markdown

## כללי כותרות

- `#` משמש רק לכותרת המסמך ולחלקים ראשיים גדולים.
- `##` משמש לנושא מרכזי.
- `###` משמש לישות, משפחה או דפוס.
- `####` משמש לשדות פנימיים של ישות כאשר נדרש.
- אין ליצור שתי כותרות זהות באותו מסמך.
- לכל ישות יש `Entity ID` ייחודי.

## כללי שורה

- כל claim בשורה נפרדת.
- כל Device name בשדה נפרד.
- כל tag בשורה או פריט נפרד.
- אין לכתוב פסקאות ארוכות שמערבבות מספר טענות.
- אין לשלב official fact ו־community claim באותו משפט ללא סימון מפורש.
- URLs נשמרים בשדות קבועים.

## גודל Chunk מומלץ

כל entity record צריך להיות עצמאי ככל האפשר ולכלול:

- identity;
- source;
- environment;
- official owner;
- community delta;
- confidence;
- verification status;
- agent-use rules.

אין להסתמך על פסקה רחוקה במסמך כדי להבין את סמכות הרשומה.

---

# כללי שליפה לסוכן

## לפני תשובה

1. לזהות environment.
2. לזהות primary owner document.
3. להשתמש במסמך זה רק אם נדרש community delta.
4. לאתר source page ו־confidence.
5. לבדוק אם הטענה version-sensitive.
6. לאמת טענה טכנית ב־Epic כאשר אפשר.

## בעת תשובה

- לציין כאשר מידע מגיע מהקהילה.
- להשתמש בשמות הטכניים באנגלית.
- לא להציג Memory קהילתי כמספר מובטח.
- לא להציג bug report כאמת אוניברסלית.
- לא להציג UEFN-only feature בפתרון Creative.
- לא לשלב Events או Functions בלתי מאומתים.
- כאשר אין מקור מספיק, לומר שהמידע אינו נתמך.

## בעת בניית מערכת

1. להגדיר player goal.
2. לזהות state owner.
3. לבחור Devices כמועמדים.
4. לאמת כל Device בנפרד.
5. לכתוב כל Event → Function connection בנפרד.
6. להגדיר reset.
7. להגדיר failure path.
8. לבצע Play Mode test.

---

# הסגר מידע

## מתי להעביר Claim להסגר

- סתירה עם Epic.
- עמוד המשתמש ב־legacy Channels בלי הקשר.
- Memory value ללא סוג מדידה.
- feature availability לא ברורה.
- page title שמתנגש עם cosmetic או Battle Royale term.
- UEFN ו־Creative מעורבבים.
- Event או Function שאינם נמצאים במקור הרשמי.
- claim שמבוסס רק על comment.
- claim ללא page או revision reference.
- claim מתוך removed או developer-only content.
- מידע שנראה מיושן.

## מבנה Quarantine Record

```yaml
quarantine_id: "community.quarantine.<slug>"
entity_id: ""
claim_text: ""
source_url: ""
reason: "official_conflict | legacy | ambiguous_environment | unsupported | stale | incomplete | other"
official_comparison: ""
required_action: "verify | test | reject | rewrite | reclassify"
status: "open | resolved | rejected"
```

---

# רישוי וייחוס

## כללי ייחוס

לכל רשומה המבוססת על Fortnite Wiki יש לשמור:

- page title;
- page URL;
- revision ID כאשר ניתן;
- retrieval date;
- attribution note;
- transformation summary כאשר הטקסט עובד או סוכם.

## טקסט ייחוס מומלץ

```text
Community-derived metadata was adapted from the cited Fortnite Wiki page on Fandom.
The source page and revision are recorded for attribution and auditability.
```

## מגבלות מדיה

- אין להניח שרישיון הטקסט חל גם על כל image, logo, screenshot או game asset.
- אין להטמיע קבצי מדיה בקורפוס ללא בדיקת page-level file license.
- אפשר לשמור URL לתמונה לצורך visual reference, אך לא להעתיק אותה אוטומטית.
- תוכן Epic נשאר כפוף לזכויות ולכללי השימוש של Epic Games.

---

# מרשם מקורות

| Source | URL | Role | Authority |
|---|---|---|---|
| Fortnite: Creative | https://fortnite.fandom.com/wiki/Fortnite:_Creative | Community entry point | Secondary |
| Category:Creative | https://fortnite.fandom.com/wiki/Category:Creative | Broad discovery | Secondary |
| Category:Devices | https://fortnite.fandom.com/wiki/Category:Devices | Device discovery | Secondary |
| Devices (Creative) | https://fortnite.fandom.com/wiki/Devices_(Creative) | Community Device list | Secondary |
| Category:Prefabs | https://fortnite.fandom.com/wiki/Category:Prefabs | Prefab discovery | Secondary |
| Prefabs | https://fortnite.fandom.com/wiki/Prefabs | Prefab hub | Secondary |
| Category:Galleries | https://fortnite.fandom.com/wiki/Category:Galleries | Gallery discovery | Secondary |
| Category:UEFN | https://fortnite.fandom.com/wiki/Category:UEFN | UEFN-related discovery | Secondary |
| Content Browser | https://fortnite.fandom.com/wiki/Content_Browser | Community interface mapping | Secondary |
| Characters (Creative) | https://fortnite.fandom.com/wiki/Characters_(Creative) | Visual character mapping | Secondary |
| Developer Features (Creative) | https://fortnite.fandom.com/wiki/Developer_Features_(Creative) | Restricted-feature discovery | Secondary / quarantine by default |
| Objective Device Gallery | https://fortnite.fandom.com/wiki/Objective_Device_Gallery | Example Gallery page | Secondary |
| Unreal Editor for Fortnite | https://fortnite.fandom.com/wiki/Unreal_Editor_for_Fortnite | Community UEFN overview | Secondary |

---

# בדיקות איכות ותנאי קבלה

## בדיקת הסכמה

- [ ] לכל רשומה יש `entity_id`.
- [ ] לכל רשומה יש `entity_type`.
- [ ] לכל רשומה יש `source_url`.
- [ ] לכל רשומה יש environment classification.
- [ ] לכל רשומה יש official owner.
- [ ] לכל רשומה יש confidence.
- [ ] לכל רשומה יש verification status.
- [ ] לכל רשומה יש allowed use ו־prohibited use.

## בדיקת Devices

- [ ] אין Event שמוצג כמאומת מתוך ה־Wiki בלבד.
- [ ] אין Function שמוצגת כמאומתת מתוך ה־Wiki בלבד.
- [ ] אין option value שמחליף את המקור הרשמי.
- [ ] legacy Channels מסומנים.
- [ ] כל Device משויך ל־`05`, `06` או `09`.
- [ ] UEFN-only או restricted entities מסומנים.

## בדיקת Prefabs ו־Galleries

- [ ] theme tags משתמשים ב־taxonomy הקבועה.
- [ ] asset contents אינם מומצאים.
- [ ] visual inspection מסומן כאשר נדרש.
- [ ] availability אינה מוצגת כעובדה ללא אימות.
- [ ] Memory כולל `cost_type`.

## בדיקת טענות

- [ ] לכל bug claim יש expected ו־reported result.
- [ ] לכל claim יש מקור.
- [ ] version-sensitive claim אינו מוצג כיציב.
- [ ] official conflict מועבר ל־Quarantine.
- [ ] comments אינם משמשים כמקור יחיד לעובדה פעילה.

## בדיקת סינון שלילי

- [ ] אין cosmetics.
- [ ] אין weapon statistics.
- [ ] אין lore.
- [ ] אין update history.
- [ ] אין vaulted/unvaulted tracking.
- [ ] אין Item Shop data.
- [ ] אין unrelated Battle Royale content.

## בדיקת שליפה

יש לבדוק את השאלות הבאות:

1. "איזה Gallery מתאים לטירה מימי הביניים?"
2. "מה ההבדל בין Community memory report לבין Creative memory calculation?"
3. "האם אפשר להשתמש ב־Conversation Device ב־Creative?"
4. "איזה Devices מקובל לשלב כדי לפתוח דלת עם פריט?"
5. "Fortnite Wiki מציג Event שאינו מופיע ב־Epic. במה להשתמש?"
6. "תן רשימת props מדויקת ב־Gallery שאין לה inventory מלא."
7. "האם bug קהילתי מסוים עדיין קיים?"
8. "איזה Prefab מתאים ל־social hub?"
9. "האם Category:UEFN אומר שהישות UEFN-only?"
10. "מהו player limit הנוכחי?"

תשובה תקינה חייבת:

- להעדיף Epic בטענות טכניות;
- לסמן community claims;
- לא להמציא asset contents;
- לא להבטיח Memory;
- לדרוש אימות נוכחי עבור limits ו־availability.

---

# תבנית דוח כיסוי

```yaml
coverage_report:
  generated_at: "YYYY-MM-DD"
  seed_categories_scanned: 0
  subcategories_scanned: 0
  pages_discovered: 0
  pages_parsed: 0
  redirects_resolved: 0
  entities_included: 0
  entities_excluded: 0
  entities_quarantined: 0
  devices_included: 0
  prefabs_included: 0
  galleries_included: 0
  genre_patterns_included: 0
  combo_patterns_included: 0
  pages_with_infobox: 0
  pages_with_memory_data: 0
  memory_records_without_cost_type: 0
  pages_with_legacy_terminology: 0
  unresolved_environments: 0
  official_conflicts: 0
  claims_tested_in_session: 0
  broken_source_urls: 0
```

---

# כללי תחזוקה

## עדכון מחזורי

בעת רענון:

1. לסרוק מחדש Seed categories.
2. לזהות added, removed, renamed ו־redirected pages.
3. להשוות revision IDs.
4. לעבד רק עמודים שהשתנו.
5. להריץ Negative Filtering.
6. להשוות שוב למקורות Epic.
7. להעביר סתירות ל־Quarantine.
8. לעדכן Coverage Report.
9. להריץ regression questions.

## שינויי שמות

- לשמור `canonical_name` נוכחי.
- לשמור שמות קודמים תחת `community_aliases`.
- Redirect אינו יוצר ישות כפולה.
- duplicate pages מתמזגים לפי Entity ID.

## Staleness

עמוד ייחשב high-risk כאשר:

- הוא משתמש ב־Channels;
- הוא מזכיר UI ישן;
- אין לו revision עדכני;
- הוא סותר current Epic page;
- הוא מתאר removed או developer-only content;
- אין בו environment boundary;
- הוא כולל Memory ללא הקשר;
- הוא מציג Events או Functions חלקיים.

## מחיקת מידע

מידע אינו נמחק רק משום שהוא ישן. יש להעבירו לאחד מהמצבים:

- `legacy`
- `quarantine`
- `rejected`
- `historical_excluded`

המידע לא יישאר ב־active retrieval כאשר הוא אינו תקף לבנייה נוכחית.

---

# שילוב בקורפוס

## תפקיד קובץ 19

קובץ זה הוא owner עבור:

- Fortnite Wiki source map;
- community entity indexing rules;
- community confidence model;
- community metadata schemas;
- Prefab ו־Gallery thematic tags;
- community combo records;
- community genre patterns;
- undocumented behavior claims;
- community Memory reports;
- Wiki-specific licensing and attribution.

קובץ זה אינו owner עבור:

- official Device behavior;
- official Creative instructions;
- official UEFN workflows;
- Verse;
- publishing;
- current limits;
- educational theory.

## עדכונים נדרשים בקבצי הניתוב

בעת הוספת קובץ זה לסט הפעיל, יש לעדכן:

- `00_MASTER_KNOWLEDGE_INDEX.md`: מספר הקבצים הפעילים מ־19 ל־20, טווח IDs מ־`00–18` ל־`00–19`, והוספת route עבור community Wiki delta.
- `01_EPIC_GAMES_DOCUMENTATION_INDEX.md`: אין לשנות את סמכותו; אפשר להוסיף cross-reference שקובץ `19` הוא מקור קהילתי בלבד.
- `04`, `05`, `06`, `07`: אפשר להוסיף Secondary Support reference לקובץ `19`, ללא שינוי ב־official authority.
- `15_GLOSSARY_FORTNITE_CREATIVE.md`: אפשר להוסיף הפניה ל־community aliases בלבד.
- `18`: אפשר להשתמש ב־community combinations כרעיונות, אך כל technical claim ממשיך לעבור ל־`03–07`.

## כלל אי־שכפול

כאשר מידע כבר נמצא בקובץ רשמי:

- אין להעתיק אותו במלואו לקובץ `19`.
- יש לשמור source link, delta summary ו־official owner.
- יש להשאיר את ההסבר המלא אצל owner המקורי.

---

# חוזה השימוש הסופי של הסוכן

הסוכן חייב לפעול לפי הכללים הבאים:

1. מסמך זה הוא מקור קהילתי משני.
2. Epic Games תמיד גובר על Fortnite Wiki.
3. אין לקחת Events, Functions, options, limits או availability מה־Wiki ללא אימות.
4. יש להשתמש ב־Wiki בעיקר לגילוי, visual mapping, asset composition, tags, combos, jargon ו־reported behaviors.
5. כל Memory value קהילתי הוא provisional.
6. כל bug או undocumented behavior הוא claim עד לבדיקתו.
7. כל Device במערכת מרובת Devices מאומת בנפרד.
8. Creative, UEFN ו־Verse נשמרים כסביבות נפרדות.
9. כאשר המידע אינו נתמך, יש לומר זאת ולא להשלים את החסר.
10. כל תשובת יישום מסתיימת בבדיקת Play Mode או launched session מתאימה.

