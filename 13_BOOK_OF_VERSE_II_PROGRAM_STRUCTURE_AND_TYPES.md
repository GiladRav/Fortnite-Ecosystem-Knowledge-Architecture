# Book of Verse II: Program Structure and Types

## Document Metadata

| Field | Value |
|---|---|
| Document ID | `13` |
| Domain | Verse language |
| Primary Environment | Verse; UEFN availability must be verified |
| Language | English; Hebrew appears only in canonical terminology fields and exact source identifiers |
| Source Priority | Epic official documentation -> current Book of Verse -> verified local technical corpus -> research and teaching sources |
| Last Verified | 2026-08-02 |
| Stability Status | Mixed: stable concepts plus version-sensitive interface and platform details |

## Document Purpose

Preserve Book of Verse structures, enums, classes, interfaces, type system, access specifiers, modules, and paths.

## Scope and Exclusions

Program-structure semantics only; effects/concurrency/persistence belong to `14`.

## When to Use This Document

- Use for data modeling, inheritance, interfaces, types, visibility, modules, and code organization.

## Authority and Availability Gate

Book of Verse can describe language-main or planned functionality before it is available in the current UEFN release. It is not the final authority for current compiler availability. Before giving executable guidance:

1. confirm the construct in Epic's current Verse Language Reference or Verse API Reference;
2. confirm that the active UEFN compiler accepts it;
3. label material that is absent from the official current reference as **experimental or unreleased**;
4. never present a Book of Verse example as guaranteed shipping UEFN behavior solely because it appears in this document.

`Live Variables` and any dependent reactive constructs are treated as unreleased until Epic's current UEFN documentation and compiler confirm otherwise.

## Quick Topic Index

- [Book of Verse Source Unit: 09_structs_enums.md](#book-of-verse-source-unit-09structsenumsmd)
- [Book of Verse Source Unit: 10_classes_interfaces.md](#book-of-verse-source-unit-10classesinterfacesmd)
- [Book of Verse Source Unit: 11_types.md](#book-of-verse-source-unit-11typesmd)
- [Book of Verse Source Unit: 12_access.md](#book-of-verse-source-unit-12accessmd)
- [Book of Verse Source Unit: 16_modules.md](#book-of-verse-source-unit-16modulesmd)

## Common Question Router

- For environment selection and corpus-wide routing, start with [`00_MASTER_KNOWLEDGE_INDEX.md`](00_MASTER_KNOWLEDGE_INDEX.md).
- For current official URLs or version-sensitive claims, use [`01_EPIC_GAMES_DOCUMENTATION_INDEX.md`](01_EPIC_GAMES_DOCUMENTATION_INDEX.md).
- For a simple English-to-Hebrew name mapping, use [`02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md`](02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md).
- For a detailed definition, use the domain glossary listed under Related Glossary Sections.

---

## Book of Verse Source Unit: 09_structs_enums.md

### Structs and Enums

Structs and enums represent Verse's value-oriented type system, providing lightweight alternatives to classes for simple data aggregation and fixed sets of named values. Unlike classes with their object-oriented features, structs and enums focus on simplicity, immutability, and value semantics.

Structs bundle related data without methods or inheritance, perfect for mathematical types, configuration data, and simple records. Enums define fixed sets of named constants, replacing magic numbers with meaningful names and providing compile-time safety through exhaustive pattern matching.

Together, structs and enums complement classes and interfaces by offering simpler, more constrained type constructors optimized for specific use cases.

#### Structs

Structs provide lightweight data containers without the object-oriented features of classes. They're value types optimized for simple data aggregation, making them perfect for mathematical types, data transfer objects, and any scenario where you need a simple bundle of related values without behavior.

Structs group related data with minimal overhead:

<!--NoCompile-->
<!-- 01 -->
```verse
damage_type:= enum:
    Physical
character := struct{}
vector2 := struct:
    X : float = 0.0
    Y : float = 0.0

color := struct:
    R : int = 0
    G : int = 0
    B : int = 0
    A : int = 255  # Alpha channel

damage_info := struct:
    Amount : int = 0
    Type : damage_type = damage_type.Physical
    Source : ?character = false
    IsCritical : logic = false
```

All struct fields are public and immutable by default. Structs cannot have methods, constructors, or participate in inheritance hierarchies. This simplicity makes them efficient and predictable.

##### Construction

Creating struct instances uses the same archetype syntax as classes:

<!--versetest
vector2 := struct:
    X : float = 0.0
    Y : float = 0.0

color := struct:
    R : int = 0
    G : int = 0
    B : int = 0
    A : int = 255
-->
<!-- 02 -->
```verse
Origin := vector2{}  # Uses defaults: (0.0, 0.0)
PlayerPos := vector2{X := 100.0, Y := 250.0}
RedColor := color{R := 255}  # Other channels default to 0/255

# Structs are values - assignment creates a copy
NewPos := PlayerPos
# NewPos is a separate instance with the same values
```

Since structs are value types, assigning a struct to a variable creates a copy of all its data. This differs from classes, which use reference semantics.

##### Comparison

Structs with all comparable fields support equality comparison:

<!--versetest
vector3i := struct:
    X : int = 0
    Y : int = 0
    Z : int = 0

PrintMsg(S:string)<transacts>:void = {}

M()<transacts>:void =
    Origin := vector3i{}
    UnitX := vector3i{X := 1}

    if (Origin = vector3i{}):
        PrintMsg("At origin")

    if (Origin = UnitX):
        PrintMsg("Same position")
<#
-->
<!-- 03 -->
```verse
vector3i := struct:
    X : int = 0
    Y : int = 0
    Z : int = 0

Origin := vector3i{}
UnitX := vector3i{X := 1}

if (Origin = vector3i{}):  # Succeeds - all fields match
    Print("At origin")

if (Origin = UnitX):  # Fails - X fields differ
    Print("Same position")
```
<!-- #> -->

Comparison happens field by field, succeeding only if all corresponding fields are equal.

##### Persistable Structs

Structs can be marked as persistable for use with Verse's persistence system:

<!--versetest
player_stats := struct<persistable>:
    HighScore : int = 0
    GamesPlayed : int = 0
    WinRate : float = 0.0

player := class<concrete><unique>{}

PlayerData : weak_map(player, player_stats) = map{}
<#
-->
<!-- 04 -->
```verse
player_stats := struct<persistable>:
    HighScore : int = 0
    GamesPlayed : int = 0
    WinRate : float = 0.0

# Can be used in persistent storage
PlayerData : weak_map(player, player_stats) = map{}
```
<!-- #> -->

Once published, persistable structs cannot be modified, ensuring data compatibility across game updates.

##### Parametric Structs

Like classes, structs can be parametric (generic). A parametric struct declares one or more type parameters, allowing the same struct definition to work with different types. This is useful when you want a lightweight value type that is reusable across different data types without defining a full class.

A parametric struct takes type parameters in its definition, just like a parametric class:

<!--NoCompile-->
<!-- 05 -->
```verse
# A wrapper that can hold a value of any type
wrapper(t:type) := struct:
    Value : t
```

The type parameter `t` can be used anywhere a concrete type would appear in field declarations. When creating instances, you provide the concrete type:

<!--versetest
wrapper(t:type) := struct:
    Value : t
-->
<!-- 06 -->
```verse
IntWrapped := wrapper(int){Value := 42}
FloatWrapped := wrapper(float){Value := 3.14}
StringWrapped := wrapper(string){Value := "hello"}
```

Parametric structs work naturally with parametric functions. A function can accept any instantiation of a parametric struct by using a `where` clause to capture the type parameter:

<!--versetest
wrapper(t:type) := struct:
    Value : t
-->
<!-- 07 -->
```verse
Unwrap(W:wrapper(t) where t:type):t = W.Value

IntValue := Unwrap(wrapper(int){Value := 10})       # IntValue is 10
FloatValue := Unwrap(wrapper(float){Value := 2.0})   # FloatValue is 2.0
```

Since the type parameter is preserved through instantiation, parametric structs can be nested. Here, a `wrapper` holds another `wrapper` as its value:

<!--versetest
wrapper(t:type) := struct:
    Value : t

Unwrap(W:wrapper(t) where t:type):t = W.Value
-->
<!-- 08 -->
```verse
Nested := wrapper(wrapper(int)){Value := wrapper(int){Value := 11}}
Inner := Unwrap(Nested)      # Inner is wrapper(int){Value := 11}
Result := Unwrap(Inner)      # Result is 11
```

Parametric structs retain all the characteristics of regular structs — they are value types with public, immutable fields and no methods or inheritance. The only addition is the ability to parameterize field types. Note that parametric structs cannot be marked `<persistable>` — persistence requires concrete, fixed types that can be serialized reliably across game updates.

#### Enums

Enums define types with a fixed set of named values, perfect for representing states, types, or any concept with a known, finite set of alternatives. They make code more readable by replacing magic numbers with meaningful names and provide compile-time safety by restricting values to the defined set.

An enum lists all possible values for a type:

<!--NoCompile-->
<!-- 05 -->
```verse
game_state := enum:
    MainMenu
    Playing
    Paused
    GameOver

damage_type := enum:
    Physical
    Fire
    Ice
    Lightning
    Poison

direction := enum:
    North
    East
    South
    West
```

Each value in the enum becomes a named constant of that enum type. The compiler ensures that variables of an enum type can only hold one of these defined values. Enums can even be empty:

<!--versetest
placeholder := enum{}
<#
-->
<!-- 06 -->
```verse
placeholder := enum{}  # Valid but rarely useful
```
<!-- #> -->

Enums introduce both a type and a set of values, and it is crucial to distinguish between them:

<!--versetest
status := enum:
    Active
    Inactive


CurrentStatus:status = status.Active
<#
-->
<!-- 07 -->
```verse
status := enum:
    Active
    Inactive

# status is the TYPE
# status.Active and status.Inactive are VALUES

CurrentStatus:status = status.Active  # OK - value of type status
```
<!-- #> -->

You cannot use the enum type where a value is expected:

<!--versetest
status := enum:
    Active
    Inactive

M()<transacts>:void =
    GoodAssignment:status = status.Active
    var CurrentStatus:status = status.Active
    set CurrentStatus = status.Inactive
<#
-->
<!-- 08 -->
```verse
# ERROR: Cannot use type as value
BadAssignment:status = status  # Compile error
set CurrentStatus = status     # Compile error

# CORRECT: Use enum values
GoodAssignment:status = status.Active  # OK
set CurrentStatus = status.Inactive    # OK
```
<!-- #> -->

This distinction prevents confusion and ensures type safety. The enum type defines what values are possible, while enum values are the actual constants you use in your code.

##### Restrictions

Enums have specific syntactic requirements that keep their usage clear and unambiguous:

**Enums must be direct right-hand side of definitions:**

<!--versetest
priority := enum:
    Low
    Medium
    High
<#
-->
<!-- 09 -->
```verse
# Valid
priority := enum:
    Low
    Medium
    High

# Invalid - cannot use enum in expressions
Result := -enum{A, B}      # Compile error
value := enum{X, Y} + 1    # Compile error
```
<!-- #> -->

**Enums must be module or class-level definitions:**

<!--versetest
my_enum := enum:
    Value1
    Value2

ProcessData():void = {}
<#
-->
<!-- 10 -->
```verse
# Valid
my_enum := enum:
    Value1
    Value2

# Invalid - cannot define local enums
ProcessData():void =
    LocalEnum := enum{A, B}  # Compile error - no local enums
```
<!-- #> -->

These restrictions ensure enums remain stable, referenceable definitions throughout your codebase rather than ephemeral local values.

##### Using Enums

Enums provide type-safe alternatives to error-prone string or integer constants:

<!--versetest
game_state := enum:
    MainMenu
    Playing
    Paused
    GameOver
-->
<!-- 11 -->
```verse
var CurrentState:game_state = game_state.MainMenu

ProcessInput(Input:string):void =
    case (CurrentState):
        game_state.MainMenu =>
            if (Input = "Start"):
                set CurrentState = game_state.Playing
        game_state.Playing =>
            if (Input = "Pause"):
                set CurrentState = game_state.Paused
        game_state.Paused =>
            if (Input = "Resume"):
                set CurrentState = game_state.Playing
            else if (Input = "Quit"):
                set CurrentState = game_state.MainMenu
        game_state.GameOver =>
            if (Input = "Restart"):
                set CurrentState = game_state.MainMenu
```

The `case` expression with enums provides powerful pattern matching with exhaustiveness checking that ensures you handle all possible values correctly.

##### Open vs Closed Enums

Enums can be marked as open or closed, fundamentally affecting how they can evolve and how they interact with pattern matching:

<!--NoCompile-->
<!-- 12 -->
```verse
# Closed enum - cannot add values after publication
day_of_week := enum<closed>:  # <closed> is the default
    Monday
    Tuesday
    Wednesday
    Thursday
    Friday
    Saturday
    Sunday

# Open enum - can add new values after publication
weapon_type := enum<open>:
    Sword
    Bow
    Staff
    # Can add Wand, Dagger, etc. in updates
```

**Closed enums** (the default) commit to a fixed set of values forever. This allows the compiler to verify that case expressions handle all possibilities exhaustively. Use closed enums for truly fixed sets: days of the week, cardinal directions, fundamental game states.

**Open enums** allow new values to be added in future versions. This flexibility comes at a cost: case expressions cannot be exhaustive since future values might exist. Use open enums for extensible sets: item types, enemy types, damage types, or any content that may grow.

##### Exhaustiveness

The interaction between enum types and case expressions follows sophisticated rules that prevent bugs while enabling both safety and flexibility. Understanding these rules is essential for working with enums effectively.

**Closed Enums with Full Coverage:**

When your case expression handles every value in a closed enum, no wildcard is needed:

<!--NoCompile-->
<!-- 13 -->
```verse
day := enum:
    Monday
    Tuesday
    Wednesday

# Exhaustive - all values covered
GetDayType(D:day):string =
    case (D):
        day.Monday => "Weekday"
        day.Tuesday => "Weekday"
        day.Wednesday => "Weekday"
    # No wildcard needed - all values handled
```

Adding a wildcard when all cases are covered triggers an unreachable code warning:

<!--versetest
day := enum:
    Monday
    Tuesday
    Wednesday

GetDayType(D:day):string =
    case (D):
        day.Monday => "Weekday"
        day.Tuesday => "Weekday"
        day.Wednesday => "Weekday"
<#
-->
<!-- 14 -->
```verse
# Warning: unreachable wildcard
GetDayType(D:day):string =
    case (D):
        day.Monday => "Weekday"
        day.Tuesday => "Weekday"
        day.Wednesday => "Weekday"
        _ => "Unknown"  # WARNING: unreachable - all values already matched
```
<!-- #> -->

**Closed Enums with Partial Coverage:**

If you do not match all values, you must either provide a wildcard or be in a `<decides>` context:

<!--NoCompile-->
<!-- 15 -->
```verse
day := enum:
    Monday
    Tuesday
    Wednesday
    Thursday

# With wildcard - OK
GetWeekStartWildCard(D:day):string =
    case (D):
        day.Monday => "Week start"
        _ => "Mid-week"

# Without wildcard but in <decides> context - OK
GetWeekStartDecides(D:day)<decides>:string =
    case (D):
        day.Monday => "Week start"
        # Missing other days causes failure

# Without either - COMPILE ERROR
# GetWeekStartBad(D:day):string =
#    case (D):
#        day.Monday => "Week start"
#        # ERROR: Missing cases and no wildcard
```

**Open Enums Always Require Wildcard or `<decides>`:**

Open enums can have new values added after publication, so they can never be exhaustive.\
This is to ensure backwards compatibility of functions using them (see also [Publishing Functions](12_BOOK_OF_VERSE_I_FOUNDATIONS.md#publishing-functions)):

<!--NoCompile-->
<!-- 16 -->
```verse
weapon := enum<open>:
    Sword
    Bow
    Staff

# Must have wildcard - OK
GetWeaponClassWildCard(W:weapon):string =
    case (W):
        weapon.Sword => "Melee"
        weapon.Bow => "Ranged"
        weapon.Staff => "Magic"
        _ => "Unknown"  # REQUIRED - future values may exist

# In <decides> context without wildcard - OK
GetWeaponClassDecides(W:weapon)<decides>:string =
    case (W):
        weapon.Sword => "Melee"
        weapon.Bow => "Ranged"
        weapon.Staff => "Magic"
        # Can fail for unknown (future) values

# Without either - COMPILE ERROR
# GetWeaponClassBad(W:weapon):string =
#    case (W):
#        weapon.Sword => "Melee"
#        weapon.Bow => "Ranged"
#        weapon.Staff => "Magic"
#        # ERROR: Open enum requires wildcard or <decides>
```

Even if you match all currently defined values in an open enum, you still need a wildcard or `<decides>` context because new values might be added in future versions.

**Summary of Exhaustiveness Rules:**

| Enum Type | Case Coverage | Wildcard | Context | Result |
|-----------|---------------|----------|---------|--------|
| Closed | Full | No | Any | ✓ Valid - exhaustive |
| Closed | Full | Yes | Any | ⚠ Warning - unreachable wildcard |
| Closed | Partial | Yes | Any | ✓ Valid |
| Closed | Partial | No | `<decides>` | ✓ Valid - unmatched values fail |
| Closed | Partial | No | Non-`<decides>` | ✗ Error - missing cases |
| Open | Any | Yes | Any | ✓ Valid |
| Open | Any | No | `<decides>` | ✓ Valid - unmatched values fail |
| Open | Any | No | Non-`<decides>` | ✗ Error - open enum needs wildcard |

These rules ensure that closed enums provide safety through exhaustiveness while open enums require explicit handling of unknown values.

##### Unreachable Case Detection

The compiler actively detects unreachable cases in case expressions, helping you identify dead code and logic errors:

**Duplicate cases** are flagged as unreachable:

<!--versetest
status := enum:
    Active
    Inactive
    Pending

GetStatusCode(S:status):int =
    case (S):
        status.Active => 1
        status.Inactive => 2
        status.Pending => 3
<#
-->
<!-- 17 -->
```verse
status := enum:
    Active
    Inactive
    Pending

# ERROR: Duplicate case is unreachable
GetStatusCode(S:status):int =
    case (S):
        status.Active => 1
        status.Inactive => 2
        status.Pending => 3
        status.Pending => 4  # ERROR: unreachable - already matched above
```
<!-- #> -->

**Cases after wildcards** are always unreachable:

<!--versetest
status := enum:
    Active
    Inactive
    Pending

GetStatusCode(S:status):int =
    case (S):
        status.Active => 1
        _ => 0
<#
-->
<!-- 18 -->
```verse
# ERROR: Case after wildcard
GetStatusCode(S:status):int =
    case (S):
        status.Active => 1
        _ => 0  # Wildcard matches everything
        status.Inactive => 2  # ERROR: unreachable - wildcard already matched
```
<!-- #> -->

These errors prevent logic bugs where you think you are handling specific cases but the code will never execute.

##### The `@ignore_unreachable` Attribute

Sometimes you intentionally want unreachable cases—for testing, migration, or defensive programming. The `@ignore_unreachable` attribute suppresses unreachable warnings and errors for specific cases:

<!--NoCompile-->
<!-- 19 -->
```verse
status := enum:
    Active
    Inactive

ProcessStatus(S:status):int =
    case (S):
        status.Active => 1
        status.Inactive => 2
        @ignore_unreachable status.Inactive => 3  # No error
        @ignore_unreachable _ => 0  # No unreachable warning
```

This attribute only affects cases it is applied to. Other unreachable cases without the attribute still produce errors:

<!--versetest
status := enum:
    Active
    Inactive

ProcessStatus(S:status):int =
    case (S):
        status.Active => 1
        status.Inactive => 2
        @ignore_unreachable status.Inactive => 3
<#
-->
<!-- 20 -->
```verse
ProcessStatus(S:status):int =
    case (S):
        status.Active => 1
        status.Inactive => 2
        @ignore_unreachable status.Inactive => 3  # Suppressed
        status.Active => 4  # ERROR: still unreachable without attribute
```
<!-- #> -->

Use `@ignore_unreachable` sparingly, primarily during refactoring or when maintaining multiple code paths for testing purposes.

##### Explicit Qualification

Enumerators can collide with identifiers in parent scopes. When this happens, you can use explicit qualification to disambiguate:

<!--NoCompile-->
<!-- 21 -->
```verse
# Top level 'Start'
Start:int = 0

# Enum wants to use 'Start' as enumerator
game_state := enum:
    (game_state:)Start  # Explicit qualification avoids collision
    Playing
    Paused

# Now both are accessible
OuterStart:int = Start             # References the int
StateStart:game_state = game_state.Start  # References the enum value
```

The syntax `(enum_name:)enumerator` explicitly qualifies the enumerator, preventing conflicts with outer-scope symbols.

**Using Reserved Words as Enum Values:**

Qualification also allows you to use reserved words and keywords as enum values, which would otherwise cause errors:

<!--NoCompile-->
<!-- 22 -->
```verse
# Using reserved words as enum values
keyword_enum := enum:
    (keyword_enum:)public    # OK: reserved word qualified
    (keyword_enum:)for       # OK: keyword qualified
    (keyword_enum:)class     # OK: reserved word qualified
    Regular                  # Normal enum value

# Without qualification - errors
# bad_enum := enum:
#    public    # Error: reserved word
#    for       # Error: reserved keyword
```

This is particularly useful when modeling language constructs, access levels, or any domain where reserved words make natural value names.

**Self-Referential Enum Values:**

You can even use the enum's own name as a value when qualified:

<!--NoCompile-->
<!-- 23 -->
```verse
recursive_enum := enum:
    (recursive_enum:)recursive_enum  # OK: qualified with enum name
    OtherValue

# Without qualification - error
# bad_recursive := enum:
  #  bad_recursive  # Error: shadows the type name
```

##### Comparison

Enum values are fully comparable, meaning they support both equality (`=`) and inequality (`<>`) operators. This makes them ideal for state tracking and conditional logic:

<!--versetest
weapon_type := enum:
    Sword
    Bow
    Staff

game_state := enum:
    MainMenu
    Playing
    Paused

PlaySwordAnimation()<transacts>:void = {}
OnStateChanged(Prev:game_state, Curr:game_state)<transacts>:void = {}
-->
<!-- 25 -->
```verse
CurrentWeapon := weapon_type.Sword
if (CurrentWeapon = weapon_type.Sword):
    PlaySwordAnimation()

CurrentState := game_state.Paused
PreviousState := game_state.Playing
if (CurrentState <> PreviousState):
    OnStateChanged(PreviousState, CurrentState)
```

Enum values from the same enum type can be compared, while values from different enum types are always unequal:

<!--versetest
letters := enum:
    A, B, C

numbers := enum:
    One, Two, Three

Test()<decides>:letters =
    letters.A = letters.A
    letters.A <> letters.B
    letters.A <> numbers.One
    letters.A
<#
-->
<!-- 26 -->
```verse
letters := enum:
    A, B, C

numbers := enum:
    One, Two, Three

Test()<decides>:letters =
    letters.A = letters.A    # Succeeds - same value
    letters.A <> letters.B   # Succeeds - different values
    letters.A <> numbers.One # Succeeds - different enum types
```
<!-- #> -->

Because enums are comparable, they can be used as map keys, stored in sets, and used with generic functions that require comparable types:

<!--versetest
game_state := enum{
    Menu
    Playing
    Paused
    GameOver
    Debug
    }
-->
<!-- 27 -->
```verse
# Enums as map keys
StateIDs:[game_state]int = map{
    game_state.Menu => 0,
    game_state.Playing => 1,
    game_state.Paused => 2
}

# In generic functions
FindStateID(States:[]game_state, Target:game_state):int =
    for (
        State : States, State = Target,
        ID := StateIDs[State]
    ):
        return ID
    -1 # Return -1 if state is not found
```

## Book of Verse Source Unit: 10_classes_interfaces.md

### Classes and Interfaces

Classes and interfaces are Verse's object-oriented building blocks.
Classes provide single inheritance with fields and methods, enabling
you to model hierarchies of game entities with shared behavior.
Interfaces define contracts for both data and behavior, supporting
multiple inheritance of specifications.

Together they provide is-a relationships (class inheritance) and
can-do contracts (interface implementation).

#### Classes

A class is a type that bundles data (fields) with operations
(methods). Class definitions must occur at module scope—you cannot
define a class inside another class, struct, interface, or function:

<!--versetest-->
<!-- 01-->
```verse
# Valid: class at module scope
MyModule := module:
    entity := class:
        ID:int

# Invalid: class inside another class
# outer := class:
#     inner := class:  # ERROR: classes must be at module scope
#         Value:int
```

<!--versetest-->
<!-- 02-->
```verse
character := class:
    Name : string
    var Health : int = 100
    var Level : int = 1
    MaxHealth : int = 100
```

Fields without `var` are immutable after construction. Fields with
`var` are mutable (see [Mutability](12_BOOK_OF_VERSE_I_FOUNDATIONS.md#book-of-verse-source-unit-05mutabilitymd)). Default values
enable convenient construction while ensuring valid initial states.

##### Object Construction

Creating instances of a class involves specifying values for its
fields through an archetype expression:

<!--versetest
character := class:
    Name : string
    var Health : int = 100
    var Level : int = 1
    MaxHealth : int = 100
	
Ignore:int=1
-->
<!-- 03-->
```verse
Hero := character{Name := "Aldric", Health := 100, Level := 5}
Villager := character{Name := "Martha"}  # default values for unspecified fields
```

Named parameters can appear in any order. Fields with defaults may be
omitted. Fields without defaults must be specified.

##### Methods

<!--versetest-->
<!-- 04-->
```verse
character := class:
    Name : string
    var Health : int = 100
    var Level : int = 1
    var MaxHealth : int = 100

    TakeDamage(Amount : int) : void =
        set Health = Max(0, Health - Amount)

    Heal(Amount : int) : void =
        set Health = Min(MaxHealth, Health + Amount)

    IsAlive()<decides>:void= Health > 0

    LevelUp() : void =
        set Level += 1
        set MaxHealth = 100 + (Level * 10)
        set Health = MaxHealth  # Full heal on level up
```

Methods have access to all fields of the class and can modify mutable
fields. They encapsulate the logic for how objects of the class should
behave, ensuring that state changes happen in controlled, predictable
ways.

All methods in non-abstract classes must have implementations. Unlike
interfaces (which can declare abstract methods), a concrete class
method declaration without an implementation is an error:

<!--versetest-->
<!-- 05-->
```verse
# Valid: method with implementation
valid_class := class:
    Compute():int = 42

# Invalid: method without implementation in concrete class
# invalid_class := class:
#     Compute():int  # ERROR: needs implementation
```

##### Blocks for Initialization

Classes can include `block` clauses that execute when an instance is
created:

<!--versetest
GetCurrentTime()<computes>:float=0.0

logged_entity := class:
    ID:int
    var CreationTime:float = 0.0

    block:
        # This executes when an instance is created
        Print("Creating entity with ID: {ID}")
        set CreationTime = GetCurrentTime()

M()<transacts>:void =
    Entity := logged_entity{ID := 42}
    # Prints: "Creating entity with ID: 42"
<#
-->
<!-- 06-->
```verse
logged_entity := class:
    ID:int
    var CreationTime:float = 0.0

    block:
        # This executes when an instance is created
        Print("Creating entity with ID: {ID}")
        set CreationTime = GetCurrentTime()

# Entity := logged_entity{ID := 42}
# Prints: "Creating entity with ID: 42"
```
<!-- #>-->

Block clauses have access to all fields of the class, including
`Self`, and can modify mutable fields. They execute in the order they
appear in the class definition:

<!--versetest-->
<!-- 07-->
```verse
multi_step_init := class:
    var Step1:int = 0
    var Step2:int = 0

    block:
        set Step1 = 10

    var Step3:int = 0

    block:
        set Step2 = Step1 + 5  # Can access earlier fields
        set Step3 = Step2 * 2

# Instance := multi_step_init{}
# Instance.Step1 = 10, Step2 = 15, Step3 = 30
```

**Execution order with inheritance:** Block execution order differs
between VMs (Verse: subclass-first, BP: superclass-first). Avoid order
dependencies for portable code.

**Why blocks instead of constructors?** Block clauses have access to
`Self`, unlike constructor functions. Use blocks for initialization
that references the object being constructed.

Additionally, field default values cannot use divergent calls. Give the
field a simple default and move initialization into a block:

<!--NoCompile-->
<!-- 06b-->
```verse
bar := class:
    var Foo:foo = foo{}

    block:
        set Foo = MakeFoo()  # Block can call divergent functions
```

**Constraints on block clauses:**

- Blocks cannot contain failure (`<decides>`) operations
- Blocks cannot call suspending (`<suspends>`) functions
- Blocks can use `defer` statements, which execute when the block exits
- Block clauses are only allowed in classes, not in interfaces,
  structs, or modules

Block clauses are particularly useful for:

- Logging object creation
- Computing derived values during initialization
- Registering objects with global systems
- Performing initialization that requires `Self` or divergent calls

##### Let Clauses in Archetypes

Archetype expressions (used to construct class and struct instances)
can include `let` clauses that introduce local variable bindings.
These are useful for computing intermediate values used by multiple
field initializers, avoiding repetition:

<!--NoCompile-->
<!-- 06c-->
```verse
MkWord8<constructor>(I:int)<decides><transacts> := Word8:
    let:
        MaxU8:int = Int[Pow(2.0, 8.0)] - 1 or Impossible("MkWord8")
    B := 0 <= I and I <= MaxU8
```

The `let` clause introduces bindings visible to subsequent field
initializers. Unlike `block`, `let` permits only variable
declarations.

##### Self

Within class methods, `Self` refers to the current instance:

<!--NoCompile-->
<!-- 08-->
```verse
character := class:
    var Name : string
    var Config:[string]string = map{}
	
    Announce() : void =
        # Using Self to pass the whole object
        LogCharacterAction(Self, "announced")


    SetOption(Key:string, Value:string):character =
        set Config[Key] = Value
        Self  # Return this instance for method chaining


    SetName(NewName:string):void =
       set Self.Name = NewName	  # Set the name of this instance
	   Self.Announce()            # Call a method of this instance
```

You can capture `Self` when creating nested objects:

<!--versetest-->
<!-- 12-->
```verse
container := class:
    ID:int

    CreateChild():child_with_parent =
        child_with_parent{Parent := Self}  # Capture this instance

child_with_parent := class:
    Parent:container

# C := container{ID := 42}
# Child := C.CreateChild()
# Child.Parent.ID = 42  # Child stores reference to C
```

##### Inheritance

Classes support single inheritance:

<!--versetest
vector3:=struct{}

entity := class:
    var Position : vector3 = vector3{}
    var IsActive : logic = true

    Activate() : void = set IsActive = true
    Deactivate() : void = set IsActive = false

character := class(entity):  # character inherits from entity
    Name : string
    var Health : int = 100

    TakeDamage(Amount : int) : void =
        set Health = Max(0, Health - Amount)
        if (Health = 0):
            Deactivate()  # Can call inherited methods

player := class(character):  # player inherits from character
    var Score : int = 0
    var Lives : int = 3

    AddScore(Points : int) : void =
        set Score += Points
<#
-->
<!-- 13-->
```verse
entity := class:
    var Position : vector3 = vector3{}
    var IsActive : logic = true

    Activate() : void = set IsActive = true
    Deactivate() : void = set IsActive = false

character := class(entity):  # character inherits from entity
    Name : string
    var Health : int = 100

    TakeDamage(Amount : int) : void =
        set Health = Max(0, Health - Amount)
        if (Health = 0):
            Deactivate()  # Can call inherited methods

player := class(character):  # player inherits from character
    var Score : int = 0
    var Lives : int = 3

    AddScore(Points : int) : void =
        set Score += Points
```
<!-- #>-->

A `player` is a `character`, and a `character` is an `entity`. You can
use a subclass wherever a superclass is expected.

**Important constraints on inheritance:**

1. **Single class inheritance only:** A class can inherit from at most
   one class, but can implement multiple interfaces:

<!--versetest-->
<!-- 14-->
```verse
base1 := class:
    Value1:int

base2 := class:
    Value2:int

# Valid: inherit from one class and multiple interfaces
interface1 := interface:
    Method1():void

interface2 := interface:
    Method2():void

derived := class<abstract>(base1, interface1, interface2):
    # Valid: one class, multiple interfaces
    Method1<override>():void = {}
    Method2<override>():void = {}

# Invalid: cannot inherit from multiple classes
# invalid := class(base1, base2):  # ERROR
```

2. **No shadowing of data members:** Subclasses cannot declare fields
   with the same name as parent fields

3. **No method signature changes:** Overriding requires the exact same
   signature

To override a method, use the `<override>` specifier with the matching signature.

##### Super

Within a subclass, you can use the `super` keyword to refer to the
superclass type. This is primarily used to access the superclass's
implementation or to construct a superclass instance:

<!--versetest-->
<!-- 17-->
```verse
entity := class:
    ID:int
    Name:string

    Display():void =
        Print("Entity {ID}: {Name}")

character := class(entity):
    Health:int

    Display<override>():void =
        # Create a superclass instance to call its method
        super{ID := ID, Name := Name}.Display()
        Print("Health: {Health}")
```

The `super` keyword represents the superclass type itself. When you
write `super{...}`, you are creating an instance of the superclass with
the specified field values. This allows you to delegate to superclass
behavior while adding subclass-specific functionality.

Within an overriding method, you can call the parent class's
implementation using the `(super:)` syntax. This is the primary way to
invoke parent method implementations while adding or modifying
behavior:

<!--versetest-->
<!-- 18-->
```verse
base := class:
    Method():void =
        Print("Base implementation")

derived := class(base):
    Method<override>():void =
        # Call parent implementation first
        (super:)Method()
        Print("Derived implementation")

# Creates instance and calls Method()
# derived{}.Method()
# Output:
# Base implementation
# Derived implementation

```

The `(super:)` syntax explicitly calls the parent class's version of
the current method. This is cleaner and more efficient than
constructing a parent instance with `super{...}` when you only need to
call parent methods.

**Basic Usage:**

<!--versetest
ToString(:vector3)<computes>:string=""
vector3:=class<final>{ X:float=0.0; Y:float=0.0; Z:float=0.0 }

entity := class:
    Position:vector3

    Move(Delta:vector3):void =
        Print("Entity moving by {Delta}")
        # Update position logic here

character := class(entity):
    var Stamina:float = 100.0

    Move<override>(Delta:vector3):void =
        # Call parent movement logic
        (super:)Move(Delta)
        # Add character-specific behavior
        set Stamina -= 1.0
<#
-->
<!--versetest-->
<!-- 19-->
```verse
entity := class:
    Position:vector3

    Move(Delta:vector3):void =
        Print("Entity moving by {Delta}")
        # Update position logic here

character := class(entity):
    var Stamina:float = 100.0

    Move<override>(Delta:vector3):void =
        # Call parent movement logic
        (super:)Move(Delta)
        # Add character-specific behavior
        set Stamina -= 1.0
```
<!-- #>-->

**With Effect Specifiers:**

The `(super:)` syntax works seamlessly with all effect specifiers:

<!--versetest
async_base := class:
    Process()<suspends>:void =
        Sleep(1.0)
        Print("Base processing")

async_derived := class(async_base):
    Process<override>()<suspends>:void =
        # Parent method suspends, so this suspends too
        (super:)Process()
        Print("Derived processing")

transactional_base := class:
    var Value:int = 0

    Update()<transacts>:void =
        set Value += 1

transactional_derived := class(transactional_base):
    var Counter:int = 0

    Update<override>()<transacts>:void =
        (super:)Update()
        set Counter += 1
<#
-->
<!--versetest-->
<!-- 20-->
```verse
async_base := class:
    Process()<suspends>:void =
        Sleep(1.0)
        Print("Base processing")

async_derived := class(async_base):
    Process<override>()<suspends>:void =
        # Parent method suspends, so this suspends too
        (super:)Process()
        Print("Derived processing")

transactional_base := class:
    var Value:int = 0

    Update()<transacts>:void =
        set Value += 1

transactional_derived := class(transactional_base):
    var Counter:int = 0

    Update<override>()<transacts>:void =
        (super:)Update()
        set Counter += 1
```
<!-- #>-->

**Virtual Dispatch Through Parent Methods:**

When parent methods call other methods, virtual dispatch still applies
based on the actual object type. This means `Self` binds to the
derived instance even when calling through `(super:)`:

<!--versetest-->
<!-- 21-->
```verse
base := class:
    # Virtual method that can be overridden
    GetValue()<computes>:int = 10

    # Parent method that uses GetValue
    ComputeDouble()<computes>:int =
        2 * GetValue()  # Calls derived GetValue if overridden

derived := class(base):
    # Override GetValue to return different value
    GetValue<override>()<computes>:int = 20

    # Override ComputeDouble to call parent, but GetValue dispatch is virtual
    ComputeDouble<override>()<computes>:int =
        # Calls base.ComputeDouble, which calls derived.GetValue!
        (super:)ComputeDouble()

# derived{}.ComputeDouble()  # Returns 40, not 20
```

In this example, even though `ComputeDouble` calls the parent
implementation, the `GetValue()` call inside the parent uses virtual
dispatch and calls the derived version.

**With Overloaded Methods:**

The `(super:)` syntax works with overloaded methods, calling the
parent's version of the same overload:

<!--versetest-->
<!-- 22-->
```verse
base := class:
    Process(X:int):void =
        Print("Base int: {X}")

    Process(S:string):void =
        Print("Base string: {S}")

derived := class(base):
    Process<override>(X:int):void =
        (super:)Process(X)  # Calls parent's int overload
        Print("Derived int: {X}")

    Process<override>(S:string):void =
        (super:)Process(S)  # Calls parent's string overload
        Print("Derived string: {S}")
```

**Return Type Covariance:**

When overriding methods with `(super:)`, the return type can be a subtype of the parent's return type (covariant return types):

<!--versetest-->
<!-- 23-->
```verse
base_type := class:
    Name:string

derived_type := class(base_type):
    Value:int

base := class:
    Create():base_type =
        base_type{Name := "base"}

derived := class(base):
    # Override with more specific return type
    Create<override>():derived_type =
        # Can still call parent even with different return type
        Parent := (super:)Create()
        derived_type{Name := Parent.Name, Value := 42}
```

##### Method Overriding

Subclasses can override methods defined in their superclasses to provide specialized behavior:

<!--versetest
character:=class:
    IsAlive()<decides><transacts>:void={}
MoveToward(:?character)<transacts>:void={}
Patrol()<transacts>:void={}
ScanForTargets()<transacts>:void={}
-->
<!-- 24-->
```verse
entity := class:
    OnUpdate<public>() : void = {}  # Default no-op implementation

enemy := class(entity):
    var Target : ?character = false

    OnUpdate<override>()<transacts> : void =
        if (Target?.IsAlive[]):
            MoveToward(Target)
        else:
            Patrol()

turret := class(entity):
    var Rotation:int= 0

    OnUpdate<override>()<transacts>: void =
        if (V:= Mod[Rotation, 360]):
            set Rotation = V
        ScanForTargets()
```

The override mechanism ensures that the correct method implementation
is called based on the actual type of the object, not the type of the
variable holding it. This is the foundation of polymorphic behavior in
object-oriented programming.

##### Constructor Functions

Classes do not have traditional constructor methods like you might find
in other object-oriented languages. Instead, Verse provides three
approaches to object construction, each suited to different needs:

- **Archetype expressions** — direct field initialization for simple
  cases. Straightforward and requires no extra definitions.
- **Block clauses** — initialization code in the class body that runs
  on every construction. Has access to `Self` and all fields,
  making it ideal for registering the object, computing derived
  values, or calling divergent functions that can't appear in field
  defaults.
- **Constructor functions** — annotated with `<constructor>`, these
  are first-class functions that can validate inputs, delegate to
  other constructors (including parent class constructors), be
  overloaded, and be passed around as values. They are the most
  powerful option and essential for inheritance hierarchies where
  subclass constructors need to initialize superclass fields.

These approaches compose: a constructor function returns an archetype
expression, which can contain `let` and `block` clauses, and the
class body can also have its own `block` clauses that execute
regardless of which constructor was used.

For simple cases where you just need to set field values, use
archetype expressions directly:

<!--versetest-->
<!-- 25-->
```verse
player := class:
    Name:string
    var Health:int = 100
    Level:int = 1

# Direct construction with archetype
# Hero := player{Name := "Aldric", Health := 150, Level := 5}
```

When you need validation, computation, or complex initialization
logic, use constructor functions annotated with `<constructor>`:

<!--versetest
player := class:
    Name:string
    var Health:int = 100
    Level:int = 1

MaxLevel:int = 99
-->
<!-- 26-->
```verse
MakePlayer<constructor>(InName:string, InLevel:int)<transacts> := player:
    Name := InName
    Level := InLevel
    Health := InLevel * 100
```

Here's an example of calling this constructor:

<!--versetest
player := class:
    Name:string
    var Health:int = 100
    Level:int = 1
MaxLevel:int = 99
MakePlayer<constructor>(InName:string, InLevel:int)<transacts> := player:
    Name := InName
    Level := InLevel
    Health := InLevel * 100
-->
<!-- 261-->
```verse
Hero := MakePlayer("Aldric", 5) # Call constructor function 
```

Constructor functions are regular functions that return class
instances, but the `<constructor>` annotation enables special
capabilities like delegating to other constructors. When calling a
constructor function from normal code, use just the function name—the
`<constructor>` annotation only appears in the definition.

Constructor functions can have effects that control their
behavior. Common effects include `<computes>`, `<allocates>`, and
`<transacts>`. A particularly useful effect is `<decides>`, which
allows constructors to fail if preconditions are not met:

<!--versetest
player := class:
    Name:string
    var Health:int = 100
    Level:int = 1

MaxLevel:int = 99
-->
<!-- 27-->
```verse
MakeValidPlayer<constructor>(InName:string, InLevel:int)<transacts><decides> := 
    player:
         Name := InName
         Level := block:
                 InLevel > 0
                 InLevel <= MaxLevel
                 InLevel
         Health := InLevel * 100
```


Constructor functions cannot use the `<suspends>` effect. Construction
must complete synchronously to maintain object consistency.

##### Overloading Constructors

You can provide multiple constructor functions with different
parameter signatures, allowing flexible object creation:

<!--versetest
vector3:=class<final>{ X:float=0.0; Y:float=0.0; Z:float=0.0 }
-->
<!-- 28-->
```verse
entity := class:
    Name:string
    var Health:int = 100
    Position:vector3

# Constructor with all parameters
MakeEntity<constructor>(Name:string, Health:int, Position:vector3) := entity:
    Name := Name
    Health := Health
    Position := Position

# Constructor with defaults
MakeEntity<constructor>(Name:string, Position:vector3) := entity:
    Name := Name
    Health := 100
    Position := Position

# Constructor for origin placement
MakeEntity<constructor>(Name:string) := entity:
    Name := Name
    Health := 100
    Position := vector3{X := 0.0, Y := 0.0, Z := 0.0}

# Each overload can be called based on arguments
# Enemy1 := MakeEntity("Goblin", 50, SpawnPoint)
# Enemy2 := MakeEntity("Guard", PatrolPoint)
# NPC := MakeEntity("Shopkeeper")
```

##### Delegating Constructors

Constructor functions can delegate to other constructors, enabling
code reuse and constructor chaining. This is particularly important
for inheritance hierarchies where subclass constructors need to
initialize superclass fields.

When delegating to a parent class constructor from a subclass, you
must initialize the subclass fields first, then call the parent
constructor using the qualified `<constructor>` syntax within the
archetype:

<!--versetest
entity := class:
    Name:string
    var Health:int

character := class(entity):
    Class:string
    Level:int

MakeEntity<constructor>(Name:string, Health:int) := entity:
    Name := Name
    Health := Health

### Subclass constructor delegates to parent constructor
MakeCharacter<constructor>(Name:string, Class:string, Level:int) := character:
    # Initialize subclass fields first
    Class := Class
    Level := Level
    # Then delegate to parent constructor
    MakeEntity<constructor>(Name, Level * 100)
<#
-->
<!-- 29-->
```verse
entity := class:
    Name:string
    var Health:int

MakeEntity<constructor>(Name:string, Health:int) := entity:
    Name := Name
    Health := Health

character := class(entity):
    Class:string
    Level:int

# Subclass constructor delegates to parent constructor
MakeCharacter<constructor>(Name:string, Class:string, Level:int) := character:
    # Initialize subclass fields first
    Class := Class
    Level := Level
    # Then delegate to parent constructor
    MakeEntity<constructor>(Name, Level * 100)

Hero := MakeCharacter("Aldric", "Warrior", 5)
```
<!-- #>-->

Constructor functions can also forward to other constructors of the same class:

<!--versetest
player := class:
    Name:string
    var Score:int

### Primary constructor
MakePlayer<constructor>(Name:string, Score:int) := player:
    Name := Name
    Score := Score

### Convenience constructor forwards to primary
MakeNewPlayer<constructor>(Name:string) := player:
    # Delegate to another constructor of the same class
    MakePlayer<constructor>(Name, 0)
<#
-->
<!-- 30-->
```verse
player := class:
    Name:string
    var Score:int

# Primary constructor
MakePlayer<constructor>(Name:string, Score:int) := player:
    Name := Name
    Score := Score

# Convenience constructor forwards to primary
MakeNewPlayer<constructor>(Name:string) := player:
    # Delegate to another constructor of the same class
    MakePlayer<constructor>(Name, 0)
```
<!-- #>-->

When delegating to a constructor of the same class, the delegation
replaces all field initialization—any fields you initialize before the
delegation are ignored. When delegating to a parent class constructor,
your subclass field initializations are preserved, and the parent
constructor initializes the parent fields.

##### Order of Execution

Understanding execution order is crucial for correct initialization:

1. **Archetype expression:** Field initializers execute in the order
   they are written in the archetype
2. **Delegating constructor:** Subclass fields are initialized first,
   then the parent constructor runs
3. **Class body blocks:** When using direct archetype construction,
   blocks in the class definition execute before field initialization

For delegating constructors to parent classes:

<!--versetest
base := class:
    BaseValue:int

derived := class(base):
    DerivedValue:int

MakeBase<constructor>(Value:int) := base:
    block:
        Print("Base constructor")
    BaseValue := Value

MakeDerived<constructor>(Base:int, Derived:int) := derived:
    # This executes first
    DerivedValue := Derived
    # Then parent constructor executes
    MakeBase<constructor>(Base)
<#
-->
<!-- 31-->
```verse
base := class:
    BaseValue:int

MakeBase<constructor>(Value:int) := base:
    block:
        Print("Base constructor")
    BaseValue := Value

derived := class(base):
    DerivedValue:int

MakeDerived<constructor>(Base:int, Derived:int) := derived:
    # This executes first
    DerivedValue := Derived
    # Then parent constructor executes
    MakeBase<constructor>(Base)
```
<!-- #>-->

Here's an example showing execution order:

<!--versetest
base := class:
    BaseValue:int

MakeBase<constructor>(Value:int) := base:
    block:
        Print("Base constructor")
    BaseValue := Value

derived := class(base):
    DerivedValue:int

MakeDerived<constructor>(Base:int, Derived:int) := derived:
    # This executes first
    DerivedValue := Derived
    # Then parent constructor executes
    MakeBase<constructor>(Base)
-->
<!-- 311-->
```verse
# Prints: "Base constructor"
# Results in: derived{BaseValue := 10, DerivedValue := 20}
Instance := MakeDerived(10, 20)
```

For classes with mutable fields, initialization sets starting values
that can change during the object's lifetime. Immutable fields must be
initialized during construction and cannot be modified afterward. This
distinction makes the construction phase critical for establishing
invariants that will hold throughout the object's existence.

#### Shadowing and Qualification

Verse has strict rules about name shadowing to prevent ambiguity and
maintain code clarity. Understanding these rules and the qualification
syntax is essential for working with inheritance hierarchies, multiple
interfaces, and nested modules.

In most contexts, you **cannot redefine names** that already exist in
an enclosing scope. This applies to functions, variables, classes,
interfaces, and modules:

<!--versetest-->
<!-- 32-->
```verse
# ERROR: Function at module level shadows class method
# F(X:int):int = X + 1
# c := class:
#     F(X:int):int = X + 2  # ERROR - shadows outer F
```

This prohibition extends across various contexts:

<!--NoCompile-->
<!-- 33-->
```verse
# ERROR: Cannot shadow classes
something := class {}

M := module:
    something := class {}  # ERROR

# ERROR: Cannot shadow variables
Value:int = 1

M := module:
     Value:int = 2        # ERROR

# ERROR: Cannot shadow data members
c := class { A:int }

A():void = {}             # ERROR - order does not matter

# ERROR: Module and function cannot share name

Id():void = {}
Id := module {}           # ERROR
```

The shadowing prohibition exists **regardless of definition order** -
it does not matter whether the outer name is defined before or after
the inner scope.

To define methods with the same name in different contexts, use
**qualified names** with the syntax `(ClassName:)MethodName`:

<!--versetest-->
<!-- 34-->
```verse
# Class with qualified method of same name
c := class:
   (c:)F(X:int):int = X + 2

# Module-level function
F(X:int):int = X + 1

# Call the module-level function
F(10)  # Returns 11

# Call the class method
c{}.F(10)  # Returns 12

# Explicit qualification (optional here)
c{}.(c:)F(10)  # Returns 12
```

The `(c:)` qualifier indicates this `F` is defined specifically in
the `c` class context, distinguishing it from the module-level
`F`. This allows the same name to coexist without shadowing errors.

##### Methods with Same Name

Using qualifiers, you can define *new methods* with the same name as
inherited methods, creating multiple distinct methods in the same
class:

<!--versetest-->
<!-- 35-->
```verse
c := class<abstract> { F(X:int):int }

d := class(c):
    F<override>(X:int):int = X + 1

e := class(d):
    (e:)F(X:int):int = X + 2 # NEW method with same name, not an override

# e now contains BOTH methods:
#    - (c:)F inherited from c (overridden in d)
#    - (e:)F newly defined in e
```

Using the above:

<!--versetest
c := class<abstract> { F(X:int):int }
d := class(c):
    F<override>(X:int):int = X + 1
e := class(d):
    (e:)F(X:int):int = X + 2 # NEW method with same name, not an override
-->
<!-- 351-->
```verse
E := e{}
E.(c:)F(10)  # Returns 11 (inherited from d's override)
E.(e:)F(10)  # Returns 12 (new method in e)
```

Key distinction:

- `F<override>` without qualifier: Overrides the inherited `F`
- `(e:)F` without `<override>`: Defines a **new** `F` specific to `e`

This allows a class to have multiple methods with the same name,
differentiated by their qualifiers, each serving different purposes in
the class hierarchy.

##### `(super:)` Qualified

The `(super:)` qualifier works with qualified method names to call the
parent class's implementation:

<!--versetest-->
<!-- 36-->
```verse
i := interface { F(X:int):int }

ci := class(i):
    (i:)F<override>(X:int):int = X + 1
    (ci:)F(X:int):int = X + 2

dci := class(ci):
    # Override both inherited methods, calling super implementations
    (i:)F<override>(X:int):int = 100 + (super:)F(X)
    (ci:)F<override>(X:int):int = 200 + (super:)F(X)
```

<!--NoCompile-->
```verse
DCI := dci{}
DCI.(i:)F(10)  # Returns 111
DCI.(ci:)F(10)  # Returns 212
```

`(super:)F(X)` within the qualified method calls the parent class's
implementation of that same qualified method. This enables you to
extend behavior for multiple method variants independently.

##### Interface Collisions

When implementing multiple interfaces with methods of the same name,
qualifiers disambiguate which interface's method you are implementing:


<!--versetest-->
<!-- 37-->
```verse
i := interface:
    B(X:int):int

j := interface:
    B(X:int):int

collision := class(i, j):
    # Implement both B methods separately
    (i:)B<override>(X:int):int = 20 + X
    (j:)B<override>(X:int):int = 30 + X
```

<!--NoCompile-->
```verse
Obj := collision{}
Obj.(i:)B(1)  # Returns 21
Obj.(j:)B(1)  # Returns 31
```

Without qualifiers, the compiler cannot determine which interface's
method you are implementing.

**Complex interface hierarchies:**

<!--versetest-->
<!-- 38-->
```verse
i := interface:
    C(X:int):int

j := interface(i):
    A(X:int):int

k := interface(i):
    B(X:int):int
    (k:)C(X:int):int  # k redefines C

multi := class(j, k):
    A<override>(X:int):int = 10 + X
    B<override>(X:int):int = 20 + X
    # Must implement C from both inheritance paths
    (i:)C<override>(X:int):int = 30 + X
    (k:)C<override>(X:int):int = 40 + X
```

<!--NoCompile-->
```verse
Obj := multi{}
Obj.(i:)C(1)  # Returns 31
Obj.(k:)C(1)  # Returns 41
```

When an interface redefines a method from a parent interface using
qualification `(k:)C`, implementing classes must provide
separate implementations for both variants.

##### Nested Module Qualification

Modules can be nested, and deeply qualified names reference members
through the entire hierarchy:

<!--versetest-->
<!-- 39-->
```verse
Top := module:
    (Top:)M<public> := module:
        (Top.M:)Value<public>:int = 1
        (Top.M:)F<public>(X:int):int = X + 10

        (Top.M:)M<public> := module:
            (Top.M.M:)Value<public>:int = 3
            (Top.M.M:)F<public>(X:int):int = X + 100
```

And a use case:

<!--versetest
Top := module:
    (Top:)M<public> := module:
        (Top.M:)Value<public>:int = 1
        (Top.M:)F<public>(X:int):int = X + 10

        (Top.M:)M<public> := module:
            (Top.M.M:)Value<public>:int = 3
            (Top.M.M:)F<public>(X:int):int = X + 100

using { Top.M }
using { Top.M.M }

-->
<!-- 391-->
```verse
# using { Top.M }
# using { Top.M.M }

# Access with full qualification
(Top.M:)F(0)          # Returns 10
(Top.M.M:)F(0)        # Returns 100

# Access via path
Top.M.F(1)            # Returns 11
Top.M.M.F(1)          # Returns 101
```

Nested modules can have the same simple name (e.g., both `M`)
when qualified with their full path, allowing hierarchical
organization without naming conflicts.

##### Restrictions

Local variables cannot shadow class members:

<!--NoCompile-->
<!-- 43-->
```verse
A := class:
    I:int
    F(X:int):void =
        I:int = 5  # ERROR - shadows member I
```

Currently, there is no `(local:)` qualifier to disambiguate, so this
pattern is not supported. You must use different names for local
variables and members.

#### Parametric Classes

Parametric classes, also known as generic classes, allow you to define
classes that work with any type. Rather than writing separate
container classes for integers, strings, players, and every other
type, you write one parametric class that accepts a type parameter.

A parametric class takes one or more type parameters in its definition:

<!--versetest
### Simple container that holds a single value
container(t:type) := class:
    Value:t
<#
-->
<!-- 46-->
```verse
# Simple container that holds a single value
container(t:type) := class:
    Value:t
```
<!-- #>-->

The syntax `container(t:type)` parameterizes the class by type `t`,
which can be used in field declarations, method signatures, and return
types.

**Multiple type parameters:**

<!--NoCompile-->
<!-- 47-->
```verse
pair(t:type, u:type) := class:
    First:t
    Second:u

Coordinate := pair(int, int){First := 10, Second := 20}
```

**Type parameters in methods:**

<!--versetest
optional_container(t:type) := class:
    var MaybeValue:?t = false

    Set(Value:t):void =
        set MaybeValue = option{Value}

    Get()<decides>:t =
        MaybeValue?

    Clear():void =
        set MaybeValue = false
<#
-->
<!-- 48-->
```verse
optional_container(t:type) := class:
    var MaybeValue:?t = false

    Set(Value:t):void =
        set MaybeValue = option{Value}

    Get()<decides>:t =
        MaybeValue?

    Clear():void =
        set MaybeValue = false
```
<!-- #> -->

##### Instantiation and Identity

Multiple instantiations with the same type arguments produce the same
type:

<!--versetest
container(t:type) := class:
    Value:t

### These are the same type
Type1 := container(int)
Type2 := container(int)
Type3 := container(int)

### All three are equal - they are the same type
<#
-->
<!-- 49-->
```verse
container(t:type) := class:
    Value:t

# These are the same type
Type1 := container(int)
Type2 := container(int)
Type3 := container(int)

# All three are equal - they are the same type
```
<!-- #>-->

This type identity is guaranteed across the program:

<!--versetest
container(t:type) := class:
    Value:t
-->
<!-- 50-->
```verse
# Create instances
C1 := container(int){Value := 1}
C2 := container(int){Value := 2}

# Both have the same type: container(int)
# Type checking treats them identically
```

The instantiation process is **deterministic and memoized**. The first
time you write `container(int)`, Verse generates a concrete
type. Every subsequent use of `container(int)` refers to that same
type, not a new copy.

This matters for:

- **Type compatibility**: Two values of `container(int)` can be used
  interchangeably
- **Memory efficiency**: Not creating duplicate type definitions
- **Semantic correctness**: Same type arguments always mean the same type

While the same type arguments always produce the same type, different
type arguments produce distinct, incompatible types:

<!--versetest
container(t:type) := class:
    Value:t
<#
-->
<!-- 52-->
```verse
container(t:type) := class:
    Value:t
```
<!-- #>-->


Here's an example showing that different instantiations create distinct types:

<!--versetest
container(t:type) := class:
    Value:t
-->
<!-- 521-->
```verse
IntContainer := container(int){Value := 42}
StringContainer := container(string){Value := "text"}

# These are different types and cannot be mixed
# IntContainer = StringContainer  # Type error!
```

`container(int)` and `container(string)` are completely different
types, with no subtype relationship. They happen to share the same
structure (both defined from `container`), but that does not make them
compatible.

While different instantiations of a parametric class are distinct
types, Verse allows certain instantiations to be used in place of
others based on **variance**. Variance determines when
`parametric_class(subtype)` can be used where
`parametric_class(supertype)` is expected (or vice versa).

The variance of a parametric type depends on how the type parameter is
used within the class definition:

###### Covariant

When a type parameter appears only in **return positions** (method
return types, field types being read), the parametric class is
**covariant** in that parameter (see
[Types](13_BOOK_OF_VERSE_II_PROGRAM_STRUCTURE_AND_TYPES.md#understanding-subtyping) for details on
variance). This means instantiations follow the same subtyping
direction as their type arguments:

<!--versetest
entity := class:
    ID:int
player := class(entity):
    Name:string
producer(t:type) := class:
    Value:t
    Get():t = Value  # Returns t - covariant position
ProcessProducer(P:producer(entity)):int = P.Get().ID
<#
-->
<!-- 53-->
```verse
# Base class hierarchy
entity := class:
    ID:int

player := class(entity):
    Name:string

# Covariant class - type parameter only in return position
producer(t:type) := class:
    Value:t

    Get():t = Value  # Returns t - covariant position

# Can use producer(player) where producer(entity) expected
ProcessProducer(P:producer(entity)):int = P.Get().ID
```
<!-- #>-->

Here's an example demonstrating covariance:

<!--versetest
### Base class hierarchy
entity := class:
    ID:int

player := class(entity):
    Name:string

### Covariant class - type parameter only in return position
producer(t:type) := class:
    Value:t

    Get():t = Value  # Returns t - covariant position

### Can use producer(player) where producer(entity) expected
ProcessProducer(P:producer(entity)):int = P.Get().ID
-->
<!-- 531-->
```verse
# Covariance allows subtype → supertype
PlayerProducer:producer(player) = producer(player){Value := player{ID := 1, Name := "Alice"}}
EntityProducer:producer(entity) = PlayerProducer  # Valid!

Result := ProcessProducer(PlayerProducer)  # Works!
```

**Why this is safe:** If you expect to get an `entity` from a
producer, receiving a `player` (which is a subtype of `entity`) is
always valid—a `player` has all the properties of an `entity`.

**Direction:** `producer(player)` → `producer(entity)` ✓ (follows
subtype direction)

###### Contravariant

When a type parameter appears only in **parameter positions** (method
parameters being consumed), the parametric class is **contravariant**
in that parameter (see [Types](13_BOOK_OF_VERSE_II_PROGRAM_STRUCTURE_AND_TYPES.md#understanding-subtyping)
for details on variance). This means instantiations follow the
**opposite** subtyping direction:


<!--versetest-->
<!-- 54-->
```verse
entity := class:
    ID:int

player := class(entity):
    Name:string

# Contravariant class - type parameter only in parameter position
consumer(t:type) := class:
    Process(Item:t):void = {}  # Accepts t - contravariant position
```

And a use case:

<!--versetest
entity := class:
    ID:int
player := class(entity):
    Name:string
consumer(t:type) := class:
    Process(Item:t):void = {}
-->
<!-- 541-->
```verse
# Contravariance allows supertype → subtype
EntityConsumer:consumer(entity) = consumer(entity){}
PlayerConsumer:consumer(player) = EntityConsumer  # Valid!

# Can use consumer(entity) where consumer(player) expected
ProcessPlayers(C:consumer(player)):void =
    C.Process(player{ID := 1, Name := "Bob"})

ProcessPlayers(EntityConsumer)                    # Works!
```

**Why this is safe:** If you have a function that accepts any
`entity`, it can certainly handle the more specific `player` type. A
`consumer(entity)` can consume anything a `consumer(player)` can
consume, plus more.

**Direction:** `consumer(entity)` → `consumer(player)` ✓ (opposite of
subtype direction)

###### Invariant

When a type parameter appears in **both parameter and return
positions**, the parametric class is **invariant** in that
parameter. No subtyping relationship exists between different
instantiations:

<!--versetest
entity := class:
    ID:int

player := class(entity):
    Name:string

### Invariant class - type parameter in both positions
transformer(t:type) := class:
    Transform(Input:t):t = Input  # Both parameter and return
<#
-->
<!-- 55-->
```verse
entity := class:
    ID:int

player := class(entity):
    Name:string

# Invariant class - type parameter in both positions
transformer(t:type) := class:
    Transform(Input:t):t = Input  # Both parameter and return
```
<!-- #>-->

Here's an example showing that no variance exists between different instantiations:

<!--versetest
entity := class:
    ID:int

player := class(entity):
    Name:string

### Invariant class - type parameter in both positions
transformer(t:type) := class:
    Transform(Input:t):t = Input  # Both parameter and return
-->
<!-- 551-->
```verse
# No variance - cannot convert in either direction
EntityTransformer:transformer(entity) = transformer(entity){}
PlayerTransformer:transformer(player) = transformer(player){}

# Invalid: Cannot use one where the other is expected
# X:transformer(entity) = PlayerTransformer  # ERROR 3509
# Y:transformer(player) = EntityTransformer  # ERROR 3509
```

**Why this is necessary:** If a `transformer(player)` could be used as
a `transformer(entity)`, you could pass any `entity` to its
`Transform` method, which expects specifically a `player`. This would
be unsafe.

**Direction:** No conversion allowed in either direction

###### Bivariant

When a type parameter is not used in any method signatures (only in
private implementation details or not at all), the parametric class is
**bivariant**. Any instantiation can be converted to any other:

<!--versetest
entity := class:
    ID:int

player := class(entity):
    Name:string

### Bivariant class - type parameter not used in public interface
container(t:type) := class:
    DoSomething():void = {}  # Doesn't use t at all
<#
-->
<!-- 56-->
```verse
entity := class:
    ID:int

player := class(entity):
    Name:string

# Bivariant class - type parameter not used in public interface
container(t:type) := class:
    DoSomething():void = {}  # Doesn't use t at all
```
<!-- #>-->


Here's an example showing that bivariant classes allow conversion in both directions:

<!--versetest
entity := class:
    ID:int

player := class(entity):
    Name:string

### Bivariant class - type parameter not used in public interface
container(t:type) := class:
    DoSomething():void = {}  # Doesn't use t at all
-->
<!-- 561-->
```verse
# Bivariant allows conversion in both directions
EntityContainer:container(entity) = container(entity){}
PlayerContainer:container(player) = container(player){}

# Both directions work
X:container(entity) = PlayerContainer  # Valid
Y:container(player) = EntityContainer  # Also valid
```

**Why this works:** Since the type parameter does not affect the
observable behavior, the instantiations are interchangeable.

##### Recursive Parametric Types

Parametric classes can reference themselves in their field types,
enabling recursive generic data structures like linked lists, trees,
and graphs. The key requirement is that the self-reference uses
**the same type parameter** — this is the only form of recursion
Verse allows. It works because the compiler can resolve the type
structure in a single pass: `list_node(int)` contains a
`?list_node(int)`, which contains a `?list_node(int)`, and so on.
The optional (`?`) provides the base case that terminates the
recursion at runtime.

Here is a generic linked list built as a recursive parametric class:

<!--versetest
### Linked list node
list_node(t:type) := class:
    Value:t
    Next:?list_node(t)  # Same type parameter 't'

### Helper to create lists
Cons(Head:t, Tail:?list_node(t) where t:type):list_node(t) =
    list_node(t){Value := Head, Next := Tail}

### Sum a linked list
SumList(List:?list_node(int)):int =
    if (Head := List?):
        Head.Value + SumList(Head.Next)
    else:
        0
<#
-->
<!-- 69-->
```verse
# Linked list node
list_node(t:type) := class:
    Value:t
    Next:?list_node(t)  # Same type parameter 't'

# Helper to create lists
Cons(Head:t, Tail:?list_node(t) where t:type):list_node(t) =
    list_node(t){Value := Head, Next := Tail}

# Sum a linked list
SumList(List:?list_node(int)):int =
    if (Head := List?):
        Head.Value + SumList(Head.Next)
    else:
        0
```
<!-- #>-->

Here's an example of using the linked list:

<!--versetest
### Linked list node
list_node(t:type) := class:
    Value:t
    Next:?list_node(t)  # Same type parameter 't'

### Helper to create lists
Cons(Head:t, Tail:?list_node(t) where t:type):list_node(t) =
    list_node(t){Value := Head, Next := Tail}

### Sum a linked list
SumList(List:?list_node(int)):int =
    if (Head := List?):
        Head.Value + SumList(Head.Next)
    else:
        0
-->
<!-- 691-->
```verse
# Usage
IntList := list_node(int){
    Value := 1
    Next := option{list_node(int){
        Value := 2
        Next := false
    }}
}
```

**Disallowed: Direct Type Alias Recursion**

You cannot define a parametric type that directly aliases to a
structural type containing itself:

<!--versetest-->
<!-- 71-->
```verse
# Invalid: Direct array recursion
# t(u:type) := []t(u)  # ERROR 3502

# Invalid: Direct map recursion
# t(u:type) := [int]t(u)  # ERROR 3502

# Invalid: Direct optional recursion
# t(u:type) := ?t(u)  # ERROR 3502

# Invalid: Direct function recursion
# t(u:type) := u->t(u)  # ERROR 3502
# t(u:type) := t(u)->u  # ERROR 3502
```

These fail because they create infinite type expansion—the compiler
cannot determine the actual structure of the type.

**Valid alternative:** Wrap the recursive reference in a class. For
example, a tree where each node holds a list of children is a
recursive parametric type — each `nested_list(t)` contains an array
of `nested_list(t)`:

<!-- NoCompile-->
<!-- 72-->
```verse
# Valid: Indirect recursion through class
nested_list(t:type) := class:
    Items:[]nested_list(t)  # OK - wrapped in class
```

Here's an example of constructing a tree with two children:

<!--versetest
### Valid: Indirect recursion through class
nested_list(t:type) := class:
    Items:[]nested_list(t)  # OK - wrapped in class
-->
<!-- 721-->
```verse
Tree := nested_list(int){
    Items := array{
        nested_list(int){Items := array{}},
        nested_list(int){Items := array{}}
    }
}
```

**Disallowed: Polymorphic Recursion**

Polymorphic recursion occurs when a parametric type references itself
with a **different type argument**:

<!-- 73-->
```verse
# Invalid: Type parameter changes
# my_type(t:type) := class:
#     Next:my_type(?t)  # ERROR 3509 - ?t is different from t

# Invalid: Alternating type parameters
# bi_list(t:type, u:type) := class:
#     Value:t
#     Next:?bi_list(u, t)  # ERROR 3509 - parameters swapped
```

**Why this is disallowed:** Polymorphic recursion makes type inference
undecidable and can create infinitely complex types. When you
instantiate `my_type(int)`, it would need `my_type(?int)`, which needs
`my_type(??int)`, and so on forever.

**Current limitation:** While polymorphic recursion is theoretically
sound in some type systems, Verse currently does not support it to
keep type checking tractable.

**Disallowed: Mutual Recursion**

Mutual recursion between multiple parametric types is not supported:

<!--versetest-->
<!-- 74-->
```verse
# Invalid: Mutual recursion
# t1(t:type) := class:
#     Next:?t2(t)  # References t2
#
# t2(t:type) := class:
#     Next:?t1(t)  # References t1
```

**Why this is disallowed:** Similar to polymorphic recursion, mutual
recursion complicates type inference and can create circular
dependencies that are difficult for the compiler to resolve.

**Workaround:** Combine into a single type:

<!-- NoCompile-->
<!-- 75-->
```verse
# Valid: Single type with multiple cases
node_type := enum:
    TypeA
    TypeB

combined_node(t:type) := class:
    Type:node_type
    Value:t
    Next:?combined_node(t)
```

**Disallowed: Inheritance Recursion**

You cannot inherit from a type variable or create recursive
inheritance through parametric types:

<!--versetest-->
<!-- 76-->
```verse
# Invalid: Inheriting from parametric self
# t(u:type) := class(t(u)){}  # ERROR 3590

# Invalid: Inheriting from type variable
# inherits_from_variable(t:type) := class(t){}  # ERROR 3590
```

**Why this is disallowed:** Inheritance requires knowing the parent's
structure,but with parametric recursion, this structure would be
self-referential before being defined.


##### Parametric Interfaces

While parametric classes get most of the attention, interfaces can
also be parametric, enabling abstract contracts that work with any
type:

<!-- TODO why is this not working?-->

<!--versetest
equivalence(t:type, u:type) := interface:
    Equal(Left:t, Right:u)<transacts><decides>:t

### Generic collection interface
collection_ifc(t:type) := interface:
    AddItem(Item:t)<transacts>:void
    RemoveItem(Item:t)<transacts><decides>:void
    Has(Item:t)<reads>:logic
<#
-->
<!-- 80-->
```verse
# Generic equality interface
equivalence(t:type, u:type) := interface:
    Equal(Left:t, Right:u)<transacts><decides>:t

# Generic collection interface
collection_ifc(t:type) := interface:
    Add(Item:t)<transacts>:void
    Remove(Item:t)<transacts><decides>:void
    Has(Item:t)<reads>:logic
```
<!-- #>-->

Classes implement parametric interfaces by providing concrete types
for the parameters:

<!-- versetest 
equivalence(t:type, u:type) := interface:
    Equal(Left:t, Right:u)<transacts><decides>:t

int_equivalence := class(equivalence(int, comparable)):
    Equal<override>(Left:int, Right:comparable)<transacts><decides>:int =
        Left = Right

### Or with type parameters matching the class
comparable_equivalence(t:subtype(comparable)) := class(equivalence(t, comparable)):
    Equal<override>(Left:t, Right:comparable)<transacts><decides>:t =
        Left = Right
<#
-->
<!-- 81-->
```verse
equivalence(t:type, u:type) := interface:
    Equal(Left:t, Right:u)<transacts><decides>:t

# Implement with specific types
int_equivalence := class(equivalence(int, comparable)):
    Equal<override>(Left:int, Right:comparable)<transacts><decides>:int =
        Left = Right

# Or with type parameters matching the class
comparable_equivalence(t:subtype(comparable)) := class(equivalence(t, comparable)):
    Equal<override>(Left:t, Right:comparable)<transacts><decides>:t =
        Left = Right
```
<!-- #> -->

Here's an example of using the parametric interface:

<!--versetest
equivalence(t:type, u:type) := interface:
    Equal(Left:t, Right:u)<transacts><decides>:t

### Implement with specific types
int_equivalence := class(equivalence(int, comparable)):
    Equal<override>(Left:int, Right:comparable)<transacts><decides>:int =
        Left = Right

### Or with type parameters matching the class
comparable_equivalence(t:subtype(comparable)) := class(equivalence(t, comparable)):
    Equal<override>(Left:t, Right:comparable)<transacts><decides>:t =
        Left = Right
-->
<!-- 811-->
```verse
# Usage
Eq := comparable_equivalence(int){}
Eq.Equal[5, 5]  # Succeeds
```

Parametric interfaces follow the same variance rules as parametric classes:

<!-- NoCompile-->
<!-- 82-->
```verse
entity := class:
    ID:int

player := class(entity):
    Name:string

# Covariant interface - returns t
producer_interface(t:type) := interface:
    Produce():t

player_producer := class(producer_interface(player)):
    Produce<override>():player = player{ID := 1, Name := "Test"}
```

Here's an example of covariant subtyping:

<!--versetest
entity := class:
    ID:int

player := class(entity):
    Name:string

### Covariant interface - returns t
producer_interface(t:type) := interface:
    Produce():t

player_producer := class(producer_interface(player)):
    Produce<override>():player = player{ID := 1, Name := "Test"}
-->
<!-- 821-->
```verse
# Covariant subtyping works
EntityProducer:producer_interface(entity) = player_producer{}
```

You can create specialized (non-parametric) interfaces from parametric ones:

<!-- NoCompile-->
<!-- 83-->
```verse
generic_handler(t:type) := interface:
    Handle(Item:t):void

# Specialize to a concrete type
int_handler := interface(generic_handler(int)):
    # Inherits Handle(Item:int):void
    # Can add more methods here

int_processor := class(int_handler):
    Handle<override>(Item:int):void =
        Print("Handling: {Item}")
```

Here's an example of using specialized interfaces in casts:

<!--versetest
generic_handler(t:type) := interface:
    Handle(Item:t):void

### Specialize to a concrete type
int_handler := interface(generic_handler(int)):
    # Inherits Handle(Item:int):void
    # Can add more methods here

int_processor := class(int_handler):
    Handle<override>(Item:int):void =
        Print("Handling: {Item}")
-->
<!-- 831-->
```verse
# Can use in casts now (specialized interfaces are non-parametric)
Base := int_processor{}
if (Handler := int_handler[Base]):
    Handler.Handle(42)
```

###### Multiple Type Parameters

Interfaces can have multiple type parameters with independent variance:

<!-- NoCompile-->
<!-- 84-->
```verse
converter_interface(input:type, output:type) := interface:
    Convert(In:input):output
    # input is contravariant, output is covariant

entity := class:
    ID:int

player := class(entity):
    Name:string

# Implement with specific types
player_to_entity := class(converter_interface(player, entity)):
    Convert<override>(In:player):entity = entity{ID := In.ID}
```

Is used here:

<!--versetest
converter_interface(input:type, output:type) := interface:
    Convert(In:input):output
    # input is contravariant, output is covariant

entity := class:
    ID:int

player := class(entity):
    Name:string

### Implement with specific types
player_to_entity := class(converter_interface(player, entity)):
    Convert<override>(In:player):entity = entity{ID := In.ID}

-->
<!-- 841-->
```verse
# Variance allows flexible usage
C:converter_interface(player, entity) = player_to_entity{}
```

##### Advanced Parametric Types

###### Effects

Parametric types can have effect specifiers that apply to all instantiations:

<!-- versetest 
### Parametric class with effects
async_container(t:type) := class<computes>:
    Property:t

### All instantiations inherit the effect
X:async_container(int) = async_container(int){Property := 1}  # <computes> effect

### Multiple effects
transactional_container(t:type) := class<transacts>:
    Property:t

assert:
    Y:transactional_container(int) = transactional_container(int){Property := 2}
<#
-->
<!-- 88-->
```verse
# Parametric class with effects
async_container(t:type) := class<computes>:
    Property:t

# All instantiations inherit the effect
X:async_container(int) = async_container(int){Property := 1}  # <computes> effect

# Multiple effects
transactional_container(t:type) := class<transacts>:
    Property:t

# Constructor inherits effects
# Y:transactional_container(int) = transactional_container(int){Property := 2}
```
<!-- #> -->

**Allowed effects:**

- `<computes>` - Allows non-terminating computation
- `<transacts>` - Participates in transactions
- `<reads>` - Reads mutable state
- `<writes>` - Writes mutable state
- `<allocates>` - Allocates resources

**Not allowed:**

- `<decides>` - Can fail
- `<suspends>` - Can suspend execution
- `<converges>` - The `<converges>` effect guarantees that a function terminates (see the [Effects](14_BOOK_OF_VERSE_III_EFFECTS_CONCURRENCY_AND_EVOLUTION.md#book-of-verse-source-unit-13effectsmd) chapter). Parametric classes cannot use it because instantiating a parametric type may involve arbitrary computation — the compiler cannot guarantee that constructing `my_type(t)` for all possible `t` will terminate.

**Effect propagation:**

<!-- versetest 
my_type(t:type) := class<computes>:
    Property:t

### This requires <computes> in the context
CreateInstance()<computes>:my_type(int) =
    my_type(int){Property := 1}
<#
-->
<!-- 89-->
```verse
# Effect on parametric type propagates to constructor
my_type(t:type) := class<computes>:
    Property:t

# This requires <computes> in the context
CreateInstance()<computes>:my_type(int) =
    my_type(int){Property := 1}
```
<!-- #> -->

The effect becomes part of the type's contract—all code constructing or working with instances must account for these effects.

###### Aliases

You can create type aliases that simplify complex parametric type expressions:

<!--versetest-->
<!-- 92-->
```verse
# Alias for map type
string_map(t:type) := [string]t

# Use the alias
PlayerScores:string_map(int) = map{
    "Alice" => 100,
    "Bob" => 95
}

# Alias for optional array
optional_array(t:type) := []?t

# Simplifies type signatures
FilterValid(Items:optional_array(int)):[]int =
    for (Item : Items; Value := Item?):
        Value
```

**Structural type aliases:**

<!--versetest-->
<!-- 94-->
```verse
# Function type aliases
transformer(input:type, output:type) := input -> output
predicate(t:type) := t -> logic

# Tuple type aliases
pair(t:type, u:type) := tuple(t, u)
triple(t:type) := tuple(t, t, t)

# Use in signatures
ApplyTransform(T:transformer(int, string), Value:int):string =
    T(Value)

CheckCondition(P:predicate(int), Value:int):logic =
    P(Value)
```

Type aliases improve readability and maintainability for complex generic types.

###### Advanced Type Constraints

Beyond basic `subtype` constraints, parametric types support specialized constraints:

**Subtype constraints:**

<!--versetest
entity:=class{ID:int=0}
player:=class(entity){}

### Constrain to subtype of a class
bounded_container(t:subtype(entity)) := class:
    Value:t

    GetID():int = Value.ID  # Can access entity members

### Valid: player is subtype of entity
### PlayerContainer := bounded_container(player){}

### Invalid: int is not subtype of entity
### IntContainer := bounded_container(int){}  # Type error

<#
-->
<!-- 95-->
```verse
# Constrain to subtype of a class
bounded_container(t:subtype(entity)) := class:
    Value:t

    GetID():int = Value.ID  # Can access entity members

# Valid: player is subtype of entity
# PlayerContainer := bounded_container(player){}

# Invalid: int is not subtype of entity
# IntContainer := bounded_container(int){}  # Type error
```
<!-- #>-->

**Castable subtype constraints:**

<!--versetest
component:=class<castable>{}
ProcessTyped(:component)<computes>:void={}

### Requires castable subtype
dynamic_handler(t:castable_subtype(component)) := class:
    Handle(Item:component):void =
        if (Typed := t[Item]):
            # Typed has the specific subtype
            ProcessTyped(Typed)

<#
-->
<!-- 96-->
```verse
# Requires castable subtype
dynamic_handler(t:castable_subtype(component)) := class:
    Handle(Item:component):void =
        if (Typed := t[Item]):
            # Typed has the specific subtype
            ProcessTyped(Typed)
```
<!-- #> -->

**Constraint propagation:**

<!--versetest
### Constraints propagate through function calls
wrapper(t:subtype(comparable)) := class:
    Data:t

Process(W:wrapper(t) where t:subtype(comparable))<computes><decides>:void =
    # Compiler knows t is comparable here
    W.Data = W.Data
<#
-->
<!-- 98-->
```verse
# Constraints propagate through function calls
wrapper(t:subtype(comparable)) := class:
    Data:t

Process(W:wrapper(t) where t:subtype(comparable))<computes><decides>:void =
    # Compiler knows t is comparable here
    W.Data = W.Data
```
<!-- #> -->

When defining parametric functions that work with parametric types,
the constraints must be compatible:

<!--versetest
base_class := class:
    ID:int
constrained(t:subtype(base_class)) := class:
    Data:t
UseConstrained(C:constrained(t) where t:subtype(base_class)):int =
    C.Data.ID
<#
-->
<!-- 99-->
```verse
base_class := class:
    ID:int

constrained(t:subtype(base_class)) := class:
    Data:t

# Valid: Constraint matches
UseConstrained(C:constrained(t) where t:subtype(base_class)):int =
    C.Data.ID

# Invalid: Missing or incompatible constraint
UseConstrained(C:constrained(t) where t:type):int =  # ERROR 
    C.Data.ID
```
<!-- #> -->

#### Class Attributes

Classes can be annotated with attributes that modify their behavior,
visibility, and capabilities. These attributes apply to all classes,
not just parametric ones.

##### Access Specifiers

Classes support fine-grained control over member visibility through
access specifiers:

<!--versetest-->
<!-- 100-->
```verse
game_state := class:
    Score<public> : int = 0                    # Anyone can read
    var Lives<private> : int = 3               # Only this class can access
    var Shield<protected> : float = 100.0      # This class and subclasses
    DebugInfo<internal> : string = ""          # Same module only

    # Public method - anyone can call
    GetLives<public>() : int = Lives

    # Protected method - subclasses can override
    OnLifeLost<protected>() : void = {}

    # Private helper - only this class
    ValidateState<private>() : void = {}
```

Default visibility is `internal` (same module only).

##### Concrete

The `<concrete>` specifier enforces that all fields have default
values, allowing construction with an empty archetype:

<!--versetest-->
<!-- 101-->
```verse
config := class<concrete>:
    MaxPlayers : int = 8
    TimeLimit : float = 300.0
    FriendlyFire : logic = false

# Can construct with empty archetype
DefaultConfig := config{}
```

A concrete class C can be constructed with C{}. A concrete class may have
subclasses that are not concrete.

##### Unique

The `<unique>` specifier creates classes and interfaces with reference
semantics where each instance has a distinct identity. When a class or
interface is marked as `<unique>`, instances become comparable using
the equality operators (= and <>), with equality based on object
identity rather than field values.

Classes marked with `<unique>` compare by identity, not by value:

<!-- versetest
vector3:=struct{X:float,Y:float,Z:float}
entity := class<unique>:
   Name : string
   Position : vector3
F()<decides>:void={
E1 := entity{Name := "Guard", Position := vector3{X := 0.0, Y := 0.0, Z := 0.0}}
E2 := entity{Name := "Guard", Position := vector3{X := 0.0, Y := 0.0, Z := 0.0}}
E3 := E1

not(E1 = E2 ) # Fails - different instances despite identical field values
E1 = E3  # Succeeds - same instance
}
<#
-->
<!-- 102-->
```verse
entity := class<unique>:
   Name : string
   Position : vector3

E1 := entity{Name := "Guard", Position := vector3{X := 0.0, Y := 0.0, Z := 0.0}}
E2 := entity{Name := "Guard", Position := vector3{X := 0.0, Y := 0.0, Z := 0.0}}
E3 := E1

E1 = E2  # Fails - different instances despite identical field values
E1 = E3  # Succeeds - same instance
```
<!-- #>-->

Without `<unique>`, class instances cannot be compared for equality at
all—the language prevents meaningless comparisons. With `<unique>`,
you gain the ability to use instances as map keys, store them in sets,
and perform identity checks, essential for tracking specific objects
throughout their lifetime.

###### Interfaces

Interfaces can also be marked with `<unique>`, which makes all
instances of classes implementing that interface comparable by
identity:

<!--versetest-->
<!-- 103-->
```verse
component := interface<unique>:
    Update():void
    Render():void

physics_component := class(component):
    Update<override>():void = {}
    Render<override>():void = {}
```

And a use case:

<!--versetest
component := interface<unique>:
    Update():void
    Render():void

physics_component := class(component):
    Update<override>():void = {}
    Render<override>():void = {}
-->
<!-- 1031-->
```verse
# Instances are comparable because component is unique
P1 := physics_component{}
P2 := physics_component{}

P1 <> P2  # true - different instances
P1 = P1   # true - same instance
```

The `<unique>` property propagates through interface inheritance. If a
parent interface is marked `<unique>`, all child interfaces and
classes implementing those interfaces automatically become comparable:

<!--versetest-->
<!-- 104-->
```verse
base_component := interface<unique>:
    Update():void

# Child interface inherits <unique> from parent
advanced_component := interface(base_component):
    AdvancedUpdate():void

# Classes implementing any interface in the hierarchy become comparable
player_component := class(advanced_component):
    Update<override>():void = {}
    AdvancedUpdate<override>():void = {}
```

And a use case:

<!--versetest
base_component := interface<unique>:
    Update():void

### Child interface inherits <unique> from parent
advanced_component := interface(base_component):
    AdvancedUpdate():void

### Classes implementing any interface in the hierarchy become comparable
player_component := class(advanced_component):
    Update<override>():void = {}
    AdvancedUpdate<override>():void = {}
-->
<!-- 1041-->
```verse
C1 := player_component{}
C2 := player_component{}
C1 <> C2  # true - comparable due to base_component being unique
```

When a class implements multiple interfaces, comparability is
determined by whether ANY of the inherited interfaces is `<unique>`:

<!--versetest-->
<!-- 105-->
```verse
updateable := interface:  # Not unique
    Update():void

renderable := interface<unique>:  # Unique
    Render():void

game_object := class(updateable, renderable):
    Update<override>():void = {}
    Render<override>():void = {}
```

<!--NoCompile-->
```verse
# game_object is comparable because renderable is unique
G1 := game_object{}
G2 := game_object{}
G1 <> G2  # true
```

Even if most interfaces are non-unique, a single `<unique>` interface
in the hierarchy makes the entire class comparable.

###### Unique in Default Values

When a `<unique>` class appears in a field's default value, each
containing object receives its own distinct instance. This guarantee
applies even when the unique class is nested within complex parametric
types:

<!--versetest-->
<!-- 106-->
```verse
token := class<unique>:
    ID:int = 0

container := class:
    MyToken:token = token{}
```

<!--NoCompile-->
```verse
C1 := container{}
C2 := container{}
C1.MyToken <> C2.MyToken  # true - each gets its own token
```

This behavior extends to `<unique>` instances within arrays,
optionals, tuples, and maps:

<!--versetest-->
<!-- 107-->
```verse
item := class<unique>{}

# Each class instantiation creates fresh unique instances in default values
with_array := class:
    Items:[]item = array{item{}}

with_optional := class:
    MaybeItem:?item = option{item{}}

with_map := class:
    ItemMap:[int]item = map{0 => item{}}
```

And a use case:

<!--versetest
item := class<unique>{}

### Each class instantiation creates fresh unique instances in default values
with_array := class:
    Items:[]item = array{item{}}

with_optional := class:
    MaybeItem:?item = option{item{}}

with_map := class:
    ItemMap:[int]item = map{0 => item{}}
-->
<!-- 1071-->
```verse
A := with_array{}
B := with_array{}
A.Items[0] <> B.Items[0]  # true - different unique instances

C := with_optional{}
D := with_optional{}
if (ItemC := C.MaybeItem?, ItemD := D.MaybeItem?):
    ItemC <> ItemD  # true - different unique instances
```

The same principle applies when parametric classes contain unique
instances in their fields:

<!--versetest
entity := class<unique>{}

registry(t:type) := class:
    DefaultEntity:entity = entity{}
    Data:t
<#
-->
<!-- 108-->
```verse
entity := class<unique>{}

registry(t:type) := class:
    DefaultEntity:entity = entity{}
    Data:t
```
<!-- #>-->

<!--versetest
entity := class<unique>{}

registry(t:type) := class:
    DefaultEntity:entity = entity{}
    Data:t
-->
<!-- 1081-->
```verse
R1 := registry(int){Data:=1}
R2 := registry(int){Data:=2}
R1.DefaultEntity <> R2.DefaultEntity  # true

R3 := registry(string){Data:="hi"}
R3.DefaultEntity <> R1.DefaultEntity  # true - even across different type parameters
```

This guarantee ensures that identity-based operations remain
reliable. If you store objects in maps keyed by unique instances, or
maintain sets of unique objects, each container genuinely owns
distinct instances rather than sharing references. The language
prevents subtle bugs where multiple objects might unexpectedly share
the same identity.

###### Overload Resolution

Types marked with `<unique>` are subtypes of the built-in `comparable`
type. This can create overload ambiguity:

<!--versetest
### Valid: non-unique interface does not conflict with comparable
regular_interface := interface:
    Method():void

Process(A:comparable, B:comparable):void = {}
Process(A:regular_interface, B:regular_interface):void = {}  # OK - no conflict

### Invalid: unique interface conflicts with comparable
assert_semantic_error(3532):
    my_unique_interface := interface<unique>:
        Method():void

    Handle(A:comparable, B:comparable):void = {}
    Handle(A:my_unique_interface, B:my_unique_interface):void = {}  # ERROR - ambiguous!
<#
-->
<!-- 109-->
```verse
# Valid: non-unique interface does not conflict with comparable
regular_interface := interface:
    Method():void

Process(A:comparable, B:comparable):void = {}
Process(A:regular_interface, B:regular_interface):void = {}  # OK - no conflict

# Invalid: unique interface conflicts with comparable
unique_interface := interface<unique>:
    Method():void

Handle(A:comparable, B:comparable):void = {}
# Handle(A:unique_interface, B:unique_interface):void = {}  # ERROR - ambiguous!
```
<!-- #>-->

Since `unique_interface` is a subtype of `comparable`, both overloads
could match when called with `unique_interface` arguments, causing a
compilation error. When designing overloaded functions, be aware that
`<unique>` types participate in the `comparable` type hierarchy.

###### Use Cases

The `<unique>` specifier is ideal for:

**Game Entities:** Where each entity in the world must be
distinguishable regardless of current state

<!--versetest
vector3:=class<final>{ X:float=0.0; Y:float=0.0; Z:float=0.0 }
entity := class<unique>:
    var Health:int = 100
    var Position:vector3
-->
<!-- 110-->
```verse
#entity := class<unique>:
#    var Health:int = 100
#    var Position:vector3

# Can track specific entities in collections
var ActiveEntities:[entity]logic = map{}
```

**Component Interfaces:** Where you need identity-based equality for
interface types

<!--versetest
entity:=class:

component := interface<unique>:
    Owner:entity
    Update():void
-->
<!-- 111-->
```verse
#component := interface<unique>:
#    Owner:entity

# Can use interface references as map keys
var ComponentRegistry:[component]string = map{}
```

**Session Objects:** Where identity matters more than current property values

<!--versetest
connection_info := class:

player_session := class<unique>:
    PlayerID:string
    var ConnectionTime:float
-->
<!-- 112-->
```verse
#player_session := class<unique>:
#    PlayerID:string
#    var ConnectionTime:float

# Track specific sessions
var ActiveSessions:[player_session]connection_info = map{}
```

**Resource Handles:** Where you need to track specific instances
rather than equivalent values

<!--versetest
gpu_resource:=class:

texture_handle := class<unique>:
    ResourceID:int
    FilePath:string
-->
<!-- 113-->
```verse
#texture_handle := class<unique>:
#    ResourceID:int
#    FilePath:string

# Manage resource lifecycle
var LoadedTextures:[texture_handle]gpu_resource = map{}
```

The `<unique>` specifier enables these patterns by providing
identity-based equality semantics, making it possible to use instances
as map keys, maintain sets of unique objects, and distinguish between
different instances even when their data is identical.

##### Abstract

The `<abstract>` specifier marks classes that cannot be instantiated
directly — they exist solely as base classes for inheritance. When you
declare a class with `<abstract>`, you are creating a template that
defines structure and behavior for subclasses to inherit and
implement.

Abstract classes serve as architectural foundations in a type
hierarchy. They define contracts through abstract methods that
subclasses must implement, while potentially providing concrete
methods and fields that subclasses inherit. This creates a powerful
pattern for code reuse and polymorphic behavior.

<!-- versetest-->
<!-- 114-->
```verse
vehicle := class<abstract>:
      Speed():float             # Abstract method
      MaxPassengers:int = 1

      # Concrete method all vehicles share
      CanTransport(Count:int)<decides>:void =
          Count <= MaxPassengers

car := class(vehicle):
      Speed<override>():float = 60.0
      MaxPassengers<override>:int = 4

bicycle := class(vehicle):
      Speed<override>():float = 15.0
```

Abstract methods within abstract classes have no implementation —
they are pure declarations that establish what subclasses must
provide. An abstract method creates a contract: any non-abstract
subclass must override all abstract methods or the code will not compile.

##### Castable

Verse supports runtime type checking for all classes and interfaces
through **fallible casts** and **infallible casts**. The `<castable>`
specifier serves a specific purpose: it enables the use of
`castable_subtype` constraints, which allow types to be used as
first-class values in type-constrained contexts.

All classes and interfaces support runtime type checking through
dynamic casts. You can cast between any class or interface types using the fallible cast
syntax `Type[Value]`:

<!--versetest-->
<!-- 114a -->
```verse
# No <castable> needed for basic dynamic casts
base := class:
    ID:int

derived := class(base):
    Name:string

# Fallible cast
ProcessBase(B:base):void =
    if (D := derived[B]):
        # Successfully cast to derived
        Print("Derived with name: {D.Name}")
    else:
        # Not a derived instance
        Print("Just a base")
```

###### When Do You Need `<castable>`

The `<castable>` specifier is required only when you want to use
`castable_subtype` constraints. These constraints enable powerful
patterns where types are used as first-class values, such as accepting
a type as a parameter and using it to perform casts:

<!--versetest
component := class<castable>{}
physics_component := class<castable>(component){}
render_component := class<castable>(component){}
ProcessSpecific(:component):void = {}
-->
<!-- 114b -->
```verse
# Requires <castable> for castable_subtype constraint
FilterByType(
    Items:[]component,
    TargetType:castable_subtype(component)  # Type as parameter
):[]component =
    for:
        Item : Items
        Specific := TargetType[Item]  # Use type variable for cast
    then:
        Specific

# Can pass different types at runtime
AllComponents:[]component = array{physics_component{}, render_component{}}
PhysicsOnly := FilterByType(AllComponents, physics_component)
```

###### Fallible and Infallible Casts

Verse provides two forms of type casting: **fallible casts** (which
can fail at runtime) and **infallible casts** (which are verified at
compile time).

Fallible casts use bracket syntax `Type[Value]` are runtime checks that succeed only if the
value is actually an instance of the target type:

<!-- versetest
vector3:=class<final>{ X:float=0.0; Y:float=0.0; Z:float=0.0 }
ToString(:vector3):string=""
-->
<!-- 115-->
```verse
# Classes with <castable> - enables castable_subtype usage
component := class<abstract><castable><allocates>:
    Name:string

physics_component := class<allocates>(component):
    Name<override>:string = "Physics"
    Velocity:vector3

render_component := class<allocates>(component):
    Name<override>:string = "Render"
    Material:string

# Fallible casts work whether or not <castable> is present
ProcessComponent(Comp:component):void =
    if (PhysicsComp := physics_component[Comp]):
        Print("Physics component with velocity: {PhysicsComp.Velocity}")
    else if (RenderComp := render_component[Comp]):
        Print("Render component with material: {RenderComp.Material}")
    else:
        Print("Unknown component type")
```

The cast expression has the `<decides>` effect—it fails if the object
is not an instance of the target type. This integrates naturally with
Verse's failure handling:

<!--versetest
vector3:=class<final><allocates>{ X:float=0.0; Y:float=0.0; Z:float=0.0 }
component := class<abstract><castable><allocates>:
    Name:string

physics_component := class<allocates>(component):
    Name<override>:string = "Physics"
    Velocity:vector3=vector3{}

SomeComponent:component=physics_component{}
UpdatePhysics(:physics_component)<computes>:void={}
-->
<!-- 116-->
```verse
GetPhysicsComponent(Comp:component)<computes><decides>:physics_component =
    # Returns physics_component or fails
    physics_component[Comp]

# Use with failure handling
if (Physics := GetPhysicsComponent[SomeComponent]):
    UpdatePhysics(Physics)
```

Infallible casts use parenthesis syntax `Type(Value)` and are only
allowed when the compiler can verify the cast is safe—that is, when the
value type is a subtype of the target type:


<!--versetest-->
<!-- 117-->
```verse
base := class:
    ID:int

derived := class(base):
    Name:string

GetDerived():derived = derived{ID := 1, Name := "Test"}
```

Use case:

<!--versetest
base := class:
    ID:int

derived := class(base):
    Name:string

GetDerived():derived = derived{ID := 1, Name := "Test"}
-->
<!-- 1171-->
```verse
# Infallible upcast - derived is a subtype of base
BaseRef:base = base(GetDerived())  # Always safe
```

Attempting an infallible downcast (from supertype to subtype) is a
compile error, as the compiler cannot guarantee safety:

<!--NoCompile-->
<!-- 118-->
```verse
DerivedRef := derived(BaseRef)  # ERROR: not a subtype relationship
```


###### Castable and Inheritance

The `<castable>` property is inherited by all subclasses. When you
mark a class as `<castable>`, every class that inherits from it
automatically becomes castable as well:

<!--versetest-->
<!-- 119-->
```verse
base := class<castable>:
    Value:int

child := class(base):
    # Automatically castable - inherits from castable base
    Name:string

grandchild := class(child):
    # Also automatically castable
    Extra:string

# Can cast through the hierarchy
ProcessBase(Instance:base):void =
    if (AsChild := child[Instance]):
        Print("It's a child: {AsChild.Name}")
    if (AsGrandchild := grandchild[Instance]):
        Print("It's a grandchild: {AsGrandchild.Extra}")
```

###### Parametric Types and Casting

Parametric types cannot be marked `<castable>`. Verse erases type
parameters at runtime—only the concrete class structure exists, not the
specific type arguments. The runtime cannot distinguish between
`container(int)` and `container(string)`, which would make
`castable_subtype` constraints unsound.

Additionally, you cannot cast to a parametric type even if it is not
marked `<castable>`. Attempting to use a parametric type as a cast
target produces a compile error:

<!--versetest
assert_semantic_error(3678):
    container(t:type) := class<castable>:
        Value:t

assert_semantic_error(3502):
    container(t:type) := class:
        Value:t
    Test()<decides>:void =
        C := container(int){Value := 42}
        if (C2 := container(string)[C]):
            {}
-->
<!-- 120-->
```verse
# Invalid: parametric classes cannot be castable
# container(t:type) := class<castable>:  # ERROR 3678
#     Value:t

# Invalid: cannot cast to parametric type
container(t:type) := class:
    Value:t

Test()<decides>:void =
    C := container(int){Value := 42}
    if (C2 := container(string)[C]):  # ERROR 3502
        {}
```

However, concrete instantiations of parametric types can be cast
targets, and non-parametric classes can be marked `<castable>` even
if they inherit from parametric types:

<!--versetest
container(t:type) := class:
    Value:t
int_container := class<castable>(container(int)):
    Extra:string
string_container := class<castable>(container(string)):
    Extra:string
Base:container(int) = int_container{Value := 42, Extra := "test"}
-->
<!-- 121-->
```verse
container(t:type) := class:
    Value:t

# Valid: concrete instantiations can be cast targets
int_container := class<castable>(container(int)):
    Extra:string

string_container := class<castable>(container(string)):
    Extra:string

# Can cast to concrete instantiations
Base:container(int) = int_container{Value := 42, Extra := "test"}
if (IC := int_container[Base]):
    Print("Extra: {IC.Extra}")  # Works!

# Cannot cast between different instantiations
# if (SC := string_container[Base]):  # Would fail at runtime
```
<!-- #>-->

###### Using castable_subtype

The `castable_subtype` type constructor works with `<castable>`
classes to enable type-safe filtered queries and dynamic type
dispatch:

<!--versetest
  component<public> := class<abstract><unique><castable>:
      Parent<public>:entity

  entity<public> := class<concrete><unique><transacts><castable>:
      FindDescendantEntities(entity_type:castable_subtype(entity)):[]entity_type = array{}
<#
-->
<!-- 122-->
```verse
  component<public> := class<abstract><unique><castable>:
      Parent<public>:entity

  entity<public> := class<concrete><unique><transacts><castable>:
      FindDescendantEntities(entity_type:castable_subtype(entity)):[]entity_type
```
<!-- #> -->

When you call `FindDescendantEntities(player)`, the function returns
only entities that are actually player instances or subclasses
thereof, verified at runtime through the castable mechanism. The type
parameter ensures type safety—the returned values have the specific
subtype you requested.

###### Permanence of Castable

Once a class is published with `<castable>`, this decision becomes
permanent. You cannot add or remove the `<castable>` specifier after
publication because doing so would break existing code that relies on
runtime type checking. Code that performs casts would suddenly fail or
behave incorrectly if the castable property changed.

This permanence is enforced through the versioning system—attempting
to change the `<castable>` status of a published class will result in
a compatibility error.

##### Final

The `<final>` specifier prevents inheritance, creating a terminal
point in a class hierarchy. When you mark a class with `<final>`, no
other class can inherit from it. For methods, `<final>` prevents
overriding in subclasses, locking the implementation at that level of
the hierarchy.

Classes marked with `<final>` serve as concrete implementations that
cannot be extended. This is particularly important for persistable
classes, which require `<final>` to ensure their structure remains
stable for serialization:

<!--versetest
player_stats:=struct<persistable>{}

player_profile := class<final><persistable>:
    Username:string = "Player"
    Level:int = 1
    Gold:int = 0

player_data := class<final><persistable>:
    Version:int = 1
    LastLogin:string = ""
    Statistics:player_stats = player_stats{}
<#
-->
<!-- 123-->
```verse
  player_profile := class<final><persistable>:
      Username:string = "Player"
      Level:int = 1
      Gold:int = 0

  player_data := class<final><persistable>:
      Version:int = 1
      LastLogin:string = ""
      Statistics:player_stats = player_stats{}
```
<!-- #>-->

The `<final>` requirement for persistable classes prevents schema
evolution problems. If subclasses could extend persistable classes,
the serialization system would face ambiguity about which fields to
persist and how to handle polymorphic deserialization.

For methods, `<final>` locks behavior at a specific point in the
inheritance chain:

<!--versetest
base_entity := class:
    GetName():string = "Entity"

game_object := class(base_entity):
    GetName<override><final>():string = "GameObject"
    # Any subclass of game_object cannot override GetName
<#
-->
<!-- 124-->
```verse
  base_entity := class:
      GetName():string = "Entity"

  game_object := class(base_entity):
      GetName<override><final>():string = "GameObject"
      # Any subclass of game_object cannot override GetName
```
<!-- #>-->

For fields, `<final>` prevents modification through archetype
construction. When a field is marked `<final>` and has a default value,
that value is locked and cannot be changed when creating instances:

<!-- versetest-->
<!-- 1241-->
```verse
foo := class<computes>:
    Val<final>:int = 0
    X:int = 5

# Valid: X can be changed during construction
ValidFoo := foo{X := 10}

# COMPILE ERROR: Cannot override final field Val
# InvalidFoo := foo{Val := 10}
```

This restriction ensures that final fields maintain their guaranteed
values throughout the object's lifetime. Final fields with default
values act as immutable constants for each instance. If you need a
field to be customizable during construction, do not mark it as
`<final>`. Final fields must also provide a default value — you cannot
declare a final field without initializing it.

###### Final on Interface Members

While `<final>` cannot be applied to interface or struct *types*
themselves, it can be used on interface *members* to prevent overriding
in implementing classes. Final interface members must provide a complete
implementation (body for methods, value for fields):

<!--versetest-->
<!-- 124001 -->
```verse
base_behavior := interface:
    # Final method with default implementation
    GetID<final>():int = 42

    # Final field with default value
    MaxCount<final>:int = 100

    # Non-final method - can be overridden
    Process():void

concrete_impl := class(base_behavior):
    # Can implement Process
    Process<override>():void = {}

    # Cannot override GetID or MaxCount - they are final
    # GetID<override>():int = 99  # ERROR
```

Final members in interfaces propagate through interface inheritance.
When an interface extends another interface with final members, those
members remain final and cannot be overridden by any implementing
classes:

<!--versetest-->
<!-- 124002 -->
```verse
base := interface:
    GetVersion<final>():int = 1

derived := interface(base):
    GetName():string

impl := class(derived):
    # Must implement GetName
    GetName<override>():string = "Implementation"

    # GetVersion remains final from base
    # GetVersion<override>():int = 2  # ERROR
```

**Important:** `<final>` on interface/struct types themselves produces
an error. Use `<final>` only on their members.

The related `<final_super>` specifier does **not** prevent further
subclassing. Instead, it guarantees that all subclasses of this class
will always directly inherit from it — there will be no intermediate
classes inserted between the `<final_super>` class and its
descendants in the inheritance chain. Subclasses can themselves be
further subclassed:

<!-- NoCompile-->
<!-- 125-->
```verse
component := class<abstract><unique><castable><final_super_base>:
      Parent:entity

physics_component := class<final_super>(component):
      Mass:float = 1.0

# Valid: further subclassing is allowed
gravity_component := class(physics_component):
      GravityScale:float = 1.0
```

`<final_super_base>` marks the root of a restricted inheritance tree.
Its purpose is to work with `GetCastableFinalSuperClass`, which
finds the `<final_super>` class in the hierarchy for a given
instance. This enables component architectures where you need to
identify the "category" of a component at runtime:

<!-- 126-->
```verse
#            base_type<castable>
#               /         \
#  a_class<final_super>   w_class
#         |                  |
#      b_class            x_class<final_super>
#         |                  |
#      c_class            y_class

# GetCastableFinalSuperClass[base_type, c_class{}]
# returns a_class — the <final_super> ancestor under base_type
```

This design is particularly valuable in component architectures
where you need a stable "category" class in the hierarchy that
runtime systems can rely on, while still allowing further
specialization below it.

##### Persistable

The `<persistable>` specifier marks types that can be saved and
restored across game sessions, enabling permanent storage of player
progress, achievements, and game state. This specifier transforms
ephemeral gameplay into lasting progression, creating the foundation
for meaningful player investment.

Persistence works through module-scoped `weak_map(player, t)`
variables, where `t` is any persistable type.  These special maps
automatically synchronize with backend storage — when players join,
their data loads; when they leave or data changes, it saves. The
system handles all serialization, network transfer, and storage
management transparently.

<!--versetest
player:=string
-->
<!-- 127-->
```verse
player_inventory := class<final><persistable>:
      Gold:int = 0
      Items:[]string = array{}
      UnlockedAreas:[]string = array{}

# This variable automatically persists across sessions
SavedInventories : weak_map(player, player_inventory) = map{}
```

The `<persistable>` specifier enforces strict structural requirements
to guarantee data integrity across versions. Classes must be `<final>`
because inheritance would complicate serialization schemas. They
cannot contain `var` fields, preserving immutability guarantees even
in persistent storage. They cannot be `<unique>` since identity-based
equality does not survive serialization. These constraints ensure that
what you save today can be reliably loaded tomorrow, next month, or
next year.

#### Interfaces

Interfaces define contracts that classes can implement, specifying
both the data and behavior that implementing classes must
provide. Unlike many traditional languages where interfaces only
declare method signatures, Verse interfaces are rich contracts that
can include fields, default method implementations, and even custom
accessor logic.

An interface can declare method signatures, provide default
implementations, and define data members:

<!--versetest-->
<!-- 128-->
```verse
damageable := interface:
    # Abstract method - implementing classes must provide
    TakeDamage(Amount:int)<transacts>:void

    # Method with default implementation
    GetHealth()<computes>:int = 100

    # Data member - implementing classes inherit or must provide
    MaxHealth:int = 100

    IsAlive()<computes>:logic = logic{GetHealth() > 0}

healable := interface:
    Heal(Amount:int):void
    GetMaxHealth():int
```

Interfaces can be purely abstract, partially concrete, or fully
implemented. A class implementing an interface must provide implementations
for its abstract methods; it inherits concrete implementations and default
field values.

##### Implementing Interfaces

<!--versetest
healable:=interface:
    TakeDamage(Amount:int)<transacts>:void ={}
    GetHealth():int = 0
    Heal(Amount:int)<transacts>:void ={}

damageable:=interface{}
-->
<!-- 129-->
```verse
character := class(damageable, healable):
    var Health : int = 100
    MaxHealth : int = 100

    TakeDamage<override>(Amount:int)<transacts>:void =
        set Health = Max(0, Health - Amount)

    GetHealth<override>()<reads>:int = Health

    Heal<override>(Amount:int)<transacts>:void =
        set Health = Min(MaxHealth, Health + Amount)
```

A class can implement multiple interfaces, achieving multiple
inheritance of contracts.

##### Interface Fields

Interfaces can declare data members that implementing classes must provide
or inherit. These fields can be either immutable or mutable, and may include
default values:

<!--versetest-->
<!-- 130-->
```verse
# Interface with various field types
entity_properties := interface:
    # Immutable field with default - classes inherit this value
    EntityID:int = 0

    # Mutable field with default
    var Health:float = 100.0

    # Field without default - classes must provide a value
    Name:string

    # Field that can be overridden
    MaxHealth:float = 100.0

player_entity := class(entity_properties):
    # Must provide Name (no default in interface)
    Name<override>:string = "Player"

    # Can override to change default
    MaxHealth<override>:float = 150.0

    # Inherits EntityID and Health with their defaults
```

Fields with defaults are inherited unless overridden. Fields without
defaults must be provided.

##### Default Implementations

Interfaces can provide complete method implementations that
implementing classes inherit automatically:

<!--versetest-->
<!-- 131-->
```verse
animated := interface:
    var CurrentFrame:int = 0
    TotalFrames:int = 10

    # Concrete implementation provided by interface
    NextFrame()<transacts><decides>:void =
        set CurrentFrame = Mod[(CurrentFrame + 1),TotalFrames] or 0

    # Can access interface fields
    ProgressPercent()<reads><decides>:rational =
        CurrentFrame / TotalFrames

sprite := class(animated):
    TotalFrames<override>:int = 20
    # Automatically inherits NextFrame and ProgressPercent implementations
```

Classes inherit these implementations without modification, allowing
interfaces to provide reusable behavior. Implementing classes can
override these methods if they need specialized behavior, but the
interface provides a working default.

##### Overriding Members

Classes can override both fields and methods from interfaces to
provide specialized implementations:

<!--versetest-->
<!-- 132-->
```verse
base_stats := interface:
    BaseHealth:int = 100

    CalculateFinalHealth():int = BaseHealth

warrior := class(base_stats):
    # Override field with different default
    BaseHealth<override>:int = 150

    # Override method for specialized calculation
    CalculateFinalHealth<override>():int =
        BaseHealth * 2  # Warriors get double health

mage := class(base_stats):
    BaseHealth<override>:int = 75

    CalculateFinalHealth<override>():int =
        BaseHealth + MagicBonus

    MagicBonus:int = 25
```

Field overrides can provide different default values or specialize to
subtypes. Method overrides replace the interface's implementation
entirely. All overrides must maintain type compatibility—fields can
only be overridden with subtypes, and method signatures must match
exactly.

##### Multiple Interfaces with Sharing

Verse interfaces are more permissive than in many other languages —
they can declare data fields, provide concrete method implementations,
and a class can implement multiple interfaces even when they share
member names. This design avoids the friction of requiring globally
unique names across all interfaces. In practice, independent interface
authors may naturally use the same names (`Enable`, `Disable`,
`Power`, `Update`), and requiring every interface to use distinct
names would create artificial naming conflicts that scale poorly —
especially when interfaces form deep hierarchies with subinterfaces
for specialized variants.

When a class implements multiple interfaces that declare fields or
methods with the same name, you use qualified names to
disambiguate:

<!--versetest-->
<!-- 133-->
```verse
magical := interface:
    Power:int = 50
    GetPowerLevel()<computes>:int = Power

physical := interface:
    Power:int = 75
    GetPowerLevel()<computes>:int = Power * 2

hybrid := class(magical, physical):
    UseHybridPowers():void =
       MagicPower := (magical:)Power         # Access magical's Power
       PhysicalPower := (physical:)Power     # Access physical's Power
       MagicLevel := (magical:)GetPowerLevel()
       PhysicalLevel := (physical:)GetPowerLevel()
```

The qualified name syntax `(InterfaceName:)MemberName` specifies which
interface's member you are accessing. Each interface maintains its own
instance of the field, allowing the class to support both contracts
simultaneously without conflict.

##### Interface Hierarchies

Interfaces can extend other interfaces, creating hierarchies of
contracts that combine data and behavior requirements:

<!--NoCompile-->
<!-- 134-->
```verse
combatant := interface(damageable, healable):
    var AttackPower:int = 10

    Attack(Target:damageable):void =
        Target.TakeDamage(AttackPower)

    GetAttackPower():int = AttackPower

boss := interface(combatant):
    Phase:int = 1

    UseSpecialAbility():void
    GetPhase():int = Phase
```

A class implementing `boss` inherits all fields and methods from the
entire hierarchy—`boss`, `combatant`, `damageable`, and
`healable`. Diamond inheritance (where an interface is inherited
through multiple paths) is fully supported, with fields properly
merged so each field exists only once in the implementing class.

**Important:** A class cannot directly inherit the same interface
multiple times (e.g., `class(interface1, interface1)` is an error),
but can inherit it indirectly through diamond inheritance. This means
`class(interface2, interface3)` is valid even if both `interface2` and
`interface3` inherit from the same base interface.

##### Fields with Accessors

Interfaces can define fields with custom getter and setter logic,
encapsulating complex behavior behind simple field access syntax:

<!--versetest
subscribable_property := interface:
    # External field with accessor methods
    var Value<getter(GetValue)><setter(SetValue)>:int = external{}

    # Internal storage
    var Storage:int = 100

    # Getter adds computation
    GetValue(:accessor):int = Storage + 10

    # Setter adds validation
    SetValue(:accessor, NewValue:int):void =
        if (NewValue >= 0):
            set Storage = NewValue

tracked_value := class(subscribable_property):

UseTrackedValue():void =
    Object := tracked_value{}

    # Uses getter - returns 110 (Storage + 10)
    Current := Object.Value

    # Uses setter - validates and updates Storage
    set Object.Value = 150
<#
-->
<!-- 135-->
```verse
subscribable_property := interface:
    # External field with accessor methods
    var Value<getter(GetValue)><setter(SetValue)>:int = external{}

    # Internal storage
    var Storage:int = 100

    # Getter adds computation
    GetValue(:accessor):int = Storage + 10

    # Setter adds validation
    SetValue(:accessor, NewValue:int):void =
        if (NewValue >= 0):
            set Storage = NewValue

tracked_value := class(subscribable_property):

UseTrackedValue():void =
    Object := tracked_value{}

    # Uses getter - returns 110 (Storage + 10)
    Current := Object.Value

    # Uses setter - validates and updates Storage
    set Object.Value = 150
```
<!-- #>-->

The `external{}` keyword indicates the field has no direct storage—all
access goes through the accessor methods. This pattern is powerful for
implementing property change notifications, validation, computed
properties, and other scenarios requiring logic around field access.

**Important:** Fields with accessors defined in interfaces cannot be
overridden in implementing classes. The accessor implementation is
fixed by the interface.

## Book of Verse Source Unit: 11_types.md

### Types

Every value has a type, and understanding the type system is
fundamental to mastering any language. Types are not merely labels -
they form a rich hierarchy that governs how values flow through your
program, what operations are permitted, and how the compiler reasons
about your code. The type system combines static verification with
practical flexibility, catching errors at compile time while still
allowing sophisticated patterns of code reuse and abstraction.

At the top of this hierarchy sits `any`, the universal supertype from
which all other types descend. At the bottom lies `false`, the empty
type that contains no values at all (the uninhabited type). Between
these extremes exists a carefully designed lattice of types, each with
its own capabilities and constraints.

#### Understanding Subtyping

Subtyping is the foundation of the type hierarchy. When we say that
type A is a subtype of type B, we mean that every value of type A can
be used wherever a value of type B is expected. This relationship
creates a natural ordering among types, from the most specific to the
most general.

Consider the relationship between `rational` and `int`. Every
integer is a rational number, but not every rational is an integer.
Therefore, `int` is a subtype of `rational`. This means you can
pass an `int` to any function expecting a `rational`, but not vice versa:

<!--versetest
GetInt(X:int):void = Print("Integer: {X}")
GetRat(X:rational):void = Print("Rational")
assert:
    MyRat:rational = 1/3
    MyInt:int = -10
    GetRat(MyInt)
<# 
-->
<!-- 01 -->
```verse
GetInt(X:int):void = Print("Integer: {X}")
GetRat(X:rational):void = Print("Rational")

MyRat:rational = 1/3
MyInt:int = -10

GetRat(MyInt)  # OK -- int is a subtype of rational
GetInt(MyRat)  # Compile error -  rational is not a subtype of int
```
<!-- #> -->

The subtyping relationship extends to composite types in sophisticated
ways. Arrays and tuples follow covariant subtyping rules for their
elements. This means that `[]int` is a subtype of `[]rational`.
Similarly, `tuple(int, int)` is a subtype of `tuple(rational,
rational)`. This covariance allows collections of more specific types
to be used where collections of more general types are expected.

Maps exhibit more complex subtyping behavior. A map type `[K1]V1` is a
subtype of `[K2]V2` when `K2` is a subtype of `K1` (contravariant in
keys) and `V1` is a subtype of `V2` (covariant in values). The
contravariance in keys might seem counterintuitive at first, but it
ensures type safety: if you can look up values using a more general
key type, you must be able to handle more specific key types as well.

Classes and interfaces introduce nominal subtyping through
inheritance. When a class inherits from another class or implements an
interface, it explicitly declares a subtyping relationship:

<!--versetest 02 -->
<!-- 02 -->
```verse
vehicle := class:
    Speed:float = 0.0

car := class(vehicle):  # car is a subtype of vehicle
    NumDoors:int = 4

sports_car := class(car):  # sports_car is a subtype of car (and vehicle)
    Turbo:logic = true
```

This inheritance hierarchy means that a `sports_car` can be used
anywhere a `car` or `vehicle` is expected, but not the reverse. The
subtype inherits all fields and methods from its supertypes while
potentially adding new ones or overriding existing ones.

#### Numeric and String Conversions

All type conversions must be explicit, a design choice that eliminates
entire categories of bugs while making the programmer's intent
clear. Converting between numeric types illustrates this principle
clearly. To convert an integer to a float, you multiply by 1.0:

<!--versetest-->
<!-- 03 -->
```verse
MyI:int   = 42
MyF:float = MyI * 1.0  # Explicit conversion to float
```

!!! note 
    The strongest reason for disallowing implicit conversions is that
	they can cause code to break when new overloadings to a function
	are added. Imagine a call to function `f` that takes a float such
	as `f(1)`, if the integer argument was implicitly converted to a 
	float and, in some future library release, an overload `f(:int)` 
	was added, the call would silently invoke that new function
	and potentially change the result of the computation.

The reverse conversion, from float to integer, requires choosing a
rounding strategy:

<!--versetest-->
<!-- 04 -->
```verse
MyF:float = 3.7
Opt1:int = Floor[MyF]  # Results in 3
Opt2:int = Ceil[MyF]   # Results in 4
Opt3:int = Round[MyF]  # Results in 4 (rounds to nearest)
```

These conversion functions are failable - they have the `<decides>`
effect and will fail if passed non-finite values like `NaN` or
`Inf`. The explicit failure forces you to handle edge cases:

<!--versetest 05 -->
<!-- 05 -->
```verse
SafeConvert(Value:float):int =
    if:
       Value <> NaN
       Value <> Inf
       Result:= Floor[Value]
    then:
       Result
    else:
       0  # Assuming that this is safe value
```

String conversions follow similar principles. The `ToString()`
function converts various types to their string representations, while
string interpolation provides a convenient syntax for embedding values
in strings:

<!--versetest-->
<!-- 06 -->
```verse
Score:int  = 1500
Msg:string = "Your score: {Score}"  # Implicit ToString() call
```

#### Type `any`

<!-- TODO add a link to the builtin types -->

Type `any` is at the top of the type hierarchy it is the universal
supertype that can hold a value of any type. Every type in Verse is a
subtype of `any`, making it the most permissive type.  It serves as an
escape hatch when you genuinely need to work with values of unknown or
varying types.

Once a value is typed as `any`, you've effectively told the compiler
"I do not know what this is," and the compiler responds by preventing
most operations. This is by design—without knowing the actual type,
the compiler cannot verify that operations are safe.

You can explicitly coerce any value to `any` using function call
syntax, `any(42)`. 

Verse automatically coerces values to `any` when their types would
otherwise be incompatible. Understanding these rules help when working
with heterogeneous data.

Mixed-type arrays and maps automatically coerces to the most specific shared
type, if no common type is found, the array coerces to `any`:

<!--versetest
SomeFunction():void={}
-->
<!-- 09 -->
```verse
MixedArray := array{42, "hello", true, 3.14} # []comparable
MixedMap := map{0=>"zero", 1=>1, 2=>2.0} # [int]comparable
ConfigMap := map{"count"=>42, "process"=>SomeFunction, "name"=>"Player"} # [string]any
```

Conditional expressions with disjoint branch types produce `any`:

<!--versetest-->
<!-- 11 -->
```verse
# If branches return different types
GetValue(UseString:logic):any =
    if (UseString?):
        "text result"
    else:
        42
```

Logical OR with disjoint types coerces to `any`:

<!--versetest-->
<!-- 12 -->
```verse
# Returns either int or string
OneOf(Flag:logic, I:int, S:string):any =
    (if (Flag?) then {option{I}} else {1=2}) or S
```

The `any` type has restrictions that reflect its role as a generic
container:

- You cannot use equality operators with `any`
- Because `any` is not comparable, it cannot be used as a map key type
- Because `any` is not castable, it is a sticky type.

##### Generic Functions and Type Preservation

Generic functions with `where t:type` constraints behave fundamentally differently from functions that accept `any`. Understanding this difference is crucial for writing type-safe code.

When you pass a value to a function with parameter type `any`, the type information is lost:

<!--versetest-->
<!-- 53 -->
```verse
AcceptAny(X:any):any = X

MyMap:[int]string = map{1 => "one"}
Result := AcceptAny(MyMap)  # Result has type any - type info lost
```

In contrast, generic functions preserve exact types:

<!--versetest-->
<!-- 54 -->
```verse
Identity(X:t where t:type):t = X

MyMap:[int]string = map{1 => "one"}
Result := Identity(MyMap)  # Result has type [int]string - type preserved
MyMap = Result  # Succeeds - same type
```

This preservation extends to all container types, including arrays, maps, tuples, and structs. The generic type parameter captures the complete type, including:

- Map key and value types
- Array element types
- Tuple component types
- Struct field types

**Practical implications:**

Container types passed through generic functions maintain their structure completely:

<!--versetest-->
<!-- 55 -->
```verse
Identity(X:t where t:type):t = X

# All key types are preserved
IntMap:[int]int = map{1 => 2, 3 => 4}
IntMap = Identity(IntMap)  # Same type

FloatMap:[float]string = map{1.0 => "one", 2.5 => "two"}
FloatMap = Identity(FloatMap)  # Same type

TupleMap:[tuple(int, string)]int = map{(1, "a") => 100}
TupleMap = Identity(TupleMap)  # Same type

# Iteration and equality work as expected
for (Key->Value : IntMap):
    Identity(IntMap)[Key] = Value  # All lookups succeed
```

This makes generic functions the preferred approach when you need to write reusable code that works with containers while maintaining type safety.

#### Class and Interface Casting

Verse provides two distinct casting mechanisms for classes and
interfaces: fallible casts for runtime type checking, and infallible
casts for compile-time verified conversions. All classes and interfaces
support dynamic casting regardless of whether they are marked with the
`<castable>` attribute—`<castable>` is only required for using the
`castable_subtype` type constraint.

Fallible casts use square bracket syntax `TargetType[value]` to
perform runtime type checks. These casts succeed and return the
casted value (`TargetType`), failing if the value is not of
a valid target type or a subtype:

<!--versetest
component := class<castable>:
    Name:string = "Component"

physics_component := class<castable>(component):
    Velocity:float = 0.0

render_component := class<castable>(component):
    Material:string = "default"

ProcessComponent(Comp:component):void =
    if (PhysicsComp := physics_component[Comp]):
        Print("Physics velocity: {PhysicsComp.Velocity}")
    else if (RenderComp := render_component[Comp]):
        Print("Render material: {RenderComp.Material}")
    else:
        Print("Unknown component type")
<#
-->
<!-- 17 -->
```verse
# Define a class hierarchy
component := class<castable>:
    Name:string = "Component"

physics_component := class<castable>(component):
    Velocity:float = 0.0

render_component := class<castable>(component):
    Material:string = "default"

# Runtime type checking with fallible casts
ProcessComponent(Comp:component):void =
    if (PhysicsComp := physics_component[Comp]):
        # Successfully cast - PhysicsComp is physics_component
        Print("Physics velocity: {PhysicsComp.Velocity}")
    else if (RenderComp := render_component[Comp]):
        # Different type - RenderComp is render_component
        Print("Render material: {RenderComp.Material}")
    else:
        # Neither type matched
        Print("Unknown component type")
```
<!-- #> -->

The cast expression fails if the runtime type does not
match, allowing you to use it directly in conditionals. The optional
binding pattern `(Variable := Expression)` both performs the cast and
binds the result to a variable when successful.

For classes marked `<unique>`, fallible casts preserve identity—a
successful cast returns the same instance, not a copy:

<!--versetest
entity := class<unique><castable>:
    ID:int
player := class<unique>(entity):
    Name:string
assert:
	P := player{ID := 1, Name := "Alice"}
	if (E := entity[P]):
		E = P
<#
-->
<!-- 18 -->
```verse
entity := class<unique><castable>:
    ID:int

player := class<unique>(entity):
    Name:string

# Create an instance
P := player{ID := 1, Name := "Alice"}

# Cast to base type
if (E := entity[P]):
    E = P  # True - same instance
```
<!-- #> -->

Fallible casts work **only with class and interface types**. You
cannot dynamically cast from or to primitive types, structs, arrays,
or other value types:

<!--versetest
assert_semantic_error(3512, 3509, 3547):
    component := class<castable>{}
    Comp := component[42]

assert_semantic_error(3512, 3509, 3547):
    component := class<castable>{}
    Comp := component[3.14]

assert_semantic_error(3512, 3509, 3547):
    component := class<castable>{}
    Comp := component["text"]

assert_semantic_error(3512, 3509, 3547):
    component := class<castable>{}
    Comp := component[array{1,2}]

assert_semantic_error(3512, 3509, 3547, 3512):
    component := class<castable>{}
    Value := int[component{}]

assert_semantic_error(3512, 3552, 3547, 3512):
    component := class<castable>{}
    Value := logic[component{}]

assert_semantic_error(3512, 3552, 3547, 3512):
    component := class<castable>{}
    Value := (?int)[component{}]
<#
-->
<!-- 19 -->
```verse
component := class<castable>{}

# Error: cannot cast from primitives
Comp := component[42]          # int to class - not allowed
Comp := component[3.14]        # float to class - not allowed
Comp := component["text"]      # string to class - not allowed
Comp := component[array{1,2}]  # array to class - not allowed

# Error: cannot cast to non-class types
Value := int[component{}]      # class to int - not allowed
Value := logic[component{}]    # class to logic - not allowed
Value := (?int)[component{}]   # class to option - not allowed
```
<!-- #>-->

The restriction exists because fallible casts rely on runtime type
information that only classes and interfaces maintain. Value types
like integers and structs do not have runtime type tags.

*Infallible* casts use parenthesis syntax `TargetType(value)` for
conversions that the compiler can verify will always succeed. These
casts require the source type to be a compile-time subtype of the
target type:

<!--versetest-->
<!-- 20 -->
```verse
component := class<castable>:
    Name:string = "Component"

physics_component := class<castable>(component):
    Velocity:float = 0.0

# Upcasting: always safe, always succeeds
Base:physics_component = physics_component{Velocity := 10.0}

BaseComp:component = component(Base) # upcast during expression
# or
AlsoBaseComp:component = Base # upcast during assignment
```

Any type can be infallibly cast to `void`, which discards the value:

<!--versetest
component:=class{}
-->
<!-- 21 -->
```verse
void(42)           # Discard an integer
void("result")     # Discard a string
void(component{})  # Discard an object
```

This implicitly happens when you call a function for its side effects
and want to ignore its return value.

##### Dynamic Type-Based Casting

Types in Verse are first-class values, which means you can store types
in variables and use them dynamically for casting. This enables
powerful patterns for runtime polymorphism:

<!--versetest
component := class<castable>{}
physics_component := class<castable>(component){}
render_component := class<castable>(component){}

ComponentType:castable_subtype(component) = physics_component

TestComponent(Comp:component, ExpectedType:castable_subtype(component)):logic =
    if (Specific := ExpectedType[Comp]):
        true
    else:
        false

assert:
   P := physics_component{}
   TestComponent(P, physics_component)
   TestComponent(P, render_component)
<#
-->
<!-- 22 -->
```verse
# Type hierarchy
component := class<castable>{}
physics_component := class<castable>(component){}
render_component := class<castable>(component){}

# Store types as values
ComponentType:castable_subtype(component) = physics_component

# Cast using the stored type
Test(Comp:component, ExpectedType:castable_subtype(component)):logic =
    if (Specific := ExpectedType[Comp]):
        true  # Component matches expected type
    else:
        false

# Use with different types
P := physics_component{}
Test(P, physics_component)  # true
Test(P, render_component)   # false
```
<!-- #> -->

This pattern is particularly powerful when the type to check is not
known until runtime:

<!--versetest
entity:=class{}
component := class<castable>:
    Owner:entity
physics_component := class<castable>(component){}
render_component := class<castable>(component){}
Components:[]component=array{}
ProcessSpecific(:component)<computes>:void={}
LoadedConfig:string=""
-->
<!-- 23 -->
```verse
# Select type based on configuration
GetComponentType(Config:string):castable_subtype(component) =
    if (Config = "physics"):
        physics_component
    else if (Config = "render"):
        render_component
    else:
        component

# Use the dynamically selected type
RequiredType := GetComponentType(LoadedConfig)
for (Comp : Components):
    if (Specific := RequiredType[Comp]):
        # Process components of the required type
        ProcessSpecific(Specific)
```

This bridges compile-time type safety with runtime flexibility,
allowing type decisions to be made based on program state while
maintaining type correctness.

#### Where Clauses

Where clauses are the mechanism for constraining type parameters in
generic code. They appear after type parameters and specify
requirements that types must satisfy to be valid arguments. This
creates a powerful system for writing generic code that is both
flexible and type-safe.

<!--versetest-->
<!-- 24 -->
```verse
# Simple subtype constraint
Process(Value:t where t:subtype(comparable)):void =
    if (Value = Value):  # We know it supports equality
        Print("Value equals itself")
```

Using the same type in multiple constraints is not yet supported, when
implemented, it will allow to write code such as:

<!--versetest
assert_semantic_error(3588, 3588, 3503, 3503, 3506, 3532):
    printable := interface:
        PrintIt():void
    F(In:t where t:subtype(comparable), t:subtype(printable)):t =
        Print("Processing: {In}")
        In
<#
-->
<!-- 25 -->
```verse
# Multiple constraints on the same type
F(In:t where t:subtype(comparable), t:subtype(printable)):t = # Not supported
    Print("Processing: {In}")
    In
```
<!-- #> -->

Where clauses become more powerful when working with multiple type parameters:

<!--versetest-->
<!-- 26 -->
```verse
# Independent constraints on different parameters
Combine(A:t1, B:t2 where t1:type, t2:type):tuple(t1, t2) =
    (A, B)

# Related constraints
Convert(From:t1, Converter:type{_(:t1):t2} where t1:type, t2:type):t2 =
    Converter(From)
```

Where clauses can express sophisticated relationships between types:

<!--versetest
Contains(Arr:[]t, Item:t where t:type)<decides><computes>:logic = false
-->
<!-- 27 -->
```verse
# Constraint that ensures compatible types for an operation
Merge(Container1:[]t, Container2:[]t where t:subtype(comparable)):[]t =
    var Result:[]t = Container1
    for (Element : Container2, not Contains[Result, Element]):
        set Result += array{Element}
    Result

# Function type constraints
ApplyTwice(F:type{_(:t):t}, Value:t where t:type):t =
    F(F(Value))
```

Where clauses enable sophisticated generic programming patterns:

<!--versetest-->
<!-- 28 -->
```verse
MapFunction(F:type{_(:a):b}, Container:[]a where a:type, b:type):[]b =
    for (Element : Container):
        F(Element)
```

#### Refinement Types

While `where` clauses constrain type parameters in generic code,
**refinement types** use `where` to constrain the *values* a type can
hold. This creates subtypes that only accept values satisfying
specific conditions, enabling domain-specific constraints enforced by
the type system.

A natural question is: why fail on out-of-range values when you could
just clamp? The answer is that clamping silently propagates wrong
values, which is acceptable for some domains (UI opacity) but
dangerous in others. In algorithms where exact values matter — bit
manipulation, hashing, Unicode code point operations, coordinate
system math — silently clamping an out-of-range value produces
incorrect results that are extremely hard to track down. Refinement
types make the constraint explicit and force the caller to handle
violations, catching bugs at their source rather than letting them
propagate.

In practice, type aliases like `positive_int` or `zero_to_one_float`
make refinement types convenient to reuse across a codebase without
repeating the constraint expression each time.

A refinement type defines a constrained subtype using value predicates:

<!--versetest
percent := type{_X:float where 0.0 <= _X, _X <= 1.0} 
-->
<!-- 29 -->
```verse
# Percentages: floats between 0.0 and 1.0
# percent := type{_X:float where 0.0 <= _X, _X <= 1.0}

# Valid assignments
Opacity:percent = 0.5
Alpha:percent = 1.0

# Invalid: out of range (runtime check fails)
# BadPercent:percent = 1.5  # Fails at assignment
```

**Syntax structure:**

<!--NoCompile-->
<!-- 30 -->
```verse
TypeName := type{_Variable:BaseType where Constraint1, Constraint2, ...}
```

- `_Variable` is a placeholder for the value being constrained
- `BaseType` is `int` or `float`
- Constraints are comparison expressions using `<=`, `<`, `>=`, or `>`

Integer refinements restrict int values to specific ranges:

<!--versetest
age := type{_X:int where 0 <= _X, _X <= 120}
ValidAge:age = 25
positive_int := type{_X:int where _X > 0}
Count:positive_int = 42
small_int := type{_X:int where _X < 100}
<#
-->
<!-- 31 -->
```verse
# Age between 0 and 120
age := type{_X:int where 0 <= _X, _X <= 120}

ValidAge:age = 25
# InvalidAge:age = 150  # Fails constraint

# Positive integers
positive_int := type{_X:int where _X > 0}

Count:positive_int = 42
# Zero:positive_int = 0  # Fails: not positive

# Range with single bound
small_int := type{_X:int where _X < 100}
```
<!-- #> -->

Float refinements handle continuous ranges with IEEE 754 semantics:

<!--versetest
normalized := type{_X:float where 0.0 <= _X, _X <= 1.0}
positive := type{_X:float where _X > 0.0}
celsius := type{_X:float where _X >= -273.15}
<#
-->
<!-- 32 -->
```verse
# Unit interval [0.0, 1.0]
normalized := type{_X:float where 0.0 <= _X, _X <= 1.0}

# Positive floats
positive := type{_X:float where _X > 0.0}

# Temperature in Celsius above absolute zero
celsius := type{_X:float where _X >= -273.15}
```
<!-- #> -->

Finite Floats (Excluding Infinity):

<!--versetest
finite := type{_X:float where -Inf < _X, _X < Inf}
assert:
	MaxFinite:finite = 1.7976931348623157e+308
	MinFinite:finite = -1.7976931348623157e+308
<#
-->
<!-- 33 -->
```verse
# Finite values only (no ±Inf)
finite := type{_X:float where -Inf < _X, _X < Inf}

# Maximum and minimum finite IEEE 754 doubles
MaxFinite:finite = 1.7976931348623157e+308
MinFinite:finite = -1.7976931348623157e+308

# Invalid: infinities excluded
# Infinite:finite = Inf  # Fails constraint
```
<!-- #> -->

##### IEEE 754 Edge Cases

**Negative and Positive Zero:**

IEEE 754 distinguishes between `+0.0` and `-0.0`. In verse Zero is just Zero,
with no distinction between positve or negative.

<!--versetest-->
When any expression evaluates to Zero, the sign is discarded:
<!-- 34 -->
```verse
# Integer Zero (type{0})
Value1 := -0
Value2 := +0

Value1 = Value2 # Succeeds
-0 = +0         # Succeeds

# Float Zero (type{0.0})
Value3 := -0.0
Value4 := +0.0

Value3 = Value4 # Succeeds
-0.0 = +0.0     # Succeeds
```

**Floating-Point Precision:**

Constraints respect exact IEEE 754 representations:

<!--versetest
small_float := type{_X:float where _X < 0.1}
assert:
    Tiny:small_float = 0.09999999999999999167332731531132594682276248931884765625
<#
-->
<!-- 36 -->
```verse
# Values strictly less than 0.1
small_float := type{_X:float where _X < 0.1}

# Valid: largest float before 0.1
Tiny:small_float = 0.09999999999999999167332731531132594682276248931884765625

# Invalid: 0.1's actual representation is slightly above 0.1
# NotSmall:small_float = 0.1000000000000000055511151231257827021181583404541015625
```
<!-- #> -->

The decimal `0.1` cannot be represented exactly in binary
floating-point, so the actual stored value is slightly above the
mathematical 0.1.

##### Constraint Expression Restrictions

Refinement type constraints have strict limitations on what
expressions are allowed:

**Only Literal Values:** Constraints must use literal numbers, not
variables or expressions:

<!--versetest
bounded := type{_X:float where _X < 100.0}

assert_semantic_error(3502):
    Limit:float = 100.0
    bad_type := type{_X:float where _X < Limit}

assert_semantic_error(3512, 3502):
    GetMax():float = 100.0
    bad_type := type{_X:float where _X < GetMax()}

assert_semantic_error(3506, 3502):
    Config := module{Max:float = 100.0}
    bad_type := type{_X:float where _X < (Config:)Max}
<#
-->
<!-- 37 -->
```verse
# Valid: literal float
bounded := type{_X:float where _X < 100.0}

# Invalid: cannot use variables
Limit:float = 100.0
bad_type := type{_X:float where _X < Limit}  # ERROR

# Invalid: cannot use function calls
GetMax():float = 100.0
bad_type := type{_X:float where _X < GetMax()}  # ERROR

# Invalid: cannot use qualified names
Config := module{Max:float = 100.0}
bad_type := type{_X:float where _X < (Config:)Max}  # ERROR
```
<!-- #> -->

This ensures constraints are statically known at compile time.

**Float Literals Required for Float Types:** When constraining floats,
bounds must be float literals (with decimal point):

<!--versetest
good_float := type{_X:float where _X <= 142.0}

assert:
     1
<#
-->
<!-- 38 -->
```verse
# Invalid: integer literal in float constraint
# bad_float := type{_X:float where _X <= 142}  # ERROR 3502

# Valid: float literal
good_float := type{_X:float where _X <= 142.0}
```
<!-- #> -->

**NaN Not Allowed:** Not a Number cannot appear in
constraints:

<!--versetest-->
<!-- 39 -->
```verse
# Invalid: NaN in constraint
# nan_type := type{_X:float where _X <= NaN}      # ERROR 3502
# nan_type := type{_X:float where NaN <= _X}      # ERROR 3502
# nan_type := type{_X:float where 0.0/0.0 <= _X}  # ERROR 3502
```

Since `NaN` comparisons are always false, such constraints would be meaningless.

**Allowed Literal Forms:**

- Float literals: `1.0`, `3.14`, `-2.5`, `1.7976931348623157e+308`
- Integer literals: `0`, `42`, `-100` (for int refinements)
- Special float values: `Inf`, `-Inf`

##### Fallible Casts

Refinement types are checked at assignment and through fallible casts:

<!--versetest-->
<!--versetest
percent := type{_X:float where 0.0 <= _X, _X <= 1.0}
GetInputFromUser<public>()<computes>:float = 50.0
ProcessPercent<public>(P:percent):void = {}
ShowError<public>(Msg:string):void = {}
assert:
   Valid:percent = 0.5
   UserInput:float = GetInputFromUser()
   if (Value := percent[UserInput]):
       ProcessPercent(Value)
   else:
       ShowError()
<#
-->
<!-- 40 -->
```verse
percent := type{_X:float where 0.0 <= _X, _X <= 1.0}

# Direct assignment (compile-time known)
Valid:percent = 0.5  # OK

# Runtime check with fallible cast
UserInput:float = GetInputFromUser()
if (Value := percent[UserInput]):
    # UserInput was in [0.0, 1.0]
    ProcessPercent(Value)
else:
    # Out of range
    ShowError()
```
<!-- #> -->

The cast `percent[UserInput]` returns `percent` succeeding if the
value satisfies the constraint, or failing otherwise.

##### Examples

Refinement types work as parameter and return types:

<!--versetest
finite := type{_X:float where -Inf < _X, _X < Inf}
Half(X:finite):float = X / 2.0
assert:
   Half(100.0)
   Half(1.0)
<#
-->
<!-- 41 -->
```verse
finite := type{_X:float where -Inf < _X, _X < Inf}

# Parameter with constraint
Half(X:finite):float = X / 2.0

Half(100.0)  # Returns 50.0
Half(1.0)    # Returns 0.5

# Cannot pass infinity
# Half(Inf)  # ERROR 3509: Inf not in finite
```
<!-- #> -->

**Coercion and Negation:**

<!--versetest
percent := type{_X:float where 0.0 <= _X, _X <= 1.0}
negative_percent := type{_X:float where _X <= 0.0, _X >= -1.0}

assert:
   MakePercent():percent = 0.5
   NegValue:negative_percent = -MakePercent()
   NegValue2:negative_percent = ---0.7
<#
-->
<!-- 42 -->
```verse
percent := type{_X:float where 0.0 <= _X, _X <= 1.0}
negative_percent := type{_X:float where _X <= 0.0, _X >= -1.0}

MakePercent():percent = 0.5

# Negation preserves constraint compatibility
NegValue:negative_percent = -MakePercent()  # -0.5 valid

# Multiple negations
NegValue2:negative_percent = ---0.7  # Triple negation = -0.7
```
<!-- #> -->

##### Overloading Restrictions

Overlapping refinement types cannot be used for function
overloading—they are ambiguous:

<!--versetest
assert_semantic_error(3532):
    percent := type{_X:float where 0.0 <= _X, _X <= 1.0}
    not_infinity := type{_X:float where Inf > _X}
    F(X:percent):float = 0.0
    F(X:not_infinity):float = X
<#
-->
<!-- 43 -->
```verse
percent := type{_X:float where 0.0 <= _X, _X <= 1.0}
not_infinity := type{_X:float where Inf > _X}

# ERROR 3532: Cannot distinguish - percent ⊂ not_infinity
# F(X:percent):float = 0.0
# F(X:not_infinity):float = X

# Calling F(0.5) would be ambiguous - which overload?
```
<!-- #>-->

However, **disjoint** refinement types can overload:
<!--versetest
positive := type{_X:float where _X > 0.0}
negative := type{_X:float where _X < 0.0}
F(X:positive):float = X
F(X:negative):float = X + 1.0
assert:
   F(1.0)=1.0
   F(-1.0)=0.0
<#
-->
<!-- 44 -->
```verse
positive := type{_X:float where _X > 0.0}
negative := type{_X:float where _X < 0.0}

# Valid: ranges do not overlap (zero excluded from both)
F(X:positive):float = X
F(X:negative):float = X + 1.0

F(1.0)   # Returns 1.0 (positive overload)
F(-1.0)  # Returns 0.0 (negative overload)
# F(0.0)  # Would fail - neither overload matches
```
<!-- #> -->

#### Comparable and Equality

The `comparable` type represents a special subset of types that
support equality comparison. Not all types can be compared for
equality - this is a deliberate design choice that prevents
meaningless comparisons and ensures that equality has well-defined
semantics.

A type is comparable if its values can be meaningfully tested for
equality. The basic scalar types are all comparable: `int`, `float`,
`rational`, `logic`, `char`, and `char32`. Compound types are
comparable if all their components are comparable. This means arrays
of integers are comparable, tuples of floats and strings are
comparable, and maps with comparable keys and values are comparable.

The equality operators `=` and `<>` are defined in terms of the
comparable type:

<!--NoCompile-->
<!-- 45 -->
```verse
operator'='(X:t, Y:t where t:subtype(comparable))<decides>:t
operator'<>'(X:t, Y:t where t:subtype(comparable))<decides>:t
```

The signatures require that both operands be subtypes of comparable
and the return type is the least upper bound of their types.

<!--versetest
assert:
    0 = 0
    0.0 = 0.0

<#
-->
<!-- 46 -->
```verse
0 = 0        # Succeeds - both are int
0.0 = 0.0    # Succeeds - both are float
0 = 0.0      # Fails - there is no implicit conversion from int to float
```
<!-- #> -->

Here is an example that highlights how the return type of `=` is computed:

<!--46b -->
```verse
I:int=1
R:rational=1/3
X:rational= (I=R)  # Compiles and fails at runtime

I:int=1
S:string="hi"
Y:comparable= (I=S)  # Compiles and fails at runtime
```

In the case of variable `X`, its type can be either `rational` or
`comparable`. For variable `Y`, the only common type between `int` and
`string` is `comparable`.


Classes require special handling for comparability. By default, class
instances are not comparable because there's no universal way to
define equality for user-defined types. However, you can make a class
comparable using the `unique` specifier:

<!--versetest
entity := class<unique>:
    ID:int
    Name:string

F()<decides>:void={
Player1 := entity{ID := 1, Name := "Alice"}
Player2 := entity{ID := 1, Name := "Alice"}
Player3 := Player1

Player1 = Player2  # Fails - different instances
Player1 = Player3  # Succeeds - same instance
}<#
-->
<!-- 47 -->
```verse
entity := class<unique>:
    ID:int
    Name:string

Player1 := entity{ID := 1, Name := "Alice"}
Player2 := entity{ID := 1, Name := "Alice"}
Player3 := Player1

Player1 = Player2  # Fails - different instances
Player1 = Player3  # Succeeds - same instance
```
<!--versetest
#>
-->

With the `unique` specifier, instances are only equal to themselves
(identity equality), not to other instances with the same field values
(structural equality). This provides a clear, predictable semantics
for class equality.

##### Comparable as a Generic Constraint

The `comparable` type is commonly used as a constraint in generic
functions to ensure operations like equality testing are available:

<!--versetest
Find(Items:[]t, Target:t where t:subtype(comparable))<decides>:int =
    Results := for (Index->Item:Items, Item = Target):
        Index
    Results[0]

assert:
    # Works with any comparable type
    Position := Find[array{"apple", "banana", "cherry"}, "banana"]  # Succeeds and returns 1
    Position = 1
<#
-->
<!-- 48 -->
```verse
Find(Items:[]t, Target:t where t:subtype(comparable))<decides>:int =
    Results := for (Index->Item:Items, Item = Target):
        Index
    Results[0]

# Works with any comparable type
Position := Find[array{"apple", "banana", "cherry"}, "banana"]  # Succeeds and returns 1
```
<!-- #>-->

##### Array-Tuple Comparison

A notable feature of Verse's equality system is that arrays and tuples
of comparable elements can be compared with each other:

<!--versetest-->
<!-- 49 -->
```verse
# Arrays can equal tuples
array{1, 2, 3} = (1, 2, 3)       # Succeeds
(4, 5, 6) = array{4, 5, 6}       # Succeeds - bidirectional

# Inequality also works
array{1, 2, 3} <> (1, 2, 4)      # Succeeds - different values
```

This comparison works structurally - the sequences must have the same
length and corresponding elements must be equal. This feature allows
functions expecting arrays to accept tuples, increasing flexibility.

##### Overload Distinctness with Comparable

You cannot create overloads where one parameter is a specific comparable type and another is the general `comparable` type, as this creates ambiguity:

<!--versetest
assert_semantic_error(3532):
    F(X:int):void = {}
    F(X:comparable):void = {}

assert_semantic_error(3532):
    unique_class := class<unique>{}
    G(X:unique_class):void = {}
    G(X:comparable):void = {}
<#
-->
<!-- 50 -->
```verse
# Not allowed - ambiguous overloads
F(X:int):void = {}
F(X:comparable):void = {}  # ERROR: int is already comparable

# Not allowed with unique classes either
unique_class := class<unique>{}
G(X:unique_class):void = {}
G(X:comparable):void = {}  # ERROR: unique_class is comparable
```
<!-- #> -->

However, you can overload with non-comparable types:

<!--versetest-->
<!-- 51 -->
```verse
# This is allowed
regular_class := class{}  # Not comparable
H(X:regular_class):void = {}
H(X:comparable):void = {}  # OK: no ambiguity
```

##### Dynamic Comparable Values

When working with heterogeneous collections, you may need to box
comparable values into the `comparable` type explicitly. These boxed
values maintain their equality semantics:

<!--versetest-->
<!-- 52 -->
```verse
AsComparable(X:comparable):comparable = X

# Boxed values compare correctly with both boxed and unboxed
array{AsComparable(1)} = array{1}              # Succeeds
array{AsComparable(1)} = array{AsComparable(1)} # Succeeds
array{AsComparable(1)} <> array{2}             # Succeeds

# With direct upcasting:
comparable(15) = comparable(15)     # Succeeds
comparable("Hello") = "Hello"       # Succeeds
```

This allows you to create collections that mix different comparable
types by boxing them all to `comparable`.

##### Map Keys and Comparable

Map keys must be comparable types. Most comparable types can be used
as map keys, including:

- All numeric types: `int`, `float`, `rational`
- Character types: `char`, `char32`
- Text: `string`
- Enumerations
- `<unique>` classes
- Optionals of comparable types: `?t` where `t` is comparable
- Arrays of comparable types: `[]t` where `t` is comparable
- Tuples of comparable types
- Maps with comparable keys and values: `[k]v`
- Structs with comparable fields

Note that while `float` can be used as a map key, floating-point
special values have specific equality semantics (see [Map
documentation](12_BOOK_OF_VERSE_I_FOUNDATIONS.md#floats) for details on
`NaN` and zero handling).

There is currently no way to make a regular class comparable by
writing a custom comparison method. Only the `<unique>` specifier
enables class comparability through identity equality.

#### Type Hierarchies

The type system forms a lattice rather than a simple tree. This means
types can have multiple supertypes, though multiple inheritance is
currently limited to interfaces. Understanding these relationships
helps you design flexible, reusable code.

##### Understanding void

Unlike `any`, which erases type information, `void` serves as a
"discard" type indicating that a value's specific type does not matter.

Functions with `void` return type can return any value, which is then
discarded by the type system:

<!--versetest
WriteToFile(:string)<transacts>:void = {}
-->
<!-- 77 -->
```verse
LogEvent(Message:string)<transacts>:void =
    WriteToFile(Message)
    42                   # Returns int, but typed as void

F():void = 1             # Valid - returns int, typed as void
F()                      # Result is void
```

Despite being typed as `void`, these functions still produce their
computed values—the values are simply not accessible through the type
system. This ensures side effects and computations occur even when the
return value is discarded:

<!--versetest-->
<!-- 78 -->
```verse
MakePair(X:string, Y:string):void = (X, Y)

# Function computes the pair even though return type is void
MakePair("hello", "world")  # Still creates ("hello", "world")
```

Functions with `void` parameters accept any argument type:

<!--versetest-->
<!-- 79 -->
```verse
Discard(X:void):int = 42

Discard(0)               # int → void 
Discard(1.5)             # float → void 
Discard("test")          # string → void 
```

Class fields can be typed as `void`, accepting any initialization
value:

<!--versetest-->
<!-- 80 -->
```verse
config := class:
    Setting:void = array{1, 2}  # Default with array
```

In function types, `void` participates in variance:

<!--versetest-->
<!-- 81 -->
```verse
IntIdentity(X:int):int = X

# Covariant return: subtype allowed in return position
F:int->void = IntIdentity  # int->int → int->void ✓
# void is supertype of int, so this works

AcceptVoid(X:void):int = 19

# Contravariant parameter: supertype in parameter position
G:int->int = AcceptVoid    # void->int → int->int ✓
# Can use function accepting void where function accepting int expected
```

However, `void` in parameter position does NOT allow conversion the
other way:

<!--versetest
### Test that this conversion is not allowed
assert_semantic_error(3509):
    IntFunction(X:int):int = X
    F:void->int = IntFunction  # ERROR: Cannot convert int->int to void->int
<#
-->
<!-- 82 -->
```verse
IntFunction(X:int):int = X
# F:void->int = IntFunction  # ERROR
# Cannot convert int parameter to void parameter in function type
```
<!-- #>-->

**void vs false**: The `false` type is the empty/bottom type
(uninhabited type) with no values. It's the opposite of `void`:

- **`void`**: Universal supertype - all types are subtypes of void, contains all values
- **`false`**: Bottom type - subtype of all types, contains zero values

Between the universal supertypes (`any`, `void`) and the bottom type
(`false`), types form natural groupings. The numeric types (`int`,
`float`, `rational`) share common arithmetic operations but do not form
a single hierarchy - they are siblings rather than ancestors and
descendants. The container types (arrays, maps, tuples, options) each
have their own subtyping rules based on their element types.

Understanding variance is crucial for working with generic
containers. Arrays and options are covariant in their element type -
if A is a subtype of B, then `[]A` is a subtype of `[]B` and `?A` is a
subtype of `?B`. This allows natural code like:


<!--versetest
RationalPrinter(X:rational):string=""
-->
<!-- 89 -->
```verse
ProcessNumbers(Nums:[]rational):void =
    for (N : Nums):
        Print("{RationalPrinter(N)}")

IntArray:[]int = array{1, 2, 3}
ProcessNumbers(IntArray)  # Works due to covariance
```

Functions exhibit more complex variance. They're contravariant in
their parameter types and covariant in their return types. A function
type `(T1)->R1` is a subtype of `(T2)->R2` if T2 is a subtype of T1
(contravariance) and R1 is a subtype of R2 (covariance). This ensures
that function subtyping preserves type safety:

<!--versetest
function_type1 := type{_(:any):int}
function_type2 := type{_(:int):any}
### Concrete function that matches function_type1
ConcreteFunc(Input:any):int = 42
### Function that takes function_type2 and uses it
UseFunction(F:function_type2, Value:int):void =
    Result:any = F(Value)  # Call with int, get any
TestSubtyping():void =
    UseFunction(ConcreteFunc, 5)
<#
-->
<!-- 90 -->
```verse
function_type1 := type{_(:any):int}
function_type2 := type{_(:int):any}

# function_type1 is a subtype of function_type2
# It accepts more general input (any vs int) - contravariance
# And returns more specific output (int vs any) - covariance

# Demonstrate: a function matching type1 can be used where type2 is expected
ConcreteFunc(Input:any):int = 42

UseFunction(F:function_type2, Value:int):void =
    Result:any = F(Value)

UseFunction(ConcreteFunc, 5)  # Works: function_type1 <: function_type2
```
<!-- #>-->

#### Type Aliases

Type aliases allow you to create alternative names for types, making
complex type signatures more readable and maintainable. They're
particularly valuable for function types, parametric types, and
frequently-used type combinations.

A type alias is created using simple assignment syntax at module scope:

<!--versetest
### At module scope
entity:=struct{}

### Simple type aliases
coordinate := tuple(float, float, float)
entity_map := [string]entity
player_id := int

### Function type aliases
update_handler := type{_(:float):void}
validator := int -> logic
transformer := type{_(:string):int}
<#
-->
<!-- 91 -->
```verse
# At module scope
entity:=struct{}

# Simple type aliases
coordinate := tuple(float, float, float)
entity_map := [string]entity
player_id := int

# Function type aliases
update_handler := type{_(:float):void}
validator := int -> logic
transformer := type{_(:string):int}
```
<!-- #> -->

Type aliases are compile-time only - they create no runtime overhead
and are purely for programmer convenience and code clarity.

**Type aliases are alternative names, not new types.** They do not
create distinct types like `newtype` in some languages. Values of the
alias and the original type are completely interchangeable:

<!--versetest
player_id := int
game_id := int
-->
<!-- 92 -->
```verse
# Assume
# player_id := int
# game_id := int

ProcessPlayer(ID:player_id):void = {}
ProcessGame(ID:game_id):void = {}

PID:player_id = 42
GID:game_id = 42

# These all work - aliases are just names
ProcessPlayer(PID)      # OK
ProcessPlayer(GID)      # OK - game_id is also int
ProcessPlayer(42)       # OK - int literal works too
ProcessGame(PID)        # OK - player_id is also int
```

Type aliases can have access specifiers that control their visibility across modules:

<!--versetest
### Public alias - accessible from other modules
PublicAlias<public> := int

### Internal alias - only accessible within defining module
InternalAlias<internal> := string

### Note: Protected/private are for classes and interfaces, not type aliases at module scope
<#
-->
<!-- 93 -->
```verse
# Public alias - accessible from other modules
PublicAlias<public> := int

# Internal alias - only accessible within defining module
InternalAlias<internal> := string

# Protected/private also work
ProtectedAlias<protected> := float  # only in classes and interfaces
```
<!-- #> -->

**Type aliases cannot be more public than the types they alias:**

<!--versetest
private_class := class{}

InternalToInternal<internal> := private_class
InternalAlias := private_class  # Defaults to internal

### Test that public alias to internal type produces error
assert_semantic_error(3593):
    M<public> := module:
        internal_class := class{}
        PublicToInternal<public> := internal_class
<#
-->
<!-- 94 -->
```verse
private_class := class{}      # No specifier = internal scope

# INVALID: Public alias to internal type
# PublicToPrivate<public> := private_class

# VALID: Same or less visibility
InternalToInternal<internal> := private_class
InternalAlias := private_class  # Defaults to internal
```
<!-- #> -->

##### Requirement

- **Type aliases can only be defined at module scope.** They cannot be
defined inside classes, functions, or any nested scope.
This restriction ensures type aliases have consistent visibility and
prevents scope-dependent type interpretations.

- Type aliases must be defined **before** they are used. Forward
references are not allowed.

- Type aliases are not first-class values and cannot be used as such.

#### Metatypes

Verse provides advanced type constructors that allow you to work with
types as values, enabling powerful patterns for runtime polymorphism
and generic instantiation. These metatypes—`subtype`,
`concrete_subtype`, and `castable_subtype`—bridge the gap between
compile-time type safety and runtime flexibility.

##### subtype

The `subtype(T)` type constructor represents runtime type values that
are subtypes of `T`. Unlike `concrete_subtype` and `castable_subtype`,
which are specialized for classes and interfaces, `subtype(T)` works
with **any type** in Verse, including primitives, enums, collections,
and function types.

<!--versetest
animal := class<computes> {}
dog := class<computes>(animal) {}

registry := class<computes><allocates>:
    var AnimalType:subtype(animal) = animal

    # Assign class types
    F0()<transacts>:void = set AnimalType = animal
    F1()<transacts>:void = set AnimalType = dog

    # Accept as parameter
    F3(ClassArg:subtype(animal))<transacts>:void = set AnimalType = ClassArg
<#
-->
<!-- 100 -->
```verse
animal := class {}
dog := class(animal) {}

# Example of using subtype as a field type
var AnimalType:subtype(animal)  # Can hold animal, dog, or any subtype of animal

# Assign class types
F0():void = set AnimalType = animal
F1():void = set AnimalType = dog  # dog is subtype of animal

# Accept as parameter
F3(ClassArg:subtype(animal)):void = set AnimalType = ClassArg
```
<!-- #>-->

The key capability of `subtype(T)` is holding type values at runtime
while maintaining type safety through the subtype relationship.

Unlike the other metatypes, `subtype(T)` accepts any type as its parameter:

<!--versetest
my_enum := enum { A, B, C }
my_class := class {}
my_interface := interface {}
-->
<!-- 101 -->
```verse
# Primitives
IntType:subtype(int) = int
LogicType:subtype(logic) = logic
FloatType:subtype(float) = float

# Enums
EnumType:subtype(my_enum) = my_enum

# Classes and interfaces
ClassType:subtype(my_class) = my_class
InterfaceType:subtype(my_interface) = my_interface

# Note: Collection types and function types in subtype() currently have issues:
# ArrayType:subtype([]int) = []int  # Error: cannot be defined
# OptionType:subtype(?string) = ?string  # Error: cannot be defined
# FuncType:subtype(type{_():void}) = type{_():void}  # Error: cannot be defined
```

This universality makes `subtype(T)` the most flexible of the metatypes, suitable for any scenario where you need to store or pass type values.

**Subtyping Relationship:**

The `subtype` constructor preserves the subtyping relationship:
`subtype(T) <: subtype(U)` if and only if `T <: U`. This means you can
assign a more specific subtype to a less specific one:

<!--versetest-->
<!-- 102 -->
```verse
super_class := class{}
sub_class := class(super_class) {}

# Covariance: sub_class <: super_class
SubtypeVar:subtype(sub_class) = sub_class
SupertypeVar:subtype(super_class) = SubtypeVar  # Valid

# Reverse fails - super_class is not <: sub_class
# SubtypeVar2:subtype(sub_class) = super_class
```

This also applies to interfaces:

<!--versetest-->
<!-- 103 -->
```verse
super_interface := interface{}
sub_interface := interface(super_interface) {}

class_impl := class(sub_interface) {}

# Covariance through interface hierarchy
SpecificType:subtype(sub_interface) = class_impl
GeneralType:subtype(super_interface) = SpecificType  # Valid
```

**Using with Interfaces:**

When working with interfaces, `subtype(T)` can hold any class that implements the interface:

<!--versetest-->
<!-- 104 -->
```verse
printable := interface:
    PrintIt():void

document := class(printable):
    PrintIt<override>():void = {}

# Can hold any type implementing printable
DocumentType:subtype(printable) = document
```

**Relationship to `type`:**

Both `subtype(T)` and `castable_subtype(T)` are subtypes of `type`, meaning they can be used where `type` is expected:

<!--versetest-->
<!-- 105 -->
```verse
c := class:
    f(C:subtype(c)):type = return(C)  # Valid: subtype(c) <: type

t := interface {}
g(x:subtype(t)):type = x  # Valid: subtype(t) <: type
```

**Restrictions:**

While `subtype(T)` is flexible, it has important restrictions:

1. **Cannot use as value:** `subtype(T)` is a type constructor, not a
   value. You cannot use `subtype(T)` itself as a value.
2. **Exactly one argument:** `subtype` requires exactly one type argument.
3. **Cannot use with attributes:** `subtype` cannot be used with
   classes that inherit from `attribute`.

##### concrete_subtype

The `concrete_subtype(t)` type constructor creates a type that
represents concrete (instantiable) subclasses of `t`. A concrete class
is one that can be instantiated directly—it has the `<concrete>`
specifier and provides default values for all fields:

<!--versetest-->
<!-- 110 -->
```verse
# Abstract base class
entity := class<abstract>:
    Name:string
    GetDescription():string

# Concrete implementations
player := class<concrete>(entity):
    Name<override>:string = "Player"
    GetDescription<override>():string = "A player character"

enemy := class<concrete>(entity):
    Name<override>:string = "Enemy"
    GetDescription<override>():string = "An enemy creature"

# Class that stores a type and can instantiate it
spawner := class:
    EntityType:concrete_subtype(entity)

    Spawn():entity =
        # Instantiate using the stored type
        EntityType{}

# Use it
# NewEntity := spawner{EntityType := player}.Spawn()
```

The key feature of `concrete_subtype` is that it ensures the stored type can be instantiated. Without this constraint, you couldn't safely call `EntityType{}` because abstract classes cannot be instantiated.

###### Requirements

A type can be used with `concrete_subtype` only if it is a class or
interface type. Additionally, the actual type value assigned must be a
concrete class—one marked with `<concrete>` and having all fields with
defaults:

<!--versetest-->
<!-- 111 -->
```verse
# Valid: concrete class with all defaults
config := class<concrete>:
    MaxPlayers:int = 8
    TimeLimit:float = 300.0

ConfigType:concrete_subtype(config) = config  # Valid

# Invalid: abstract class cannot be concrete_subtype
abstract_base := class<abstract>:
    Value:int

# This would be an error:
# BaseType:concrete_subtype(abstract_base) = abstract_base
```

When you have a `concrete_subtype`, you can instantiate it with the
empty archetype `{}`, but you cannot provide field initializers—the
concrete class must provide all necessary defaults:

<!--versetest-->
<!-- 112 -->
```verse
entity_base := class<abstract>:
    Health:int

warrior := class<concrete>(entity_base):
    Health<override>:int = 100

EntityType:concrete_subtype(entity_base) = warrior

# Valid: empty archetype uses defaults
# Instance := EntityType{}

# Invalid: cannot initialize fields through metatype
# Instance := EntityType{Health := 150}
```

##### castable_subtype

The `castable_subtype(t)` type constructor represents types that are
subtypes of `t` and marked with the `<castable>` specifier. This
constraint is required when you want to use types as first-class
values—storing them in variables, passing them as parameters, or
returning them from functions—and then use those type values to perform
casts. Note that the `<castable>` specifier is not required for basic
dynamic casting; all classes and interfaces support the `Type[value]`
cast syntax regardless of `<castable>`:

<!--versetest
entity:=class{}
vector3:=class{}
-->
<!-- 113 -->
```verse
# Castable base class
component := class<abstract><castable>:
    Owner:entity

# Castable subtypes
physics_component := class<castable>(component):
    Velocity:vector3

render_component := class<castable>(component):
    Material:string

# Function accepting castable subtype
ProcessComponent(CompType:castable_subtype(component), Comp:component):void =
    # Can use CompType to perform type-safe casts
    if (Specific := CompType[Comp]):
        # Comp is now known to be of type CompType
```

##### final_super and Type Queries

The `castable_subtype` works with the `<final_super>` specifier and
`GetCastableFinalSuperClass` function to enable sophisticated runtime
type queries. This combination provides a powerful mechanism for
component systems and polymorphic architectures.

The `<final_super>` specifier marks classes as stable anchor points in
inheritance hierarchies. These "final super classes" act as canonical
representatives for families of related types:

<!--versetest
entity:=class{}
vector3:=class{}
-->
<!-- 114 -->
```verse
component := class<castable>:
    Owner:entity

# Stable anchor for the physics component family
physics_component := class<final_super>(component):
    Velocity:vector3

# Specific implementations inherit from the anchor
rigid_body := class(physics_component):
    Mass:float

soft_body := class(physics_component):
    SpringConstant:float
```

By marking `physics_component` as `<final_super>`, you declare it as the canonical representative for all physics-related components. Even though `rigid_body` and `soft_body` are distinct types, they both belong to the "physics_component family" anchored at `physics_component`.

###### GetCastableFinalSuperClass

The `GetCastableFinalSuperClass` function queries the type hierarchy to find the `<final_super>` class between a base type and a derived type. Two variants exist:

<!--NoCompile-->
<!-- 115 -->
```verse
# Takes an instance
GetCastableFinalSuperClass(BaseType, instance)<decides>:castable_subtype(BaseType)

# Takes a type
GetCastableFinalSuperClassFromType(BaseType, Type)<decides>:castable_subtype(BaseType)
```

Both return a `castable_subtype` representing the least specific `<final_super>` class that:

1. Directly inherits from the specified base type
2. Is in the inheritance chain of the instance/type

The function fails if no appropriate `<final_super>` class exists.

Consider this hierarchy:


<!--versetest
vector3:=class{}
-->
<!-- 116 -->
```verse
component := class<castable>:
    ID:int

# Direct final_super subclass of component
physics_component := class<final_super>(component):
    Velocity:vector3

# Descendants of physics_component
rigid_body := class(physics_component):
    Mass:float

character_body := class(rigid_body):
    Health:int
```

Query results:


<!--versetest
entity:=class{}
vector3:=class{}
component:=class{}
character_body:=class(component){ID :int, Velocity :vector3, Mass :float, Health :int}
-->
<!-- 117 -->
```verse
# All instances in the physics_component family return physics_component
Body := character_body{ID:=1, Velocity:=vector3{}, Mass:=10.0, Health:=100}

if (Family := GetCastableFinalSuperClass[component, Body]):
    # Family = physics_component (the final_super anchor)
    # Even though Body is character_body, the family anchor is physics_component
```

The function "walks up" the inheritance chain from `character_body` → `rigid_body` → `physics_component` and stops at `physics_component` because:

1. It has `<final_super>`
2. It directly inherits from the queried base (`component`)

**When Queries Succeed and Fail?**

**Succeeds when:**

- A `<final_super>` class directly inherits from the base type
- The instance/type inherits from that `<final_super>` class

<!--versetest
base := class<castable>:
    Value:int=1
anchor := class<final_super>(base):
    Extra:string=""
derived := class(anchor){ More:string="" }

### Test that the calls succeed (do not fail)
TestQueries()<decides>:void =
    if:
        Result1 := GetCastableFinalSuperClass[base, derived{}]  # Returns anchor
        Result2 := GetCastableFinalSuperClass[base, anchor{}]   # Returns anchor
    then:
        void
<#
-->
<!-- 118 -->
```verse
base := class<castable>:
    Value:int

anchor := class<final_super>(base):
    Extra:string

derived := class(anchor):
    More:string

# Valid: anchor is final_super of base, derived inherits from anchor
GetCastableFinalSuperClass[base, derived{}]  # Returns anchor
GetCastableFinalSuperClass[base, anchor{}]   # Returns anchor
```
<!-- #>-->

**Fails when:**

- No `<final_super>` class exists between base and instance
- The queried type itself is the instance type (cannot query from same level)
- Instance is not a subtype of the base


###### Multiple Final Supers

You can have multiple `<final_super>` classes at different levels. The
function returns the one directly inheriting from the queried base:

<!--versetest
base := class<castable>:
    ID:int=1
first_anchor := class<final_super>(base):
    Category:string=""
second_anchor := class<final_super>(first_anchor):
    Subcategory:string=""
leaf := class(second_anchor){ Specific:string="" }

### Test that the calls succeed
TestQueries()<decides>:void =
    if:
        Result1 := GetCastableFinalSuperClass[base, leaf{}]  # Returns first_anchor
        Result2 := GetCastableFinalSuperClass[first_anchor, leaf{}]  # Returns second_anchor
    then:
        void
<#
-->
<!-- 120 -->
```verse
base := class<castable>:
    ID:int

first_anchor := class<final_super>(base):
    Category:string

second_anchor := class<final_super>(first_anchor):
    Subcategory:string

leaf := class(second_anchor):
    Specific:string

# Query from base returns first_anchor
GetCastableFinalSuperClass[base, leaf{}]  # Returns first_anchor

# Query from first_anchor returns second_anchor
GetCastableFinalSuperClass[first_anchor, leaf{}]  # Returns second_anchor
```
<!-- #>-->


This layered approach allows hierarchical categorization where
different levels represent different granularities of type families.

###### GetCastableFinalSuperClassFromType

The type-based variant works identically but takes a type instead of instance:

<!--versetest
component:=class<castable>{}
physics_component := class<final_super>(component){}
rigid_body := class(physics_component){}

### Test both functions work
TestBothVariants()<decides>:void =
    if:
        TypeFamily := GetCastableFinalSuperClassFromType[component, rigid_body]
        InstanceFamily := GetCastableFinalSuperClass[component, rigid_body{}]
    then:
        void
<#
-->
<!-- 123 -->
```verse
# Same behavior, different syntax
TypeFamily := GetCastableFinalSuperClassFromType[component, rigid_body]
InstanceFamily := GetCastableFinalSuperClass[component, rigid_body{}]

# Both return the same castable_subtype
```
<!-- #>-->

This is useful when working with type values directly rather than instances.

##### castable_concrete_subtype

The `castable_concrete_subtype(t)` type constructor combines the requirements of both `castable_subtype` and `concrete_subtype`, representing types that are:
- Subtypes of `t`
- Marked with `<castable>` (enabling runtime type queries)
- Marked with `<concrete>` (enabling instantiation)

This is useful when you need to ensure that type parameters are both castable and concrete:

<!--versetest
entity := class{}

component := class<abstract>:
    Owner:entity

physics_component := class<castable><concrete>(component):
    Velocity:float = 0.0

assert:
    # Must be both castable (for type queries) and concrete (for instantiation)
    CreateAndCast(CompType:castable_concrete_subtype(component)):component =
        # Can instantiate because it is concrete
        Instance := CompType{}
        # Can cast because it is castable
        if (Specific := CompType[Instance]):
            Specific
        else:
            Instance
-->
<!--NoCompile-->
<!-- 138 -->
```verse
entity := class{}

component := class<abstract>:
    Owner:entity

physics_component := class<castable><concrete>(component):
    Velocity:float = 0.0

# Function that requires both <castable> and <concrete>
CreateAndCast(CompType:castable_concrete_subtype(component)):component =
    # Can instantiate because CompType is <concrete>
    Instance := CompType{}
    # Can cast because CompType is <castable>
    if (Specific := CompType[Instance]):
        Specific
    else:
        Instance
```

##### classifiable_subset

Building on the concept of runtime type queries introduced by
`castable_subtype`, Verse provides `classifiable_subset`—a
sophisticated mechanism for maintaining sets of runtime types. Where
`castable_subtype` represents a single type value,
`classifiable_subset` represents a collection of types, tracking which
classes are present in a system and supporting queries based on type
hierarchies.

This feature is particularly valuable for component-based
architectures, where you need to track which component types an entity
possesses, query for specific capabilities, or filter operations based
on type compatibility. Rather than maintaining separate boolean flags
or type tags, `classifiable_subset` provides a type-safe,
hierarchy-aware registry of runtime types.

Three related types work together to provide both immutable and
mutable type sets:

**`classifiable_subset(t)`** represents an immutable set of runtime
types, where `t` must be a `<castable>` base type. Once created, the
set cannot be modified, making it suitable for configuration,
capability descriptions, or any scenario where the type set should
remain stable.

**`classifiable_subset_var(t)`** provides a mutable variant with
`Read()` and `Write()` operations, enabling dynamic type sets that
change during program execution. This is essential for runtime systems
where component types are added or removed as entities evolve.

**`classifiable_subset_key(t)`** represents keys used to identify
specific instances when adding them to a mutable set. These keys
enable removal of specific instances later, supporting lifecycle
management of registered types.

Unlike ordinary classes, `classifiable_subset` types cannot be
directly instantiated. You must use the constructor functions
`MakeClassifiableSubset()` and `MakeClassifiableSubsetVar()`:

<!--versetest
component:=class<castable>{}
physics_component := class<final_super>(component){}
rigid_body := class(physics_component){}
render_component := class<castable>(component){}
-->
<!-- 124 -->
```verse
# Immutable set, initially empty
EmptySet:classifiable_subset(component) = MakeClassifiableSubset()

# Immutable set with initial instances
InitialSet:classifiable_subset(component) =
    MakeClassifiableSubset(array{physics_component{}, render_component{}})

# Mutable set
DynamicSet:classifiable_subset_var(component) = MakeClassifiableSubsetVar()
```

The base type `t` must be `<castable>`, ensuring runtime type queries
are possible. This restriction is enforced at compile time:

<!--versetest
component:=class<computes><castable>{}
f()<reads>:void =
    ComponentSet:classifiable_subset(component) = MakeClassifiableSubset()

<#
-->
<!-- 125 -->
```verse
ComponentSet:classifiable_subset(component) = MakeClassifiableSubset()

# Invalid: non-castable types cannot be used
regular_class := class:
    Value:int

# This would be an error:
# BadSet:classifiable_subset(regular_class) = MakeClassifiableSubset()
```
<!-- #> -->

You cannot subclass these types or create instances through ordinary
construction syntax. This ensures that all sets use the proper
internal representation for efficient type queries.

###### Type Hierarchy Semantics

The crucial insight of `classifiable_subset` is that it tracks runtime
types, not individual instances. When you add an instance to the set,
the system records that instance's actual runtime type. More
importantly, type queries respect the inheritance hierarchy:


<!--versetest
entity:=class{}
vector3:=class{}
component := class<castable>{}
physics_component := class<castable>(component):
    Velocity:vector3=vector3{}

rigid_body_component := class<castable>(physics_component):
    Mass:float=0.0
-->
<!-- 126 -->
```verse
# Add a rigid body instance
Set:classifiable_subset(component) =
    MakeClassifiableSubset(array{rigid_body_component{}})

# Query results respect hierarchy
Set.Contains[component]             # true - rigid_body is a component
Set.Contains[physics_component]     # true - rigid_body is a physics_component
Set.Contains[rigid_body_component]  # true - directly present
```

This hierarchy awareness makes `classifiable_subset` fundamentally
different from a simple set of type tags. The `Contains` operation
asks "does this set contain any type that is-a T?" rather than "does
this set contain exactly T?".

When you add instances of different types, each distinct runtime type
is tracked separately:

<!--versetest
component := class<castable>{}
physics_component := class<castable>(component){}
rigid_body_component := class<castable>(physics_component){ }
render_component := class<castable>(component){}
audio_component := class<castable>(component){}
-->
<!-- 127 -->
```verse
# Add multiple different types
TheSet:classifiable_subset_var(component) = MakeClassifiableSubsetVar()
Key1 := TheSet.Add(physics_component{})
Key2 := TheSet.Add(render_component{})
Key3 := TheSet.Add(audio_component{})

TheSet.Contains[component]          # succeeds - all three are components
TheSet.Contains[physics_component]  # succeeds - physics_component present
TheSet.Contains[render_component]   # succeeds - render_component present
```

The set remembers each distinct type that was added. When you remove an instance by its key, that specific type is removed only if it was the last instance of that type:

<!--versetest
component := class<castable>{}
physics_component := class<castable>(component){}
rigid_body_component := class<castable>(physics_component){ }
-->
<!-- 128 -->
```verse
# Add multiple instances of same type
TheSet:classifiable_subset_var(component) = MakeClassifiableSubsetVar()
Key1 := TheSet.Add(physics_component{})
Key2 := TheSet.Add(physics_component{})

TheSet.Contains[physics_component]  # succeeds

TheSet.Remove[Key1]
TheSet.Contains[physics_component]  # still succeeds - Key2 remains

TheSet.Remove[Key2]
# TheSet.Contains[physics_component]  # fail - last instance removed
```

###### Core Operations

The `classifiable_subset` types provide several operations for
querying and manipulating type sets:

**Contains** checks whether any type in the set matches or is a
subtype of the queried type:

<!--versetest
component := class<castable>{}
physics_component := class<castable>(component){}
render_component := class<castable>(component){}
-->
<!-- 129 -->
```verse
TheSet:classifiable_subset(component) =
    MakeClassifiableSubset(array{physics_component{}})

if (TheSet.Contains[component]):
    # Physics component is present (and is a component)

if (TheSet.Contains[render_component]):
    # No render component present
```

**ContainsAll** verifies that all types in an array are present in the set:

<!--versetest
component := class<castable>{}
physics_component := class<castable>(component){}
render_component := class<castable>(component){}
-->
<!-- 130 -->
```verse
TheSet:classifiable_subset(component) =
    MakeClassifiableSubset(array{physics_component{}})

if (TheSet.ContainsAll[array{physics_component, render_component}]):
    # Both physics and render components are present
```

**ContainsAny** checks whether at least one type from an array is present:

<!--NoCompile-->
<!-- 131 -->
```verse
if (TheSet.ContainsAny[array{physics_component, audio_component}]):
    # Either physics or audio component (or both) is present
```

**Add** (mutable sets only) adds an instance and returns a key for later removal:


<!--versetest
component := class<castable>{ Name:string = "Component"}
physics_component := class<castable>(component){}
-->
<!-- 132 -->
```verse
TheSet:classifiable_subset_var(component) = MakeClassifiableSubsetVar()
Key := TheSet.Add(physics_component{})
# Can later remove using Key
```

**Remove** (mutable sets only) removes a previously added instance by its key:

<!--versetest
component := class<castable>{}
physics_component := class<castable>(component){}
-->
<!-- 133 -->
```verse
TheSet:classifiable_subset_var(component) = MakeClassifiableSubsetVar()

Key := TheSet.Add(physics_component{})

if (TheSet.Remove[Key]):
    # Successfully removed
else:
    # Key was not present (already removed or never added)
```

**FilterByType** creates a new set containing only types that are compatible (assignable to or from) the specified type:


<!--versetest
component := class<castable>{}
physics_component := class<castable>(component){}
render_component := class<castable>(component){}
audio_component := class<castable>(component){}

### Test FilterByType
TestFilterByType()<decides>:void =
    TheSet:classifiable_subset(component) = MakeClassifiableSubset(array{
        physics_component{}, render_component{}, audio_component{}})

    # Filter to physics-related types
    PhysicsSet := TheSet.FilterByType(physics_component)
    if:
        PhysicsSet.Contains[physics_component]  # true
        not PhysicsSet.Contains[render_component]   # false - unrelated sibling
        PhysicsSet.Contains[component]          # true - base type compatible
    then:
        void
<#
-->
<!-- 134 -->
```verse
TheSet:classifiable_subset(component) = MakeClassifiableSubset(array{
    physics_component{}, render_component{}, audio_component{}})

# Filter to physics-related types
PhysicsSet := TheSet.FilterByType(physics_component)
PhysicsSet.Contains[physics_component]  # true
PhysicsSet.Contains[render_component]   # false - unrelated sibling
PhysicsSet.Contains[component]          # true - base type compatible
```
<!-- #>-->

The filtering respects both upward and downward compatibility in the
type hierarchy, keeping types that could be assigned to or from the
filter type.

**Union** combines two sets using the `+` operator:

<!--versetest
component := class<castable>{}
physics_component := class<castable>(component){}
render_component := class<castable>(component){}
audio_component := class<castable>(component){}
entity := class{}
-->
<!-- 135 -->
```verse
Set1:classifiable_subset(component) =
    MakeClassifiableSubset(array{physics_component{}})
Set2:classifiable_subset(component) =
    MakeClassifiableSubset(array{render_component{}})

Combined := Set1 + Set2
Combined.Contains[physics_component]  # true
Combined.Contains[render_component]   # true
```

For mutable sets, the Read/Write operations enable copying and updating:

<!--versetest
component := class<castable>{}
physics_component := class<castable>(component){}
render_component := class<castable>(component){}
audio_component := class<castable>(component){}
-->
<!-- 136 -->
```verse
Set1:classifiable_subset_var(component) = MakeClassifiableSubsetVar()
Set1.Add(physics_component{})

Set2:classifiable_subset_var(component) = MakeClassifiableSubsetVar()
Set2.Write(Set1.Read())  # Copy Set1's contents to Set2
```

###### Design Considerations

Several important constraints govern `classifiable_subset` usage:

The base type must be `<castable>` to enable runtime type
queries. This requirement ensures that type checks can be performed
efficiently.

You cannot subclass `classifiable_subset` types or create instances
except through the designated constructor functions. This restriction
maintains internal invariants required for correct type tracking.

Keys from one set cannot be used with a different set—they are bound to
the specific set instance where the element was added.

The type parameter must be consistent across operations. You cannot
add a `physics_component` to a `classifiable_subset(render_component)`
even if both inherit from `component`:

<!--versetest
component := class<castable>{}
physics_component := class<castable>(component){}
render_component := class<castable>(component){}
audio_component := class<castable>(component){}
-->
<!-- 137 -->
```verse
render_set:classifiable_subset(render_component) = MakeClassifiableSubset()
physics_comp:physics_component = physics_component{}

# This would be a type error - physics_component is not a render_component
# render_set.Add(physics_comp)
```

Mutable sets require careful lifetime management. Keys become invalid
when their corresponding instances are removed, and attempting to
remove an already-removed key triggers a failure.

Performance characteristics matter for large type sets. While
`Contains` queries are efficient due to the internal representation,
operations like `FilterByType` may need to examine each type in the
set.

When designing systems with `classifiable_subset`, consider whether
immutable or mutable sets better fit your needs. Immutable sets
provide stronger guarantees and work well for configuration, while
mutable sets support dynamic systems where component types change
frequently.

The hierarchy-aware semantics mean that adding a derived type makes
queries for base types succeed. This is usually desirable but requires
awareness—if you only want exact type matches, `classifiable_subset`
may not be the right tool.

## Book of Verse Source Unit: 12_access.md

### Access Specifiers

Access specifiers control visibility and accessibility of code
elements. They provide a nuanced spectrum of access levels that
reflect the complex reality of modern software development,
particularly in the context of a persistent, global metaverse where
code from many authors must coexist safely.

Five primary visibility levels are defined that form a carefully
designed hierarchy, each serving specific architectural
needs. Understanding when and why to use each level is crucial for
creating well-structured, maintainable code.

| Specifier | Visibility | Usage |
|-----------|------------|-------|
| `<public>` | Universally accessible | Members intended for external use |
| `<internal>` | Only within the module (default) | Module-private implementation |
| `<private>` | Only in immediate enclosing scope | Local to class/struct |
| `<protected>` | Current class and subtypes | Inheritance hierarchies |
| `<scoped>` | Current scope and enclosing scopes | Special use cases |
| `<epic_internal>` | Scopes with the /Verse.org, /UnrealEngine.com, and /Fortnite.com domains | `<epic_internal>` is only usable by Epic-authored code |

#### Public

The `<public>` specifier represents the broadest level of access,
making an identifier universally accessible from any code that can
reference the containing module or type. When you mark something as
public, you are making a strong commitment about its availability and
stability:

<!--versetest
Test01 := module:
    PlayerManager<public> := module:
        MaxPlayers<public>:int = 100

        player<public> := class:
            Name<public>:string
            Level<public>:int = 1
<#
-->
<!-- 01 -->
```verse
PlayerManager<public> := module:
    MaxPlayers<public>:int = 100

    player<public> := class:
        Name<public>:string
        Level<public>:int = 1
```
<!-- #> -->

Public members form the contract between your code and the outside
world. In the metaverse context, public declarations are particularly
significant because they represent guarantees that extend potentially
forever—once published, removing or incompatibly changing a public
member breaks the promise you've made to other developers who depend
on your code.

The public specifier can be applied to modules, classes, interfaces,
structs, enums, methods, and data members. When applied to a type
definition itself, it makes the type available for use outside its
defining module. When applied to members within a type, it makes those
members accessible to any code that has access to an instance of that
type.

#### Protected

The `<protected>` specifier creates a middle ground between public and
private, allowing access within the defining class and any classes
that inherit from it. This level exists specifically to support
inheritance hierarchies while maintaining encapsulation:

<!--versetest
vector3:=class{}
MaxHealth:int=1
-->
```verse
game_entity := class:
    var Position<protected>:vector3 = vector3{x:=0.0, y:=0.0, z:=0.0}
    var Health<protected>:int = 100

    UpdatePosition<protected>(NewPos:vector3):void =
        set Position = NewPos
        OnPositionChanged()

    OnPositionChanged<protected>():void = {}  # Overridable by subclasses

player := class(game_entity):
    MoveToSpawn():void =
        UpdatePosition(GetSpawnLocation())  # Can access protected member
        set Health = MaxHealth              # Can modify protected variable
```

Protected access enables the template method pattern and other
inheritance-based designs while preventing external code from
accessing implementation details that should remain within the class
hierarchy. This is particularly valuable for game entities and other
hierarchical structures where parent classes need to share behavior
with children without exposing that behavior to the world.

#### Private

The `<private>` specifier provides the strictest access control,
limiting visibility to the immediately enclosing scope. Private
members are truly internal implementation details that can be changed
freely without affecting any external code:

<!--versetest
item:=struct{Weight:float=0.0}
inventory := class:
    var Items<private>:[]item = array{}
    var Capacity<private>:int = 20
    var CurrentWeight<private>:float = 0.0
    MaxWeight:float=20.0

    AddItem<public>(NewItem:item, At:int)<transacts><decides>:void =
        ValidateCapacity[NewItem]
        set Items[At] = NewItem
        set CurrentWeight = CurrentWeight + NewItem.Weight

    ValidateCapacity<private>(NewItem:item)<reads><decides>:void =
        Items.Length < Capacity
        CurrentWeight + NewItem.Weight <= MaxWeight
<#
-->
<!-- 03 -->
```verse
inventory := class:
    var Items<private>:[]item = array{}
    var Capacity<private>:int = 20
    var CurrentWeight<private>:float = 0.0
    MaxWeight:float=20.0

    AddItem<public>(NewItem:item, At:int)<transacts><decides>:void =
        ValidateCapacity[NewItem]
        set Items[At] = NewItem
        set CurrentWeight = CurrentWeight + NewItem.Weight

    ValidateCapacity<private>(NewItem:item)<reads><decides>:void =
        Items.Length < Capacity
        CurrentWeight + NewItem.Weight <= MaxWeight
```
<!-- #> -->

Private members are the building blocks of encapsulation. They allow
you to maintain invariants, hide complexity, and create clean
abstractions. Changes to private members never break external code,
giving you the freedom to refactor and optimize implementation details
as needed.

#### Internal

The `<internal>` specifier, which is the default access level when no
specifier is provided, makes members accessible within the defining
module but not outside it. This creates a natural boundary for
collaborative code that needs to share implementation details without
exposing them publicly:

<!--versetest
game_entity:=class{}
collision_info:=class{}
ApplyGravity(:game_entity,:float):void={}
CheckCollisions(:game_entity):void={}

Physics := module:
    gravity_constant:float = 9.81

    collision_detector := class<abstract>:
        DetectCollision<internal>(A:game_entity, B:game_entity):?collision_info

    physics_world := class:
        var Entities<internal>:[]game_entity = array{}

        SimulateStep<internal>(DeltaTime:float):void =
            for (Entity : Entities):
                ApplyGravity(Entity, DeltaTime)
                CheckCollisions(Entity)
<#
-->
<!-- 04 -->
```verse
Physics := module:
    # Internal types and constants
    gravity_constant:float = 9.81

    collision_detector := class<abstract>:
        DetectCollision<internal>(A:game_entity, B:game_entity):?collision_info

    physics_world := class:
        var Entities<internal>:[]game_entity = array{}

        SimulateStep<internal>(DeltaTime:float):void =
            for (Entity : Entities):
                ApplyGravity(Entity, DeltaTime)
                CheckCollisions(Entity)
```
<!-- #> -->

Internal access is ideal for module-wide utilities, shared
implementation details, and helper functions that multiple classes
within a module need but shouldn't be exposed to external code. It
provides a clean separation between the module's public interface and
its implementation machinery.

#### Scoped

The `<scoped>` specifier creates custom access boundaries between
modules or code locations. Unlike the fixed visibility levels of
`public`, `internal`, and `private`, `scoped` access allows you to
explicitly grant access to particular modules while excluding all
others—creating a kind of "friend" relationship between program
entities.

##### Scoped Definitions

A scoped access level is created using the `scoped{...}` expression,
which takes one or more module references:

<!--NoCompile-->
```verse
Collaboration := module:
    # Create a scope that includes both ModuleA and ModuleB
    Shared<public> := scoped{ModuleA, ModuleB}

    # This class is only accessible within ModuleA and ModuleB
    SharedResource<Shared> := class:
        Data<public>:int = 42
```

The scoped definition creates an access level that can then be used as
a specifier on classes, functions, variables, and other
definitions. Code within any of the listed entities can access the
scoped member, while code outside those modules cannot—even if it can
see the containing scope.

##### Cross-Module Collaboration

The most powerful use of scoped access is enabling controlled
collaboration between modules. A definition can be created in one
module but scoped to another, making it accessible where it is needed
while keeping it hidden elsewhere:

<!--versetest
bounding_box:=class{}
Graphics := module:
    CollidableShape<scoped{Physics}> := interface:
        GetBounds():bounding_box

Physics := module:
    using{Graphics}

    sphere_collider := class<abstract>(CollidableShape):
        GetBounds<override>():bounding_box
<#
-->
<!-- 06 -->
```verse
Graphics := module:
    # Define an interface scoped to the physics module
    CollidableShape<scoped{Physics}> := interface:
        GetBounds():bounding_box

Physics := module:
    using{Graphics}

    # Physics can implement the interface even though it is defined in graphics
    sphere_collider := class<abstract>(CollidableShape):
        GetBounds<override>():bounding_box
```
<!-- #> -->

This pattern allows graphics to define contracts that physics
implements without exposing those implementation details publicly. The
interface exists at the boundary between the two modules but is not
part of either module's public API.

You can scope a definition to multiple modules, creating a shared
private space for collaboration:

<!--versetest
Gameplay := module:
    SharedGameplayScope := scoped{Inventory, Crafting}

    Item<SharedGameplayScope> := class:
        ID<public>:int
        Properties<public>:[string]string

    CreateItem<SharedGameplayScope>(TheID:int):Item = Item{ID:=TheID, Properties:=map{}}

Inventory := module:
    using{Gameplay}

    AddToInventory(ItemID:int):void =
        NewItem := CreateItem(ItemID)

Crafting := module:
    using{Gameplay}

    CraftItem(Recipe:[]int)<decides>:Item =
        CreateItem(Recipe[0])
<#
-->
<!-- 07 -->
```verse
Gameplay := module:
    # This scope includes both the inventory and crafting modules
    SharedGameplayScope := scoped{Inventory, Crafting}

    # Items can be accessed by both inventory and crafting
    Item<SharedGameplayScope> := class:
        ID<public>:int
        Properties<public>:[string]string

    # Factory function available to both systems
    CreateItem<SharedGameplayScope>(TheID:int):Item = Item{ID:=TheID, Properties:=map{}}

Inventory := module:
    using{Gameplay}

    AddToInventory(ItemID:int):void =
        NewItem := CreateItem(ItemID)  # Can access scoped function
        # Implementation...

Crafting := module:
    using{Gameplay}

    CraftItem(Recipe:[]int)<decides>:Item =
        # Can create items and access their properties
        CreateItem(Recipe[0])
```
<!-- #> -->

##### Scoped Read or Write Access

Like other access specifiers, scoped can be applied separately to read
and write operations on variables:

<!--  BUG?  Or at least unhelpful error message

a:=class<computes>{}
F()<computes>:a= a{}
b := class{ G:a = F() }

Gives:
  Line 8: Verse compiler error V3582: Divergent calls (calls that might not complete) cannot be used to define data-members.
-->


<!--versetest
ModuleA:=module{}
ModuleB:=module{}
game_state:=class{}

SharedScope := scoped{ModuleA, ModuleB}

state_manager := class:
    var<SharedScope> GameState<public>:game_state = game_state{}

    var<SharedScope> SyncCounter<SharedScope>:int = 0
<#
-->
<!-- 08 -->
```verse
SharedScope := scoped{ModuleA, ModuleB}

state_manager := class:
    # Public read access, but only ModuleA and ModuleB can write
    var<SharedScope> GameState<public>:game_state = game_state{}

    # Only ModuleA and ModuleB can read or write this internal state
    var<SharedScope> SyncCounter<SharedScope>:int = 0
```
<!-- #> -->

This pattern is particularly useful for shared state that multiple
modules need to coordinate on without exposing write access publicly.

##### Visibility and Access Paths

An important subtlety of scoped access is that it grants access to a
specific member, but does not make intermediate types or modules
visible. To access a scoped member, you must be able to see the entire
path to it:

<!--NoCompile-->
```verse
Outer := module:
    # Internal to outer
    Inner := module:
        # Scoped to TargetModule
        SharedClass<scoped{TargetModule}> := class:
            Value:int = 42

TargetModule := module:
    using{Outer}

    # ERROR: Can't see Outer.Inner because Inner is internal to Outer
    # even though SharedClass is scoped to us
    UseShared():void = Outer.Inner.SharedClass{}
```

For scoped access to work, either the containing scope must be
accessible (public or also scoped appropriately), or the scoped member
must be accessed through a public interface that exposes it.

A definition can only have one scoped access level—you cannot apply
multiple scoped specifiers:

<!-- NoCompile-->
```verse
# ERROR: Cannot have multiple access level specifiers
InvalidScope<scoped{ModuleA}><scoped{ModuleB}> := class{}
```

##### Scoped Access and Inheritance

When a class member has scoped access, overriding members in
subclasses can maintain or narrow that access, following normal
inheritance rules:

<!--versetest
ModuleA:=module{}
ModuleB:=module{}
SharedScope := scoped{ModuleA, ModuleB}

base := class:
    ComputeValue<SharedScope>():int = 42

derived := class(base):
    ComputeValue<override>():int = 100
<#
-->
<!-- 11 -->
```verse
SharedScope := scoped{ModuleA, ModuleB}

base := class:
    # Accessible only in ModuleA and ModuleB
    ComputeValue<SharedScope>():int = 42

derived := class(base):
    # Can override with same or more restrictive access
    ComputeValue<override>():int = 100  # Now internal to this module
```
<!-- #> -->

##### Using Scoped for API Boundaries

Scoped access excels at creating controlled API boundaries where
certain functionality should be shared between specific modules but
not exposed as part of the public interface:

<!--NoCompile-->
```verse
Networking := module:
    # Public scope for modules that need network access
    NetworkScope<public> := scoped{PlayerSystem, Matchmaking, Telemetry}

    # Core networking available to specific systems
    SendPacket<NetworkScope>(Data:[]uint8):void =
        # Implementation...

    # Internal statistics
    var<NetworkScope> BytesSent<NetworkScope>:int = 0
```

This creates an explicit architectural boundary—only the modules
listed in the scope can access the networking primitives, while other
code must use higher-level public APIs.

##### Design Considerations

Scoped access represents an architectural commitment between
modules. When using it effectively:

- Use scoped for legitimate cross-module collaboration that does not
  belong in the public API
- Keep scope definitions at the module level where they can be documented and maintained
- Prefer scoping to explicit modules rather than deeply nested scopes
- Consider whether protected or internal access might be simpler for your use case
- Document why particular modules are included in a scope

The scoped specifier fills a unique niche between internal and public
access, enabling sophisticated module architectures where multiple
components need to collaborate intimately without exposing those
implementation details to the wider codebase.

#### Separating Read and Write Access

An innovative feature is the ability to apply different access
specifiers to reading and writing operations on the same
variable. This fine-grained control allows you to create variables
that are widely readable but narrowly writable, implementing common
patterns like read-only properties elegantly:

<!--versetest
game_state := class:
    var<protected> Score<public>:int = 0

    var<private> PlayerCount<public>:int = 0

    var<private> SessionID<internal>:string
<#
-->
<!-- 13 -->
```verse
game_state := class:
    # Public read, protected write
    var<protected> Score<public>:int = 0

    # Public read, private write
    var<private> PlayerCount<public>:int = 0

    # Internal read, private write
    var<private> SessionID<internal>:string
```
<!-- #> -->

This dual-specifier system solves a common problem in object-oriented
programming where you want to expose state for reading without
allowing external modification. Rather than requiring getter methods
or property syntax, Verse makes this pattern a first-class language
feature.

The syntax places the write-access specifier on the `var` keyword and
the read-access specifier on the identifier itself. This visual
separation makes the access levels immediately clear when reading
code. The write specifier must be at least as restrictive as the read
specifier — you cannot write to a variable that is privately readable
but publicly writable, as this would violate basic encapsulation
principles.

#### Best Practices

Understanding when to use each access level requires thinking about
your code's architecture and evolution. The principle of least
privilege suggests starting with the most restrictive access that
works and only broadening it when necessary.

For public  APIs, every public  member is a commitment.  Before making
something public, consider  whether it truly needs to be  part of your
module's contract or if it is  an implementation detail that happens to
be  needed elsewhere  temporarily.  Public members  should be  stable,
well-documented, and designed for longevity.

Protected access should be used thoughtfully in inheritance
hierarchies. Not everything in a base class needs to be protected—only
those members that form the inheritance contract between parent and
child classes. Overuse of protected access can create tight coupling
between classes in a hierarchy.

Private access is your default for implementation details. Most helper
functions, intermediate calculations, and state management should be
private. This gives you maximum flexibility to refactor and optimize
without breaking dependent code.

The dual-specifier pattern for variables is particularly powerful for
maintaining invariants. By making variables publicly readable but
privately or protectively writable, you can expose state for
observation while maintaining complete control over modifications:

<!--versetest
resource_manager := class:
    var<private> TotalResources<public>:int = 1000
    var<private> AllocatedResources<public>:int = 0
    var<private> AvailableResources<public>:int = 1000

    AllocateResources<public>(Amount:int)<decides><transacts>:void =
        Amount <= AvailableResources
        set AllocatedResources = AllocatedResources + Amount
        set AvailableResources = AvailableResources - Amount
<#
-->
<!-- 14 -->
```verse
resource_manager := class:
    var<private> TotalResources<public>:int = 1000
    var<private> AllocatedResources<public>:int = 0
    var<private> AvailableResources<public>:int = 1000

    AllocateResources<public>(Amount:int)<decides><transacts>:void =
        Amount <= AvailableResources
        set AllocatedResources = AllocatedResources + Amount
        set AvailableResources = AvailableResources - Amount
```
<!-- #> -->

#### Annotations and Metadata

Verse provides an annotation system for attaching metadata to
definitions using the `@` prefix syntax. Annotations provide compiler
directives and metadata that affect how code is treated during
compilation and evolution.

##### Built-in Annotations

###### @deprecated

!!! warning "Internal Feature"
    @deprecated attribute is currently an internal feature and cannot be used by end-users.

The `@deprecated` annotation marks definitions that should no longer
be used. When code references a deprecated definition, the compiler
produces a warning, alerting developers to update their code:

<!--versetest
@deprecated
OldFunction():void =
    Print("This function is deprecated")

@deprecated
legacy_player := class:
    Name:string

UseDeprecated():void =
    OldFunction()
<#
-->
<!-- 15 -->
```verse
# Mark a definition as deprecated
@deprecated
OldFunction():void =
    Print("This function is deprecated")

# Mark a class as deprecated
@deprecated
legacy_player := class:
    Name:string

# Attempting to use deprecated code produces a warning
UseDeprecated():void =
    OldFunction()  # Warning: OldFunction is deprecated
```
<!-- #> -->

Deprecated definitions can use other deprecated definitions without
warnings, but non-deprecated code cannot use deprecated definitions
without triggering warnings. This allows gradual migration of
deprecated APIs:

<!--versetest
@deprecated
OldAPI():int = 42
@deprecated
MigrateOldAPI():int = OldAPI()


<#
-->
<!-- 16 -->
```verse
@deprecated
OldAPI():int = 42

# Valid: deprecated can call deprecated
@deprecated
MigrateOldAPI():int = OldAPI()

# Warning: non-deprecated calling deprecated
# NewCode():int = OldAPI()
```
<!-- #> -->

The `@deprecated` annotation can be applied to:
- Functions and methods
- Classes, interfaces, structs, and enums
- Individual enum values
- Data members
- Modules

###### @experimental

!!! warning "Internal Feature"
    @experimental attribute is currently an internal feature and cannot be used by end-users.

The `@experimental` annotation marks features that are not yet stable
and may change or be removed in future versions. Experimental features
can only be used when the `AllowExperimental` package flag is enabled:

<!--NoCompile-->
```verse
# Mark a feature as experimental
@experimental
experimental_class := class:
    NewFeature:int

# Using experimental features requires AllowExperimental flag
# Without flag: error
# With AllowExperimental:=true: allowed
UseExperimental(Obj:experimental_class):void =
    Print("Using experimental feature")
```

Experimental definitions behave similarly to deprecated
ones—experimental definitions can freely use other experimental
definitions, but stable code cannot use experimental definitions
unless the `AllowExperimental` flag is set.

The `@experimental` annotation cannot be applied to:
- Local variables
- Override methods (base method's experimental status is inherited)

###### @available

The `@available` annotation controls when a definition becomes
available based on version numbers. This enables gradual API rollout
and version-specific functionality:

<!--versetest
using { /Verse.org/Native }
@available{MinUploadedAtFNVersion := 3000}
NewFeature():void =
    Print("New feature")
@available{MinUploadedAtFNVersion := 2900}
OldImplementation():int = 42

@available{MinUploadedAtFNVersion := 3000}
NewImplementation():int = 100

<#
-->
<!-- 18 -->
```verse
using { /Verse.org/Native }  # Required for @available

# Available only in version 3000 and later
@available{MinUploadedAtFNVersion := 3000}
NewFeature():void =
    Print("New feature")

# Multiple definitions can coexist for different versions
@available{MinUploadedAtFNVersion := 2900}
OldImplementation():int = 42

@available{MinUploadedAtFNVersion := 3000}
NewImplementation():int = 100
```
<!-- #> -->

The `@available` annotation can be applied to the same kinds of
definitions as `@deprecated`.

##### Custom Attributes

!!! warning "Internal Feature"
    Custom attributes are currently an internal feature and cannot be created by end-users.

You can create custom attributes by inheriting from the special
`attribute` class. Custom attributes allow you to attach
domain-specific metadata to your code:

<!--versetest
@attribscope_class
gameplay_element := class<computes>(attribute):
    Category:string
    Priority:int
@gameplay_element{Category := "Combat", Priority := 1}
weapon_system := class:
    Damage:int
<#
-->
<!-- 19 -->
```verse
# Define a custom attribute
@attribscope_class
gameplay_element := class<computes>(attribute):
    Category:string
    Priority:int

# Use the custom attribute
@gameplay_element{Category := "Combat", Priority := 1}
weapon_system := class:
    Damage:int
```
<!-- #> -->

###### Attribute Scopes

When defining custom attributes, you must specify where they can be
applied using scope annotations:

- **@attribscope_class** - Can be applied to regular classes
- **@attribscope_attribclass** - Can be applied to attribute classes (classes that inherit from `attribute`)
- **@attribscope_enum** - Can be applied to enums
- **@attribscope_interface** - Can be applied to interfaces
- **@attribscope_function** - Can be applied to functions and methods
- **@attribscope_data** - Can be applied to data members

Example of scoped custom attributes:

<!--versetest
@attribscope_function
performance_critical := class<computes>(attribute):
    MaxExecutionTimeMs:int

@attribscope_data
serializable_field := class<computes>(attribute):
    SerializationKey:string

entity := class<abstract>:
    @serializable_field{SerializationKey := "entity_id"}
    ID:int

    @performance_critical{MaxExecutionTimeMs := 16}
    Update():void

<#
-->
<!-- 20 -->
```verse
# Attribute that can only be applied to functions
@attribscope_function
performance_critical := class(attribute):
    MaxExecutionTimeMs:int

# Attribute that can only be applied to data members
@attribscope_data
serializable_field := class(attribute):
    SerializationKey:string

# Use them appropriately
entity := class<abstract>:
    @serializable_field{SerializationKey := "entity_id"}
    ID:int

    @performance_critical{MaxExecutionTimeMs := 16}
    Update():void
```
<!-- #> -->

Attempting to use an attribute in the wrong location produces a
compiler error. For example, a function-scoped attribute cannot be
applied to a class.

**Reading attributes:** Custom attributes are currently metadata for
external tooling — the compiler, LSP, and the Unreal Editor can read
and act on them, but there is no Verse API to query attributes at
runtime. Attributes are used to apply rules, constraints, or extra
data that tools outside the language consume, such as serialization
hints, editor annotations, or performance directives.

##### Getter and Setter Accessors

!!! warning "Internal Feature"
    Getter and setter accessors are currently an internal feature and cannot be used by end-users.

While not strictly annotations, the `<getter(...)>` and
`<setter(...)>` specifiers provide a related form of metadata for
controlling field access. These can be applied to both class and
interface fields to define custom access logic:

<!--versetest
entity := class:
    var Health<getter(GetHealth)><setter(SetHealth)>:int = external{}

    var InternalHealth:int = 100

    GetHealth(:accessor):int = InternalHealth

    SetHealth(:accessor, NewValue:int):void =
        if (NewValue >= 0, NewValue <= 100):
            set InternalHealth = NewValue

<#
-->
<!-- 21 -->
```verse
entity := class:
    # External field with custom accessors
    var Health<getter(GetHealth)><setter(SetHealth)>:int = external{}

    var InternalHealth:int = 100

    GetHealth(:accessor):int = InternalHealth

    SetHealth(:accessor, NewValue:int):void =
        if (NewValue >= 0, NewValue <= 100):
            set InternalHealth = NewValue
```
<!-- #> -->

Constraints on accessors:

- Must include both `<getter(...)>` and `<setter(...)>` - cannot have only one
- The field must have `= external{}` or no default value (with archetype initialization required)
- Fields with accessors cannot be overridden in subclasses
- The field must be mutable (marked with `var`)
- Not all types are supported for accessor fields
- Accessor fields are currently only allowed in epic_internal scopes

For more details on accessor patterns, see [Fields with Accessors](13_BOOK_OF_VERSE_II_PROGRAM_STRUCTURE_AND_TYPES.md#book-of-verse-source-unit-10classesinterfacesmd).

##### Localization

The `<localizes>` specifier marks definitions as localizable messages
for internationalization. Localized messages use the `message` type
and can be extracted for translation into different languages:

<!--versetest
WelcomeMessage<localizes> : message = "Welcome to the game!"

ShowWelcome():void =
    Print(Localize(WelcomeMessage))
<#
-->
<!-- 22 -->
```verse
# Simple localized message
WelcomeMessage<localizes> : message = "Welcome to the game!"

# Call Localize to get the string
ShowWelcome():void =
    Print(Localize(WelcomeMessage))
```
<!-- #> -->

###### Message Parameters

Localized messages can accept parameters for dynamic content interpolation:

<!--versetest
GreetPlayer<localizes>(PlayerName:string) : message = "Hello, {PlayerName}!"

ShowGreeting(Name:string):void =
    Print(Localize(GreetPlayer(Name)))
<#
-->
<!-- 23 -->
```verse
# Message with parameter interpolation
GreetPlayer<localizes>(PlayerName:string) : message = "Hello, {PlayerName}!"

# Use with arguments
ShowGreeting(Name:string):void =
    Print(Localize(GreetPlayer(Name)))
    # Outputs: "Hello, Aldric!" (if Name = "Aldric")
```
<!-- #> -->

**Supported parameter types:**
- `string` - Text values
- `int` - Integer values (formatted with comma separators)
- `float` - Floating-point values

**Parameter interpolation syntax:**
- Use `{ParameterName}` to insert parameter values
- Parameters can be used multiple times or not at all
- Only parameter names and Unicode code points allowed in braces

<!--versetest
### Multiple parameters, some repeated
ScoreMessage<localizes>(Player:string, Score:int) : message =
    "Congratulations {Player}! Your score is {Score}. Great job, {Player}!"

### Outputs: "Congratulations Alice! Your score is 1,500. Great job, Alice!"

### Not all parameters required in message text
OptionalParam<localizes>(Name:string, Score:int) : message =
    "Thanks for playing!"  # Score parameter ignored
<#
-->
<!-- 24 -->
```verse
# Multiple parameters, some repeated
ScoreMessage<localizes>(Player:string, Score:int) : message =
    "Congratulations {Player}! Your score is {Score}. Great job, {Player}!"

# Outputs: "Congratulations Alice! Your score is 1,500. Great job, Alice!"

# Not all parameters required in message text
OptionalParam<localizes>(Name:string, Score:int) : message =
    "Thanks for playing!"  # Score parameter ignored
```
<!-- #> -->

###### Integer Formatting

Integer parameters are automatically formatted with comma separators for readability:

<!--versetest
HighScore<localizes>(Points:int) : message = "New record: {Points} points!"

<#
-->
<!-- 25 -->
```verse
HighScore<localizes>(Points:int) : message = "New record: {Points} points!"

# Localize(HighScore(190091)) produces: "New record: 190,091 points!"
```
<!-- #> -->

###### Named and Default Parameters

Localized messages support named parameters and default values:

<!--versetest
ConfigMessage<localizes>(?MaxPlayers:int = 8, ?TimeLimit:int = 300):message =
    "Game settings: {MaxPlayers} players, {TimeLimit} seconds"

assert:
    Localize(ConfigMessage())                           # Uses defaults
    Localize(ConfigMessage(?MaxPlayers := 16))          # Override one
    Localize(ConfigMessage(?TimeLimit := 600, ?MaxPlayers := 32))  # Override both
<#
-->
<!-- 26 -->
```verse
ConfigMessage<localizes>(?MaxPlayers:int = 8, ?TimeLimit:int = 300):message =
    "Game settings: {MaxPlayers} players, {TimeLimit} seconds"

# Can be called with any combination
Localize(ConfigMessage())                           # Uses defaults
Localize(ConfigMessage(?MaxPlayers := 16))          # Override one
Localize(ConfigMessage(?TimeLimit := 600, ?MaxPlayers := 32))  # Override both
```
<!-- #> --> 

###### Tuple Parameters

Messages can accept tuple parameters, which are destructured in the parameter list:

<!--versetest
LocationMessage<localizes>(Player:string, (X:int, Y:int)) : message =
    "{Player} is at position ({X}, {Y})"

### Test the call
TestTupleParam():void =
    Localize(LocationMessage("Hero", (10, 20)))
<#
-->
<!-- 27 -->
```verse
LocationMessage<localizes>(Player:string, (X:int, Y:int)) : message =
    "{Player} is at position ({X}, {Y})"

# Call with tuple
Localize(LocationMessage("Hero", (10, 20)))
# Outputs: "Hero is at position (10, 20)"
```
<!-- #>-->

###### String Escaping and Unicode

**Unicode code points:**

<!--versetest
UnicodeMessage<localizes> : message = "The letter is {0u004d}"
<#
-->
<!-- 28 -->
```verse
UnicodeMessage<localizes> : message = "The letter is {0u004d}"
# Outputs: "The letter is M"
```
<!-- #> -->

**Escaped braces** (to show literal braces):

<!--versetest
EscapedMessage<localizes>(Name:string) : message =
    "Use \{Name\} to insert {Name}"
<#
-->
<!-- 29 -->
```verse
EscapedMessage<localizes>(Name:string) : message =
    "Use \{Name\} to insert {Name}"
# Localize(EscapedMessage("value")) produces: "Use {Name} to insert value"
```
<!-- #> -->

**Special characters:**

<!--versetest
SpecialChars<localizes> : message =
    "Supports: \\r\\n\\t\\\"\\'\\#\\<\\>\\&\\~"
<#
-->
<!-- 30 -->
```verse
SpecialChars<localizes> : message =
    "Supports: \\r\\n\\t\\\"\\'\\#\\<\\>\\&\\~"
```
<!-- #> -->

**Whitespace and comments** are allowed in interpolation:

<!--versetest
SpacedParam<localizes>(Name:string) : message = "Hello { Name }"
CommentedParam<localizes>(Name:string) : message = "Hello {Name}"
<#
-->
<!-- 31 -->
```verse
SpacedParam<localizes>(Name:string) : message = "Hello { Name }"
CommentedParam<localizes>(Name:string) : message = "Hello {<# comment #>Name}"
```
<!-- #> -->

###### Scope Requirements

Localized messages **must be defined at module or snippet scope**. They cannot be defined inside functions:

<!--NoCompile-->
```verse
# Valid: module scope
MyModule := module:
    ModuleMessage<localizes> : message = "Valid"

# Valid: snippet scope
TopLevelMessage<localizes> : message = "Valid"

BadFunction():void =
    LocalMessage<localizes> : message = "Invalid"  # ERROR
```

###### Inheritance and Override

Localized messages can be overridden in class hierarchies:

<!--versetest
base_ui := class:
    Title<localizes>:message = "Base Title"
    Description<localizes>:message = "Base description"

derived_ui := class(base_ui):
    Title<localizes><override>:message = "Derived Title"
<#
-->
<!-- 33 -->
```verse
base_ui := class:
    Title<localizes>:message = "Base Title"
    Description<localizes>:message = "Base description"

derived_ui := class(base_ui):
    # Override the title message
    Title<localizes><override>:message = "Derived Title"
    # Inherits Description from base
```
<!-- #> -->

Localized messages can also be abstract:

<!--versetest
quest_base := class<abstract>:
    TaskDescription<localizes><public> : message
    CompletionMessage<localizes><protected> : message = "Quest complete!"

fetch_quest := class<final>(quest_base):
    TaskDescription<localizes><override> : message = "Collect 10 items"
<#
-->
<!-- 34 -->
```verse
quest_base := class<abstract>:
    # Abstract message - must be implemented by subclasses
    TaskDescription<localizes><public> : message
    # Concrete message with default
    CompletionMessage<localizes><protected> : message = "Quest complete!"

fetch_quest := class<final>(quest_base):
    TaskDescription<localizes><override> : message = "Collect 10 items"
```
<!-- #> -->

###### Restrictions and Errors

**Must use explicit type annotation:**

The type annotation `: message` is required. Implicit typing is not supported:

<!--versetest

GoodMessage<localizes> : message = "Text"
<#
-->
<!-- 35 -->
```verse
# ERROR: Missing type annotation
# BadMessage<localizes> := "Text"  # ERROR 3639

# Valid: Explicit type
GoodMessage<localizes> : message = "Text"
```
<!-- #> -->

**RHS must be string literal:**

<!--versetest

ValidMessage<localizes> : message = "AB"
<#
-->
<!-- 36 -->
```verse
# ERROR: Expression not allowed
# InvalidMessage<localizes> : message = "A" + "B"  # ERROR 3638

# Valid: Literal only
ValidMessage<localizes> : message = "AB"
```
<!-- #> -->

**Restricted parameter types:**

Not all types are supported as parameters:

<!--versetest

my_class := class{Value:int}
<#
-->
<!-- 37 -->
```verse
# ERROR: Optional types not supported
# OptionalMsg<localizes>(Player:?string) : message = "{Player}"  # ERROR 3509

# ERROR: Custom classes not supported
my_class := class{Value:int}
# ClassMsg<localizes>(Obj:my_class) : message = "{Object}"  # ERROR 3509
```
<!-- #> -->

**Interpolation syntax restrictions:**

Only parameter names and Unicode code points are allowed inside `{}`:

<!--versetest

ParamMessage<localizes>(Name:string) : message = "{Name}"
<#
-->
<!-- 38 -->
```verse
# ERROR: Expressions not allowed
# ExprMessage<localizes>(Name:string) : message = "{"Hello"}"  # ERROR 3652

# Valid: Parameter names only
ParamMessage<localizes>(Name:string) : message = "{Name}"
```
<!-- #> -->

**Non-parameter identifiers are escaped:**

If you reference an identifier that is not a parameter, it gets escaped in the output:

<!--versetest
GlobalName:string = "World"

RefMessage<localizes>(Greeting:string) : message =
    "{Greeting} to {GlobalName}"

<#
-->
<!-- 39 -->
```verse
GlobalName:string = "World"

RefMessage<localizes>(Greeting:string) : message =
    "{Greeting} to {GlobalName}"

# Localize(RefMessage("Hello")) produces: "Hello to \{GlobalName\}"
# Note: GlobalName is escaped because it is not a parameter
```
<!-- #> -->

###### Access Specifiers

Localized messages support standard access specifiers:

<!--versetest
MyModule := module:
    PublicMessage<localizes><public> : message = "Public message"
    InternalMessage<localizes> : message = "Internal message"

    some_class := class:
        PrivateMessage<localizes><private> : message = "Private message"
<#
-->
<!-- 40 -->
```verse
MyModule := module:
    PublicMessage<localizes><public> : message = "Public message"
    InternalMessage<localizes> : message = "Internal message"

    some_class := class:
        PrivateMessage<localizes><private> : message = "Private message"
```
<!-- #> -->

###### Best Practices

**Keep messages translatable:**
- Use complete sentences, not fragments that might be concatenated
- Avoid gender or number assumptions that do not translate well
- Provide context through parameter names

**Design for different languages:**
- Don't assume word order - let translators rearrange parameter positions
- Allow repeated parameter use for languages that need it
- Keep formatting codes (like comma separators) automated

**Organization:**
- Group related messages in the same module
- Use descriptive names that indicate message purpose
- Consider using abstract base classes for message families

<!--versetest
PlayerJoined<localizes>(PlayerName:string, TeamName:string) : message =
    "{PlayerName} joined team {TeamName}"

<#
-->
<!-- 41 -->
```verse
# Good: Clear, complete, flexible
PlayerJoined<localizes>(PlayerName:string, TeamName:string) : message =
    "{PlayerName} joined team {TeamName}"

# Avoid: Fragments that might be concatenated
# PlayerPrefix<localizes>(Name:string) : message = "Player {Name}"
# JoinedSuffix<localizes>(Team:string) : message = "joined {Team}"
```
<!-- #> -->

#### Evolution

Access specifiers play a crucial role in code evolution. Changing
access levels after publication can break compatibility:

- Narrowing access (public to private) breaks external code that
  depends on the member
- Widening access (private to public) is generally safe but creates
  new commitments
- Changing protected members affects the inheritance contract

The `<castable>` specifier on classes has special compatibility
requirements—once published, it cannot be added or removed, as this
would affect the safety of dynamic casts throughout the codebase.

When designing for long-term evolution, consider using internal access
for members that might eventually become public. This allows you to
test and refine APIs within your module before committing to public
exposure.

## Book of Verse Source Unit: 16_modules.md

### Modules

Modules and paths are fundamental concepts for code organization,
namespace management, and the ability to share and reuse code across
projects. Think of modules as containers that group related
functionality together, similar to packages in other programming
languages, but with stronger guarantees about versioning and
compatibility.

In the context of game development, modules allow you to separate
different aspects of your game logic into manageable, reusable
pieces. For example, you might have one module for player inventory
management, another for combat mechanics, and yet another for UI
interactions. Each module encapsulates its own functionality while
exposing only the necessary interfaces to other parts of your code.

The module system is designed to support the vision of a persistent,
shared Metaverse where code can be published once and used by anyone,
anywhere, with confidence that it will continue to work even as the
original author updates and improves it. This is achieved through
strict backward compatibility rules and a global namespace system that
ensures every piece of published code has a unique, permanent address.

Each module is intrinsically linked to the file system structure of
your project. When you create a folder in your Verse project, that
folder automatically becomes a module. The module's name is simply the
folder's name, making the relationship between your file organization
and your code organization completely transparent.

All `.verse` files within the same folder are considered part of that
module and share the same namespace. This means that if you have three
files - `player.verse`, `inventory.verse`, and `equipment.verse` - all
in a folder called `PlayerSystems`, they all contribute to the
`PlayerSystems` module and can reference each other's definitions
without any import statements. This automatic grouping makes it easy
to split large modules across multiple files for better organization
while maintaining the logical unity of the module.

#### Paths

Paths are the addressing system that makes Verse's vision of a shared,
persistent Metaverse possible. Just as every website on the internet
has a unique URL, every module has a unique path that identifies it
globally. This path system is more than just a naming convention -
it is a fundamental part of how Verse manages code distribution,
versioning, and dependencies.

Paths borrow conceptually from web domains with adaptations for the
needs of a programming language. A path starts with a forward slash
`/` and typically includes a domain-like identifier followed by one or
more path segments. This creates a hierarchical namespace that is both
human-readable and globally unique.

The format `/domain/path/to/module` serves several important purposes:

- **Persistent and unique identification**: Once a module is published
  at a particular path, that path belongs to it forever. No other
  module can ever claim the same path, ensuring that dependencies
  always resolve to the correct code.

- **Ownership and authority**: The domain portion of the path (like
  `Fortnite.com` or `Verse.org`) indicates who owns and maintains the
  module. This helps developers understand the source and
  trustworthiness of the code they are using.

- **Discoverability**: Because paths follow a predictable pattern,
  developers can often guess or easily find the modules they
  need. Documentation and tooling can also leverage this structure to
  provide better discovery experiences.

- **Hierarchical organization**: The path structure naturally supports
  organizing related modules together. For example, all UI-related
  modules might live under `/YourGame.com/UI/`, making them easy to
  find and understand as a group.

Epic Games provides several standard modules that are commonly used:

- `/Verse.org/Verse` - Core language features and standard library functions
- `/Verse.org/Random` - Random number generation utilities
- `/Verse.org/Simulation` - Simulation and timing utilities
- `/Fortnite.com/Devices` - Integration with Fortnite Creative devices
- `/UnrealEngine.com/Temporary/Diagnostics` - Debugging and diagnostic tools
- `/UnrealEngine.com/Temporary/SpatialMath` - 3D math and spatial operations

The use of "Temporary" in some paths indicates that these modules are
provisional and may be reorganized in future versions of Verse. This
naming convention helps set expectations about the stability of the
API.

When you create your own modules, they can exist at various levels of
the path hierarchy:

- `/YourGame/` - Top-level module for your game
- `/YourGame/Player/` - Player-related functionality
- `/YourGame/Player/Inventory/` - Specific inventory management
- `/pizlonator@fn.com/NightDeath/` - Personal or experimental modules

The ability to include email-like identifiers (such as
`pizlonator@fn.com`) allows individual developers to claim their own
namespace without needing to own a domain. This democratizes the
module system while still maintaining uniqueness guarantees.

#### Creating Modules

A module can contain:

- Constants and variables
- Functions
- Classes, interfaces, and structs
- Enums
- Other module definitions
- Type definitions

When you create a subfolder in a Verse project, a module is
automatically created for that folder. The file structure directly
maps to the module hierarchy.

You can create modules within a `.verse` file using the following syntax:

<!--versetest
m := module{
module1 := module:
    MyConstant<public>:int = 42

    MyClass<public> := class:
        Value:int = 0

module2 := module
{
    AnotherConstant<public>:string = "Hello"
}
}
<#
-->
<!-- 01 -->
```verse
# Colon syntax
module1 := module:
    # Module contents here
    MyConstant<public>:int = 42

    MyClass<public> := class:
        Value:int = 0

# Bracket syntax (also supported)
module2 := module
{
    # Module contents here
    AnotherConstant<public>:string = "Hello"
}
```
<!-- #> -->

Modules can contain other modules, creating a hierarchy:

<!--versetest
m := module{
BaseModule<public> := module:
    submodule<public> := module:
        submodule_class<public> := class:
            Value:int = 100

    module_class<public> := class:
        Name:string = ""
}
<#
-->
<!-- 02 -->
```verse
BaseModule<public> := module:
    submodule<public> := module:
        submodule_class<public> := class:
            Value:int = 100

    module_class<public> := class:
        Name:string = ""
```
<!-- #> -->

The file structure `ModuleFolder/BaseModule` is equivalent to:

<!--versetest
m := module{
ModuleFolder := module:
    BaseModule := module:
        Submodule := module:
            submodule_class := class:
                Value:int = 0
}
<#
-->
<!-- 03 -->
```verse
ModuleFolder := module:
    BaseModule := module:
        Submodule := module:
            submodule_class := class:
                # Class definition
```
<!-- #> -->

##### Restrictions

Module bodies have strict requirements about what they can
contain. Understanding these restrictions helps avoid common errors
when defining modules.

**Modules Can Only Contain Definitions:**

A module body can only contain definition statements—declarations that
bind names to values. You cannot include arbitrary expressions or
executable statements:

<!--NoCompile-->
<!-- 04 -->
```verse
# Valid: All definitions
Config := module:
    MaxValue:int = 100
    DefaultName:string = "Player"

    CalculateScore(Base:int):int = Base * 10

    player_class := class:
        Name:string

# Invalid: Contains non-definition expressions
BadModule := module:
    MaxValue:int = 100
    1 + 2  # ERROR 3560: Not a definition

# Invalid: Contains function call
BadModule2 := module:
    InitFunction():void = {}
    InitFunction()  # ERROR 3585: Cannot call function in module body
```

The restriction ensures that module initialization is deterministic and does not execute arbitrary code when the module is loaded.

**Type Annotations Required:**

All data definitions at module scope must explicitly specify their type. Type inference with `:=` alone is not allowed:

<!--NoCompile-->
<!-- 05 -->
```verse
# Invalid: Missing type annotation
BadModule := module:
    Value := 42  # ERROR 3547: Must specify type domain

# Valid: Explicit type annotation
GoodModule := module:
    Value:int = 42  # OK: Type explicitly specified
```

This requirement makes module interfaces explicit and helps with separate compilation and module evolution.

**Valid Module Contents:**

Modules can contain these categories of definitions:

<!--versetest
m := module{
Utilities := module:
    Version:int = 1
    AppName:string = "MyApp"

    Calculate(X:int):int = X * 2

    data_class := class:
        Value:int

    data_interface := interface:
        GetValue():int

    data_struct := struct:
        X:float
        Y:float

    status := enum:
        Active
        Inactive

    Nested := module:
        NestedFunction():void = {}

    coordinate := tuple(float, float)

    positive_int := type{X:int where X > 0}
}
<#
-->
<!-- 06 -->
```verse
Utilities := module:
    # Constants with explicit types
    Version:int = 1
    AppName:string = "MyApp"

    # Functions
    Calculate(X:int):int = X * 2

    # Classes, interfaces, structs
    data_class := class:
        Value:int

    data_interface := interface:
        GetValue():int

    data_struct := struct:
        X:float
        Y:float

    # Enums
    status := enum:
        Active
        Inactive

    # Nested modules
    Nested := module:
        NestedFunction():void = {}

    # Type aliases
    coordinate := tuple(float, float)

    # Refinement types
    positive_int := type{X:int where X > 0}
```
<!-- #> -->

Unlike functions, classes, or data values, modules are not first-class
citizens in Verse. You cannot treat modules as values that can be
stored, passed, or manipulated at runtime.

**Cannot Assign Modules to Variables:**

<!--NoCompile-->
<!-- 07 -->
```verse
MyModule := module:
    Value<public>:int = 42

# Invalid: Cannot treat module as value
M:MyModule = MyModule  # ERROR 
```

Modules exist purely as namespaces and organizational constructs at
compile time. The module identifier `MyModule` can only be used in
specific contexts.

**Cannot Pass Modules as Arguments:**

<!--NoCompile-->
<!-- 08 -->
```verse
MyModule := module:
    X<public>:int = 1

# Invalid: Cannot pass module as parameter
ProcessModule(M:module):void = {}  # ERROR
ProcessModule(MyModule)  # ERROR
```

There is no `module` type that can be used in function signatures.

**Cannot Create Collections of Modules:**

<!--NoCompile-->
<!-- 09 -->
```verse
ModuleA := module:
    Value:int = 1

ModuleB := module:
    Value:int = 2

# Invalid: Cannot create tuple or array of modules
Modules := (ModuleA, ModuleB)  # ERROR
```

#### Importing Modules

The import system is designed to be explicit and predictable. Unlike
some languages that automatically import commonly used modules or
search multiple locations for dependencies, Verse requires you to
explicitly declare every external module you want to use. This
explicitness helps prevent naming conflicts and makes dependencies
clear.

The `using` statement is the primary mechanism for importing modules
into your Verse code. It usually is placed at the top of your file, before
any other code definitions, and makes the contents of the specified module
available in your current scope.

The basic syntax is straightforward - the keyword `using` followed by
the module path in curly braces:

<!--NoCompile-->
```verse
using { /Verse.org/Random }
using { /Fortnite.com/Devices }
using { /Verse.org/Simulation }
using { /UnrealEngine.com/Temporary/Diagnostics }
```

When you import a module, all its public members become available in
your code. However, you still need to qualify them with the module
name unless the names are unambiguous. This qualification requirement
helps maintain code clarity and prevents accidental use of the wrong
definition when multiple modules define similar names.

**Using is a Statement, Not an Expression:**

The `using` directive is a statement-level declaration that must
appear at the top level of your code. You cannot use it as an
expression or embed it in other expressions:

<!-- 11 -->
```verse
# Invalid: using in expression context
# f():void = using{MyModule}  # ERROR 3669

# Invalid: using in conditional
# if (using{MyModule}, Condition?):
#     DoSomething()  # ERROR 3669

# Invalid: using in class/struct/interface body
# my_class := class:
#     using{MyModule}  # ERROR 3537
#     Field:int

# Invalid: using module path in function body
# ProcessData():void =
#     using{/MyProject/UtilityModule}  # ERROR 3669
#     Calculate()
```

Module `using` statements must appear at the file or module level, not
nested within other constructs. This ensures that imports are visible
and consistent throughout the scope where they are declared.

While module imports with paths are not allowed in function bodies,
Verse does support **local scope `using`** with local variables and
parameters. See [Local Scope Using](#local-scope-using) below for
details.

**Valid using placement:**

<!--NoCompile-->
<!-- 12 -->
```verse
# At file level (most common)
using { /Verse.org/Random }
using { /Verse.org/Simulation }

ProcessData():void =
    # Use imported functions
    Value := GetRandomFloat(0.0, 1.0)

# Within module definition
Utilities := module:
    using { /Verse.org/Random }

    GenerateId<public>():int =
        GetRandomInt(1, 1000000)
```

##### Import Resolution

When Verse encounters a `using` statement, it follows a specific resolution process:

1. **Absolute paths** (starting with `/`) are resolved from the global module registry
2. **Relative paths** (without leading `/`) are resolved relative to the current module's location
3. **Nested modules** can be accessed through their parent modules

This resolution process happens at compile time, meaning that all
imports must be resolvable when your code is compiled. There's no
runtime module loading or dynamic imports in Verse.

##### Local and Relative Imports

For modules within your own project, you have flexibility in how you reference them:

<!--NoCompile-->
<!-- 13 -->
```verse
# Absolute import from your project root
using { /MyGameProject/Systems/Combat }

# Import from a sibling folder
using { ../UI/MainMenu }

# Import from the same directory
using { PlayerController }

# Import from a subdirectory
using { Subsystems/WeaponSystem }
```

The choice between absolute and relative imports often depends on your
project structure and whether you plan to reorganize your
modules. Absolute imports are more stable when refactoring, while
relative imports can make module groups more portable.

##### Nested Imports

Nested modules present special considerations for importing. The order
in which you import modules matters, and there are multiple valid
approaches:

<!--versetest
GameSystems := module:
    Inventory<public> := module{}
m:=module{
using { GameSystems }
using { Inventory }

using { GameSystems.Inventory }

using { GameSystems }
}
<#
-->
<!-- 14 -->
```verse
# Method 1: Import parent first, then child
using { GameSystems }
using { Inventory }  # Assumes Inventory is nested in GameSystems

# Method 2: Direct path to nested module
using { GameSystems.Inventory }

# Method 3: Import parent and access child through qualification
using { GameSystems }
# Later in code: GameSystems.Inventory.Item

# IMPORTANT: This order causes an error
# using { Inventory }      # Error: Inventory not found
# using { GameSystems }   # Too late, Inventory import already failed
```
<!-- #> -->

The restriction on import order exists because Verse resolves imports
sequentially. When you import a nested module directly, Verse needs to
know about its parent module first. This is why importing the parent
before the child always works, while the reverse order fails.

##### Module Aliases with import

The `import` expression creates a local alias for a module, binding
its path to a name. Unlike `using`, which brings a module's public
members directly into scope, `import` lets you access them through
the alias with dot notation:

<!--NoCompile-->
<!-- 14b -->
```verse
# using: members available directly
using { /MyProject/Utilities }
Result := HelperFunction()  # HelperFunction is in scope

# import: members accessed through alias
Utils := import(/MyProject/Utilities)
Result := Utils.HelperFunction()  # accessed via alias
```

This is useful when you want to avoid name collisions, or when you
need to make the origin of a definition explicit in your code:

<!--NoCompile-->
<!-- 14c -->
```verse
Physics := import(/MyProject/Systems/Physics)
Graphics := import(/MyProject/Systems/Graphics)

# Clear which Transform is being used
PhysicsTransform := Physics.Transform{}
GraphicsTransform := Graphics.Transform{}
```

Module aliases created with `import` are visible across all snippets
within the same module. An `import` can also be combined with `using`
to both alias a module and bring its members into scope:

<!--NoCompile-->
<!-- 14d -->
```verse
Graphics := import(/MyProject/Systems/Graphics)
using { Graphics }  # now Graphics members are also directly available
```

Note that `import` only works with module paths. Attempting to import
a path that resolves to a class or other non-module definition is an
error.

##### Scope and Visibility

Imports have file scope - they only affect the file in which they
appear. If you have multiple `.verse` files in the same module, each
file needs its own import statements for external modules. However,
files within the same module can see each other's definitions without
imports:

<!--versetest
m := module{
health_component := class:
    CurrentHealth:float = 100.0

armor_component := class:
    HealthComp:health_component = health_component{}
}
<#
-->
<!-- 15 -->
```verse
# File: player_module/health.verse
health_component := class:
    CurrentHealth:float = 100.0

# File: player_module/armor.verse
# No import needed for health_component since it is in the same module
armor_component := class:
    HealthComp:health_component = health_component{}
```
<!-- #> -->

##### Import Conflicts

When two imported modules define members with the same name, you need to disambiguate:

<!--NoCompile-->
<!-- 16 -->
```verse
using { /GameA/Combat }
using { /GameB/Combat }

# Both modules might define CalculateDamage
# You must use qualified names:
DamageA := Combat.CalculateDamage(10.0)  # Error: ambiguous
DamageA := (/GameA/Combat:)CalculateDamage(10.0)  # OK: fully qualified
DamageB := (/GameB/Combat:)CalculateDamage(10.0)  # OK: fully qualified
```

##### Qualified Names

After importing, you can refer to module contents using qualified
names. Verse provides two forms of qualification: standard dot
notation for most cases, and special qualified access syntax for
disambiguation.

When you need to disambiguate between identifiers with the same name
from different modules, or when you want to explicitly specify the
scope of an identifier, use a qualified access expression using
parentheses and a colon:


<!-- BUG? Or bad error message?

m := module{ item<public> := class{} }

x := module{
item := class{}
F():void =
    A := (local:)item{} 
    B := (m:)item{}
}


LogVerseBuild: Error: C:/VerseBook/Book/verse/16_modules/17.versetest(8,10, 8,22): Script Error 3506: Unknown identifier `item`. Did you mean any of:
InventoryModule.item
item
LogVerseBuild: Error: C:/VerseBook/Book/verse/16_modules/17.versetest(9,10, 9,33): Script Error 3506: Unknown identifier `item`. Did you mean any of:
InventoryModule.item

-->

<!--NoCompile-->
<!-- 17 -->
```verse
# Qualified access syntax: (qualifier:)identifier

using { CombatModule }
using { MagicModule }

ProcessDamage():void =
    # Both modules define CalculateDamage
    PhysicalDamage := (CombatModule:)CalculateDamage(100.0)
    MagicalDamage := (MagicModule:)CalculateDamage(100.0)

    # Explicitly qualify local vs module identifiers
    LocalItem := item{Name := "Sword"}  # Local definition
    ModuleItem := (InventoryModule:)item{Name := "Shield"}  # From module
```

The qualified access expression `(module:)identifier` is particularly useful in several scenarios:

1. **Resolving naming conflicts**: When multiple imported modules export the same identifier
2. **Explicit scoping**: When you want to make it clear which module an identifier comes from for readability
3. **Accessing shadowed names**: When a local definition shadows a module member
4. **Generic programming**: When working with parametric types where the qualifier might be computed

#### Module-Scoped Variables

Variables defined at module scope are global to any game instance where the variable is in scope.

Restrictions on module-scoped definitions:

- Direct `var` declarations of simple types (like `var X:int = 0`) are not allowed at module scope
- Instances of `<unique>` classes with `<allocates>` can be created at module scope, as long as their construction does not actually allocate mutable memory
- For persistent mutable state, use `weak_map` with appropriate key types (see below)

Use `weak_map(session, t)` for variables that persist for the duration of a game session:

<!--versetest
session := class<unique>{}
GetSession()<transacts>:session = session{}
-->
<!-- 20 -->
```verse
var GlobalCounter:weak_map(session, int) = map{}

IncrementCounter()<transacts>:void =
    CurrentValue := if (Value := GlobalCounter[GetSession()]) then Value + 1 else 0
    if (set GlobalCounter[GetSession()] = CurrentValue) {}
```

Use `weak_map(player, t)` for data that persists across game sessions:

<!--versetest
player := class<unique><persistent><module_scoped_var_weak_map_key>{}
var PlayerSaveData:weak_map(player, player_data) = map{}

player_data := class<final><persistable>:
    Level:int = 1
    Experience:int = 0
    UnlockedItems:[]string = array{}

SavePlayerProgress(Player:player, NewData:player_data)<decides>:void =
    set PlayerSaveData[Player] = NewData
<#
-->
<!-- 21 -->
```verse
var PlayerSaveData:weak_map(player, player_data) = map{}

player_data := class<final><persistable>:
    Level:int = 1
    Experience:int = 0
    UnlockedItems:[]string = array{}

SavePlayerProgress(Player:player, NewData:player_data)<decides>:void =
    set PlayerSaveData[Player] = NewData
```
<!-- #> -->

#### Metaverse and Publishing

When you publish a module to the Metaverse, the module path becomes
globally accessible, its public members become part of the module's
API, and from that point the module must maintain backward
compatibility.

The following example of shows how evolution works:

<!--NoCompile-->
<!-- 22 -->
```verse
# Initial publication
Thing<public>:rational = 1/3

# Valid updates:
# - Change the value (not the type)
Thing<public>:rational = 10/3

# - Make the type more specific (subtype)
Thing<public>:int = 20  # int is a subtype of rational

# Invalid updates (would be rejected):
# - Remove the member
# - Change to incompatible type
# Thing<public>:string = "hello"  # Would fail
```

#### Local Qualifiers

The `(local:)` qualifier can disambiguate identifiers within
functions. This is critical for evolution compatibility—when external
modules add new public definitions after your code is published,
`(local:)` ensures your local definitions take precedence.

<!--versetest
m := module{
ExternalModule<public> := module:
    ShadowX<public>:int = 10

MyModule := module:
    using{ExternalModule}


    Foo():float =
        (local:)ShadowX:float = 0.0
        (local:)ShadowX
}
<#
-->
<!-- 23 -->
```verse
# External module adds ShadowX after your code published
ExternalModule<public> := module:
    ShadowX<public>:int = 10  # Added later!

MyModule := module:
    using{ExternalModule}

    # Without (local:) - shadowing conflict
    # Foo():float =
    #     ShadowX:float = 0.0  # Error: conflicts with ExternalModule.ShadowX
    #     ShadowX

    # With (local:) - clear disambiguation
    Foo():float =
        (local:)ShadowX:float = 0.0  # Local variable
        (local:)ShadowX              # Returns 0.0, not 10
```
<!-- #> -->

The `(local:)` qualifier can be used in these contexts:

**Function parameters:**

<!--versetest
m := module{
ProcessValue((local:)Value:int):int =
    (local:)Value + 1
}
<#
-->
<!-- 24 -->
```verse
ProcessValue((local:)Value:int):int =
    (local:)Value + 1
```
<!-- #> -->

**Function body data definitions:**

<!--versetest
m := module{
Compute():int =
    (local:)Result:int = 42
    (local:)Result
}
<#
-->
<!-- 25 -->
```verse
Compute():int =
    (local:)Result:int = 42
    (local:)Result
```
<!-- #> -->

**For loop variables:**

<!--versetest
m := module{
SumValues():int =
    var Total:int = 0
    for ((local:)I := 0..10):
        set Total += (local:)I
    Total
}
<#
-->
<!-- 26 -->
```verse
SumValues():int =
    var Total:int = 0
    for ((local:)I := 0..10):
        set Total += (local:)I
    Total
```
<!-- #> -->

**If conditions:**

<!--versetest
GetValue<public>()<computes><decides>:float = 10.0
-->
<!-- 27 -->
```verse
CheckValue():float =
    if (X := GetValue[], (local:)X > 5.0):
        (local:)X
    else:
        0.0
```

**Block scopes:**

<!--versetest
m := module{
ComputeInBlock():int =
    block:
        (local:)Temp:int = 10
        (local:)Temp * 2
}
<#
-->
<!-- 28 -->
```verse
ComputeInBlock():int =
    block:
        (local:)Temp:int = 10
        (local:)Temp * 2
```
<!-- #> -->

**Class blocks:**

<!--NoCompile-->
<!-- 29 -->
```verse
my_class := class:
    var Value<public>:int = 0
    block:
        (local:)Value:int = 42
        set (my_class:)Value = (local:)Value
```

The `(local:)` qualifier **cannot** be used in these contexts:


**Nested Scope Limitation:**

Currently, you **cannot** redefine a `(local:)` qualified identifier in nested blocks:

<!--NoCompile-->
```verse
# Error: cannot redefine local identifier
F((local:)X:int):int =
    block:
        (local:)X:float = 5.5  # Error: X already defined in function
    (local:)X
```

This limitation may be lifted in future versions to support more complex scoping patterns.

#### Automatic Qualification

!!! warning "Unreleased Feature"
    Automatic qualification has not yet been fully implemented. This section documents planned functionality that is not currently available. The behavior described here, particularly regarding how the compiler transforms identifiers in published code, should not be relied upon until officially released.

When you write Verse code, you use simple, unqualified identifiers for
clarity and readability. However, the Verse compiler will internally
transform all identifiers into fully-qualified forms that explicitly
specify their scope and origin. This process, called **automatic
qualification**, will ensure that every identifier is unambiguous and can
be resolved to exactly one definition.

Understanding automatic qualification will help you understand how Verse
will resolve names, why certain errors occur, and how the module system
will maintain correctness even in complex codebases with many modules and
overlapping names.

The compiler will qualify several categories of identifiers:

1. **Top-level definitions** - Functions, variables, classes, modules at package scope
2. **Type references** - All types, including built-in types like `int` and `string`
3. **Function parameters** - Local parameters get the `(local:)` qualifier
4. **Class and interface members** - Methods, fields, nested within composite types
5. **Module members** - Public and internal definitions within modules
6. **Nested scopes** - References within nested modules, classes, and functions

Verse uses several patterns to qualify identifiers based on their scope:

**Package-level qualification**: Definitions at the root of a package
are qualified with the package path:

<!--NoCompile-->
```verse
# What you write:
Function(X:int):int = X

# How the compiler sees it:
(/YourPackage:)Function((local:)X:(/Verse.org/Verse:)int):(/Verse.org/Verse:)int = (local:)X
```

The package path `/YourPackage` becomes the qualifier for `Function`,
while the parameter `X` gets the special `(local:)` qualifier, and the
built-in type `int` is qualified with its standard library path
`/Verse.org/Verse`.

**Local scope qualification**: Function parameters and local variables are marked with `(local:)`:

<!--NoCompile-->
```verse
# What you write:
ProcessValue(Input:int, Multiplier:int):int =
    Input * Multiplier

# How the compiler sees it:
(/YourPackage:)ProcessValue((local:)Input:(/Verse.org/Verse:)int, (local:)Multiplier:(/Verse.org/Verse:)int):(/Verse.org/Verse:)int =
    (local:)Input * (local:)Multiplier
```

**Nested scope qualification**: Members within classes, interfaces, or modules get qualified with their container's path:

<!--NoCompile-->
```verse
# What you write:
player_class := class:
    Health:float = 100.0

    TakeDamage(Amount:float):void =
        set Health = Health - Amount

# How the compiler sees it:
(/YourPackage:)player_class := class:
    (/YourPackage/player_class:)Health:(/Verse.org/Verse:)float = 100.0

    (/YourPackage/player_class:)TakeDamage((local:)Amount:(/Verse.org/Verse:)float):(/Verse.org/Verse:)void =
        set (/YourPackage/player_class:)Health = (/YourPackage/player_class:)Health - (local:)Amount
```

Notice how `Health` and `TakeDamage` are qualified with `/YourPackage/player_class` to indicate they are members of the class.

**Module member qualification**: Definitions within modules are qualified with the module path:

<!--NoCompile-->
```verse
# What you write:
Config := module:
    MaxPlayers<public>:int = 100

    GetPlayerLimit<public>():int = MaxPlayers

# How the compiler sees it:
(/YourPackage:)Config := module:
    (/YourPackage/Config:)MaxPlayers<public>:(/Verse.org/Verse:)int = 100

    (/YourPackage/Config:)GetPlayerLimit<public>():(/Verse.org/Verse:)int =
        (/YourPackage/Config:)MaxPlayers
```

All built-in types are qualified with their standard library
paths. This makes it explicit where these types come from and
maintains consistency with user-defined types:

<!--NoCompile-->
```verse
# Common built-in types and their full qualifications:
int       → (/Verse.org/Verse:)int
float     → (/Verse.org/Verse:)float
string    → (/Verse.org/Verse:)string
logic     → (/Verse.org/Verse:)logic
message   → (/Verse.org/Verse:)message
```

When you write `X:int`, the compiler expands it to `X:(/Verse.org/Verse:)int`, making the type's origin explicit.

##### Example

Here's a more realistic example showing how qualification would work across multiple scopes:

<!--NoCompile-->
```verse
# What you write:
GameSystem := module:
    BaseValue:int = 42

    Calculator := module:
        Multiplier:int = 2

        Calculate(Input:int):int =
            Input * Multiplier + BaseValue

# How the compiler will see it (when implemented):
(/YourGame:)GameSystem := module:
    (/YourGame/GameSystem:)BaseValue:(/Verse.org/Verse:)int = 42

    (/YourGame/GameSystem:)Calculator := module:
        (/YourGame/GameSystem/Calculator:)Multiplier:(/Verse.org/Verse:)int = 2

        (/YourGame/GameSystem/Calculator:)Calculate((local:)Input:(/Verse.org/Verse:)int):(/Verse.org/Verse:)int =
            (local:)Input * (/YourGame/GameSystem/Calculator:)Multiplier + (/YourGame/GameSystem:)BaseValue
```

Notice how:

- The parameter `Input` is `(local:)`
- `Multiplier` is qualified with its containing module path
- `BaseValue` is qualified with the outer module path
- All type references are qualified with the Verse standard library path

**Important Note on Shadowing**: Automatic qualification will only apply to published code, not your source code. Verse currently enforces strict anti-shadowing rules to prevent confusion and maintain code clarity. For example, this code does **not** compile:

<!--NoCompile-->
```verse
# This does NOT compile - shadowing is not allowed
Thing := module:
    Thing := module:  # ERROR: Cannot shadow outer Thing
        Potato := module{}
```

Even with automatic qualification, nested definitions cannot shadow outer definitions with the same name. If you want to intentionally shadow something, you must use explicit qualifiers to make your intent clear. This strict approach helps prevent bugs and makes code evolution safer.

##### Qualification with Using

When you import modules with `using`, the compiler still qualifies all identifiers, but it can resolve unqualified names to the imported modules:

<!-- NoCompile-->
```verse
# What you write:
using { /Verse.org/Random }

GenerateRandomValue():float =
    GetRandomFloat(0.0, 1.0)

# How the compiler sees it:
using { /Verse.org/Random }

(/YourGame:)GenerateRandomValue():(/Verse.org/Verse:)float =
    (/Verse.org/Random:)GetRandomFloat(0.0, 1.0)
```

The compiler resolves `GetRandomFloat` to `(/Verse.org/Random:)GetRandomFloat` based on the `using` statement.

##### When It Matters

Once implemented, you will rarely need to think about automatic or manual qualification during normal
development, as the compiler will handle it transparently. However,
understanding it will help in several situations:

**Debugging name resolution errors**: When the compiler reports
ambiguous or unresolved identifiers, understanding qualification helps
you see why:

<!--NoCompile-->
```verse
using { /ModuleA }
using { /ModuleB }

# Both modules define Calculate
Result := Calculate(10)  # ERROR: Ambiguous - could be either module
```

The error occurs because the compiler cannot automatically qualify `Calculate` - it could be either `(/ModuleA:)Calculate` or `(/ModuleB:)Calculate`.

**Shadowing conflicts**: When a local variable has the same name as a module member:

<!--NoCompile-->
```verse
MyModule := module:
    Value:int = 100

    Process(Value:int):int =
        # Without explicit qualification, this is ambiguous
        Value + Value  # Which Value? Module or parameter?
```

The compiler needs qualification to distinguish `(/MyModule:)Value` from `(local:)Value`.

**Understanding error messages**: Compiler error messages sometimes
show qualified names to precisely identify which definition is
involved:

```
Error: Cannot assign (/Verse.org/Verse:)string to (/Verse.org/Verse:)int at line 42
```

This makes it clear that the error involves the built-in `string` and
`int` types, not user-defined types with the same names.

**Working with generated or reflected code**: Tools that generate
Verse code or analyze code structure work with the qualified form, so
understanding it helps when working with such tools.

##### Explicit Qualification

While the compiler automatically qualifies identifiers, you can also
explicitly qualify them using the qualified access syntax
`(qualifier:)identifier`. This is useful when you want to override
automatic resolution or make your intent explicit:

<!-- 45 FAILURE
  Line 11: Verse compiler error V3509: The assignment's left hand expression type `int` cannot be assigned to
-->
```verse
GameSystem := module:
    Value:int = 100

    # Explicitly qualify to avoid any ambiguity
    GetValue():int = (GameSystem:)Value

    # Use local qualifier for parameters
    SetValue((local:)Value:int):void =
        set (GameSystem:)Value = (local:)Value
```

Explicit qualification is particularly valuable when:

- Resolving naming conflicts between imported modules
- Making code more self-documenting
- Overriding shadowing behavior
- Working with dynamic or computed qualifiers

#### Local Scope Using

While module-level `using` imports modules by their paths, Verse also
supports **local scope `using`** within function bodies to enable
member access inference from local variables and parameters. This
feature makes code cleaner when working with objects that have many
member accesses.

Local scope `using` takes a local variable or parameter identifier
(not a module path) and makes its members accessible without explicit
qualification:

<!--versetest
m := module{
entity := class:
    Name:string = "Entity"
    var Health:int = 100

    UpdateHealth(Amount:int):void =
        set Health = Health + Amount

ProcessEntity(E:entity):void =
    Print(E.Name)
    E.UpdateHealth(-10)
    Print("{E.Health}")

    using{E}
    Print(Name)
    UpdateHealth(-10)
    Print("{Health}")
}
<#
-->
<!-- 46 -->
```verse
entity := class:
    Name:string = "Entity"
    var Health:int = 100

    UpdateHealth(Amount:int):void =
        set Health = Health + Amount

ProcessEntity(E:entity):void =
    # Explicit member access
    Print(E.Name)
    E.UpdateHealth(-10)
    Print("{E.Health}")

    # With local using - inferred member access
    using{E}
    Print(Name)         # Inferred as: E.Name
    UpdateHealth(-10)   # Inferred as: E.UpdateHealth(-10)
    Print("{Health}")       # Inferred as: E.Health
```
<!-- #> -->

The `using{E}` expression makes all members of `E` accessible without the `E.` prefix within the current scope.

##### With Local Variables

Local `using` works with variables defined in the same function:

<!--versetest
player := class:
    var Name:string = ""
    var Score:int = 0
-->
<!-- 47 -->
```verse
CreateAndProcess():void =
    Player := player{Name := "Alice", Score := 100}

    # Without using
    Print(Player.Name)
    set Player.Score = Player.Score + 50

    # With using
    using{Player}
    Print(Name)         # Inferred as: Player.Name
    set Score = Score + 50  # Inferred as: Player.Score
```

##### Block Scoping

The `using` scope is limited to the block where it appears and any nested blocks:

**Using in same block:**

<!--versetest
data_record := class:
    Value:int = 0
    UpdateField<public>(V:int):void = {}
-->
<!-- 48 -->
```verse
ProcessData():void =
    block:
        Data := data_record{}
        using{Data}
        UpdateField(Value)  # Inferred as: Data.UpdateField(Data.Value)
    # Data members no longer accessible here
```

**Using from outer block:**

<!--versetest
data_record := class:
    Value:int = 0
    UpdateField<public>(V:int):void = {}
-->
<!-- 49 -->
```verse
ProcessData():void =
    Data := data_record{}
    block:
        using{Data}  # Can use variable from outer scope
        UpdateField(Value)  # Works - Data in scope
```

**Nested block inheritance:**

<!--versetest
data_record := class:
    Value:int = 0
    UpdateField<public>(V:int):void = {}
-->
<!-- 50 -->
```verse
ProcessData():void =
    Data := data_record{}
    using{Data}  # Applies to this block and nested blocks

    block:
        # Inner block inherits outer using
        UpdateField(Value)  # Still infers Data.UpdateField(Data.Value)
```

##### Order

Member inference only works **after** the `using` expression is encountered:

<!--NoCompile-->
```verse
# ERROR: Cannot infer before using
ProcessData(Data:data_record):void =
    UpdateField()  # ERROR - before using
    using{Data}
    UpdateField()  # OK - after using

# ERROR: Using scope does not extend backward
ProcessData(Data:data_record):void =
    block:
        using{Data}
        UpdateField()  # OK - within using scope
    UpdateField()  # ERROR - after using scope ended
```

The `using` statement acts as a declaration point - inference is not retroactive.

##### Conflict Resolution

You can have multiple `using` expressions in the same scope, but conflicting member names must be explicitly qualified:

<!--versetest
m := module{
player_stats := class:
    Health:int = 100
    Mana:int = 50
    GetInfo():string = "Player"

enemy_stats := class:
    Health:int = 80
    Armor:int = 20
    GetInfo():string = "Enemy"

ProcessCombat(Player:player_stats, Enemy:enemy_stats):void =
    using{Player}
    Print(GetInfo())
    Print("{Mana}")

    using{Enemy}
    Print("{Armor}")


    Print("{Player.Health}")
    Print("{Enemy.Health}")
    Print(Player.GetInfo())
    Print(Enemy.GetInfo())
}
<#
-->
<!-- 52 -->
```verse
player_stats := class:
    Health:int = 100
    Mana:int = 50
    GetInfo():string = "Player"

enemy_stats := class:
    Health:int = 80
    Armor:int = 20
    GetInfo():string = "Enemy"

ProcessCombat(Player:player_stats, Enemy:enemy_stats):void =
    using{Player}
    Print(GetInfo())  # Player.GetInfo()
    Print("{Mana}")       # Player.Mana (no conflict)

    using{Enemy}
    # Now both are in scope
    Print("{Armor}")      # Enemy.Armor (no conflict with Player)

    # ERROR: Conflicts must be qualified
    # Print(Health)   # Ambiguous - both have Health
    # Print(GetInfo())  # Ambiguous - both have GetInfo

    # Must qualify conflicting members
    Print("{Player.Health}")
    Print("{Enemy.Health}")
    Print(Player.GetInfo())
    Print(Enemy.GetInfo())
```
<!-- #> -->

When members exist in multiple `using` contexts, you must explicitly qualify to disambiguate.

##### Mutable Member

Local `using` works with mutable fields through the `set` keyword:

<!--versetest
m := module{
config := class:
    var Volume:float = 1.0
    var Quality:int = 2

UpdateSettings(Settings:config):void =
    using{Settings}

    set Volume = 0.8
    set Quality = 3
}
<#
-->
<!-- 53 -->
```verse
config := class:
    var Volume:float = 1.0
    var Quality:int = 2

UpdateSettings(Settings:config):void =
    using{Settings}

    # Mutable field access
    set Volume = 0.8     # Inferred as: set Settings.Volume = 0.8
    set Quality = 3      # Inferred as: set Settings.Quality = 3
```
<!-- #> -->

#### Troubleshooting

When working with modules, you may encounter various issues. Understanding these common problems and their solutions will help you debug module-related errors more efficiently.

##### Module Not Found Errors

**Problem**: The compiler reports that a module cannot be found when you try to import it.

**Common Causes and Solutions**:

1. **Incorrect path**: Double-check the module path in your `using` statement. Remember that paths are case-sensitive.

<!--NoCompile-->
<!-- 54 -->
```verse
# Wrong: different case
using { /verse.org/random }  # Error: module not found

# Correct: proper case
using { /Verse.org/Random }  # Works
```

2. **Missing parent module import**: When importing nested modules, ensure the parent is imported first.

<!--NoCompile-->
<!-- 55 -->
```verse
# Wrong: child before parent
using { Inventory }  # Error if Inventory is nested

# Correct: parent first
using { GameSystems }
using { Inventory }
```

3. **File location mismatch**: Ensure your file structure matches your module structure. If you have a folder named `PlayerSystems`, all files in that folder are part of the `PlayerSystems` module.

##### Access Denied Errors

**Problem**: You can't access a member of an imported module.

**Common Causes and Solutions**:

1. **Missing access specifier**: Members without the `<public>` specifier are internal by default.

<!--NoCompile-->
<!-- 56 -->
```verse
# In ModuleA
SecretValue:int = 42  # Internal by default
PublicValue<public>:int = 100  # Explicitly public

# In another module
using { ModuleA }
X := ModuleA.SecretValue  # Error: not accessible
Y := ModuleA.PublicValue  # Works
```

2. **Protected or private members**: These are not accessible outside their defining scope.

<!--NoCompile-->
<!-- 57 -->
```verse
# In a class
class_a := class:
    PrivateField<private>:int = 10
    ProtectedField<protected>:int = 20
    PublicField<public>:int = 30

# Outside the class
Obj := class_a{}
X := Obj.PrivateField  # Error: private
Y := Obj.PublicField   # Works
```

##### Circular Dependency Errors

**Problem**: Two modules try to import each other, creating a circular dependency.

**Solution**: Restructure your code to avoid circular dependencies:

1. **Extract common code**: Move shared definitions to a third module that both can import.
2. **Use interfaces**: Define interfaces in a separate module to break the dependency cycle.
3. **Reconsider architecture**: Circular dependencies often indicate a design issue that needs rethinking.

##### Name Collision Errors

**Problem**: Two imported modules define members with the same name.

**Solution**: Use fully qualified names to disambiguate:

<!--NoCompile-->
```verse
using { /GameA/Combat }
using { /GameB/Combat }

# Ambiguous
Damage := CalculateDamage(10.0)  # Error: which CalculateDamage?

# Explicit
DamageA := (/GameA/Combat:)CalculateDamage(10.0)  # Clear
DamageB := (/GameB/Combat:)CalculateDamage(10.0)  # Clear
```

##### Persistence Issues

**Problem**: Module-scoped variables are not persisting as expected.

**Common Causes and Solutions**:

1. **Wrong type used**: Ensure you are using `weak_map(player, t)` for player persistence.
2. **Type not persistable**: Check that your custom types have the `<persistable>` specifier.
3. **Initialization timing**: Make sure you are initializing persistent data at the right time in the game lifecycle.

##### Local Qualifier Conflicts

**Problem**: Shadowing errors when local identifiers conflict with module members.

**Solution**: Use the `(local:)` qualifier to disambiguate:

<!--versetest
m := module{
ModuleX := module:
    Value:int = 10

    ProcessValue((local:)Value:int):int =
        (ModuleX:)Value + (local:)Value
}
<#
-->
<!-- 59 -->
```verse
ModuleX := module:
    Value:int = 10

    ProcessValue((local:)Value:int):int =
        (ModuleX:)Value + (local:)Value  # Clear distinction
```
<!-- #> -->

---

## Related Documents

- [00_MASTER_KNOWLEDGE_INDEX.md](00_MASTER_KNOWLEDGE_INDEX.md)
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
- [14_BOOK_OF_VERSE_III_EFFECTS_CONCURRENCY_AND_EVOLUTION.md](14_BOOK_OF_VERSE_III_EFFECTS_CONCURRENCY_AND_EVOLUTION.md)
- [15_GLOSSARY_FORTNITE_CREATIVE.md](15_GLOSSARY_FORTNITE_CREATIVE.md)
- [16_GLOSSARY_UEFN.md](16_GLOSSARY_UEFN.md)
- [17_GLOSSARY_VERSE.md](17_GLOSSARY_VERSE.md)

## Related Glossary Sections

- [17_GLOSSARY_VERSE.md](17_GLOSSARY_VERSE.md)

## Official Sources

- [Book of Verse](https://verselang.github.io/book/)
- [Verse language reference](https://dev.epicgames.com/documentation/en-us/fortnite/verse-language-reference)

## Version and Stability Notes

- Treat UI labels, device option names, experimental features, publishing rules, memory limits, and eligibility requirements as version-sensitive.
- Prefer the official index and release notes when this document conflicts with a newer Epic page.
- Preserve exact API identifiers and Verse syntax; do not translate them.

## Source Coverage Notes

- Local sources were merged by topic and assigned one primary owner document; cross-links replace duplicate body text where practical.
- Administrative program plans, schedules, marketing material, fundraising text, and production-only QA files are not part of agent memory.
- Unique transferable technical, design, research, ethical, or instructional knowledge found inside excluded wrappers was distilled into the appropriate owner document.
