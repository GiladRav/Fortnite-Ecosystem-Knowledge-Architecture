# Book of Verse I: Foundations

## Document Metadata

| Field | Value |
|---|---|
| Document ID | `12` |
| Domain | Verse language |
| Primary Environment | Verse; UEFN availability must be verified |
| Language | English; Hebrew appears only in canonical terminology fields and exact source identifiers |
| Source Priority | Epic official documentation -> current Book of Verse -> verified local technical corpus -> research and teaching sources |
| Last Verified | 2026-08-02 |
| Stability Status | Mixed: stable concepts plus version-sensitive interface and platform details |

## Document Purpose

Preserve Book of Verse foundations: overview, expressions, primitive/container types, operators, mutability, functions, control flow, and failure.

## Scope and Exclusions

Language concepts are preserved from the current Book of Verse source; UEFN API integration belongs to `09`.

## When to Use This Document

- Use for Verse syntax, values, expressions, containers, functions, control flow, and failure semantics.

## Authority and Availability Gate

Book of Verse can describe language-main or planned functionality before it is available in the current UEFN release. It is not the final authority for current compiler availability. Before giving executable guidance:

1. confirm the construct in Epic's current Verse Language Reference or Verse API Reference;
2. confirm that the active UEFN compiler accepts it;
3. label material that is absent from the official current reference as **experimental or unreleased**;
4. never present a Book of Verse example as guaranteed shipping UEFN behavior solely because it appears in this document.

`Live Variables` and any dependent reactive constructs are treated as unreleased until Epic's current UEFN documentation and compiler confirm otherwise.

## Quick Topic Index

- [Book of Verse Source Unit: 00_overview.md](#book-of-verse-source-unit-00overviewmd)
- [Book of Verse Source Unit: 01_expressions.md](#book-of-verse-source-unit-01expressionsmd)
- [Book of Verse Source Unit: 02_primitives.md](#book-of-verse-source-unit-02primitivesmd)
- [Book of Verse Source Unit: 03_containers.md](#book-of-verse-source-unit-03containersmd)
- [Book of Verse Source Unit: 04_operators.md](#book-of-verse-source-unit-04operatorsmd)
- [Book of Verse Source Unit: 05_mutability.md](#book-of-verse-source-unit-05mutabilitymd)
- [Book of Verse Source Unit: 06_functions.md](#book-of-verse-source-unit-06functionsmd)
- [Book of Verse Source Unit: 07_control.md](#book-of-verse-source-unit-07controlmd)
- [Book of Verse Source Unit: 08_failure.md](#book-of-verse-source-unit-08failuremd)

## Common Question Router

- For environment selection and corpus-wide routing, start with [`00_MASTER_KNOWLEDGE_INDEX.md`](00_MASTER_KNOWLEDGE_INDEX.md).
- For current official URLs or version-sensitive claims, use [`01_EPIC_GAMES_DOCUMENTATION_INDEX.md`](01_EPIC_GAMES_DOCUMENTATION_INDEX.md).
- For a simple English-to-Hebrew name mapping, use [`02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md`](02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md).
- For a detailed definition, use the domain glossary listed under Related Glossary Sections.

---

## Book of Verse Source Unit: 00_overview.md

### The Verse Programming Language

#### Overview

Verse is a multi-paradigm programming language developed by Epic Games for creating gameplay in Unreal Editor for Fortnite and building experiences in the metaverse. Drawing from functional, logic, and imperative traditions, Verse represents a departure from traditional programming languages, designed for long-term evolution and stability.

Verse is built on three fundamental principles:

- **It's Just Code**:
Complex concepts that might require special syntax or constructs in other languages are expressed as regular Verse code. There's no magic—everything is built from the same primitive constructs, creating a uniform and predictable programming model.

- **Just One Language**:
The same language constructs work at both compile-time and run-time. There is no preprocessor. What you write is what executes, whether during compilation or at runtime.

- **Metaverse First**:
Verse is designed for a future where code runs in a single global simulation—the metaverse. This influences every aspect of the language, from its strong compatibility guarantees to its effect system that tracks side effects and ensures safe concurrent execution.

Verse aims to be:

- **Simple enough** for first-time programmers to learn, with consistent rules and minimal special cases.

- **Expressive enough** for sophisticated game logic and distributed systems, with advanced features that scale to large codebases.

- **Safe enough** for untrusted code to run in a shared environment, with strong sandboxing and effect tracking.

- **Fast enough** for real-time games and simulations, with an implementation that can optimize pure computations aggressively.

- **Stable enough** to last for decades, with strong backward compatibility guarantees and careful evolution.

**Why Verse?**

Traditional programming languages carry decades of historical baggage and design compromises. Verse starts fresh, learning from the past but not being bound by it. It's designed for a future where:

- Code lives forever in a persistent metaverse
- Millions of developers contribute to a shared codebase
- Programs must be safe, concurrent, and composable by default
- Backward compatibility is not optional but essential
- The boundary between compile-time and runtime is fluid

Ready to dive in? Start with [Built-in Types](12_BOOK_OF_VERSE_I_FOUNDATIONS.md#book-of-verse-source-unit-02primitivesmd) to understand Verse's fundamental data types, or jump to [Expressions](12_BOOK_OF_VERSE_I_FOUNDATIONS.md#book-of-verse-source-unit-01expressionsmd) to see how everything in Verse computes values.

For experienced programmers coming from other languages, the [Failure System](12_BOOK_OF_VERSE_I_FOUNDATIONS.md#book-of-verse-source-unit-08failuremd) and [Effects](14_BOOK_OF_VERSE_III_EFFECTS_CONCURRENCY_AND_EVOLUTION.md#book-of-verse-source-unit-13effectsmd) sections highlight some of Verse's distinctive features.

#### Key Features

**Everything is an Expression**

In Verse, there are no statements—everything is an expression that produces a value. This creates a composable system where any piece of code can be used anywhere a value is expected.

<!--versetest
Condition()<computes><decides> :void= {}
Array :[]int= array{1}
-->
<!-- 01 -->
```verse
# Even control flow produces values
Result := if (Condition[]) then "yes" else "no"

# Loops are expressions
Multiply := for (X : Array) { X * 42 }
```

**Failure as Control Flow**

Instead of boolean conditions and exceptions, Verse uses failure as a primary control flow mechanism. Expressions can succeed (producing a value) or fail (producing no value), enabling natural control flow patterns:

<!--versetest
ValidateInput(x:string)<computes><decides>:void= {}
ProcessData(x:string)<computes>:void= {}
myclass := class{
Data:string="hi"
M()<decides>:void=
    ValidateInput[Data] # Square brackets indicate that this function may fail
    ProcessData(Data)   # Data is only processed if valid, parentheses mean must succeed
}
<#
-->
<!-- 02 -->
```verse
ValidateInput[Data] # Square brackets indicate that this function may fail
ProcessData(Data)   # Data is only processed if valid, parentheses mean must succeed
```
<!-- #> -->

The [Failure](12_BOOK_OF_VERSE_I_FOUNDATIONS.md#book-of-verse-source-unit-08failuremd) chapter covers failable expressions and failure contexts in depth, and [Control Flow](12_BOOK_OF_VERSE_I_FOUNDATIONS.md#book-of-verse-source-unit-07controlmd) explains if expressions.

**Strong Static Typing with Inference**

Verse features a powerful type system that catches errors at compile time while minimizing the need for type annotations through inference. See [Types](13_BOOK_OF_VERSE_II_PROGRAM_STRUCTURE_AND_TYPES.md#book-of-verse-source-unit-11typesmd) for more on the type system and subtyping.

<!--versetest-->
<!-- 03 -->
```verse
X := 42                    # Type inferred 
Name := "Verse"            # Type inferred
```

**Effect Tracking**

Functions declare their side effects through specifiers like `<computes>`, `<reads>`, `<writes>`, `<transacts>`, `<decides>`, and `<suspends>`. These effect specifiers make it immediately clear what a function can do beyond computing its return value:

<!--versetest
x := class:
    GetCurrentValue()<reads>:int=1
    var Score:int=0
    PureCompute()<computes>:int = 2 + 2            # No side effects
    ReadState()<reads>:int = GetCurrentValue()     # Can read mutable state
    UpdateGame()<transacts>:void = set Score += 10 # Can read, write, allocate
<#
-->
<!-- 04 -->
```verse
PureCompute()<computes>:int = 2 + 2            # No side effects
ReadState()<reads>:int = GetCurrentValue()     # Can read mutable state
UpdateGame()<transacts>:void = set Score += 10 # Can read, write, allocate
```
<!-- #> -->

The [Effects](14_BOOK_OF_VERSE_III_EFFECTS_CONCURRENCY_AND_EVOLUTION.md#book-of-verse-source-unit-13effectsmd) chapter provides complete details on the effect system.

**Built-in Concurrency**

Concurrency is a first-class feature with structured concurrency primitives that make concurrent programming safe and predictable.

<!--versetest
TaskA()<suspends>:void={}
TaskB()<suspends>:void={}
TaskC():void={}
FastPath()<suspends>:void={}
SlowButReliablePath()<suspends>:void={}
M()<suspends>:void=
    # Run tasks concurrently and wait for all
    sync:
        TaskA()
        TaskB()
        TaskC()

    # Race tasks and take first result
    race:
        FastPath()
        SlowButReliablePath()
<#
-->
<!-- 05 -->
```verse
# Run tasks concurrently and wait for all
sync:
    TaskA()
    TaskB()
    TaskC()

# Race tasks and take first result
race:
    FastPath()
    SlowButReliablePath()
```
<!-- #> -->

**Speculative Execution**

Verse can speculatively execute code and roll back changes if the execution fails, enabling flexible patterns for validation and error handling.

<!--versetest
TryComplexOperation()<computes><decides>:void={}
-->
<!-- 06 -->
```verse
if (TryComplexOperation[]):
    # Changes performed by TryComplexOperation[] are committed
else:
    # Changes are rolled back automatically
```

**Reactive Programming with Live Variables**

> **Unreleased / language-main warning:** The following explanation and code are preserved from Book of Verse. Do not present `var live`, `when`, or dependent reactive constructs as executable UEFN code unless the current Epic Verse Language Reference, release notes, and active compiler explicitly confirm support.

Verse provides first-class support for reactive programming through live variables that automatically recompute when their dependencies change, reducing the need for manual event handling.

<!--NoCompile-->
<!--versetest
Log(:string)<transacts>:void={}
-->
<!-- 07 -->
```verse
var MaxHealth:int = 100
var Damage:int = 0
var live Health:int = MaxHealth - Damage

# Health automatically updates when dependencies change
set Damage = 20      # Health becomes 80
set MaxHealth = 150  # Health becomes 130

# Reactive constructs for event handling
when(Health < 25):
    Log("Low health warning!")
```

Verse provides a foundation for building interactive experiences in persistent virtual environments.

#### An Example

The following example demonstrates key language features by building an inventory management system for a game, showing how Verse's constructs create robust, maintainable code.

<!--versetest
### Define item rarity as an enumeration - showing Verse's type system
item_rarity := enum<persistable>:
    common
    uncommon
    rare
    epic
    legendary

### Struct for immutable item data - functional programming style
item_stats := struct<persistable>:
    Damage:float = 0.0
    Defense:float = 0.0
    Weight:float = 1.0
    Value:int = 0

### Class for game items - object-oriented features with functional constraints
game_item := class<final><persistable>:
    Name:string
    Rarity:item_rarity = item_rarity.common
    Stats:item_stats = item_stats{}
    StackSize:int = 1

    # Method with decides effect - can fail
    GetRarityMultiplier()<computes><decides>:float =
        case(Rarity):
            item_rarity.common => 1.0
            item_rarity.uncommon => 1.5
            item_rarity.rare => 2.0
            item_rarity.epic => 3.0
            _ => {false?; 0.0}  # Fails if the item is legendary or unexpected

    # Computed property using closed-world function
    GetEffectiveValue()<reads><decides>:int=
        Floor[Stats.Value * GetRarityMultiplier[]]

### Inventory system with state management and effects
inventory_system := class:
    var Items:[]game_item = array{}
    var MaxWeight:float = 100.0
    var Gold:int = 1000

    # Method demonstrating failure handling and transactional semantics
    AddItem(NewItem:game_item)<transacts><decides>:void =
        # Calculate new weight - speculative execution
        CurrentWeight := GetTotalWeight()
        NewWeight := CurrentWeight + NewItem.Stats.Weight

        # This check might fail, rolling back any changes
        NewWeight <= MaxWeight

        # Only executes if weight check passes
        set Items += array{NewItem}
        Print("Added {NewItem.Name} to inventory")

    # Method with query operator and failure propagation
    RemoveItem(ItemName:string)<transacts><decides>:game_item =
        var RemovedItem:?game_item = false
        var NewItems:[]game_item = array{}

        for (Item : Items):
            if (Item.Name = ItemName, not RemovedItem?):
                set RemovedItem = option{Item}
            else:
                set NewItems += array{Item}
        set Items = NewItems
        RemovedItem?  # Fails if item not found

    # Purchase with complex failure logic and rollback
    PurchaseItem(ShopItem:game_item)<transacts><decides>:void =
        # Multiple failure points - any failure rolls back all changes
        Price := ShopItem.GetEffectiveValue[]
        Price <= Gold  # Fails if not enough gold

        # Tentatively deduct gold
        set Gold = Gold - Price

        # Try to add item - might fail due to weight
        AddItem[ShopItem]

        # All succeeded - changes are committed
        Print("Purchased {ShopItem.Name} for {Price} gold")

    # Higher-order function with type parameters and where clauses
    FilterItems(Predicate:type{_(:game_item)<computes><decides>:void})<reads><decides>:[]game_item =
        for (Item : Items, Predicate[Item]):
            Item

    GetTotalWeight()<transacts>:float =
        var Total:float = 0.0
        for (Item : Items):
            set Total += Item.Stats.Weight
        Total

### Player class using composition
player_character := class:
    Name:string
    var Level:int = 1
    var Experience:int = 0
    var Inventory:inventory_system = inventory_system{}

    LevelUpThreshold:int = 100

    GainExperience(Amount:int)<transacts>:void =
        set Experience += Amount

        # Automatic level up check with failure handling
        loop:
            RequiredXP := LevelUpThreshold * Level
            if (Experience >= RequiredXP):
                set Experience -= RequiredXP
                set Level += 1
                Print("{Name} leveled up to {Level}!")
            else:
                break

    # Method showing qualified access
    EquipStarterGear()<transacts><decides>:void =
        StarterSword := game_item{
            Name := "Rusty Sword"
            Rarity := item_rarity.common
            Stats := item_stats{Damage := 10.0, Weight := 5.0, Value := 50}
        }
        # These might fail if inventory is full
        Inventory.AddItem[StarterSword]

### Example usage demonstrating control flow and failure handling
assert:
    # Create a player (can't fail)
    Hero := player_character{Name := "Verse Hero"}

    # Try to equip starter gear (might fail)
    if (Hero.EquipStarterGear[]):
        Print("Hero equipped with starter gear")

    # Demonstrate transactional behavior
    ExpensiveItem := game_item{
        Name := "Golden Crown"
        Rarity := item_rarity.epic
        Stats := item_stats{Value := 2000, Weight := 90.0}  # Very heavy!
    }

    # This might fail due to weight or insufficient gold
    if (Hero.Inventory.PurchaseItem[ExpensiveItem]):
        Print("Purchase successful!")
    else:
        Print("Purchase failed - gold remains at {Hero.Inventory.Gold}")

    # Use higher-order functions with nested function predicate
    IsRareOrLegendary(I:game_item)<computes><decides>:void =
        I.Rarity = item_rarity.rare or I.Rarity = item_rarity.legendary

    RareItems := Hero.Inventory.FilterItems[IsRareOrLegendary]

    Print("Found {RareItems.Length} rare items")
<#
-->
<!-- 08 -->
```verse
# Module declaration - start by importing utility functions
using { /Verse.org/VerseCLR }

# Define item rarity as an enumeration - showing Verse's type system
item_rarity := enum<persistable>:
    common
    uncommon
    rare
    epic
    legendary

# Struct for immutable item data - functional programming style
item_stats := struct<persistable>:
    Damage:float = 0.0
    Defense:float = 0.0
    Weight:float = 1.0
    Value:int = 0

# Class for game items - object-oriented features with functional constraints
game_item := class<final><persistable>:
    Name:string
    Rarity:item_rarity = item_rarity.common
    Stats:item_stats = item_stats{}
    StackSize:int = 1

    # Method with decides effect - can fail
    GetRarityMultiplier()<decides>:float =
        case(Rarity):
            item_rarity.common => 1.0
            item_rarity.uncommon => 1.5
            item_rarity.rare => 2.0
            item_rarity.epic => 3.0
            _ => {false?; 0.0}  # Fails if the item is legendary or unexpected

    # Computed property using closed-world function
    GetEffectiveValue()<reads><decides>:int=
        Floor[Stats.Value * GetRarityMultiplier[]]

# Inventory system with state management and effects
inventory_system := class:
    var Items:[]game_item = array{}
    var MaxWeight:float = 100.0
    var Gold:int = 1000

    # Method demonstrating failure handling and transactional semantics
    AddItem(NewItem:game_item)<transacts><decides>:void =
        # Calculate new weight - speculative execution
        CurrentWeight := GetTotalWeight()
        NewWeight := CurrentWeight + NewItem.Stats.Weight

        # This check might fail, rolling back any changes
        NewWeight <= MaxWeight

        # Only executes if weight check passes
        set Items += array{NewItem}
        Print("Added {NewItem.Name} to inventory")

    # Method with query operator and failure propagation
    RemoveItem(ItemName:string)<transacts><decides>:game_item =
        var RemovedItem:?game_item = false
        var NewItems:[]game_item = array{}

        for (Item : Items):
            if (Item.Name = ItemName, not RemovedItem?):
                set RemovedItem = option{Item}
            else:
                set NewItems += array{Item}
        set Items = NewItems
        RemovedItem?  # Fails if item not found

    # Purchase with complex failure logic and rollback
    PurchaseItem(ShopItem:game_item)<transacts><decides>:void =
        # Multiple failure points - any failure rolls back all changes
        Price := ShopItem.GetEffectiveValue[]
        Price <= Gold  # Fails if not enough gold

        # Tentatively deduct gold
        set Gold = Gold - Price

        # Try to add item - might fail due to weight
        AddItem[ShopItem]

        # All succeeded - changes are committed
        Print("Purchased {ShopItem.Name} for {Price} gold")

    # Higher-order function with type parameters and where clauses
    FilterItems(Predicate:type{_(:game_item)<computes><decides>:void})<reads><decides>:[]game_item =
        for (Item : Items, Predicate[Item]):
            Item

    GetTotalWeight()<transacts>:float =
        var Total:float = 0.0
        for (Item : Items):
            set Total += Item.Stats.Weight
        Total

# Player class using composition
player_character<public> := class:
    Name<public>:string
    var Level:int = 1
    var Experience:int = 0
    var Inventory:inventory_system = inventory_system{}

    LevelUpThreshold := 100

    GainExperience(Amount:int)<transacts>:void =
        set Experience += Amount

        # Automatic level up check with failure handling
        loop:
            RequiredXP := LevelUpThreshold * Level
            if (Experience >= RequiredXP):
                set Experience -= RequiredXP
                set Level += 1
                Print("{Name} leveled up to {Level}!")
            else:
                break

    # Method showing qualified access
    EquipStarterGear()<transacts><decides>:void =
        StarterSword := game_item{
            Name := "Rusty Sword"
            Rarity := item_rarity.common
            Stats := item_stats{Damage := 10.0, Weight := 5.0, Value := 50}
        }
        # These might fail if inventory is full
        Inventory.AddItem[StarterSword]

# Example usage demonstrating control flow and failure handling
RunExample<public>()<suspends>:void =
    # Create a player (can't fail)
    Hero := player_character{Name := "Verse Hero"}

    # Try to equip starter gear (might fail)
    if (Hero.EquipStarterGear[]):
        Print("Hero equipped with starter gear")

    # Demonstrate transactional behavior
    ExpensiveItem := game_item{
        Name := "Golden Crown"
        Rarity := item_rarity.epic
        Stats := item_stats{Value := 2000, Weight := 90.0}  # Very heavy!
    }

    # This might fail due to weight or insufficient gold
    if (Hero.Inventory.PurchaseItem[ExpensiveItem]):
        Print("Purchase successful!")
    else:
        Print("Purchase failed - gold remains at {Hero.Inventory.Gold}")

    # Use higher-order functions with nested function predicate
    IsRareOrLegendary(I:game_item)<computes><decides>:void =
        I.Rarity = item_rarity.rare or I.Rarity = item_rarity.legendary

    RareItems := Hero.Inventory.FilterItems[IsRareOrLegendary]

    Print("Found {RareItems.Length} rare items")
```
<!-- #> -->

This example demonstrates Verse in a practical context. Let's explore what makes this code uniquely Verse:

**Type System and Data Modeling**

The example begins with Verse's rich type system. Types flow naturally through the code; many type annotations are omitted as they can be inferred. When we do specify types, like `Items:[]game_item`, they document intent rather than just satisfy the compiler. The `item_rarity` enum provides type-safe constants without the boilerplate of traditional enumerations. The `item_stats` struct marked as `<persistable>` can be saved and loaded from persistent storage, essential for game saves. The `game_item` class is marked `<final>` and `<persistable>` so its instances can be saved and restored; because persistable data is serialized by value, such classes cannot also be `<unique>`.

**Failure as Control Flow**

Throughout the code, failure drives control flow rather than exceptions or error codes. The `<decides>` effect marks functions that can fail, and failure propagates naturally through expressions. When `GetRarityMultiplier()` encounters an unknown rarity, it does not throw an exception or return a sentinel value - it simply fails, and the calling code handles this gracefully.
The `AddItem` method demonstrates how failure creates declarative validation. The expression `NewWeight <= MaxWeight` either succeeds (allowing execution to continue) or fails (preventing the item from being added). There's no explicit control flow - just a declarative assertion of what must be true.

**Transactional Semantics and Speculative Execution**

Methods marked with `<transacts>` provide automatic rollback on failure. In `PurchaseItem`, we deduct gold from the player, then try to add the item. If adding fails (perhaps due to weight limits), the gold deduction is automatically rolled back. This eliminates entire categories of bugs related to partial state updates.
This transactional behavior extends to complex operations. When multiple changes need to succeed or fail together, Verse ensures consistency without need for manual clean up.

**Functions as First-Class Values**

The `FilterItems` method accepts a predicate function, demonstrating higher-order programming. The nested function `IsRareOrLegendary` in `RunExample` shows how functions can be defined locally and passed around like any other value. This functional programming style combines naturally with the imperative and object-oriented features.

**Optional Types and Query Operators**

The inventory removal logic uses optional types (`?game_item`) to represent values that might not exist. The query operator `?` extracts values from options, failing if the option is empty. This eliminates null pointer exceptions while providing convenient syntax for handling absent values.

**Pattern Matching and Control Flow**

The `case` expression in `GetRarityMultiplier` demonstrates pattern matching. Unlike a switch statement, `case` is an expression that produces a value. The underscore `_` provides a catch-all pattern, though in this example it leads to failure.
The `if` expression similarly produces values and can bind variables in its condition. The compound conditions show how multiple operations can be chained with automatic failure propagation.

**Module System and Access Control**

The code begins with `using` statements that import functionality from other modules. The path-based module system ensures that dependencies are unambiguous and permanently addressable. Access specifiers like `<public>` control visibility at a fine-grained level.

**Immutable by Default**

Data structures are immutable unless explicitly marked with `var`. This eliminates large classes of bugs and makes concurrent programming safer. When we do need mutation, it is explicit and tracked by the effect system. See [Mutability](12_BOOK_OF_VERSE_I_FOUNDATIONS.md#book-of-verse-source-unit-05mutabilitymd) for complete details on `var` and `set`.

#### Naming Conventions

Verse has a set of naming conventions that make code readable and predictable. While the language does not enforce these conventions, following them ensures your code integrates well with the broader Verse ecosystem and is immediately familiar to other Verse developers.

Identifiers should be in PascalCase (CamelCase starting with uppercase):

<!--versetest
player_record := struct:
    Name:string

PlayerDatabase(Id:int)<decides>:player_record =
    if (Id = 0):
        player_record{Name := "Alice"}
    else if (Id = 1):
        player_record{Name := "Bob"}
    else:
        false?
        player_record{Name := ""}
-->
<!-- 09 -->
```verse
# Variables and constants use PascalCase
PlayerHealth:int = 100
MaxInventorySize:int = 50
IsGameActive:logic = true

# Functions use PascalCase
CalculateDamage(Base:float, Multiplier:float):float =
    Base * Multiplier

GetPlayerName(Id:int)<decides>:string =
    PlayerDatabase[Id].Name

# Classes and structs use snake_case
player_character := class:
    Name:string
    Level:int

inventory_item := struct:
    ItemId:int
    Quantity:int

# Enums and their values use snake_case
game_state := enum:
    main_menu
    in_game
    paused
    game_over
```

Generic type parameters use single lowercase letters or short descriptive names:

<!--versetest-->
<!-- 10 -->
```verse
# Single letter for simple generics
Find(Array:[]t, Target:t where t:type):?int = false

# Descriptive names for complex relationships
Transform(Input:in_t, Processor:type{_(:in_t):out_t} where in_t:type, out_t:type):?out_t = false
```


Module names always use PascalCase, including path segments:

<!--NoCompile-->
<!-- 11 -->
```verse
# Module definition
InventorySystem := module:
    # Module contents

# Path segments also use PascalCase
using { /Fortnite.com/Characters/PlayerController }
using { /MyGame.com/Systems/CombatSystem }
using { /Verse.org/Random }
```

Class and struct fields use PascalCase, and methods follow the same PascalCase convention as functions:

<!--versetest-->
<!-- 12 -->
```verse
player := class:
    Name:string          # PascalCase for fields
    var Health:float= 0.0

    # Methods use PascalCase like functions
    TakeDamage(Amount:float):void =
        set Health = Max(0.0, Health - Amount)

    IsAlive():logic =
        logic{Health > 0.0}
```

#### Code Formatting

Verse code follows consistent formatting patterns to emphasize readability.

Use four spaces to indent code blocks. The colon introduces a block, with subsequent lines indented:

<!--versetest
Condition()<decides><transacts>:void = {}
DoSomething()<transacts>:void = {}
DoSomethingElse()<transacts>:void = {}
Inventory:[]int = array{1, 2, 3}
ProcessItem(Item:int)<transacts>:void = {}
UpdateDisplay()<transacts>:void = {}
ImplementationHere()<transacts>:void = {}

-->
<!-- 13 -->
```verse
if (Condition[]):
    DoSomething()
    DoSomethingElse()

for (Item : Inventory):
    ProcessItem(Item)
    UpdateDisplay()

class_definition := class:
    Field1:int
    Field2:string

    Method():void =
        ImplementationHere()
```

Complex expressions benefit from clear formatting that shows structure:

<!--versetest
player_type := struct{Health:int = 75}
BaseDamage:float = 100.0
LevelMultiplier:float = 1.5
BonusPercentage:float = 10.0
rarity_type := enum{common; uncommon; rare; epic; legendary}
-->
<!-- 14 -->
```verse
Player:player_type = player_type{}
Rarity:rarity_type = rarity_type.rare

# Multi-line conditionals
Result := if (Player.Health > 50):
    "healthy"
else if (Player.Health > 20):
    "injured"
else:
    "critical"

# Chained operations with clear precedence
FinalDamage :=
    BaseDamage *
    LevelMultiplier *
    (1.0 + BonusPercentage / 100.0)

# Pattern matching with aligned cases
DamageMultiplier := case(Rarity):
    rarity_type.common => 1.0
    rarity_type.uncommon => 1.5
    rarity_type.rare => 2.0
    rarity_type.epic => 3.0
    rarity_type.legendary => 5.0
```

Functions follow a consistent pattern with effects and return types clearly specified:

<!--versetest
difficulty_level := enum{easy; medium; hard}
ValidateAmount(Amount:int)<transacts><decides>:void = {}
DeductBalance(Amount:int)<transacts>:void = {}
RecordTransaction()<transacts>:void = {}
GetBaseReward(Difficulty:difficulty_level)<decides>:?int = option{100}
CalculateTimeBonus(CompletionTime:float):int = 50
-->
<!-- 15 -->
```verse
# Simple pure function
Add(X:int, Y:int)<computes>:int = X + Y

# Function with effects
ProcessTransaction(Amount:int)<transacts><decides>:void =
    ValidateAmount[Amount]
    DeductBalance(Amount)
    RecordTransaction()

# Multi-line function with clear structure
CalculateReward(
    PlayerLevel:int,
    Difficulty:difficulty_level,
    CompletionTime:float
)<decides>:int =
    BaseReward := GetBaseReward[Difficulty]?
    LevelBonus := PlayerLevel * 10
    TimeBonus := CalculateTimeBonus(CompletionTime)
    BaseReward + LevelBonus + TimeBonus
```

#### Comments

Comments are ignored during execution but help with understanding and maintaining code. Verse offers several styles of comments to suit different documentation needs. The simplest is the single-line comment, which begins with `#` and continues to the end of the line:

<!--versetest-->
<!-- 16 -->
```verse
CalculateDamage := 100 * 1.5   # Apply critical hit multiplier
```

When you need to document something within a line of code without breaking it up, inline block comments provide the perfect solution. These are enclosed between `<#` and `#>`:

<!--versetest
BaseValue:int = 100
Multiplier:int = 2
Bonus:int = 10
-->
<!-- 17 -->
```verse
Result := BaseValue <# original amount #> * Multiplier <# scaling factor #> + Bonus
```

The same can be used to write multi-line block comments, making them ideal for explaining complex algorithms or providing detailed context:

<!--versetest-->
<!-- 18 -->
```verse
<# This function implements the quadratic damage falloff formula
   used throughout the game. The falloff ensures that damage
   decreases smoothly with distance, creating strategic positioning
   choices for players. #>
CalculateFalloffDamage(Distance:float, MaxDamage:float):float =
    MaxDamage  # Implementation here
```

Block comments nest, which allows you to temporarily disable code that already contains comments without having to remove or modify existing documentation:

<!--versetest-->
<!-- 19 -->
```verse
<# Temporarily disabled for testing
   OriginalFunction()  <# This had a bug #>
   NewFunction()       # Testing this approach
#>
```

Indented comments begin with a `<#>` on its own line; everything indented by four spaces on subsequent lines becomes part of the comment:

<!--versetest
DoSomething():void = {}
-->
<!-- 20 -->
```verse
<#>
    This entire block is a comment because it is indented.
    It provides a clean way to write longer documentation
    without cluttering each line with comment markers.

DoSomething()  # Not part of the comment.
```

#### Syntactic Styles

Verse offers flexible syntax to accommodate different programming styles. The same logic can be expressed using braces, indentation, or inline forms, allowing you to choose the clearest representation for each context.

The braced style uses curly braces to delimit blocks, familiar from C-family languages:

<!--versetest
Score:int = 85
-->
<!-- 21 -->
```verse
Result := if (Score > 90) {
    "excellent"
} else if (Score > 70) {
    "good"
} else {
    "needs improvement"
}
```

The indented style uses colons and indentation to define structure, similar to Python:

<!--versetest
Score:int = 85
-->
<!-- 22 -->
```verse
Result := if (Score > 90):
    "excellent"
else if (Score > 70):
    "good"
else:
    "needs improvement"
```

For simple expressions, the inline style keeps everything on one line:

<!--versetest
Score:int = 85
-->
<!-- 23 -->
```verse
Result := if (Score > 90) then "excellent" else if (Score > 70) then "good" else "needs improvement"
```

The dotted style uses a period to introduce the expression:

<!--versetest
Score:int = 85
-->
<!-- 24 -->
```verse
Result := if (Score > 90). "excellent" else if (Score > 70). "good" else. "needs improvement"
```

You can even mix styles when it makes sense:

<!--versetest
ComplexCondition()<transacts><decides>:void = {}
AnotherCheck()<transacts><decides>:void = {}
YetAnotherValidation()<transacts><decides>:void = {}
-->
<!-- 25 -->
```verse
Result := if:
    ComplexCondition[] and
    AnotherCheck[] and
    YetAnotherValidation[]
then { "condition met" } else { "condition not met" }
```

All these forms produce the same result. The choice between them is about readability and context. 
Use braces when working with existing brace-heavy code, indentation for cleaner vertical layouts,
and inline forms for simple expressions. This flexibility lets you write code that reads naturally.

## Book of Verse Source Unit: 01_expressions.md

### Expressions

Everything is an expression. This design principle sets Verse apart
from many other languages where statements and expressions are
distinct concepts. Every piece of code you write produces a value,
even constructs you might expect to be purely side-effecting. This
creates a programming model where code can be composed and combined in
ways that feel natural and predictable.

#### Primary Expressions

Everything starts with primary expressions—the atomic units from which
more complex expressions are built. These include literals,
identifiers, parenthesized expressions, and the tuple construct that
provides lightweight data aggregation.

##### Basic Values

Literals are source code representations of constant values.
Verse provides literals for all its primitive types: integers, floats, characters,
strings, booleans, and functions. Each type has its own literal syntax and rules
governing valid values and their interpretation at compile time.

<!--versetest
point := struct{X:float, Y:float}
Condition:logic = true
-->
<!-- 01 -->
```verse
Result := if (Condition?) then 42 else 3.14  # Integer and float literals
array{1, 2, 3}                               # Integer literals in array construction
point{X:=0.0, Y:=1.0}                        # Float literals in object construction
```

###### Integer Literals

Integer literals represent whole numbers and can be written in two formats:

*Decimal notation* uses standard digits:

<!--versetest-->
<!-- 02 -->
```verse
Count := 42
Negative := -17
Zero := 0
Large := 9223372036854775807                # Maximum 64-bit signed integer literals
```

*Hexadecimal notation* uses the `0x` prefix followed by hex digits
(0-9, a-f, A-F):

<!--versetest-->
<!-- 03 -->
```verse
Byte := 0xFF
Address := 0x1F4A
LowercaseHex := 0xabcdef
UppercaseHex := 0xABCDEF
```

**Literal Limits vs Runtime Behavior:**

Integer literals must fit within a 64-bit signed integer range
(`-9223372036854775808` to `9223372036854775807`). This is a compile-time
restriction on what values you can write directly in your code.

At runtime, integer values use arbitrary precision arithmetic and can grow
beyond 64-bit limits through computation. However, integers exceeding 64-bit
range have limited support (e.g., cannot be used in string interpolation
or persisted).

###### Float Literals

Floating-point literals represent decimal numbers, they must include a
decimal point and in some cases the `f64` suffix.

<!--versetest-->
<!-- 04 -->
```verse
Pi := 3.14159
Half := 0.5
Explicit := 12.34f64    # Explicit bit-depth suffix
```

Scientific notation expresses very large or small numbers using exponents:

<!--versetest-->
<!-- 05 -->
```verse
Large := 1.0e10         # 10,000,000,000 (sign optional)
Small := 1.0e-5         # 0.00001
WithSign := 2.5e+3      # 2,500 (explicit + sign)
Compact := 1.5e2        # 150 (no sign defaults to +)
```

Float literals must include a decimal point (`1.0` is valid, but `1` is an integer). A final decimal point without digits is invalid (`1.` is a syntax error). All floats are 64-bit (IEEE 754 double precision); the `f64` suffix is optional. Unary operators work as with integers: `-1.0`, `+1.0`.

**Overflow and Underflow Behavior:**

Float literals outside the IEEE 754 double-precision range produce
**compile-time errors**:

<!--versetest-->
<!-- 06 -->
```verse
#TooBig := 1.7976931348623159e+308    # Compile error: literal overflow
Maximum := 1.7976931348623158e+308    # OK: Maximum finite float
```

However, **runtime** float arithmetic follows standard IEEE 754 semantics:

<!--versetest-->
<!-- 666 -->
```verse
# Runtime overflow produces infinity
Large := 1.0e308
Overflow := Large * 10.0    # Overflow produces infinity

# Division by zero produces infinity
PosInf := 1.0 / 0.0
NegInf := -1.0 / 0.0

# Underflow produces denormalized numbers or zero
Small := 1.0e-320
Smaller := Small / 1.0e10   # Underflows gracefully
```

Float operations follow IEEE 754 semantics. Operations that would
produce NaN (like `0.0 / 0.0`, `Inf - Inf`, or `Sqrt(-1.0)`) return
NaN values rather than failing. NaN propagates through arithmetic
operations.

###### Character Literals

Character literals represent individual text units. Verse has two character types with different literal syntax:

`char` literals represent UTF-8 code units (single bytes, 0-255):

<!--versetest-->
<!-- 07 -->
```verse
LetterA := 'a'          # Printable ASCII character
Space := ' '
Tab := '\t'             # Escape sequence
LetterA := 0o61         # Hexadecimal notation: 0oXX (97 decimal = 'a')
```

`char32` literals represent Unicode code points:

<!--versetest-->
<!-- 08 -->
```verse
Emoji := '😀'           # Non-ASCII automatically char32
Accented := 'é'
ChineseChar := '好'
HexUnicode := 0u1f600   # Hex notation: 0uXXXXX (😀)
```

Type inference from literals:

- ASCII characters (`U+0000` to `U+007F`): `'a'` has type `char`
- Non-ASCII characters: `'😀'` has type `char32`
- No implicit conversion between `char` and `char32`

Escape sequences work in both `char` and strings:

| Escape | Meaning | Codepoint |
|--------|---------|-----------|
| `\t`   | Tab     | U+0009 |
| `\n`   | Newline | U+000A |
| `\r`   | Carriage return | U+000D |
| `\"`   | Double quote | U+0022 |
| `\'`   | Single quote | U+0027 |
| `\\`   | Backslash | U+005C |
| `\{`   | Left brace (string interpolation) | U+007B |
| `\}`   | Right brace (string interpolation) | U+007D |
| `\<`   | Less than | U+003C |
| `\>`   | Greater than | U+003E |
| `\&`   | Ampersand | U+0026 |
| `\#`   | Hash      | U+0023 |
| `\~`   | Tilde     | U+007E |

Numeric character notation works as follows:

- `0oXX` for `char` (hexadecimal notation, `0o00` to `0oFF` for values 0-255)
- `0uXXXXXX` for `char32` (hexadecimal notation, `0u000000` to `0u10ffff`)

Character literals cannot be empty or contain multiple characters.

###### String Literals

String literals represent text sequences and support interpolation for embedding expressions. Basic strings use double quotes:

<!--versetest-->
<!-- 09 -->
```verse
Greeting := "Hello, World!"
Empty := ""
WithEscapes := "Line 1\nLine 2\tTabbed"
```

String interpolation embeds expressions using curly braces:

<!--versetest
Format(D:float, ?Decimals:int):string=""
-->
<!-- 10 -->
```verse
Name := "Alice"
Age := 30

# Simple interpolation
Message := "Hello, {Name}!"                      # "Hello, Alice!"

# Expression interpolation
Info := "Age next year: {Age + 1}"               # "Age next year: 31"

# Function calls
Score := 100
Text := "Score: {ToString(Score)}"               # "Score: 100"

# Function calls with named arguments
Distance := 5.5
Formatted := "Distance: {Format(Distance, ?Decimals:=2)}"
```

Multi-line strings can span multiple lines using interpolation braces for continuation:

<!--versetest-->
<!-- 11 -->
```verse
LongMessage := "This is a multi-line {
}string that continues across {
}multiple lines."
# Result: "This is a multi-line string that continues across multiple lines."

OtherMessage := "Another message{
}    with some empty{
}    spaces."
# Result := "Another message    with some empty    spaces."
```

The compiler ignores empty interpolants:

<!--versetest-->
<!-- 12 -->
```verse
Text1 := "ab{}cd"        # Same as "abcd"
Text2 := "ab{
}cd"                    # Same as "abcd" (newline ignored)
```

Curly braces must be escaped (`"\{ \}"`) to appear as literal characters in strings. The `string` type is an alias for `[]char` (array of UTF-8 code units). Since UTF-8 code units are single bytes, strings are byte sequences rather than Unicode character sequences. For example, `"José".Length` returns `5` (5 code units/bytes, not 4 characters, since é takes 2 code units).

String-array equivalence:

<!--versetest-->
<!-- 13 -->
```verse
Test1 := logic{"abc" = array{'a', 'b', 'c'}}    # True
Test2 := logic{"" = array{}}                    # True
```

The compiler removes comments from strings:

<!--versetest-->
<!-- 14 -->
```verse
Text1 := "abc<#comment#>def"     # Same as "abcdef"
```

###### Boolean Literals

The `logic` type has two literal values:

<!--versetest-->
<!-- 15 -->
```verse
IsReady := true
IsComplete := false
```

Use boolean values with the query operator `?` or in comparisons:

<!--versetest
StartGame():void = {}
ShowResults():void = {}
IsReady:logic = true
IsComplete:logic = false
-->
<!-- 16 -->
```verse
if (IsReady?):
    StartGame()

if (IsComplete = true):
    ShowResults()
```


The `logic{}` expression creates boolean values from failable expressions (see [Failure](12_BOOK_OF_VERSE_I_FOUNDATIONS.md#book-of-verse-source-unit-08failuremd) for details on failable expressions):

<!--versetest
Operation()<computes><decides>:void = {}
Optional:?int = option{1}
X:int = 1
Y:int = 1
-->
<!-- 17 -->
```verse
# Converts <decides> expression to logic value
Success := logic{Operation[]}        # True if succeeds, false if fails
HasValue := logic{Optional?}         # True if optional has value
IsEqual := logic{X = Y}              # True if equal, false otherwise
```

The `logic{}` expression requires at least a superficial possibility of failure. Pure expressions without `<decides>` effect cause errors:

<!--versetest-->
<!-- 18 -->
```verse
# ERROR: logic{0} has no decides effect
# ERROR: logic{} is empty
Valid := logic{false?}               # OK: false? can fail
```

Multiple expressions inside `logic{}` can be separated by semicolons or commas (see [Semicolons vs Commas](#semicolons-vs-commas) for details):

<!--versetest-->
<!-- 19 -->
```verse
Result1 := logic{true?; true?}       # Semicolon separator
Result2 := logic{true?, true?}       # Comma separator
```

###### Path Literals

Path literals identify modules and packages using a hierarchical naming scheme:

<!--NoCompile-->
<!-- 21 -->
```verse
/Verse.org/Verse                    # Standard library path
/YourGame/Player/Inventory          # Custom module path
/user@example.com/MyModule          # Personal namespace
```

Path syntax follows specific rules:

- Starts with `/`
- Contains label (alphanumeric, `.`, `-`)
- Identifiers must start with letter or `_`

The Modules chapter covers path literals in detail.

##### Identifiers and References

Identifiers serve as references to values, whether they are constants,
variables, functions, or types. An identifier consists of:

- **First character:** Letter (A-Z, a-z) or underscore (`_`)
- **Subsequent characters:** Letters, digits (0-9), or underscores
- **Reserved:** Single underscore `_` cannot be used as an identifier

Identifiers are case-sensitive and use only ASCII characters—Unicode
characters are not supported in identifiers.

<!--NoCompile-->
<!-- 22 -->
```verse
int               # Reference to the int type
GetValue          # Reference to a function
Counter           # Reference to a variable
my_class          # Reference to a class
_private          # Leading underscore allowed
variable123       # Digits allowed after first character

# Invalid identifiers:
# 123invalid      # Cannot start with digit
# my-variable     # Hyphen not allowed
# café            # Unicode not supported
# _               # Single underscore is reserved
```

The language does not syntactically distinguish between different kinds
of identifiers (types, functions, variables)—the context determines how
each identifier is used.

##### Parentheses and Grouping

Parentheses serve dual purposes: they group expressions to control
evaluation order, and they create tuple expressions. A parenthesized
expression simply evaluates to the value of its contents, allowing you
to override the default operator precedence or improve readability:

<!--versetest
A:int = 1
B:int = 2
C:int = 3
X:int = 5
Y:int = 10
Positive:string = "positive"
Negative:string = "negative"
-->
<!-- 23 -->
```verse
(A + B) * C       # Group addition before multiplication
if (X > 0 and Y > 0) then Positive else Negative
```

##### Tuples

Tuples provide a way to group two or more values with little
ceremony. The syntax distinguishes between parentheses used for
grouping and those used for tuple construction through the presence of
commas:

<!--versetest
X:int = 5
Y:int = 10
-->
<!-- 24 -->
```verse
(X, Y)            # Two-element tuple
(1, "hello", true) # Mixed-type tuple
```

Tuples can be accessed using function-call syntax with a single integer argument:

<!--versetest-->
<!-- 25 -->
```verse
point := (10, 20)
x := point(0)     # Access first element
y := point(1)     # Access second element
```

Write tuple types as follows:

<!--versetest
GetPoint():tuple(int,int) = (10, 20)
GetData():tuple(int,string,logic) = (42, "hello", true)
<#
-->
<!-- 26 -->
```verse
tuple(int,int)
tuple(int,string,logic)
```
<!-- #> -->

While the compiler accepts single-element tuple types like `tuple(int)`,
there is currently no syntax to construct a single-element tuple value.

#### Postfix Operations

Postfix operations are operations that follow their operand and can be
chained together. This creates a left-to-right reading order that
feels natural and allows for intuitive composition.

##### Member Access

The dot operator provides access to members of objects, modules, and
other structured values. Member access expressions evaluate to the
value of the specified member:

<!--NoCompile-->
<!-- 27 -->
```verse
Player.Health           # Access field
Config.MaxPlayers       # Access nested value
math.Sqrt(16.0)         # Access module function
Point.X                 # Access struct field
```

Member access can be chained, creating paths through nested structures:

<!--versetest
item := class{Name:string = "Sword"}
inventory := class{Items:[]item = array{item{}}}
player_type := class{Inventory:inventory = inventory{}}
game := class{Players:[]player_type = array{player_type{}}}
M()<decides>:void =
    Game:game = game{}
    Game.Players[0].Inventory.Items[0].Name
<#
-->
<!-- 28 -->
```verse
Game.Players[0].Inventory.Items[0].Name
```
<!-- #> -->

##### Computed Access

Square brackets provide computed access to elements, whether for
arrays, maps, or other indexable structures. Verse evaluates the expression within
brackets to determine which element to access:

<!--versetest
ComputeIndex():int = 0
M()<decides>:void =
    Array:[]int = array{1, 2, 3}
    Map:[string]int = map{"key" => 42}
    Matrix:[][]int = array{array{1, 2}, array{3, 4}}
    Row:int = 0
    Col:int = 1
    Data:[]int = array{10, 20, 30}
    Array[0]
    Map["key"]
    Matrix[Row][Col]
    Data[ComputeIndex()]
<#
-->
<!-- 29 -->
```verse
Array[0]                # Array indexing
Map["key"]              # Map lookup
Matrix[Row][Col]        # Nested indexing
Data[ComputeIndex()]    # Dynamic index computation
```
<!-- #> -->

The square bracket syntax `Func[]` is **required** for calling
functions that may fail (those with the `<decides>` effect). Use regular
parentheses `Func()` for functions that always succeed. Array
indexing also uses `[]` because it can fail when the index is out of bounds.

```verse
GetValue()<decides>:int = ...
GetData():int = ...

# Must use [] for functions that may fail
if (X := GetValue[]):
    Print("Got: {X}")

# Must use () for functions that always succeed
Y := GetData()

# ERROR: Cannot use () for failable functions
# Z := GetValue()  # Compile error!
```

##### Function Calls

Function calls use parentheses with comma-separated arguments. The
language treats function calls as expressions that evaluate to the
function's return value:

<!--versetest
Sqrt(X:int):float = 4.0
MaxOf(A:int, B:int):int = if (A > B) then A else B
Initialize():void = {}
GetData():int = 42
Transform():int = 10
Process(X:int, Y:int)<decides>:void = {}
M()<decides>:void =
    A:int = 5
    B:int = 10
    Sqrt(16)
    MaxOf(A, B)
    Initialize()
    Process[GetData(), Transform()]
<#
-->
<!-- 30 -->
```verse
Sqrt(16)                        # Single argument
MaxOf(A, B)                     # Multiple arguments
Initialize()                    # No arguments
Process[GetData(), Transform()] # Nested calls, outer call may fail
```
<!-- #> -->

#### Object Construction

Object construction uses a distinctive brace syntax to indicates the
creation of a new instance. The syntax requires explicit field
initialization using the `:=` operator:

<!--versetest
point := struct{ X:int, Y:int }
player := struct{Name:string, Level:int, Health:int}
config := struct { MaxPlayers:int, Difficulty:string, EnablePvP:logic }
-->
<!-- 31 -->
```verse
point{X:=10, Y:=20}
player{Name:="Hero", Level:=1, Health:=100}
config{
    MaxPlayers := 16,
    EnablePvP := true,
    Difficulty := "normal"
}
```

The use of `:=` for field initialization reinforces that these are
binding operations—you're binding values to fields at construction
time. Object constructors can be nested, creating complex
initialization expressions:

<!--versetest
point:=struct{ X:int, Y:int}
inventory:=struct{Capacity:int}
player:=struct{ Position:point, Inventory:inventory}
config:=struct{Difficulty:string}
game_state:=struct{Player:player, Settings:config}
-->
<!-- 32 -->
```verse
Game := game_state{
    Player := player{
        Position := point{X:=0, Y:=0},
        Inventory := inventory{Capacity:=20}
    },
    Settings := config{Difficulty:="hard"}
}
```

#### Control Flow as Expressions

One of Verse's distinctive features is that control flow constructs
are expressions, not statements. This means that if-expressions,
loops, and case expressions all produce values that can be used in
larger expressions.

##### Conditional

The if-then-else construct is an expression that evaluates to one of
two values based on a condition:

<!--versetest
ComputeA():int=1
ComputeB():int=1
X:int = 5
Condition:logic = true
-->
<!-- 33 -->
```verse
Result := if (X > 0) then "positive" else "negative"
Value := if (Condition=true) then ComputeA() else ComputeB()
```

The else clause can be omitted, though this affects the type of the
expression. Verse supports multiple syntactic forms for
if-expressions, including parenthesized conditions and indented
bodies:

<!--versetest
Condition:logic = true
Value1:int = 42
Value2:int = 100
-->
<!-- 34 -->
```verse
# Standard form
if (Condition?) then Value1 else Value2

# Indented form
if:
    Condition?
then:
    Value1
else:
    Value2
```

##### For

For expressions iterate over collections and produce values. The basic
form iterates over elements:

<!--versetest
Process(Item:int):void={}
Collection:[]int = array{1, 2, 3}
-->
<!-- 35 -->
```verse
for (Item : Collection) { Process(Item) }
```

An extended form provides access to both index and item--in the case
of a `Map`, indices are not limited to integers:

<!--versetest
Collection:[]int = array{1, 2, 3}
-->
<!-- 36 -->
```verse
for (Index -> Item : Collection) {
    Print("Item at {Index} is {Item}")
}
```

Since for expressions are themselves expressions, they produce array
values and compose with other expressions. Verse evaluates the body of a for
expression for each successful iteration, and these evaluations determine
the value of the expression as a whole.

##### Loop

Loop expressions provide indefinite iteration, continuing until
explicitly terminated through failure or other control flow:

<!--versetest
GetNext():int=1
Done(Value:int)<computes><decides>:void={}
Process(Value:int):void={}
M():void=
    loop {
        Value := GetNext()
        if (Done[Value]) then break
        Process(Value)
    }
<#
-->
<!-- 37 -->
```verse
loop {
    Value := GetNext()
    if (Done[Value]) then break
    Process(Value)
}
```
<!-- #> -->

The loop construct can use indented syntax for clarity.

A loop expression produces a value of type `true`, regardless of what
expressions appear in its body. This value has no practical use—loops are typically used for their side effects rather than their return value.

```verse
Result := loop:
    ProcessData()
    if (ShouldStop[]):
        break
# Result has type 'true' (and returns `true`)
```

##### Case

Case expressions provide multi-way branching based on value matching:

<!--versetest
color := enum:
    Red
    Yellow
    Green
    Other
Color:color = color.Red
-->
<!-- 38 -->
```verse
Description := case(Color) {
    color.Red => "Danger",
    color.Yellow => "Warning",
    color.Green => "Safe",
    _ => "Unknown"
}
```

The `_` pattern serves as a catch-all, ensuring the case expression is
exhaustive. Case expressions evaluate to the value of the matched
branch, making them useful for value computation as well as control
flow.

#### Binary Operations

Binary expressions follow a carefully designed precedence hierarchy
that balances mathematical conventions with programming practicality.

##### Assignment and Binding

At the lowest precedence level, assignment operators bind values to
identifiers. The `:=` operator creates immutable bindings, while `set
=` performs mutable assignment:

<!--versetest-->
<!-- 39 -->
```verse
X := 42           # Immutable binding
Y := X * 2        # Binding to computed value
Z := W := 10      # Right-associative chaining
```

Assignment operators are right-associative, meaning that `a := b := c`
groups as `a := (b := c)`. This allows for natural chaining of
assignments while maintaining clarity about evaluation order.

Compound assignments provide shorthand for common update patterns:

<!--versetest
F()<transacts>:void=
    var Counter :int = 0
    var Total :int = 0
    Factor:=2
    set Counter += 1
    set Total *= Factor
<#
-->
<!-- 40 -->
```verse
set Counter += 1      # Equivalent to: set Counter = Counter + 1
set Total *= Factor   # Equivalent to: set Total = Total * Factor
```
<!-- #> -->

Compound assignment operators evaluate the left-hand side expression only once, which is observable when the expression has side effects:

<!--versetest
assert:
    var TestArray:[]int = array{10, 20, 30, 40, 50}
    var Index:int = 0
    Inc():int =
        set Index += 1
        Index

    # Compound assignment: Inc() called ONCE
    set TestArray[Inc()] += 1

    # Verify: Index = 1 (Inc called once)
    Index = 1
    # TestArray[1] = 20 + 1 = 21
    TestArray[1] = 21
-->
```verse
var Index:int = 0
Inc():int =
    set Index += 1
    Index

# Compound assignment calls Inc() one
set Array[Inc()] += 1
# Result: Array[1] = Array[1] + 1

# Expanded form would call Inc() twice
# set Array[Inc()] = Array[Inc()] + 1
# Result: Array[1] = Array[2] + 1  (different!)
```

In the compound assignment `set Array[Inc()] += 1`, Verse calls the function `Inc()`
once to determine the index, then reads that location,
increments it, and stores the result back.

##### Range Expressions

The range operator (`..`) creates integer ranges for iteration in
`for` loops. Ranges are **inclusive on both ends** and can only appear
directly in for loop iteration clauses:

<!--versetest
End()<computes>:int=10
Count:int=10
Start:int=1
Process(I:int):void={}
F():void=
    for (I := 1..10):
        for (J := I..(I+10)):
            for (K:= J..End()) {}
<#
-->
<!-- 41 -->
```verse
1..10             # Range from 1 to 10 (inclusive)
Start..End        # Variable-defined range
for (I := 0..Count):  # Must use := syntax, not :
    Process(I)
```
<!-- #> -->

Ranges are not first-class values. They cannot be stored in variables
or used outside of `for` loop iteration clauses. See the [Range
Operator Restrictions](12_BOOK_OF_VERSE_I_FOUNDATIONS.md#for-expressions)
section for details.

##### Logical Operations

Logical operators combine boolean values with short-circuit
evaluation. Their result is either success or failure. Verse uses
keyword operators (`and`, `or`, `not`) rather than symbols, improving
readability:

<!--versetest
ProcessQuadrant()<computes>:void = {}
Validated:logic= true
UseDefault()<computes><decides>:void = {}
IsReady()<computes><decides>:void = {}
Wait()<computes>:void = {}
M()<transacts>:void =
    X:int = 5
    Y:int = 10
    if (X > 0 and Y > 0) then ProcessQuadrant()
    Result := logic{Validated? or UseDefault[]}
    if (not IsReady[]) then Wait()
<#
-->
<!-- 42 -->
```verse
if (X > 0 and Y > 0) then ProcessQuadrant()
Result := logic{Validated? or UseDefault[]}
if (not IsReady[]) then Wait()
```
<!-- #> -->

The precedence ensures that `and` binds tighter than `or`, matching
mathematical logic conventions, the `logic{}` expression turns success
or failure into a value:

<!--NoCompile-->
<!-- 43 -->
```verse
# Evaluates as: (ExpA and ExpB) or (ExpC and ExpD)
Condition := logic{ExpA and ExpB or ExpC and ExpD}
```

**Important:** Variable bindings do not escape from logical operations.
When you use `:=` inside `and`, `or`, or `not` expressions, those
bindings are only evaluated for short-circuit control flow and are **not**
accessible afterward:

<!--NoCompile-->
<!-- 998 -->
```verse
Arr:[]int = array{10, 20}

# ERROR: Bindings in logical operations are NOT accessible
if ((X := Arr[0]) and (Y := Arr[1])):
    # X and Y are not bound here - this will cause a compilation error!
    Z := X + Y

# Simple if binding DOES work
if (X := Arr[0]):
    # OK: X is accessible here
    Y := X + 1
```

##### Comparison Operations

Comparison operators also either succeed or fail and can be chained
for range checking:

<!--versetest
InRange():void={}
Value:int = 50
X:int = 75
Minimum:int = 0
Maximum:int = 100
A:int = 5
B:int = 10
-->
<!-- 44 -->
```verse
if (0 <= Value <= 100) then InRange()
IsValid := logic{X > Minimum and X < Maximum}
Same := logic{A = B}
Different := logic{A <> B}
```

All comparison operators have the same precedence and evaluate
**left-to-right**. Crucially, *comparison operators return their left
operand* when the comparison succeeds, and *comparison chains have special
syntax* that checks all adjacent pairs.

<!--versetest
assert:
    X := 0 < 10
    X = 0  # Returns left operand (0)

    Value:int = 50
    Result := 0 <= Value <= 100
    Result = 0  # Chain returns leftmost operand (0)

    # Chain checks BOTH comparisons
    Value2:int = 75
    not(10 <= Value2 <= 50)  # Fails because 75 > 50
<#
-->
<!-- 999 -->
```verse
X := 0 < 10
# X equals 0 (the left operand)

0 <= Value <= 100
# Special chain syntax that checks BOTH:
#   - 0 <= Value (lower bound)
#   - Value <= 100 (upper bound)
# Returns 0 (leftmost operand) if both succeed
```
<!-- #> -->

Verse does **not** evaluate the comparison chain `A <= B <= C` as `(A <= B) <= C`.
Instead, it is special syntax that checks both `A <= B` **and** `B <= C`, while
returning the leftmost operand (`A`) on success. This enables natural
mathematical notation for ranges without requiring `and` operators.

##### Arithmetic Operations

Arithmetic operations follow standard mathematical precedence, with
multiplication and division binding tighter than addition and
subtraction:

<!--versetest
A:int = 1
B:int = 2
C:int = 3
-->
<!-- 45 -->
```verse
Result := A + B * C      # Multiplication first
Average := (A + B) / 2   # Parentheses override precedence
```

Integer division by zero fails and has the `<decides>` effect.
When dividing integers, `X / Y` can fail if `Y` is `0`, allowing you to handle
this case safely:

<!--versetest
X:int = 10
Y:int = 0
assert:
    not(Result := X / Y)
-->
<!-- 997 -->
```verse
if (Result := X / Y):
    Print("Division succeeded")
else:
    Print("Cannot divide by zero")
```

Float division by zero does not fail; it returns infinity according to
IEEE 754 floating-point semantics.

Unary operators have the highest precedence among arithmetic operations:

<!--versetest
Flag:logic = true
Value:int = 1
X:int = 1
Y:int = 2
-->
<!-- 46 -->
```verse
Negative := -Value
Inverted := logic{not Flag=true}
Result := -X * Y    # Unary minus applies to x only
```

#### Set Expressions

While Verse emphasizes immutability, practical programming sometimes
requires mutation. Set expressions provide mutation of variables and
fields:

<!--versetest
c := class { var Field:int = 0 }
GetObj()<transacts>:c = c{}
GetArr()<transacts>:[]int = array{1}
GetMap()<transacts>:[string]string = map{ "hi" => "hp" }
Element:int = 5
Value:int = 100
Index:int = 0
Key:string = "key"
MappedValue:string = "value"
assert:
    var X:int = 0
    var Obj:c = GetObj()
    var Arr:[]int = GetArr()
    var Map:[string]string = GetMap()

    set X = 10
    set Obj.Field = Value
    set Arr[Index] = Element
    set Map[Key] = MappedValue
<#
-->
<!-- 47 -->
```verse
set X = 10                    # Variable assignment
set Obj.Field = Value         # Field assignment
set Arr[Index] = Element      # Array element assignment
set Map[Key] = MappedValue    # Map entry assignment
```
<!-- #> -->

Set expressions are themselves expressions that **return the value being
assigned** (the right-hand side). For example, `set Obj.Field = Value`
returns `Value`, not `Obj`. This allows chaining assignments:

```verse
set Y = set X = 5  # Both X and Y become 5
```

Though set expressions have a value, they are typically used for their side
effects. The left-hand side must be a valid LValue—something that can be
assigned to.

Verse supports complex LValues, allowing updates deep within data structures:

<!--versetest
item := class{Name:string = "Item"}
inventory := class{var Items:[]item = array{item{}}}
player := class{var Inventory:inventory = inventory{}}
game := class{var Players:[]player = array{player{}}}
M()<transacts><decides>:void =
    Game:game = game{}
    CurrentPlayer:int = 0
    Slot:int = 0
    NewItem:item = item{}
    set Game.Players[CurrentPlayer].Inventory.Items[Slot] = NewItem
<#
-->
<!-- 48 -->
```verse
set Game.Players[CurrentPlayer].Inventory.Items[Slot] = NewItem
```
<!-- #> -->

#### Semicolons vs Commas

Verse uses semicolons and commas as separators in various contexts,
but they have fundamentally different semantics in most
situations. Understanding when each is appropriate is essential for
writing correct Verse code.

**Semicolons** (within parentheses) create *sequences* - they evaluate expressions in order and return the value of the last expression:

<!--versetest
assert:
    Result := (1; 2; 3)
    Result = 3
-->
<!-- 49 -->
```verse
Result := (1; 2; 3)     # Evaluates 1, then 2, then 3; returns 3
# Note: Parentheses are required
# Result := 1; 2         # ERROR: Not valid without parentheses
```

**Commas** (within parentheses) create *tuples* - they group multiple values into a single composite value:

<!--versetest-->
<!-- 50 -->
```verse
Result := (1, 2, 3)     # Creates a tuple of three elements
# Result = (1, 2, 3) (type: tuple(int, int, int))
# Note: Parentheses are required
# Result := 1, 2         # ERROR: Not valid without parentheses
```

##### Context-Specific Behavior

In expression contexts (like assignments), semicolons and commas require
parentheses to create sequences and tuples. The distinction is clear when
comparing parenthesized expressions:

<!--versetest-->
<!-- 51 -->
```verse
# Semicolon: sequence (returns last value)
X := (0; 1)              # X = 1, type is int

# Comma: tuple (groups values)
Y := (0, 1)              # Y = (0, 1), type is tuple(int, int)
```

This applies to function return values as well:

<!--versetest-->
<!-- 52 -->
```verse
GetInt():int = (1.0; 2)                    # Returns 2 (int)
GetTuple():tuple(float, int) = (1.0, 2)    # Returns (1.0, 2)
```

Semicolons in argument position create a *sequence that executes
before the call*, with only the last value passed as the argument:

<!--versetest
Process(X:int):void={}
LogEvent(S:string):int=1
-->
<!-- 53 -->
```verse
# Semicolon executes side effects, then passes last value
Process(LogEvent("called"); 42)   # Logs "called", then calls Process(42)

# Equivalent to:
LogEvent("called")
Process(42)
```

This pattern enables side effects in argument position:

<!--versetest
MultiplyByTen(X:int):int = X * 10
-->
<!-- 54 -->
```verse
Result := MultiplyByTen(2; 3)     # Evaluates 2 (discards it), calls Multiply(3)
Result = 30
```

Commas separate distinct arguments in the standard way:

<!--versetest
Add(A:int, B:int):int = A + B
-->
<!-- 55 -->
```verse
Sum := Add(10, 20)                # Two separate arguments
Sum = 30
```

Semicolons are *not allowed* in parameter lists - you must use commas:

<!--versetest
assert_semantic_error(3540):
    InvalidFunc(A:int; B:int):void = {}
-->
<!-- 56 -->
```verse
# VALID: Comma-separated parameters
ValidFunc(A:int, B:int):void = {}

# INVALID: Semicolon in parameters
# InvalidFunc(A:int; B:int):void = {}
```

##### In Specific Scopes

Within block expressions (braces), semicolons and commas are interchangeable as separators between definitions:

<!--versetest-->
<!-- 57 -->
```verse
# In block scope, all three separators work:
block:
    X:int = 0; Y:int = 0      # Semicolon separator

block:
    X:int = 0, Y:int = 0      # Comma separator

block:
    X:int = 0                 # Newline separator (most common)
    Y:int = 0
```

In `logic{}` constructor - both semicolons and commas work, but with
different semantics based on the construct's behavior:

<!--versetest-->
<!-- 58 -->
```verse
# Both evaluate all expressions and return logic value
Result1 := logic{true?; true?}    # Sequence of queries
Result2 := logic{true?, true?}    # Also valid
```

In `option{}` constructor - follows the standard sequence vs tuple rule:

<!--versetest-->
<!-- 59 -->
```verse
# Semicolon: sequence, wraps last value
Option1 := option{1; 2}?          # 2

# Comma: tuple, wraps the tuple
Option2 := option{1, 2}?          # (1, 2)
```

In `for` expressions - semicolon typically separates the iteration clause from filter conditions, while commas separate multiple conditions:

<!--versetest-->
<!-- 60 -->
```verse
# Semicolon separates iteration from filter
for (X := 1..3; X <> 2) { X }

# Comma separates multiple filter conditions
for (X := 1..3, X <> 2) { X }      # Same meaning in this context
```

In `array{}` constructors, you can separate elements with commas **or**
semicolons (but not mixed):

<!--versetest-->
<!-- 61 -->
```verse
CommaArray := array{1, 2, 3}       # Commas work
SemiArray := array{1; 2; 3}        # Semicolons also work
# MixedArray := array{1, 2; 3}     # ERROR: Cannot mix separators
```

##### Newlines as Separators

In addition to semicolons and commas, **newlines** can serve as
separators in compound expressions and blocks. Newlines behave like
semicolons - they create sequences:

<!--versetest-->
<!-- 62 -->
```verse
# These are equivalent:
Result1 := (1; 2; 3)

Result2 := (
    1
    2
    3
)
# Both return 3
```

#### Compound and Block Expressions

Compound expressions, delimited by braces, group multiple expressions
into a single expression. The value of a compound expression is the
value of its last sub-expression:

<!--versetest
ComputeIntermediate():int=3
CalculateAdjustment(o:int):int=3
-->
<!-- 63 -->
```verse
Result := {
    Temp := ComputeIntermediate()
    Adjustment := CalculateAdjustment(Temp)
    Temp + Adjustment
}
```

Compound expressions create new scopes for variables, allowing local bindings that do not affect the enclosing scope:

<!--versetest-->
<!-- 64 -->
```verse
block:
    X := 10    # Local to this block
    Y := 20
    X + Y
               # X and Y no longer accessible
```

You can separate expressions within a compound using semicolons, commas,
or newlines. Semicolons and newlines create sequences (returning the
last value), while commas create tuples. See [Semicolons vs
Commas](#semicolons-vs-commas) for the complete
rules:

<!--versetest
A:int = 1
B:int = 2
C:int = 3
M():void =
    X := { A; B; C }
    Y := { A, B, C }
    Z := {
        A
        B
        C
    }
<#
-->
<!-- 65 -->
```verse
{ A; B; C }           # Semicolon separation (returns C)
{ A, B, C }           # Comma separation (returns tuple (A, B, C))
{                     # Newline separation (returns C)
    A
    B
    C
}
```
<!-- #> -->

#### Array Expressions

Array expressions create array values using the `array` keyword
followed by elements in braces:

<!--versetest-->
<!-- 66 -->
```verse
NumArray := array{1, 2, 3, 4, 5}
Empty := array{}
Mixed := array{1, "two", 3.0}  # Mixed types if allowed
```

You can also construct arrays using indented syntax for clarity with
longer lists:

<!--versetest-->
<!-- 67 -->
```verse
Colors := array:
    "red"
    "green"
    "blue"
    "yellow"
```

## Book of Verse Source Unit: 02_primitives.md

### Primitive Data Types

Verse provides a rich set of primitive types that cover fundamental
programming needs. The numeric types `int`, `float`, and `rational`
handle mathematical operations, counters, and measurements. The
`logic` type represents boolean values for conditions and flags. The
`char`, `char32`, and `string` types handle text for character
data, player names, and messages. Two special types, `any` and `void`,
serve unique roles in the type hierarchy as the supertype of all types
and the empty type respectively.

Let's explore each primitive type in detail, starting with the numeric types that form the backbone of game logic.

#### Intrinsics

*intrinsic functions* are built-in operations provided directly by the
runtime that cannot be implemented in pure Verse code. These functions
receive special compiler treatment and form the foundation for many
language features. Intrinsic functions are special because they:

- **Implemented by the runtime**: Written in C++ or other native code, not Verse
- **Cannot be replicated in Verse**: Require access to runtime internals or low-level operations
- **Receive compiler recognition**: The compiler knows about them and may optimize their use

Examples include mathematical operations like `Abs()`, collection
methods like `Find()`, and type conversions like `ToString()`.

Most intrinsic functions *cannot be referenced as first-class
values*. This means you can call them directly, but you cannot store
them in variables or pass them as function arguments:

<!--versetest
assert_semantic_error(3502):
    F := Abs
-->
<!-- 01 -->
```verse
Result := Abs(-42)  # Returns 42

# Invalid: Cannot reference without calling
# F := Abs  # ERROR
# Invalid: Cannot pass as parameter
# ApplyFunction(Abs, -42)  # ERROR
```

This restriction exists because intrinsics often require special
calling conventions or optimizations that do not fit the standard
function model. If you need to pass intrinsic functionality around,
wrap it in a regular function or nested function.

#### Integers

The `int` type represents integer, non-fractional values. An `int` can
contain a positive number, a negative number, or zero. At runtime,
integers are arbitrary precision and can grow beyond any fixed size.
However, integer *literals* must fit within a 64-bit signed range
(`-9,223,372,036,854,775,808` to `9,223,372,036,854,775,807`), and
integers exceeding 64-bit have limited support (e.g., cannot be used
in string interpolation or persisted).

You can include `int` values within your code as literals.

<!--versetest-->
<!-- 02 -->
```verse
A :int= -42                    # civilian size
#B := 42424242424242424242424242424242424242424242424242 # scary numbers...
                               # ...can be computed but not written as literals

AnswerToTheQuestion :int= 42   # A variable that never changes
CoinsPerQuiver :int= 100       # A quiver costs this many coins
ArrowsPerQuiver :int= 15       # A quiver contains this many arrows

# Mutable variables (see Mutability chapter for details on var and set)
var Coins :int= 225           # The player currently has 225 coins
var Arrows :int= 3            # The player currently has 3 arrows
var TotalPurchases :int= 0    # Track total purchases
```

You can use the four basic math operations with integers: `+` for
addition, `-` for subtraction, `*` for multiplication, and `/` for
division.

<!--versetest
MyInt:int=10
MyHugeInt:int=1010100101
-->
<!-- 03 -->
```verse
var C :int= (-MyInt + MyHugeInt - 2) * 3   # arithmetic
set C += 1                                 # like saying, set C = C + 1
set C *= 2                                 # like saying, set C = C * 2
```

For integers, the operator `/` is failable, and the result is a
`rational` type if it succeeds.

#### Rationals

The `rational` type represents exact fractions as ratios of
integers. Unlike `int` or `float`, you cannot write a `rational`
literal directly—integer division using the `/` operator creates
rationals.

<!--versetest-->
<!-- 04 -->
```verse
X := 7 / 3    # X has type rational, representing exactly 7÷3
```

Rationals provide *exact arithmetic* without the precision loss of
floating-point numbers, making them ideal for game logic requiring
precise fractional calculations (resource distribution, turn-based
systems, probability calculations).

Integer division with `/` produces a rational value. Division by zero fails:

<!--versetest-->
<!-- 05 -->
```verse
Half := 5 / 2           # rational: exactly 5/2
Third := 10 / 3         # rational: exactly 10/3
Quarter := 1 / 4        # rational: exactly 1/4

if (not (1 / 0)):
    # Division by zero fails
```

Rationals are automatically reduced to lowest terms for equality comparisons:

<!--versetest-->
<!-- 06 -->
```verse
# All these are equal - reduced to 5/2
(5 / 2) = (10 / 4)      # true
(5 / 2) = (15 / 6)      # true
(10 / 4) = (15 / 6)     # true
```

This normalization ensures that mathematically equivalent rationals
compare as equal regardless of how they were constructed.

Negative signs are normalized to the numerator:

<!--versetest-->
<!-- 07 -->
```verse
(1 / -3) = (-1 / 3)     # true: negative moves to numerator
(-1 / -3) = (1 / 3)     # true: double negative becomes positive
```

This canonical form simplifies equality checking and ensures
consistent behavior.

An important property: *`int` is a subtype of `rational`*. This means
any integer can be used where a rational is expected:

<!--versetest-->
<!-- 08 -->
```verse
ProcessRational(X:rational):rational = X

# Can pass integers directly
ProcessRational(5) = 5/1     # 5 is implicitly 5/1 (rational)
ProcessRational(0) = 0/1     # 0 is implicitly 0/1 (rational)
```

However, you *cannot* return a rational where an int is expected—that
would be a narrowing conversion:

<!--versetest
assert_semantic_error(3510):
    BadFunction(X:rational):int = X
<#
-->
<!-- 09 -->
```verse
BadFunction(X:rational):int = X  # Error
```
<!-- #> -->

Whole number rationals equal their integer equivalents:

<!--versetest-->
<!-- 10 -->
```verse
(2 / 1) = 2             # true
2 = (2 / 1)             # true
(4 / 2) = 2             # true: 4/2 reduces to 2/1, equals 2
(9 / 3) = 3             # true: 9/3 reduces to 3/1, equals 3
```

This enables seamless mixing of integer and rational values in calculations.

Two functions convert rationals to integers:

- **`Floor`** — rounds toward negative infinity (down on number line)
- **`Ceil`** — rounds toward positive infinity (up on number line)

<!--versetest-->
<!-- 11 -->
```verse
# Positive rationals
Floor(5 / 2)= 2         # 2.5 → 2 (down)
Ceil(5 / 2) = 3         # 2.5 → 3 (up)

# Negative rationals - note direction!
Floor((-5) / 2) = -3    # -2.5 → -3 (toward negative infinity)
Ceil((-5) / 2) = -2     # -2.5 → -2 (toward positive infinity)

# With negative denominator
Floor(5 / -2) = -3      # Same as (-5)/2
Ceil(5 / -2) = -2       # Same as (-5)/2

# Both negative
Floor((-5) / -2) = 2    # 2.5 → 2
Ceil((-5) / -2) = 3     # 2.5 → 3
```

`Floor` rounds toward negative infinity, *not* toward zero. This
matches mathematical convention but differs from truncation. When the
argument is a rational, `Floor` does not fail, but if passed a `float`
it is a `decides` function.

When you apply `Floor` or `Ceil` to an integer-valued rational (one
that reduces to a whole number), it returns that integer:

<!--versetest-->
<!-- 11001 -->
```verse
# Integer-valued rationals
PositiveRational:rational = 5
NegativeRational:rational = -5

Floor(PositiveRational) = 5   # Returns the integer 5
Ceil(PositiveRational) = 5    # Returns the integer 5
Floor(NegativeRational) = -5  # Returns the integer -5
Ceil(NegativeRational) = -5   # Returns the integer -5

# Also works for rationals that reduce to integers
Floor(10 / 2) = 5             # 10/2 = 5/1, returns 5
Ceil(10 / 2) = 5              # 10/2 = 5/1, returns 5
```

Rationals can be used as parameter and return types:

<!--versetest-->
<!-- 12 -->
```verse
# Function returning rational
Half(X:int)<computes><decides>:rational = X / 2

# Use the result
if (Result := Half[7]):
    Floor(Result) = 3   # 7/2 = 3.5, Floor gives 3
    Ceil(Result) = 4    # 7/2 = 3.5, Ceil gives 4
```


Because `int` is a subtype of `rational`, you *cannot* overload based
solely on these types:

<!--versetest
assert_semantic_error(3532):
    ProcessValue(X:int):void = {}
    ProcessValue(X:rational):void = {}
<#
-->
<!-- 13 -->
```verse
ProcessValue(X:int):void = {}
ProcessValue(X:rational):void = {}  # Error!
```
<!-- #> -->

The compiler sees `int` as more specific than `rational`, so the
signatures would be ambiguous.

Rationals excel at resource distribution and fairness calculations:

<!--versetest-->
<!-- 14 -->
```verse
# Fair resource distribution
DistributeResources(TotalGold:int, NumPlayers:int)<decides>:int =
    GoldPerPlayer := TotalGold / NumPlayers
    Floor(GoldPerPlayer)  # Converts to whole gold pieces (can be 0)

# To fail when there's insufficient gold, check > 0
DistributeResourcesOrFail(TotalGold:int, NumPlayers:int)<decides>:int =
    GoldPerPlayer := TotalGold / NumPlayers
    Floor(GoldPerPlayer) > 0  # Fails if each player gets 0

# Item affordability calculation
Coins:int = 225
CoinsPerQuiver:int = 100
ArrowsPerQuiver:int = 15

if (NumberOfQuivers := Floor(Coins / CoinsPerQuiver)):
    TotalArrows:int = NumberOfQuivers * ArrowsPerQuiver
    # Player can afford 2 quivers = 30 arrows
```

#### Floats

The `float` type represents all non-integer numerical values. It can
hold large values and precise fractions, such as `1.0`, `-50.5`, and
`3.14159`. A float is an IEEE 64-bit float, which means it can contain
a positive or negative number that has a decimal point in the range
`[-2^1024 + 1, … , 0, … , 2^1024 - 1]`, or has the value `NaN` (Not a
Number). The implementation differs from the IEEE standard in the
following ways:

- There is only one `NaN` value.
- `NaN` is equal to itself.
- Every number is equal to itself.
- `0` cannot be negative.

You can include float values within your code as literals:

<!--versetest-->
<!-- 15 -->
```verse
A:float = 1.0
B := 2.14
MaxHealth : float = 100.0

var C:float = A + B
C = 3.14              # succeeds
set C -= 3.14
C = 0.0               # succeeds
# C = 0              # compile error; 0 is not a `float` literal
```

You can use the four basic math operations with floats: `+` for
addition, `-` for subtraction, `*` for multiplication, and `/` for
division. There are also combined operators for doing the basic math
operations (addition, subtraction, multiplication, and division), and
updating the value of a variable:

<!--versetest-->
<!-- 16 -->
```verse
var CurrentHealth : float = 100.0
set CurrentHealth /= 2.0    # Halves the value of CurrentHealth
set CurrentHealth += 10.0   # Adds 10 to CurrentHealth
set CurrentHealth *= 1.5    # Multiplies CurrentHealth by 1.5
```

##### Special Float Values

Float operations follow IEEE 754 semantics, which include special
values for infinity and Not-a-Number (NaN):

<!--versetest-->
<!-- 16a -->
```verse
# Infinity from division by zero
PosInf := 1.0 / 0.0
NegInf := -1.0 / 0.0

# NaN from invalid operations
NaNFromZeroDiv := 0.0 / 0.0
NaNFromSqrt := Sqrt(-1.0)
NaNFromInfDiv := PosInf / PosInf

# NaN propagates through operations
Result := NaNFromZeroDiv + 100.0  # Result is NaN
Product := NaNFromSqrt * 2.0      # Product is NaN
```

Division by zero produces `Inf` or `-Inf` rather than failing. Invalid
operations like `0.0 / 0.0`, `Sqrt(-1.0)`, or `Inf - Inf` produce NaN
values. Arithmetic operations with NaN propagate the NaN to the
result. Unlike standard IEEE 754, Verse's NaN equals itself when
compared.

To convert an `int` to a `float`, multiply it by `1.0`: `MyFloat:=MyInt*1.0`.

#### Mathematical Functions

Verse provides intrinsic mathematical functions for common numerical
operations. These functions are optimized by the runtime and work with
both `int` and `float` types.

The `Abs()` function returns the absolute value of a number—its
distance from zero without regard to sign:

<!--NoCompile-->
<!-- 17 -->
```verse
# Signatures
Abs(X:int):int
Abs(X:float):float
```

<!--versetest-->
<!-- 18 -->
```verse
Abs(5)    # Returns 5
Abs(-5)   # Returns 5
Abs(0)    # Returns 0
Abs(3.14) # Returns 3.14
```

The `Min()` and `Max()` functions return the minimum or maximum of two values:

<!--NoCompile-->
<!-- 19 -->
```verse
# Signatures
Min(A:int, B:int):int
Min(A:float, B:float):float
Max(A:int, B:int):int
Max(A:float, B:float):float
```

<!--versetest-->
<!-- 20 -->
```verse
# NaN propagates through comparison
Max(NaN, 5.0)   # Returns NaN
Min(NaN, 5.0)   # Returns NaN
Max(NaN, NaN)   # Returns NaN

# Infinity handling
Max(Inf, 100.0)    # Returns Inf
Min(-Inf, 100.0)   # Returns -Inf
Max(-Inf, -Inf)    # Returns -Inf
Min(Inf, Inf)      # Returns Inf
```

Verse provides multiple rounding functions that convert floats to integers with different rounding strategies:

<!--NoCompile-->
<!-- 21 -->
```verse
# Signatures
Floor(X:float)<reads><decides>:int   # Round down
Ceil(X:float)<reads><decides>:int    # Round up
Round(X:float)<reads><decides>:int   # Round to nearest even (IEEE-754)
Int(X:float)<reads><decides>:int     # Truncate toward zero
```

Round to nearest even (ties go to even):

<!--versetest-->
<!-- 22 -->
```verse
Round[1.5]    # Returns 2 (tie: 1.5 rounds to even 2)
Round[0.5]    # Returns 0 (tie: 0.5 rounds to even 0)
Round[2.5]    # Returns 2 (tie: 2.5 rounds to even 2)
Round[-1.5]   # Returns -2 (tie: -1.5 rounds to even -2)
Round[-0.5]   # Returns 0 (tie: -0.5 rounds to even 0)

Round[1.4]    # Returns 1 (no tie, rounds down)
Round[1.6]    # Returns 2 (no tie, rounds up)
```

The "round to nearest even" strategy (also called banker's rounding)
avoids bias when rounding many tie values.

Some additional mathematical functions:

<!--versetest-->
<!-- 23 -->
```verse
# Signature
# Sqrt(X:float):float

# Negative inputs return NaN
Sqrt(-1.0)    # Returns NaN

# Special values
Sqrt(Inf)     # Returns Inf
Sqrt(NaN)     # Returns NaN

# Signature
# Pow(Base:float, Exponent:float):float

Pow(2.0, 3.0)     # Returns 8.0 (2³)
Pow(10.0, 2.0)    # Returns 100.0
Pow(4.0, 0.5)     # Returns 2.0 (square root)
Pow(2.0, -1.0)    # Returns 0.5 (reciprocal)

# Special cases
Pow(0.0, 0.0)     # Returns 1.0 (by convention)
Pow(NaN, 0.0)     # Returns 1.0 (0 exponent always 1)
Pow(1.0, NaN)     # Returns 1.0 (1 to any power is 1)

# Exp(X:float):float

Exp(0.0)      # Returns 1.0
Exp(1.0)      # Returns 2.718... (e)
Exp(-1.0)     # Returns 0.368... (1/e)

# Special values
Exp(-Inf)     # Returns 0.0
Exp(Inf)      # Returns Inf
Exp(NaN)      # Returns NaN

# Signature
# Ln(X:float):float

Ln(1.0)       # Returns 0.0
# Ln(2.718...)     # Returns 1.0 (ln(e) = 1)
Ln(10.0)      # Returns 2.302...

# Invalid inputs
Ln(-1.0)      # Returns NaN (negative)
Ln(0.0)       # Returns -Inf (log of zero)

# Special values
Ln(Inf)       # Returns Inf
Ln(NaN)       # Returns NaN

# Signature
# Log(Base:float, Value:float):float

Log(10.0, 100.0)   # Returns 2.0 (log₁₀(100) = 2)
Log(2.0, 8.0)      # Returns 3.0 (log₂(8) = 3)
Log(2.0, 2.0)      # Returns 1.0 (logₙ(n) = 1)
```

Verse provides standard trigonometric functions operating on radians:

<!--versetest-->
<!-- 27 -->
```verse
# Signatures
# Sin(Angle:float):float
# Cos(Angle:float):float
# Tan(Angle:float):float

# Common angles (using PiFloat constant)
Sin(0.0)              # Returns 0.0
Sin(PiFloat / 2.0)    # Returns 1.0
Sin(PiFloat)          # Returns 0.0
Sin(-PiFloat / 2.0)   # Returns -1.0

Cos(0.0)              # Returns 1.0
Cos(PiFloat / 2.0)    # Returns 0.0
Cos(PiFloat)          # Returns -1.0

Tan(0.0)              # Returns 0.0
Tan(PiFloat / 4.0)    # Returns 1.0
Tan(-PiFloat / 4.0)   # Returns -1.0

# Special values
Sin(NaN)              # Returns NaN
Sin(Inf)              # Returns NaN

# Signatures
# ArcSin(X:float):float   # Returns angle in [-π/2, π/2]
# ArcCos(X:float):float   # Returns angle in [0, π]
# ArcTan(X:float):float   # Returns angle in [-π/2, π/2]
# ArcTan(Y:float, X:float):float  # Two-argument arctangent

# Inverse relationships
ArcSin(0.0)    # Returns 0.0
ArcSin(1.0)    # Returns π/2
ArcSin(-1.0)   # Returns -π/2

ArcCos(1.0)    # Returns 0.0
ArcCos(0.0)    # Returns π/2
ArcCos(-1.0)   # Returns π

ArcTan(0.0)    # Returns 0.0
ArcTan(1.0)    # Returns π/4
ArcTan(-1.0)   # Returns -π/4

# Verify inverse relationship
Angle := PiFloat / 6.0  # 30 degrees
Sin(ArcSin(Sin(Angle))) = Sin(Angle)  # True

# ArcTan(Y, X) returns angle of point (X, Y) from origin
ArcTan(1.0, 1.0)     # Returns π/4 (45 degrees)
ArcTan(1.0, 0.0)     # Returns π/2 (90 degrees)
ArcTan(0.0, 1.0)     # Returns 0.0 (0 degrees)
ArcTan(1.0, -1.0)    # Returns 3π/4 (135 degrees)
ArcTan(-1.0, -1.0)   # Returns -3π/4 (-135 degrees)
```

Hyperbolic functions are analogs of trigonometric functions for
hyperbolas. They are useful in physics simulations, catenary curves,
and certain mathematical models.

<!--versetest-->
<!-- 28 -->
```verse
# Signatures
# Sinh(X:float):float    # Hyperbolic sine
# Cosh(X:float):float    # Hyperbolic cosine
# Tanh(X:float):float    # Hyperbolic tangent
# ArSinh(X:float):float  # Inverse hyperbolic sine
# ArCosh(X:float):float  # Inverse hyperbolic cosine
# ArTanh(X:float):float  # Inverse hyperbolic tangent

Sinh(0.0)     # Returns 0.0
Sinh(1.0)     # Returns 1.175...
Cosh(0.0)     # Returns 1.0
Cosh(1.0)     # Returns 1.543...
Tanh(0.0)     # Returns 0.0
Tanh(1.0)     # Returns 0.761...

# Special values
Sinh(-Inf)    # Returns -Inf
Sinh(Inf)     # Returns Inf
Cosh(-Inf)    # Returns Inf
Cosh(Inf)     # Returns Inf
Tanh(-Inf)    # Returns -1.0
Tanh(Inf)     # Returns 1.0

ArSinh(0.0)   # Returns 0.0
ArCosh(1.0)   # Returns 0.0
ArTanh(0.0)   # Returns 0.0

# Special values
ArSinh(-Inf)  # Returns -Inf
ArSinh(Inf)   # Returns Inf
ArCosh(Inf)   # Returns Inf
ArCosh(-1.0)  # Returns NaN (domain error)
```

For integer division with remainder, Verse provides `Mod` and
`Quotient`. Both functions are failable—they fail when the divisor is
zero.

<!--versetest-->
<!-- 29 -->
```verse
# Signatures
# Mod(Dividend:int, Divisor:int)<decides>:int
# Quotient(Dividend:int, Divisor:int)<decides>:int

# Positive operands
Mod[15, 4]      # Returns 3
Quotient[15, 4] # Returns 3
# Relationship: 15 = 3*4 + 3

# Negative dividend
Mod[-15, 4]      # Returns 1
Quotient[-15, 4] # Returns -4
# Relationship: -15 = -4*4 + 1

# Negative divisor
Mod[-1, -2]      # Returns 1
Quotient[-1, -2] # Returns 1

# Division by zero fails
if (not Mod[10, 0]):
    Print("Cannot mod by zero")
if (not Quotient[10, 0]):
    Print("Cannot divide by zero")
```

The modulo result always satisfies:

<!--versetest
assert:
    Dividend:int = 15
    Divisor:int = 4
    Dividend = Quotient[Dividend, Divisor] * Divisor + Mod[Dividend, Divisor]

assert:
    Dividend:int = -15
    Divisor:int = 4
    Dividend = Quotient[Dividend, Divisor] * Divisor + Mod[Dividend, Divisor]

assert:
    Dividend:int = -1
    Divisor:int = -2
    Dividend = Quotient[Dividend, Divisor] * Divisor + Mod[Dividend, Divisor]
<#
-->
<!-- 30 -->
```verse
Dividend = Quotient[Dividend, Divisor] * Divisor + Mod[Dividend, Divisor]
```
<!-- #> -->

The sign of the result follows specific rules:

- `Mod` result is always non-negative, in the range `0 <= Result < |Divisor|` (Euclidean division)
- `Quotient` adjusts accordingly to maintain the identity

There are also some utility functions:

<!--versetest-->
<!-- 31 -->
```verse
# Signatures
# Sgn(X:int):int
# Sgn(X:float):float

Sgn(10)       # Returns 1
Sgn(0)        # Returns 0
Sgn(-5)       # Returns -1

Sgn(3.14)     # Returns 1.0
Sgn(0.0)      # Returns 0.0
Sgn(-2.71)    # Returns -1.0

# Special float values
Sgn(Inf)      # Returns 1.0
Sgn(-Inf)     # Returns -1.0
Sgn(NaN)      # Returns NaN
```

Lerp interpolates between two values:

<!--versetest-->
<!-- 32 -->
```verse
# Signature
# Lerp(From:float, To:float, Parameter:float):float

Lerp(0.0, 10.0, 0.0)    # Returns 0.0 (0% = From)
Lerp(0.0, 10.0, 0.5)    # Returns 5.0 (50%)
Lerp(0.0, 10.0, 1.0)    # Returns 10.0 (100% = To)
Lerp(0.0, 10.0, 2.0)    # Returns 20.0 (extrapolation)
Lerp(10.0, 20.0, 0.3)   # Returns 13.0

# Works with negative ranges
Lerp(-10.0, 10.0, 0.5)  # Returns 0.0
```

The formula is: `From + Parameter * (To - From)`

`IsFinite` checks if a float is finite and suceeds if the value
is not NaN, Inf, or -Inf. And fails otherwise:

<!--versetest

assert:
    (5.0).IsFinite[]
    (0.0).IsFinite[]
    (-100.0).IsFinite[]
    not (Inf).IsFinite[]
    not (-Inf).IsFinite[]
    not (NaN).IsFinite[]
    (15.16).IsFinite[] = 15.16
<#
-->
<!-- 33 -->
```verse
# Method on float values
# X.IsFinite()<computes><decides>:float

(5.0).IsFinite[]      # succeeds
(0.0).IsFinite[]      # succeeds
(-100.0).IsFinite[]   # succeeds

(Inf).IsFinite[]  # fails
(-Inf).IsFinite[] # fails
(NaN).IsFinite[]  # fails

# Returns the same number if succeeds
(15.16).IsFinite[] = 15.16 # succeeds, both are equal

# Useful for validation
# SafeCalculation(X:float, Y:float)<decides>:float =
#     X.IsFinite[] and Y.IsFinite[]
#     Result := X / Y
#     Result.IsFinite[]
#     Result
```
<!-- #> -->

Verse provides constants for common mathematical values:

<!--versetest-->
<!-- 34 -->
```verse
PiFloat # 3.14159265358979323846...
Inf     # Positive infinity
-Inf    # Negative infinity (negation of Inf)
NaN     # Not a Number
```

#### Booleans

The `logic` type represents the Boolean values `true` and `false`.

<!--versetest-->
<!-- 35 -->
```verse
A:logic = true
B := false

# A = B          # fails
A?                # succeeds
# B?             # fails

true?             # succeeds
# false?         # fails
```

The `logic` type only supports query operations and comparison
operations.  Query expressions use the query operator `?` to check if
a logic value is true and fail if the logic value is `false`.  For
comparison operations, use the failable operator `=` to test if two
logic values are the same, and `<>` to test for inequality.

Many programming languages find it idiomatic to use a type like
`logic` to signal the success or failure of an operation. In Verse, we
use success and failure instead for that purpose, whenever
possible. The conditional only executes the `then` branch if the guard
succeeds:

<!--versetest
ShowTargetLockedIcon():void={}
TargetLocked:?int = option{42}
-->
<!-- 36 -->
```verse
if (TargetLocked?):
    ShowTargetLockedIcon()
```

To convert an expression that has the `<decides>` effect to `true` on
success or `false` on failure, use `logic{ exp }`:

<!--versetest
using{ /Verse.org/Random }
Frequency:int = 10
F()<decides>:void=
    GotIt := logic{GetRandomInt(0, Frequency) <> 0}
    GotIt?
<#
-->
<!-- 37 -->
```verse
GotIt := logic{GetRandomInt(0, Frequency) <> 0}   # if success
GotIt?                                            # then this succeeds
GotIt = false                                     # and this fails
not GotIt?                                        # and this fails too
```
<!-- #> -->

#### Characters and Strings

Text is represented in terms of characters and strings.  A `char` is a
single **UTF-8 code unit** (not a full Unicode code point). A string
is therefore an array of characters, written as `[]char`. For
convenience, Verse provides the type alias `string` for `[]char`:

<!--versetest-->
<!-- 38 -->
```verse
MyName :string = "Joseph"
MyAlterEgo := "José"
```

Verse uses UTF-8 as the character encoding scheme. Each UTF-8 code unit
is one byte. A Unicode code point may require between one and four
code units. Code points with lower values use fewer bytes, while
higher values require more.

For example:

- `"a"` requires one byte (`{0o61}`),
- `"á"` requires two bytes (`{0oC3}{0oA1}`),
- `"🐈"` (cat emoji) requires four bytes (`{0u1f408}`).

Thus, strings are sequences of code units, not necessarily sequences
of Unicode characters in the abstract sense.

Because strings are arrays of `char`, you can index into them with
`[]`. Indexing has the `<decides>` effect: it succeeds when the index
is valid and fails otherwise.

<!--versetest
MyName:string="J"
-->
<!-- 39 -->
```verse
TheLetterJ := MyName[0]     # succeeds
TheLetterJ = 'J'            # succeeds
# MyName[100]               # fails
```

The length of a string is the number of UTF-8 code units it contains,
accessed via `.Length`. Note that this is *not the same as the number
of Unicode characters*:

<!--versetest-->
<!-- 40 -->
```verse
"José".Length = 5           # succeeds; 5 UTF-8 code units
"Jose".Length = 4           # succeeds; 4 UTF-8 code units
```

Because `string` is just `[]char`, strings declared as `var` can be mutated:

<!--versetest-->
<!-- 41 -->
```verse
var OuterSpaceFriend :string = "Glorblex"
set OuterSpaceFriend[0] = 'F'
```

Strings can be concatenated using the `+` operator:

<!--versetest
MyName:string="Joe"
MyAlterEgo:string="Jak"
-->
<!-- 42 -->
```verse
MyAttemptAtFormatting := "My name is " + MyName + " but my alter ego is " + MyAlterEgo + "."
```

Verse also supports string interpolation for more readable formatting:

<!--versetest
MyName:string="3"
MyAlterEgo:string="asdsa"
-->
<!-- 43 -->
```verse
Formatting := "My name is {MyName} but my alter ego is {MyAlterEgo}."
```

Interpolation works for any value that has a `ToString()` function in scope.

Write literal characters with single quotes. The type depends on
whether the character falls within the ASCII range (`U+0000`–`U+007F`)
or not:

- `'e'` has type `char`,
- `'é'` has type `char32`.

<!--versetest-->
<!-- 44 -->
```verse
A :char = 'e'                       # ok
B :char32 = 'é'                     # ok
# C :char = 'é'                     # error: type of 'é' is char32
# D :char32 = 'e'                   # error: type of 'e' is char
```

Character literals can also be written using numeric escape sequences:

<!--versetest-->
<!-- 45 -->
```verse
E :char = 0o65                      # ok; same as 'e'
F :char32 = 0u00E9                  # ok; same as 'é'
```

- `char` represents a single UTF-8 code unit (one byte, `0oXX`).
- `char32` represents a full Unicode code point (`0uXXXXX`).

Hex notation:

- `0oXX` for `char`: two hex digits (0o00 to 0off)
- `0uXXXXX` for `char32`: up to six hex digits (0u00000 to 0u10ffff)

Unlike some languages, Verse does not allow implicit conversion between characters and integers.

**Character escape sequences** work in both character and string literals:

| Escape | Meaning | Codepoint |
|--------|---------|-----------|
| `\t` | Tab | U+0009 |
| `\n` | Newline | U+000A |
| `\r` | Carriage return | U+000D |
| `\"` | Double quote | U+0022 |
| `\'` | Single quote | U+0027 |
| `\\` | Backslash | U+005C |
| `\{` | Left brace | U+007B |
| `\}` | Right brace | U+007D |
| `\<` | Less than | U+003C |
| `\>` | Greater than | U+003E |
| `\&` | Ampersand | U+0026 |
| `\#` | Hash/pound | U+0023 |
| `\~` | Tilde | U+007E |

Examples:

<!--versetest-->
<!-- 46 -->
```verse
Tab := '\t'
Newline := '\n'
Quote := '\"'
Brace := '\{'
```

Strings can be compared using the failable operators `=` (equality)
and `<>` (inequality). Comparison is done by code point, and is case
sensitive.  Equality depends on exact code unit sequences, not visual
appearance. Unicode allows multiple encodings for the same abstract
character. For example, `"é"` may appear as the single code point
`{0u00E9}`, or as the two-code-point sequence `"e"` (`{0u0065}`) plus
a combining accent (`{0u0301}`). These two strings look the same, but
they are not equal in Verse.

Checking whether a player has selected the correct item:

<!--versetest-->
<!-- 47 -->
```verse
ExpectedItemInternalName :string = "RedPotion"
SelectedItemInternalName :string = "BluePotion"

if (SelectedItemInternalName = ExpectedItemInternalName):
    true
else:
    false
```

Padding a timer with leading zeros:

<!--versetest-->
<!-- 48 -->
```verse
SecondsLeft :int = 30
SecondsString :string = ToString(SecondsLeft)    # convert int to string

var Combined :string = "Time Remaining: "
if (SecondsString.Length > 2):
    set Combined += "99"               # clamp to maximum
else if (SecondsString.Length < 2):
    set Combined += "0{SecondsString}" # pad with zero
else:
    set Combined += SecondsString
```

String interpolation supports complex expressions, not just simple variables:

<!--versetest
Format(D:float, ?Decimals:int):string=""
-->
<!-- 49 -->
```verse
# Expression interpolation
Age := 30
Message := "Next year: {Age + 1}"

# Function calls with named arguments
Distance := 5.5
Formatted := "Distance: {Format(Distance, ?Decimals:=2)}"
```

Strings can span multiple lines using interpolation braces for continuation:

<!--versetest-->
<!-- 50 -->
```verse
LongMessage := "This is a multi-line {
}string that continues across {
}multiple lines."

# Attention to whitespace:
AnotherMessage := "This is another {
}  multi-line message with     {
    # This comment is ignored
}    many spaces."
```

Empty interpolants `{}` are ignored, which is useful for line
continuation without adding content.

Since `string` is `[]char`, strings and character arrays can be compared:

<!--versetest-->
<!-- 51 -->
```verse
"abc" = array{'a', 'b', 'c'}    # Succeeds
"" = array{}                     # Succeeds - empty string equals empty array
```

Block comments within strings are removed during parsing:

<!--versetest-->
<!-- 52 -->
```verse
Text := "abc<#this comment is removed#>def"    # Same as "abcdef"
```

##### ToString()

The `ToString()` function converts values to their string
representations. It's polymorphic—multiple overloads exist for
different types:

<!--versetest
<#
-->
<!-- 53 -->
```verse
# Signatures
ToString(X:int):string
ToString(X:float):string
ToString(X:char):string
ToString(X:string):string  # Identity function
```
<!-- #> -->

String interpolation implicitly calls `ToString()` on embedded values:

<!--versetest-->
<!-- 54 -->
```verse
Age := 25
Score := 98.5

# These are equivalent:
Message1 := "Age: " + ToString(Age) + ", Score: " + ToString(Score)
Message2 := "Age: {Age}, Score: {Score}"
# Both produce: "Age: 25, Score: 98.5"
```

This makes `ToString()` essential for formatting output, even when you
do not call it directly.

`ToString()` only works on primitive types. User-defined classes and
structs do not have automatic string conversion.

##### ToDiagnostic()

The `ToDiagnostic()` function converts values to diagnostic string
representations, useful for debugging and logging. While similar to
`ToString()`, it may provide more detailed or implementation-specific
information:

<!--versetest
SomeValue:int=1
-->
<!-- 55 -->
```verse
# Usage (exact signature depends on type)
DiagnosticText := ToDiagnostic(SomeValue)
```

`ToDiagnostic()` is primarily used for debugging output rather than
user-facing strings. The exact format it produces may vary between VM
implementations and is not guaranteed to be stable across versions.

#### Type type

The `type` type is a *metatype* - a type whose values are themselves
types. Every Verse type can be used as a value of type `type`. This
enables powerful generic programming through parametric functions,
where types are parameters that can be passed around and constrained.

You can create variables and parameters that hold type values:

<!--versetest-->
<!-- 75 -->
```verse
# Variable holding a type value
IntType:type = int
StringType:type = string
# Function that takes a type as parameter
CreateDefault(t:type):?t = false
# Usage
X:?int = CreateDefault(int)      # T = int, returns false
Y:?string = CreateDefault(string)  # T = string, returns false
```

All Verse types can be type values:

<!-- TODO: Cannot convert - type expressions like []int, [string]int, tuple(), ?int,
     int->string, subtype(), and type{} cannot be assigned to variables at module scope -->

<!--NoCompile-->
<!-- 76 -->
```verse
# Primitives
PrimitiveType:type = int

# User-defined types
my_class := class {}
ClassType:type = my_class

my_struct := struct {Value:int}
StructType:type = my_struct

# Collection types
ArrayType:type = []int
MapType:type = [string]int
TupleType:type = tuple(int, string)
OptionType:type = ?int

# Function types
FuncType:type = int->string

# Parametric types
generic_class(t:type) := class {Data:t}
ParametricType:type = generic_class(int)

# Metatypes
SubtypeValue:type = subtype(my_class)

# Type literals
TypeLiteralValue:type = type{_(:int):string}
```

This universality makes `type` the foundation for Verse's generic
programming - any type can be abstracted over.

##### Type Parameters

The most common use of `type` is in **where clauses** to create
parametric (generic) functions:

<!--versetest-->
<!-- 77 -->
```verse
# Identity function - works with any type
Identity(X:t where t:type):t = X

# Usage - type parameter inferred
Identity(42)        # t = int
Identity("hello")   # t = string
Identity(true)      # t = logic
```

The `where t:type` constraint means "`t` can be any Verse type." The
type system infers `t` from the argument and ensures type safety
throughout the function.

While `where t:type` accepts any type, you can use more specific
constraints like `subtype` to limit which types are valid:

<!--versetest
Sort(Items:[]t where t:subtype(comparable)):[]t =
    Items
<#
-->
<!-- 78 -->
```verse
# Only accepts types that are subtypes of comparable
Sort(Items:[]t where t:subtype(comparable)):[]t =
    # Can use comparison operations because t is comparable
    ...
```
<!-- #> -->

For comprehensive documentation on parametric functions, see the
Functions chapter.

##### Type as First-Class Values

Unlike many languages where types only exist at compile time, Verse
treats types as *first-class values* that can be computed, stored, and
manipulated:

<!--versetest-->
<!-- 79 -->
```verse
# Function that returns a type value
GetTypeForSize(Size:int):type =
    if (Size <= 8):
        int
    else:
        string

# Store type in data structure
TypeRegistry:[string]type = map{
    "Integer" => int,
    "Text" => string,
    "Flag" => logic
}
```

**Passing types between functions:**

<!--versetest
CreateArray(ElementType:type, Size:int):[]ElementType =
    array{}

MakeIntArray():[]int =
    CreateArray(int, 10)
<#
-->
<!-- 80 -->
```verse
# Helper function that takes a type parameter
CreateArray(ElementType:type, Size:int):[]ElementType =
    # This pattern works in some contexts
    ...

# Function that uses the helper
MakeIntArray():[]int =
    CreateArray(int, 10)
```
<!-- #> -->

##### Returning Options of Type Parameters

A common pattern is to have functions return `?t` where `t` is a type
parameter, allowing the function to work with any type while
potentially failing:

<!--versetest
MaybeValue(Value:t, Condition:logic where t:type):?t =
    if (Condition?) then option{Value} else false

assert:
    X:?int = MaybeValue(5, false)
    Y:?float = MaybeValue(3.14, true)
<#
-->
<!-- 817 -->
```verse
# return type `t` must be the same type as the `Value` param type
MaybeValue(Value:t, Condition:logic where t:type):?t =
    if (Condition?) then option{Value} else false

# Usage
X:?int = MaybeValue(5, false)  # Returns false as ?int
Y:?float = MaybeValue(3.14, true)  # Returns option{3.14} as ?float
```
<!-- #> -->


<!--versetest
MaybeValueExplicit(T:type, Value:t, Condition:logic where t:subtype(T)):?T =
    if (Condition?):
        option{Value}
    else:
        false

assert:
    X:?int = MaybeValueExplicit(int, 5, false)
    Y:?float = MaybeValueExplicit(float, 3.14, true)
<#
-->
<!-- 818 -->
```verse
# Alternative: explicitly pass the type parameter
MaybeValueExplicit(T:type, Value:t, Condition:logic where t:subtype(T)):?T =
    if (Condition?):
        option{Value}
    else:
        false

# Usage
X:?int = MaybeValueExplicit(int, 5, false)  # Returns false as ?int
Y:?float = MaybeValueExplicit(float, 3.14, true)  # Returns option{3.14} as ?float
# Z:?int = MaybeValueExplicit(int, 3.14, true) # ERROR: float not subtype of int
```
<!-- #> -->

This pattern is particularly useful for generic containers and factory
functions that may or may not be able to produce a value.

##### Type Constraints

The `type` constraint in where clauses is the most permissive - it
accepts any Verse type. For more specific requirements, Verse provides
additional constraints:

<!--versetest-->
<!-- 82 -->
```verse
# Most permissive: any type
Generic(X:t where t:type):t = X

# More specific: must be subtype of comparable
RequiresComparison(X:t where t:subtype(comparable))<decides>:void =
    X = X  # Can use = because t is comparable

# Even more specific: must be exact subtype
RequiresExactType(X:t, Y:u where t:type, u:subtype(t)):t =
    X  # Y is guaranteed to be compatible with t
```

The type system enforces these constraints at compile time, preventing
invalid type usage.

##### Limitations

While `type` enables powerful abstractions, there are some limitations:

**Cannot construct arbitrary types generically:**

<!--NoCompile-->
<!-- 83 -->
```verse
# Cannot do this - no way to construct a value of arbitrary type t
MakeValue(T:type):T = ???  # What would this return for T=int? T=string?
```

**Cannot inspect type structure at runtime:**

<!--versetest
<#
-->
<!-- 84 -->
```verse
# Cannot do this - no runtime type introspection
GetFieldNames(T:type):string = ???
```
<!-- #> -->

**Type parameters must be inferred or explicit:**

<!--versetest
Identity(X:t where t:type):t = X

assert:
    Identity(42)

<#
-->
<!-- 85 -->
```verse
# Type parameter must be determinable from usage
Identity(X:t where t:type):t = X

# OK: t inferred from argument
Identity(42)

# ERROR: t cannot be inferred from no arguments
MakeDefault(where t:type):t = ???
```
<!-- #> -->

#### Any

The `any` type is the *supertype of all types*. Every type in the
language is a subtype of `any`. Because of this, `any` itself supports
very few operations: whatever functionality `any` provides must also
be implemented by every other type. In practice, there is very little
you can do directly with values of type `any`. Still, it is important
to understand the type, because it sometimes arises when working with
code that mixes different kinds of values, or when the type checker
has no more precise type to assign.

One way `any` appears is when combining values that do not share a
more specific supertype. For example:

<!--versetest
letters := enum:
    A
    B
    C

letter := class:
    Value : char
-->
<!-- 86 -->
```verse
Main(Arg : int) : void =
    X := if (Arg > 0) then:
        letters.A
    else:
        letter{Value := 'D'}
```

In this example, the code assigns `X` either a value of type `letters` or
of type `letter`. Since these two types are unrelated, the compiler
assigns `X` the type `any`, which is their lowest common supertype.

A more useful role for `any` is as the type of a parameter that is
required syntactically but not actually used. This pattern can arise
when implementing interfaces that require a certain method signature.

<!--versetest-->
<!-- 87 -->
```verse
FirstInt(X:int, :any) : int = X
```

Here, the second parameter is ignored. Because it can be any value of
any type, it is given the type `any`.

In more general code, the same idea can be expressed using *parametric
types*, making the function flexible while still precise:

<!--versetest-->
<!-- 88 -->
```verse
First(X:t, :any where t:type) : t = X
```

This version works for any type `t`, returning a value of type `t`
while discarding the unused argument of type `any`.

#### Void

The `void` type represents the absence of a meaningful result and is
used in places where no result is returned. Technically, `void` is
a function that accepts any value and evaluates to `false`.

This design allows a function with return type `void` to have a body
that evaluates to any type, while ensuring that callers cannot use
the result. The body passes the value it produces to `void`, which
discards it and returns `false`.

A function whose purpose is to perform an effect, rather than compute
a value, has return type `void`.

<!--versetest-->
<!-- 89 -->
```verse
LogMessage(Msg:string) : void =
    Print(Msg)
```

Here, `LogMessage` performs an action (printing) but does not return a
result. The `void` return type makes that explicit.

## Book of Verse Source Unit: 03_containers.md

### Container Types

Container types in Verse manage collections and structured data. Optionals represent values that may or may not be present. Tuples group multiple values of different types into ordered sequences. Arrays hold zero or more values with efficient indexed access. Maps associate keys with values for fast lookups. Weak maps extend regular maps with weak reference semantics for persistent storage.

Let's explore each container type in detail, starting with optionals that elegantly handle the presence or absence of values.

#### Optionals

An optional is an immutable container that either holds a value of type `t` or nothing at all. The type is written `?t`. Optionals are useful whenever a value may or may not be present, such as when looking up a key in a map or calling a function that can fail. By making this possibility explicit in the type, Verse allows programmers to handle "no result" situations directly and consistently, instead of relying on ad hoc error codes or special values.

You can create a non-empty optional with `option{...}`, which wraps a value into an optional. For example:

<!--versetest-->
<!-- 01 -->
```verse
A:?int = option{42}    # an optional containing the integer 42
```

If you want to represent "no value," you use the special constant `false`. This is how Verse spells the empty optional:

<!--versetest-->
<!-- 02 -->
```verse
var B:?int = false     # this optional has no element
B = false              # still empty
```

To extract the element of an optional, you write `?` after the optional expression. This produces a `<decides>` expression that succeeds if the optional has an element and fails otherwise. For example:

<!--versetest
A:?int = option{42}
-->
<!-- 03 -->
```verse
S := A? + 2            # succeeds with 44 because A contains 42
```

If `A` had been `false`, then the attempt to use `A?` would fail and so would the whole computation. A failing case makes this clearer:

<!--versetest
B:?int = false
-->
<!-- 04 -->
```verse
# X := B? + 1       # Fails because B is false and has no element
```

This shows how Verse integrates optionals tightly with the effect system: the presence or absence of a value can cause an entire computation to succeed or fail.

The `option{...}` form also works in the opposite direction. When you have a computation with the `<decides>` effect, wrapping it in `option{...}` converts it to an optional. On success you get a non-empty optional; on failure you get `false`:

<!--versetest
GetAFloatOrFail()<transacts><decides>:float = 3.14
-->
<!-- 05 -->
```verse
MaybeAFloat := option{GetAFloatOrFail[]}
```

This symmetry is important. The `?` operator unwraps an optional into a `<decides>` expression, while `option{...}` wraps a `<decides>` expression into an optional. Together they provide a smooth bridge between computations that may fail and values that may be absent.

Although an optional value itself is immutable, you can keep one in a variable and change which optional the variable points to. The keyword `set` is used for this:

<!--versetest-->
<!-- 06 -->
```verse
var C:?int = false
set C = option{2}      # C now refers to an optional containing 2
C? = 2                 # succeeds, since C is not empty
```

This ability is useful whenever you want to track success or failure over time, such as gradually computing a result and updating the variable only when you succeed.

A common use case is searching for something that may or may not be there. Imagine a function `Find` that looks through an array of integers and returns the index of the element you want. If the element exists, the function returns `option{index}`; if not, it returns `false`. The caller can then safely decide what to do:

<!--versetest
NumberArray:[]int = array{10, 20, 30}
-->
<!-- 07 -->
```verse
Find(N:[]int, X:int):?int =
    for (I := 0..N.Length-1):
        if (N[I] = X) then return option{I}
    return false
    
Idx:?int = Find(NumberArray, 20)    # returns option{1}
Y := Idx?                           # unwraps the optional
Y = 1
```

Here the optional signals the possibility of failure directly in the type. The `?` operator makes it easy to use the result in an expression, while `option{...}` allows you to turn conditional computations back into optionals. The effect is that the idea of "maybe a value, maybe not" becomes a first-class part of the language, rather than an afterthought, and programmers are encouraged to handle the absence of values in a disciplined way.

#### Tuple

A tuple is a container that groups two or more values. Unlike arrays, Tuples allow you to combine values of mixed types and treat them as a unit. The elements of a tuple appear in the order in which you list them, and you access them by their position, called the index. Because the number of elements is always known at compile time, a tuple is both simple to create and safe to use.

The term *tuple* is a back formation from *quadruple*, *quintuple*, *sextuple*, and so on. Conceptually, a tuple is like an unnamed data structure with ordered fields, or like a fixed-size array where each element may have a different type.

Write a tuple literal by enclosing a comma-separated list of expressions in parentheses. For example:

<!--versetest-->
<!-- 08 -->
```verse
Tuple1 := (1, 2, 3)
```

The order of elements matters, so `(3, 2, 1)` is a completely different value. Since tuples allow mixed types, you might write:

<!--versetest-->
<!-- 09 -->
```verse
Tuple2 := (1, 2.0, "three")
```

Tuples can also nest inside each other:

<!--versetest-->
<!-- 10 -->
```verse
X:tuple(int,tuple(int,float,string),string) = (1, (10, 20.0, "thirty"), "three")
```

Tuples are useful when you want to return multiple values from a function or when you want a lightweight grouping of values without the overhead of defining a struct or class. Write the type of a tuple with the `tuple` keyword followed by the types of the elements, but in most cases the compiler can infer it. For instance, you can write `MyTuple : tuple(int, float, string) = (1, 2.0, "three")`, or simply `MyTuple := (1, 2.0, "three")` and let the compiler deduce the type.

Access the elements of a tuple using a zero-based index operator written with parentheses. If `MyTuple := (1, 2.0, "three")`, then `MyTuple(0)` is the integer `1`, `MyTuple(1)` is the float `2.0`, and `MyTuple(2)` is the string `"three"`. Because the compiler knows the number of elements in every tuple, tuple indexing cannot fail: any attempt to use an out-of-bounds index results in a compile-time error.

Another feature of tuples is *expansion*. When you pass a tuple to a function as a single argument, Verse automatically expands its elements as if the function had been called with each element separately. For example:

<!--versetest-->
<!-- 11 -->
```verse
F(Arg1:int, Arg2:string):void =
    Print("{Arg1}, {Arg2}")

G():void =
    MyTuple := (1, "two")
    F(MyTuple)   # expands to F(1, "two")
```

Tuples also play a role in structured concurrency. The `sync` expression produces a tuple of results, allowing several computations that unfold over time to be evaluated simultaneously. In this way, tuples provide not only a convenient grouping mechanism but also a foundation for composing concurrent computations.

Tuples can also be automatically converted to arrays when used with array concatenation operators `+` and `+=`. See [From Tuples to Arrays](#from-tuples-to-arrays) for more details.

#### Arrays

An array is an immutable container that holds zero or more values of the same type `t`. The elements of an array are ordered, and each can be accessed by a zero-based index. Write arrays with square brackets in their type, for example `[]int` or `[]float`, and create them with the `array{...}` literal form. For instance, `A : []int = array{}` creates an empty array, while `B : []int = array{1, 2, 3}` creates an array of three integers. Accessing elements by index is a failable operation: `B[0]` succeeds with the value `1`, while `B[10]` fails because the index is out of bounds.

Arrays can be concatenated with the `+` operator, and when declared as `var` they can be extended with the shorthand operator `+=`. For example, `var C:[]int= B + array{4}` gives `C` the value `array{1,2,3,4}`, and `set C += array{5}` updates it to `array{1,2,3,4,5}`. Tuples can also be used directly with these operators, and will be automatically converted to arrays. The length of an array is available through the `.Length` member, so `C.Length` here would be `5`. Elements are always stored in the order they are inserted, and indexing starts at `0`. Thus `array{10,20,30}[0]` is `10`, and the last valid index of any array is always one less than its length.

Although arrays themselves are immutable, variables declared with `var` can be reassigned to new arrays, or can appear to have their elements changed. For example, `var D:[]int = array{1,2,3}` allows the update `set D[0] = 3`, after which `D` will hold `array{3,2,3}`. What actually happens is that Verse creates a brand new array under the hood, with the specified element updated. In effect, `set D[0] = 3` is compiled into `set D = array{3,D[1],D[2]}`. The old array continues to exist if another variable was referencing it, which means that if `A` and `B` both start as `array{1}` and we update `A[0]`, then `A` and `B` will diverge: `A[0]` is now `2` while `B[0]` is still `1`.

Arrays are useful whenever you want to store multiple values of the same type, such as a list of players in a game: `Players:[]player = array{Player1,Player2}`. Access is by index, for example `Players[0]` is the first player. Since indexing is failable, it is often combined with `if` expressions or iteration. For instance, the following code safely prints out every element of an array:

<!--versetest-->
<!-- 12 -->
```verse
ExampleArray : []int = array{10, 20, 30}
for (Index := 0..ExampleArray.Length - 1):
    if (Element := ExampleArray[Index]):
        Print("{Element} in ExampleArray at index {Index}")
```

produces

```
10 in ExampleArray at index 0
20 in ExampleArray at index 1
30 in ExampleArray at index 2
```

Because arrays are values, "changing" them always means replacing the old array with a new one. With `var` this feels natural, since variables can be reassigned. For example, you can concatenate arrays and then update an element:

<!--versetest-->
<!-- 13 -->
```verse
Array1 : []int = array{10, 11, 12}
var Array2 : []int = array{20, 21, 22}
set Array2 = Array1 + Array2 + array{30, 31}
if (set Array2[1] = 77) {}
```

After this code runs, iterating through `Array2` prints `10, 77, 12, 20, 21, 22, 30, 31`.

Tuples can be used directly with the `+` and `+=` operators on arrays, and will be automatically converted to arrays. This provides a concise way to add multiple elements without wrapping them in `array{...}`:

<!--versetest-->
<!-- 77 -->
```verse
var Numbers:[]int = array{1, 2, 3}

# Concatenate using a tuple - automatically converted to array
set Numbers = Numbers + (4, 5, 6)

# Shorthand form also works with tuples
set Numbers += (7, 8, 9)

# Result: array{1, 2, 3, 4, 5, 6, 7, 8, 9}
```

This tuple-to-array conversion with operators is distinct from tuple expansion in function calls. With operators, the tuple elements are added to the array as individual items, just as if you had written `array{4, 5, 6}`.

Arrays can also be nested to form multi-dimensional structures, similar to rows and columns of a table. For example, the following creates a two-dimensional 4×3 array of integers:

<!--versetest-->
<!-- 14 -->
```verse
var Counter : int = 0
Example : [][]int =
    for (Row := 0..3):
        for (Column := 0..2):
            set Counter += 1
```

This array can be visualized as

```
Row 0:  1  2  3
Row 1:  4  5  6
Row 2:  7  8  9
Row 3: 10 11 12
```

and is accessed with two indices: `Example[0][0]` is `1`, `Example[0][1]` is `2`, and `Example[1][0]` is `4`. You can loop through all rows and columns with nested iteration. Arrays in Verse are not restricted to rectangular shapes: each row can have a different length, producing a jagged structure. For example,

<!--versetest-->
<!-- 15 -->
```verse
Example : [][]int =
    for (Row := 0..3):
        for (Column := 0..Row):
            Row * Column
```

produces a triangular array with rows of increasing length: row 0 has a single `0`, row 1 has `0, 1`, row 2 has `0, 2, 4`, and row 3 has `0, 3, 6, 9`.

Nested arrays with complex initialization work naturally as class field defaults:

<!--versetest
tile_class := class:
    Position:tuple(int, int)
    var IsOccupied:logic = false

game_board := class:
    Tiles:[][]tile_class =
        for (Y := 0..9):
            for (X := 0..9):
                tile_class{Position := (X, Y)}

    GetTile(X:int, Y:int)<computes><decides>:tile_class =
        Row := Tiles[Y]
        Row[X]
assert:
<# 
-->
<!-- 16 -->
```verse
# Game board with tile grid
tile_class := class:
    Position:tuple(int, int)
    var IsOccupied:logic = false

game_board := class:
    # Initialize 10×10 grid of tiles
    Tiles:[][]tile_class =
        for (Y := 0..9):
            for (X := 0..9):
                tile_class{Position := (X, Y)}

    # Get tile at specific position
    GetTile(X:int, Y:int)<computes><decides>:tile_class =
        Row := Tiles[Y]
        Row[X]

# Create board instance
Board := game_board{}

# Access specific tile
if (CenterTile := Board.GetTile[5, 5]):
    set CenterTile.IsOccupied = true
```
<!--
#>
   Board := game_board{}
   if (CenterTile := Board.GetTile[5, 5]):
       set CenterTile.IsOccupied = true
-->

When you create an empty array with `array{}`, Verse infers the element type from the variable's type annotation:

<!--versetest-->
<!-- 17 -->
```verse
IntArray : []int = array{}       # Empty array of integers
FloatArray : []float = array{}   # Empty array of floats
```

Without a type annotation, the compiler cannot determine what type of array you want, so you must either provide the type explicitly or include at least one element that establishes the type.

Arrays determine their element type from the common supertype of all elements. When you create an array with values of different but related types, Verse finds the most specific type that encompasses all elements:

<!--versetest
class1 := class {}
class2 := class(class1) {}
class3 := class(class1) {}
-->
<!-- 18 -->
```verse
# Array element type is class1 (common supertype)
MixedArray : []class1 = array{class2{}, class3{}}
```

This applies to any type hierarchy, including interfaces. If you mix completely unrelated types, the element type becomes `any`:

<!--versetest-->
<!-- 19 -->
```verse
# Array of comparable - different types sharing comparable in common
DisjointArray : []comparable = array{42, 13.37, true}

# Array of any - different types with no common supertype
AnyArray : []any = array{15.61, "Message", void}
```

##### From Tuples to Arrays

Verse provides automatic conversion between tuples and arrays in specific contexts, enabling flexible function calls while maintaining type safety. This conversion is *one-way*: tuples can become arrays, but arrays cannot become tuples.

Tuples can be directly assigned to array variables when all tuple elements are compatible with the array's element type:

<!--versetest-->
<!-- 20 -->
```verse
# Homogeneous tuple to array
X:tuple(int, int) = (1, 2)
Y:[]int = X            # Valid - both elements are int
Y[1] = 2               # Can use as normal array

# Longer tuples work too
NumTuple:tuple(int, int, int, int) = (1, 2, 3, 4)
NumberArray:[]int = NumTuple
NumberArray.Length = 4
```

This conversion creates an array containing all the tuple's elements in order.

When a function has a single array parameter, you can call it with multiple arguments, which automatically form an array:

<!--versetest-->
<!-- 21 -->
```verse
ProcessNumbers(Nums:[]int):int = Nums.Length

# All these are equivalent:
ProcessNumbers(1, 2, 3)           # Multiple args → array
ProcessNumbers((1, 2, 3))         # Tuple literal → array
Values := (1, 2, 3)
ProcessNumbers(Values)             # Tuple variable → array
```

This "variadic-like" syntax provides convenience while keeping the function signature simple:

<!--versetest-->
<!-- 22 -->
```verse
Sum(Nums:[]int):int =
    var Total:int = 0
    for (N : Nums):
        set Total += N
    Total

Sum(1, 2, 3, 4)                   # Returns 10
Sum((5, 6))                       # Returns 11
Values := (10, 20, 30)
Sum(Values)                       # Returns 60
```

Array conversion only succeeds when **all tuple elements are compatible** with the array's element type:

<!--versetest
F(X:[]int):int = X.Length
entity := class:
    ID:int

player := class(entity):
    Name:string

ProcessEntities(E:[]entity):int = E.Length
GetP()<transacts>:player = player{ID := 1, Name := "Alice"}
GetE()<transacts>:entity = entity{ID := 2}
<#
-->
<!-- 23 -->
```verse
# Homogeneous tuple - all int
F(X:[]int):int = X.Length
F(1, 2, 3)                        # Valid

# Subtype compatibility
entity := class:
    ID:int

player := class(entity):
    Name:string

ProcessEntities(E:[]entity):int = E.Length

P := player{ID := 1, Name := "Alice"}
E := entity{ID := 2}
ProcessEntities(P, E)             # Valid - player is subtype of entity
```
<!-- #> -->

Functions taking `[]any` accept **any tuple**, regardless of element types:

<!--versetest-->
<!-- 24 -->
```verse
GetLength(Items:[]any):int = Items.Length

# All valid - any tuple works
GetLength(1, 2.0)                 # Mixed types OK
GetLength("a", 42, true)          # Different types OK
GetLength((1, 2.0, "hello"))      # Explicit tuple OK
```

This enables generic functions that work with heterogeneous data.

When tuple elements share a common supertype (via inheritance or interface), they convert to an array of that supertype:

<!--versetest
interface1 := interface:
    GetID():int

class1 := class(interface1):
    GetID<override>():int = 1

class2 := class(interface1):
    GetID<override>():int = 2

ProcessInterfaces(Items:[]interface1):int = Items.Length

assert:
    X:class1 = class1{}
    Y:class2 = class2{}
    ProcessInterfaces(X, Y) = 2
<#
-->
<!-- 25 -->
```verse
interface1 := interface:
    GetID():int

class1 := class(interface1):
    GetID<override>():int = 1

class2 := class(interface1):
    GetID<override>():int = 2

ProcessInterfaces(Items:[]interface1):int = Items.Length

X:class1 = class1{}
Y:class2 = class2{}

# Valid - both classes implement interface1
ProcessInterfaces(X, Y)           # Returns 2
```
<!-- #> -->

The compiler finds the most specific common supertype and uses it for the array element type.

Tuple-to-array conversion works with nested structures:

**Nested arrays:**

<!--versetest
ProcessMatrix(Matrix:[][]int):int = Matrix.Length
-->
<!-- 26 -->
```verse
# Nested tuples → nested arrays
MatrixData := ((1, 2), (3, 4))
ProcessMatrix(MatrixData)             # Valid

# Or with explicit nesting
ProcessMatrix((1, 2), (3, 4))   # Valid
```

**Optional arrays:**

<!--versetest-->
<!-- 27 -->
```verse
ProcessOptional(Items:?[]int)<decides>:int = Items?[0]

# Optional tuple → optional array
Values := option{(1, 2)}
ProcessOptional[Values]           # Valid
```

**Tuples containing arrays:**

<!--versetest-->
<!-- 28 -->
```verse
ProcessComplex(Data:tuple([]int, int)):int = Data(0).Length

# First element of tuple becomes array
ProcessComplex(((1, 2), 3))       # Valid - (1,2) becomes []int
```

##### Array Slicing

Arrays support slicing operations through the `.Slice` method, which extracts a contiguous portion of an array. Slicing is a failable operation—it succeeds only when the indices are valid.

The two-parameter form `Array.Slice[Start, End]` returns elements from index `Start` up to but not including index `End`:

<!--versetest-->
<!-- 29 -->
```verse
NumArray : []int = array{10, 20, 30, 40, 50}
if (Slice := NumArray.Slice[1, 4]):
    Slice = array{20, 30, 40}
```

The one-parameter form `Array.Slice[Start]` returns all elements from `Start` to the end:

<!--versetest
NumArray : []int = array{10, 20, 30, 40, 50}
-->
<!-- 30 -->
```verse
if (Slice := NumArray.Slice[2]):
    Slice = array{30, 40, 50}
```

Slicing fails if indices are negative, out of bounds, or if `Start` is greater than `End`. Creating an empty slice is valid when `Start` equals `End`:

<!--versetest
NumArray:[]int = array{10, 20, 30, 40, 50}
-->
<!-- 31 -->
```verse
NumArray.Slice[2, 2]  # Succeeds with array{}
# NumArray.Slice[2, 1]  # Would fail - Start > End
# NumArray.Slice[-1, 2] # Would fail - negative index
# NumArray.Slice[0, 10] # Would fail - End beyond array length
```

Slicing also works on strings and character tuples, returning a string:

<!--versetest-->
<!-- 32 -->
```verse
"hello".Slice[1, 4] = "ell"
```

##### Array Methods

Arrays provide intrinsic methods for searching, removing, and replacing elements. These operations create new arrays rather than modifying existing ones, maintaining Verse's immutability guarantees.

The `Find()` method searches for the first occurrence of an element and returns its index, or fails if not found:

<!--versetest
M():void =
    SomeArray:[]int = array{1, 2, 3}
    if (Example := SomeArray.Find[2]) {}
<#
-->
<!-- 33 -->
```verse
Array.Find(Element:t where t:subtype(comparable))<decides>:int
```
<!-- #> -->

<!--versetest-->
<!-- 34 -->
```verse
NumArray := array{1, 2, 3, 1, 2, 3}

if (Index := NumArray.Find[2]):
    # Index is 1 (first occurrence)
    Print("Found at index {Index}")

if (not NumArray.Find[0]):
    # Element not in array
    Print("Not found")

# With strings
Strings := array{"Apple", "Orange", "Strawberry"}

if (Index := Strings.Find["Strawberry"]):
    Print("Found at {Index}") # Prints "Found at 2"
```

`Find()` returns the first found index on success (`int`), or fails if the element was not found, enabling safe handling of missing elements without exceptions or special sentinel values.

`RemoveFirstElement()` removes the first occurrence:

<!--versetest
M():void =
    SomeArray:[]int = array{1, 2, 3}
    if (Updated := SomeArray.RemoveFirstElement[2]) {}
<#
-->
<!-- 35 -->
```verse
Array.RemoveFirstElement(Element:t where t:subtype(comparable))<decides>:[]t
```
<!-- #> -->

<!--versetest-->
<!-- 36 -->
```verse
NumArray := array{1, 2, 3, 1, 2, 3}

if (Updated := NumArray.RemoveFirstElement[2]):
    # Updated is array{1, 3, 1, 2, 3}
    Print("Removed first 2")

if (not NumArray.RemoveFirstElement[0]):
    # Element not found
    Print("Element not in array")
```

`RemoveAllElements()` removes all occurrences:

<!--versetest-->
<!-- 37 -->
```verse
NumArray := array{1, 2, 3, 1, 2, 3}
Updated := NumArray.RemoveAllElements(2)
Updated = array{1, 3, 1, 3}

# Returns unchanged array if element not found
Same := NumArray.RemoveAllElements(0)
Same = array{1, 2, 3, 1, 2, 3}
```

`Remove()` removes element at specific position:

<!--NoCompile-->
<!--00-->
```verse
Array.Remove(From:int, To:int)<decides>:[]t
```

<!--versetest-->
<!-- 38 -->
```verse
NumArray := array{10, 20, 30, 40}

if (Updated := NumArray.Remove[1,2]):
    # Updated is array{10, 30, 40}

# Negative index would fail
# if (not NumArray.Remove[-1,0]):

# Out of bounds would fail
# if (not NumArray.Remove[6,10]):
```

`ReplaceFirstElement()` replace first occurrence:

<!--NoCompile-->
```verse
Array.ReplaceFirstElement(OldValue:t, NewValue:t where t:subtype(comparable))<decides>:[]t
```

<!--versetest-->
<!-- 39 -->
```verse
NumArray := array{1, 2, 3, 1, 2, 3}

if (Updated := NumArray.ReplaceFirstElement[2, 99]):
    # Updated is array{1, 99, 3, 1, 2, 3}

if (not NumArray.ReplaceFirstElement[0, 99]):
    # Element not found - fail
```

`ReplaceAllElements()` replace all occurrences:

<!--NoCompile-->
```verse
Array.ReplaceAllElements(OldValue:t, NewValue:t where t:subtype(comparable)):[]t
```

<!--versetest-->
<!-- 40 -->
```verse
NumArray := array{1, 2, 3, 1, 2, 3}
Updated := NumArray.ReplaceAllElements(2, 99)
# Updated is array{1, 99, 3, 1, 99, 3}

# Returns unchanged array if element not found
Same := NumArray.ReplaceAllElements(0, 99)
# Same is array{1, 2, 3, 1, 2, 3}
```

`ReplaceElement()` replaces at specific index:

<!--NoCompile-->
```verse
Array.ReplaceElement(Index:int, NewValue:t)<decides>:[]t
```

<!--versetest-->
<!-- 41 -->
```verse
NumArray := array{10, 20, 30, 40}

if (Updated := NumArray.ReplaceElement[1, 99]):
    # Updated is array{10, 99, 30, 40}

if (not NumArray.ReplaceElement[-1, 99]):
    # Negative index fails

if (not NumArray.ReplaceElement[10, 99]):
    # Out of bounds fails
```

`ReplaceAll()` is a pattern-based replacement:

<!--versetest-->
<!-- 42 -->
```verse
NumArray := array{1, 2, 3, 4, 2, 3, 5}
Pattern := array{2, 3}
Replacement := array{99}
Updated := NumArray.ReplaceAll(Pattern, Replacement)
Updated = array{1, 99, 4, 99, 5}

# Works with different length patterns
NumArray2 := array{1, 2, 2, 1, 2, 2, 1}
Updated2 := NumArray2.ReplaceAll(array{2, 2}, array{9, 9, 9})
Updated2 = array{1, 9, 9, 9, 1, 9, 9, 9, 1}

# Strings are []char
SomeMessage := "Hey, this is a string, Hello!"
NewMessage := SomeMessage.ReplaceAll("He", "Apples") # Note: Case sensitive!
NewMessage = "Applesy, this is a string, Applesllo!"
```

`ReplaceAll()` finds contiguous subsequences matching `Pattern` and replaces each with `Replacement`. The replacement can be any length, including empty.

`Insert()` inserts an element at a specific position:

<!--NoCompile-->
<!-- 00 -->
```verse
Array.Insert(Index:int, Element:[]t)<decides>:[]t
```

<!--versetest-->
<!-- 43 -->
```verse
NumArray := array{10, 20, 40}

if (Updated := NumArray.Insert[2, array{30}]):
    # Updated is array{10, 20, 30, 40}
    # Inserted at index 2, existing elements shift right

# Can insert at start
if (Updated2 := NumArray.Insert[0, array{5}]):
    # Updated2 is array{5, 10, 20, 40}

# Can insert at end (index = Length is valid)
if (Updated3 := NumArray.Insert[NumArray.Length, array{50}]):
    # Updated3 is array{10, 20, 40, 50}

# Out of bounds fails
if (not NumArray.Insert[-1, array{5}]):
    # Negative index fails

if (not NumArray.Insert[NumArray.Length + 1, array{5}]):
    # Beyond Length fails
```

The `Concatenate()` function combines an array of arrays into a single flat array:

<!--versetest
M():void =
    Result := Concatenate(array{1}, array{2, 3})
<#
-->
<!-- 44 -->
```verse
Concatenate(Arrays:[][]t):[]t
```
<!-- #> -->

Thanks to tuple-to-array coercion, you can pass multiple array arguments directly and they are automatically gathered into the array-of-arrays parameter. Unlike the `+` operator which joins exactly two arrays, `Concatenate()` accepts any number of array arguments:

<!--versetest-->
<!-- 45 -->
```verse
# Empty call returns empty array
Empty := Concatenate()
Empty = array{}

# Single array passed as an array-of-arrays
Single := Concatenate(array{array{1, 2, 3}})
Single = array{1, 2, 3}

# Two arrays
TwoArrays := Concatenate(array{1, 2}, array{3, 4})
TwoArrays = array{1, 2, 3, 4}

# Multiple arrays
Many := Concatenate(array{1}, array{2, 3}, array{4}, array{5, 6})
Many = array{1, 2, 3, 4, 5, 6}
```

Verse handles empty arrays seamlessly:

<!--versetest-->
<!-- 46 -->
```verse
# Empty arrays contribute nothing
Result1 := Concatenate(array{1, 2}, array{}, array{3})
Result1 = array{1, 2, 3}
Result2 := Concatenate(array{}, array{}, array{})
Result2 = array{}

# Can concatenate many empty arrays
# EmptyResult := Concatenate(for (I := 0..100): array{})
# EmptyResult = array{}
```

**Comparison with `+` operator:**

<!--versetest-->
<!-- 48 -->
```verse
# Using + operator (binary)
First := array{1, 2}
Second := array{3, 4}
Third := array{5, 6}
ChainedResult := First + Second + Third  # Works but requires multiple operations

# Using Concatenate
ConcatenatedResult := Concatenate(First, Second, Third)  # Single operation

ChainedResult = ConcatenatedResult
```

Arrays in Verse are thus immutable values with predictable behavior, but through `var` they offer the convenience of mutable variables. They can be concatenated, iterated, sliced, searched, and manipulated, making them one of the most flexible and fundamental data structures in the language.

#### Maps

Maps are one of the core container types, alongside arrays and optionals. If arrays are ordered sequences indexed by integers, and optionals are the smallest container of all, holding either zero or one value, then Maps generalize both ideas: like arrays, they provide efficient lookup, but instead of being limited to integer indices, they allow any *comparable* type as a key. You can think of a map as an array indexed by arbitrary keys, or as a larger optional that can hold many key–value associations at once.

A map is an immutable associative container that stores zero or more key–value pairs of type `[k]v`, written as `(Key:k, Value:v)`. Maps are the standard way to associate values with other values: you supply a key, and the map returns the value associated with it.

Maps are useful whenever you want to store data that is naturally indexed by something other than an integer position. For example, you might want to store the weights of different objects keyed by their names:

<!--versetest-->
<!-- 50 -->
```verse
Empty := map{}

var Weights:[string]float = map{
    "ant" => 0.0001,
    "elephant" => 500.0,
    "galaxy" => 500000000000.0
}
```

Looking up a value in a map uses square brackets. The expression succeeds if the key is present and fails if it is not. Lookups are designed to be fast, with amortized *O(1)* time complexity:

<!--versetest
Weights:[string]float = map{"ant" => 0.0001}
-->
<!-- 51 -->
```verse
Weights["ant"]  # succeeds, since "ant" key exists in map
# Weights["car"] would fail
```

If you want to update a map stored in a variable, you use `set`. This works both for adding a new key–value pair and for changing the value of an existing key. If you try to modify a key that is not present, the operation fails:

<!--versetest-->
<!-- 52 -->
```verse
var Friendliness:[string]int = map{"peach" => 1000}

set Friendliness["pelican"] = 17     # succeed: add a new value with the given key
set Friendliness["peach"] += 2000    # succeed: update an existing value with the given key
# set Friendliness["tomato"] += 1000   # would fail: can't update a value which key does not exist
```

Every map also carries its size, accessible as the `Length` field:

<!--versetest
Friendliness:[string]int = map{"peach" => 1000, "pelican" => 17}
-->
<!-- 53 -->
```verse
Friendliness.Length = 2         # succeed: the map has 2 entries
```

When constructing a map with duplicate keys, only the last value is kept. This is because a map enforces uniqueness of keys, so earlier entries are silently overwritten:

<!--versetest-->
<!-- 54 -->
```verse
WordCount:[string]int = map{
    "apple" => 0,
    "apple" => 1,
    "apple" => 2
}
# WordCount contains only {"apple" => 2}
```

Maps can also be iterated over, letting you traverse all key–value pairs exactly in the order they were inserted:

<!--versetest-->
<!-- 55 -->
```verse
ExampleMap:[string]string = map{
    "a" => "apple",
    "b" => "bear",
    "c" => "candy"
}

for (Key -> Value : ExampleMap):
    Print("{Value} in ExampleMap at key {Key}")
```

This produces:

- "apple in ExampleMap at key a"
- "bear in ExampleMap at key b"
- "candy in ExampleMap at key c"

Sometimes you want to remove an entry from a map. Since maps are immutable, "removing" means creating a new map that excludes the given key. For example, here is a function that removes an element from a `[string]int` map:

<!--versetest-->
<!-- 56 -->
```verse
RemoveKeyFromMap(TheMap:[string]int, ToRemove:string):[string]int =
    var NewMap:[string]int = map{}
    for (Key -> Value : TheMap, Key <> ToRemove):
        set NewMap = ConcatenateMaps(NewMap, map{Key => Value})
    return NewMap
```

The key type of a map must belong to the class `comparable`, which guarantees that two keys can be checked for equality. All basic scalar types such as `int`, `float`, `rational`, `logic`, `char`, and `char32` are comparable, and so are compound types like arrays, maps, tuples, and `struct`s whose components are comparable.  Classes and interfaces (without the `<unique>` specifier) cannot be used as keys, since their instances do not provide a built-in notion of equality. However, classes and interfaces marked with `<unique>` can be used as keys because they support identity-based equality.

Not all types can be used as map keys. A type must be comparable—meaning values of that type can be checked for equality. Here's a comprehensive guide to what can and cannot be used as map keys:

**Types that can be used as map keys:**

- `logic` - boolean values
- `int`, `float`, `rational` - numeric types
- `char`, `char32` - character types
- `string` - text
- Enumerations - custom enum types
- Classes and Interfaces marked with `<unique>`
- `?t` where `t` is comparable - optionals of comparable types
- `[]t` where `t` is comparable - arrays of comparable elements
- `tuple(t0, t1, ...)` where all elements are comparable - tuples of comparable types
- `struct` types where all fields are comparable

##### Map Key Type Examples

The following examples demonstrate various comparable types used as map keys:

**Tuples as keys:**

<!--versetest-->
<!-- 71 -->
```verse
# Coordinate system using tuple keys
Grid:[tuple(int, int)]string = map{
    (0, 0) => "origin",
    (1, 0) => "east",
    (0, 1) => "north",
    (-1, 0) => "west"
}
```

**Structs as keys:**

<!--versetest-->
<!-- 72 -->
```verse
point := struct{X:int, Y:int}
Landmarks:[point]string = map{
    point{X := 0, Y := 0} => "origin",
    point{X := 10, Y := 20} => "tower"
}
```

**Enums as keys:**

<!--versetest-->
<!-- 73 -->
```verse
direction := enum{North, South, East, West}
Instructions:[direction]string = map{
    direction.North => "Go up",
    direction.South => "Go down",
    direction.East => "Turn right",
    direction.West => "Turn left"
}
```

**Rational numbers as keys:**

<!--versetest-->
<!-- 74 -->
```verse
Fractions:[rational]string = map{
    1/2 => "half",
    1/3 => "third",
    2/3 => "two thirds",
    1/1 => "whole"
}
```

Equivalent rational numbers (like `1/1` and `2/2`) are treated as the same key.

**Unicode characters as keys:**

<!--versetest-->
<!-- 75 -->
```verse
Translations:[char32]string = map{
    '😀' => "grinning face",
    '你' => "you (Chinese)",
    '好' => "good (Chinese)"
}
```

**Special float values:**

Float special values like `NaN` and `Inf` can be used as map keys:

<!--versetest-->
<!-- 76 -->
```verse
SpecialFloats:[float]string = map{
    Inf => "positive infinity",
    -Inf => "negative infinity",
    0.0 => "zero"
}
```

**Types that cannot be used as map keys:**

- `false` - the empty type
- `type` - type values themselves
- Function types like `t -> u`
- `subtype(t)` - subtype expressions
- Classes (without `<unique>`)
- Interfaces (without `<unique>`)

Attempting to use a non-comparable type as a key results in a compile-time error.

Like arrays, maps infer their key and value types from the common supertype of all keys and values. When you create a map with mixed but related types, Verse finds the most specific types that encompass all keys and all values:

<!--versetest
class1 := class<unique> {}
class2 := class<unique>(class1) {}
class3 := class<unique>(class1) {}
-->
<!-- 57 -->
```verse
    Instance2 := class2{}
    Instance3 := class3{}

    # Key type is class1 (common supertype of class2 and class3)
    # Value type remains int
    MixedKeyMap : [class1]int = map{Instance2 => 1, Instance3 => 2}
```
##### Ordering and Equality

Maps preserve insertion order, which is significant for both iteration and equality checks. When you insert entries into a map, they maintain the order of insertion. Two maps are equal only if they contain the same key–value pairs **in the same order**:

<!--versetest-->
<!-- 58 -->
```verse
var Scores:[string]int = map{}
set Scores["Alice"] = 100
set Scores["Bob"] = 90
set Scores["Carol"] = 95

# This map equals Scores
Map1 := map{"Alice" => 100, "Bob" => 90, "Carol" => 95}
Scores = Map1

# This map does NOT equal Scores - different order
Map2 := map{"Bob" => 90, "Alice" => 100, "Carol" => 95}
not Scores = Map2
```

When a map literal contains duplicate keys, the last value overwrites earlier values, but the key's position remains from its **first** occurrence:

<!--versetest
-->
<!-- 59 -->
```verse
Map := map{0 => "zero", 1 => "one", 0 => "ZERO", 2 => "two"}
# Equivalent to map{0 => "ZERO", 1 => "one", 2 => "two"}
# The key 0 stays in its original position
```

Iteration over the map will visit entries in their preserved insertion order.

##### Empty Map Types

Empty maps can infer their key and value types from context, similar to arrays:

<!--versetest
-->
<!-- 60 -->
```verse
StringToInt : [string]int = map{}  # Empty map with inferred types

var Scores : [string]int = map{}
set Scores = ConcatenateMaps(Scores, map{"Alice" => 100})
```

Without type context, you may need to provide explicit type annotations.

##### Variance

Maps are **covariant** in both their key and value types. A map type `[K1]V1` is a subtype of `[K2]V2` when:

- **Keys are covariant**: `K1` is a subtype of `K2` (more specific keys → more general keys)
- **Values are covariant**: `V1` is a subtype of `V2` (more specific values → more general values)

This covariance is necessary because map iteration exposes the key type. When you iterate a map, you receive the actual key objects, which must be safely usable as the declared key type.

While map types are covariant, map lookup operations accept keys that are `comparable` to the key type, which may appear contravariant. This is a convenience for lookups but does not affect the variance of the map type itself.

<!--versetest
animal := class<unique> {}
dog := class<unique>(animal) {}
-->
<!-- 61 -->
```verse
# assume
# animal := class<unique> {}
# dog := class<unique>(animal) {}

# Map TYPE variance is COVARIANT
DogMap : [dog]int = map{dog{} => 1}
AnimalMap : [animal]int = DogMap  # ✓ Works - covariant assignment

# Map LOOKUP operations appear contravariant-like
MyDogMap : [dog]int = map{dog{} => 42}
DogKey : dog = dog{}
SupertypeKey : animal = DogKey  # Points to the same dog instance

# Lookup with exact key type:
if (Val1 := MyDogMap[DogKey]) {}  # ✓ Works

# Lookup with supertype key - also works!
if (Val2 := MyDogMap[SupertypeKey]) {}  # ✓ Also works

# This works because lookup only requires the key to be `comparable`
# to the map's key type. Both keys refer to the same unique object.
```

When modifying a mutable map through `set`, you can only insert keys and values that match the map's declared types:

<!--versetest

animal := class<unique> {}
dog := class<unique>(animal) {}

class1 := class<unique> {}
class2 := class<unique>(class1) {}
-->
<!-- 62 -->
```verse
var Map : [dog]int = map{}
Key2 : dog = dog{}
Key1 : animal = Key2

set Map[Key2] = 1      # Valid - exact type match
# set Map[Key1] = 2    # ERROR - cannot use supertype as key
```

##### Nested Maps

Maps can contain other maps as values, enabling multi-level associations:

<!--versetest
-->
<!-- 63 -->
```verse
# Map from strings to maps of ints to strings
NestedMap : [string][int]string = map{
    "numbers" => map{1 => "one", 2 => "two"},
    "letters" => map{0 => "a", 1 => "b"}
}

if (InnerMap := NestedMap["numbers"]):
    if (Value := InnerMap[1]):
        Value = "one"
```

Maps can be used as keys of other maps if all values and keys from it are comparable.

##### Concatenating Maps

The `ConcatenateMaps()` function merges two maps into a single map:

<!--versetest
M():void =
    Map1 := map{1 => "one"}
    Map2 := map{2 => "two"}
    Result := ConcatenateMaps(Map1, Map2)
<#
-->
<!-- 64 -->
```verse
ConcatenateMaps(Map1:[k]v, Map2:[k]v):[k]v
```
<!-- #> -->

`ConcatenateMaps()` takes exactly two maps and combines them into one. When maps contain duplicate keys, values from the **second** map override values from the first:

<!--versetest-->
<!-- 65 -->
```verse
Map1 := map{1 => "one", 2 => "two"}
Map2 := map{3 => "three", 4 => "four"}

Combined := ConcatenateMaps(Map1, Map2)
Combined = map{1 => "one", 2 => "two", 3 => "three", 4 => "four"}

# To merge more than two maps, chain calls
Map3 := map{5 => "five"}
All := ConcatenateMaps(ConcatenateMaps(Map1, Map2), Map3)
All = map{1 => "one", 2 => "two", 3 => "three", 4 => "four", 5 => "five"}
```

**Handling duplicate keys:**

<!--versetest-->
<!-- 66 -->
```verse
Base := map{1 => "original", 2 => "base"}
Override := map{2 => "updated", 3 => "new"}

Result := ConcatenateMaps(Base, Override)
Result = map{1 => "original", 2 => "updated", 3 => "new"}
# Key 2 was overridden by the later map
```

The right-to-left precedence ensures that later maps take priority, enabling a natural override pattern.

**Empty maps:**

<!--versetest-->
<!-- 67 -->
```verse
# Empty maps contribute nothing
FirstMap := map{1 => "a"}
EmptyMap : [int]string = map{}

Combined := ConcatenateMaps(FirstMap, EmptyMap)
Combined = map{1 => "a"}
```

**Type constraints:**

The resulting map type will coerce to the most specific shared type from the input maps:

<!--versetest-->
<!-- 68 -->
```verse
# Maps with the same key and value types
FirstMap := map{1 => "a"}
SecondMap := map{2 => "b"}
Combined := ConcatenateMaps(FirstMap, SecondMap)
Combined = map{1 => "a", 2 => "b"}
```

#### Weak Maps

The `weak_map` type is a specialized supertype of `map` designed for persistent data storage with weak key references. It behaves similarly to ordinary maps for individual entry access, but deliberately restricts bulk operations. You cannot ask for its length, you cannot iterate over its entries, and you cannot use `ConcatenateMaps`. These restrictions enable efficient weak reference semantics and integration with Verse's persistence system.

A `weak_map` is declared with `weak_map(k,v)` and can be initialized from an ordinary `map{}`. Updating and accessing individual entries works the same way as regular maps:

<!--versetest
-->
<!-- 69 -->
```verse
var MyWeakMap:weak_map(int,int) = map{}

set MyWeakMap[0] = 1
Value := MyWeakMap[0]         # succeeds with 1

set MyWeakMap = map{0 => 2}   # reassignment still works (for local variables)
```

Because `weak_map` is a supertype of `map`, you can assign regular maps to weak_map variables when needed, but you lose the ability to count or iterate once you are working with a weak map.

##### Restrictions

**No Length Property:**

<!--versetest
M():void =
    var MyWeakMap:weak_map(int,int) = map{1 => 2}
<#
-->
<!-- 70 -->
```verse
var MyWeakMap:weak_map(int,int) = map{1 => 2}
# ERROR: weak_map has no Length property
# Size := MyWeakMap.Length
```
<!-- #> -->

**No Iteration:**

<!--versetest
M():void =
    var MyWeakMap:weak_map(int,int) = map{1 => 2, 3 => 4}
<#
-->
<!-- 71 -->
```verse
var MyWeakMap:weak_map(int,int) = map{1 => 2, 3 => 4}
# ERROR: Cannot iterate over weak_map
# for (Entry : MyWeakMap) {}
```
<!-- #> -->

**Cannot Coerce to Comparable:**

<!--versetest
M():void =
    var MyWeakMap:weak_map(int,int) = map{}
<#
-->
<!-- 72 -->
```verse
var MyWeakMap:weak_map(int,int) = map{}
# ERROR: weak_map cannot be converted to comparable
# C:comparable = MyWeakMap
```
<!-- #> -->

**Cannot Join with Regular Maps:**

<!--versetest
M():void =
    var MyWeakMap:weak_map(int,int) = map{1 => 2}
<#
-->
<!-- 73 -->
```verse
var MyWeakMap:weak_map(int,int) = map{1 => 2}

# ERROR: Cannot join weak_map with regular map to produce regular map
# Result:[int]int = if (true?) then MyWeakMap else map{3 => 4}
```
<!-- #> -->

##### Module-Scoped weak_map Variables

When using `weak_map` as a module-scoped variable (for persistent data), there are additional restrictions:

**Cannot Read Complete Map:**

<!--versetest
M():void =
    var LocalData:weak_map(int, int) = map{}
    if (set LocalData[1] = 100) {}
<#
-->
<!-- 74 -->
```verse
# Module-scoped persistent weak_map
var PlayerData:weak_map(player, int) = map{}

GetAllData():weak_map(player, int) =
    # ERROR: Cannot read complete module-scoped weak_map
    # PlayerData
    map{}  # Must construct new map instead
```
<!-- #> -->

**Cannot Write Complete Map:**

<!--versetest
M():void =
    var LocalData:weak_map(int, int) = map{}
    set LocalData = map{}
<#
-->
<!-- 75 -->
```verse
var PlayerData:weak_map(player, int) = map{}

ResetAllData():void =
    # ERROR: Cannot replace module-scoped weak_map
    # set PlayerData = map{}
    {}
```
<!-- #> -->

**Individual Entry Access Works:**

<!--versetest
M()<transacts>:void =
    var LocalData:weak_map(int, int) = map{}

    GetScore(Key:int):int =
        if (Score := LocalData[Key]):
            Score
        else:
            0

    SetScore(Key:int, Score:int)<transacts>:void =
        if (set LocalData[Key] = Score) {}
<#
-->
<!-- 76 -->
```verse
var PlayerData:weak_map(player, int) = map{}

# OK: Can read individual entries
GetPlayerScore(Player:player):int =
    if (Score := PlayerData[Player]):
        Score
    else:
        0

# OK: Can write individual entries
SetPlayerScore(Player:player, Score:int):void =
    set PlayerData[Player] = Score
```
<!-- #> -->

This restriction exists because module-scoped weak_maps integrate with the persistence system, which only tracks individual entry updates, not complete map replacements.

For module-scoped `var weak_map` variables, both key and value types have strict requirements:

**Key Type Must Have `<module_scoped_var_weak_map_key>` Specifier:**

<!--versetest
regular_class := class<unique> {}

M():void =
    var LocalData:weak_map(regular_class, int) = map{}
<#
-->
<!-- 77 -->
```verse
# Valid key type
persistent_class := class<unique><allocates><computes><persistent><module_scoped_var_weak_map_key> {}

var ValidData:weak_map(persistent_class, int) = map{}

# Invalid key type - missing specifier
regular_class := class<unique><allocates><computes> {}

# ERROR: Key type lacks <module_scoped_var_weak_map_key>
# var InvalidData:weak_map(regular_class, int) = map{}
```
<!-- #> -->

**Value Type Must Be Persistable:**

<!--versetest
regular_struct := struct:
    Value:int

M():void =
    var LocalData:weak_map(int, regular_struct) = map{}
<#
-->
<!-- 78 -->
```verse
persistent_class := class<unique><allocates><computes><persistent><module_scoped_var_weak_map_key> {}

# Valid: persistable value type
persistable_struct := struct<persistable>:
    Value:int

var ValidData:weak_map(persistent_class, persistable_struct) = map{}

# Invalid: non-persistable value type
regular_struct := struct:
    Value:int

# ERROR: Value type must be persistable
# var InvalidData:weak_map(persistent_class, regular_struct) = map{}
```
<!-- #> -->

Common key types that satisfy the requirements:

- **`player`** - The standard key type for player-specific data
- **`persistent_key`** - Custom persistent keys with validity tracking
- **`session_key`** - Transient keys that do not persist across sessions

##### Covariance

The `weak_map` type is **covariant** in its key type, meaning you can use a weak_map with a subclass key type where a parent class key type is expected:

<!--versetest
base_class := class<unique> {}
derived_class := class(base_class) {}

value_struct := struct {}

CreateDerivedMap():weak_map(derived_class, value_struct) =
    map{}

F():void=
    BaseMap:weak_map(base_class, value_struct) = CreateDerivedMap()
<#
-->
<!-- 79 -->
```verse
base_class := class<unique> {}
derived_class := class(base_class) {}

value_struct := struct {}

CreateDerivedMap():weak_map(derived_class, value_struct) =
    map{}

# OK: weak_map is covariant in key type
BaseMap:weak_map(base_class, value_struct) = CreateDerivedMap()

# ERROR 3509: Cannot go the other way (contravariance)
# DerivedMap:weak_map(derived_class, value_struct) = BaseMap
```
<!-- #> -->

This covariance also allows regular maps to be assigned to weak_maps with compatible key types:

<!--versetest
base_class := class<unique> {}
derived_class := class(base_class) {}
value_struct := struct {}

F():void=
    DerivedKey := derived_class{}
    RegularMap:[derived_class]value_struct = map{DerivedKey => value_struct{}}

    WeakMap:weak_map(base_class, value_struct) = RegularMap
<#
-->
<!-- 80 -->
```verse
DerivedKey := derived_class{}
RegularMap:[derived_class]value_struct = map{DerivedKey => value_struct{}}

# OK: Regular map converts to weak_map with covariant key
WeakMap:weak_map(base_class, value_struct) = RegularMap
```
<!-- #> -->

##### Partial Field Updates

When the value type is a struct or class, you can update individual fields of stored values:

<!--versetest
player_data := class:
    var Level:int = 0
    var Score:int = 0

GetPlayerData()<transacts>:player_data = player_data{}

M()<transacts>:void =
    var LocalData:weak_map(int, player_data) = map{}

    UpdateLevel(Key:int, NewLevel:int)<transacts>:void =
        Data := GetPlayerData()
        set Data.Level = NewLevel
        set Data.Score = 0
        if (set LocalData[Key] = Data) {}

        if (Stored := LocalData[Key]):
            set Stored.Level = NewLevel + 1
<#
-->
<!-- 81 -->
```verse
player_data := struct<persistable>:
    Level:int
    Score:int

var PlayerData:weak_map(player, player_data) = map{}

UpdatePlayerLevel(Player:player, NewLevel:int):void =
    # Set entire struct first
    set PlayerData[Player] = player_data{Level := NewLevel, Score := 0}

    # Then update just one field
    set PlayerData[Player].Level = NewLevel + 1
```
<!-- #> -->

##### Transaction and Rollback Semantics

Like all mutable state in Verse, `weak_map` updates participate in transaction semantics. If a `<decides>` expression fails, all changes are rolled back:

<!--versetest
player := class<unique> {}

F():void=
    var GameData:weak_map(player, int) = map{}
    AttemptUpdate():void =
        if:
            set GameData[player{}] = 100
            set GameData[player{}] = 200
            false?
    AttemptUpdate()
<#
-->
<!-- 82 -->
```verse
var GameData:weak_map(int, int) = map{}

AttemptUpdate():void =
    if:
        set GameData[1] = 100
        set GameData[2] = 200
        false?  # Transaction fails

    # Both updates rolled back
    # GameData[1] still does not exist
    # GameData[2] still does not exist
```
<!-- #> -->

This applies to complete map replacements (for local variables), individual entries, and partial field updates.

## Book of Verse Source Unit: 04_operators.md

### Operators

Operators are functions that perform actions on their operands. They provide concise syntax for common operations like arithmetic, comparison, logical operations, and assignment.

#### Operator Formats

Verse operators come in three formats based on their position relative to their operands:

**Prefix Operators**

Prefix operators appear before their single operand:

- `not Expression` - Logical negation
- `-Value` - Numeric negation
- `+Value` - Numeric positive (for alignment)

**Infix Operators**

Infix operators appear between their two operands:

- `A + B` - Addition
- `A * B` - Multiplication
- `A = B` - Equality comparison
- `A and B` - Logical AND

**Postfix Operators**

Postfix operators bind to the expression on their left. While some (like `.`) appear between two elements, they are classified as postfix because they operate on the left-hand expression:

- `Value?` - Query operator for logic values
- `Object.Member` - Member access (the `.` operates on the object to its left)
- `Array[Index]` - Array indexing (the `[]` operates on the array to its left)
- `Function()` - Function call (the `()` operates on the function to its left)
- `Constructor{}` - Object construction (the `{}` operates on the type to its left)

Although `.` appears *between* `Player` and `Respawn` in `Player.Respawn()`, it is considered postfix because it binds to `Player` and selects a member from it. The right side (`Respawn`) is not a separate operand but a member selector

#### Precedence

When multiple operators appear in the same expression, Verse evaluates them according to their precedence level. Higher precedence operators evaluate first. Operators with the same precedence evaluate left to right (except for assignment and unary operators which are right-associative).

The precedence levels from highest to lowest are:

| Precedence | Operators | Category | Format | Associativity | Example |
|------------|-----------|----------|--------|---------------|--|
| 11 | `.`, `[]`, `()`, `{}`, `?` (postfix) | Member access, Indexing, Call, Construction, Query | Postfix | Left | `BossDefeated?`, `Player.Respawn()`|
| 10 | `+`, `-` (unary), `not` | Unary operations | Prefix | Right | `+Score`, `-Distance`, `not HasCooldown?` |
| 9 | `*`, `/` | Multiplication, Division | Infix | Left | `Score * Multiplier` |
| 8 | `+`, `-` (binary) | Addition, Subtraction | Infix | Left | `X + Y`, `Health - Damage` |
| 7 | `=` (relational), `<>`, `<`, `<=`, `>`, `>=` | Relational comparison | Infix | Right | `Player <> Target`, `Score > 100` |
| 5 | `and` | Logical AND | Infix | Left | `HasPotion? and TryUsePotion[]` |
| 4 | `or` | Logical OR | Infix | Left | `IsAlive? or Respawn()` |
| 3 | `..` | Range | Infix | Left | `0..100`, `-15..50` |
| 2 | ~~Lambda expressions~~ | ~~Function literals~~ (not yet supported) | Special | N/A | N/A |
| 1 | `:=`, `set =` | Assignment | Infix | Right | `X := 15`, `set Y = 25` |

The `=` symbol serves two distinct purposes in Verse:
- **Relational comparison** (precedence 7): When used as an operator in expressions, `A = B` tests equality and returns a logic value
- **Assignment** (precedence 1): When used with the `set` keyword, `set X = Value` assigns a new value to an existing variable

This is different from `:=`, which always means "define and initialize" for new variables. The context determines which meaning of `=` applies.

#### Arithmetic Operators

Arithmetic operators perform mathematical operations on numeric values. They work with both `int` and `float` types, with some special behaviors for type conversion and integer division.

##### Basic Arithmetic

| Operator | Operation | Types | Notes |
|----------|-----------|-------|-------|
| `+` | Addition | `int`, `float` | Also concatenates strings and arrays |
| `-` | Subtraction | `int`, `float` | Can be used as unary negation |
| `*` | Multiplication | `int`, `float` | Converts `int` to `float` when mixed |
| `/` | Division | `int` (failable), `float` | Integer division returns `rational` |

<!--versetest-->
<!-- 01-->
```verse
# Basic arithmetic
Sum := 10 + 20      # 30
Diff := 50 - 15     # 35
Prod := 6 * 7       # 42
Quot := 20.0 / 4.0  # 5.0

# Unary operators
Negative := -42     # -42
Positive := +42     # 42 (for alignment)

# Integer division (failable, returns rational)
if (Result := 10 / 3):
    IntResult := Floor(Result)  # 3

# Type conversion through multiplication
IntValue:int = 42
FloatValue:float = IntValue * 1.0  # Converts to 42.0
```

##### Compound Assignments

Compound assignment operators combine an arithmetic operation with assignment:

| Operator | Equivalent To | Types |
|----------|---------------|-------|
| `set +=` | `set X = X + Y` | `int`, `float`, `string`, `array` |
| `set -=` | `set X = X - Y` | `int`, `float` |
| `set *=` | `set X = X * Y` | `int`, `float` |
| `set /=` | `set X = X / Y` | `float` only |

<!--versetest-->
<!-- 02-->
```verse
var Score:int = 100
set Score += 50    # Score is now 150
set Score -= 25    # Score is now 125
set Score *= 2     # Score is now 250

var Health:float = 100.0
set Health /= 2.0  # Health is now 50.0

# Arrays can use += with both arrays and tuples
var Items:[]int = array{1, 2, 3}
set Items += array{4, 5}  # Items is now array{1, 2, 3, 4, 5}
set Items += (6, 7)       # Items is now array{1, 2, 3, 4, 5, 6, 7}

# Note: set /= does not work with integers due to failable division
# var IntValue:int = 10
# set IntValue /= 2  # Compile error!
```

##### Bitwise Operations

Verse provides bitwise operations for integers through four intrinsic
functions: `BitAnd`, `BitOr`, `BitXor`, and `BitNot`. These operate on
the two's complement binary representation of integers.

<!--versetest-->
<!-- 02001 -->
```verse
# Bitwise AND - sets bit only if both inputs have it set
BitAnd(12, 10) = 8      # 1100 & 1010 = 1000
BitAnd(0, 12345) = 0    # All bits cleared by zero
BitAnd(-1, 42) = 42     # -1 has all bits set (identity)

# Bitwise OR - sets bit if either input has it set
BitOr(12, 10) = 14      # 1100 | 1010 = 1110
BitOr(0, 12345) = 12345 # Identity with zero
BitOr(-1, 42) = -1      # -1 has all bits set

# Bitwise XOR - sets bit if inputs differ
BitXor(12, 10) = 6      # 1100 ^ 1010 = 0110
BitXor(42, 42) = 0      # Same values cancel out
BitXor(-1, 0) = -1      # Flips all bits of zero

# Bitwise NOT - inverts all bits: ~X = -X - 1
BitNot(0) = -1          # All bits flip
BitNot(-1) = 0          # All bits flip back
BitNot(12) = -13        # -(12 + 1) = -13
```

**Important:** Bitwise operations work only with `int` type, not
`float` or `rational`. The operations follow two's complement
arithmetic, where negative numbers are represented with the sign bit
set and remaining bits inverted plus one.

Common patterns using bitwise operations:

<!--versetest-->
<!-- 02002 -->
```verse
# Check if a bit is set (test bit at position N)
Flags := 10                         # 10 = binary 1010: bits 1 and 3 set
BitAnd(Flags, 2) = 2                # Bit 1 is set (2 = binary 0010)
BitAnd(Flags, 4) = 0                # Bit 2 is clear (4 = binary 0100)

# Set a bit (turn on bit at position N)
BitOr(Flags, 1) = 11                # Result: 11 (binary 1011)

# Clear a bit (turn off bit at position N)
BitAnd(Flags, BitNot(8)) = 2        # Result: 2 (binary 0010)

# Toggle a bit (flip bit at position N)
BitXor(Flags, 1) = 11               # Result: 11 (binary 1011)

# Test even/odd (check if lowest bit is set)
BitAnd(Flags, 1) = 0                # Even (lowest bit clear)
```

De Morgan's laws apply to bitwise operations:

<!--versetest-->
<!-- 02003 -->
```verse
# NOT(A AND B) = (NOT A) OR (NOT B)
BitNot(BitAnd(15, 9)) = BitOr(BitNot(15), BitNot(9))

# NOT(A OR B) = (NOT A) AND (NOT B)
BitNot(BitOr(15, 9)) = BitAnd(BitNot(15), BitNot(9))
```

On the Verse VM, bitwise operations support arbitrarily large integers
(bignums beyond 2^64). On the Blueprint VM, values must fit within the
64-bit signed integer range (-2^63 to 2^63-1).

#### Comparison Operators

Comparison operators test relationships between values and are failable expressions that succeed or fail based on the comparison result.

##### Relational Operators

| Operator | Meaning | Supported Types | Example |
|----------|---------|-----------------|---------|
| `<` | Less than | `int`, `float` | `Score < 100` |
| `<=` | Less than or equal | `int`, `float` | `Health <= 0.0` |
| `>` | Greater than | `int`, `float` | `Level > 5` |
| `>=` | Greater than or equal | `int`, `float` | `Time >= MaxTime` |

##### Equality Operators

| Operator | Meaning | Supported Types | Example |
|----------|---------|-----------------|---------|
| `=` | Equal to | All comparable types | `Name = "Player1"` |
| `<>` | Not equal | All comparable types | `State <> idle` |

<!--versetest
HandlePlayerDeath():void={}
EnableAdminMode():void={}
ShowMenu():void={}
UnlockAchievement():void={}
game_state := enum{Playing, Paused}
Score:int = 1500
HighScore:int = 1000
Health:float = 0.0
PlayerName:string = "Admin"
CurrentState:game_state = game_state.Paused
Level:int = 15
-->
<!-- 03-->
```verse
# Numeric comparisons
if (Score > HighScore):
    Print("New high score!")

if (Health <= 0.0):
    HandlePlayerDeath()

# Example with other comparable types
if (PlayerName = "Admin"):
    EnableAdminMode()

if (CurrentState <> game_state.Playing):
    ShowMenu()

# Comparison in complex expressions
if (Level >= 10 and Score > 1000):
    UnlockAchievement()
```

The following types support equality comparison operations (`=` and `<>`):

- Numeric types: `int`, `float`, `rational`
- Boolean: `logic`
- Text: `string`, `char`, `char32`
- Enumerations: All `enum` types
- Collections: `array`, `map`, `tuple`, `option` (if elements are comparable)
- Structs: If all fields are comparable
- Unique classes: Classes marked with `<unique>` (identity equality only)

Comparisons between different types generally fail:

<!--versetest
assert:
    not (0 = 0.0)
    not ("5" = 5)
<#
-->
<!-- 04-->
```verse
0 = 0.0  # Fails: int vs float
"5" = 5  # Fails: string vs int
```
<!-- #>-->

#### Logical Operators

Logical operators work with failable expressions and control the flow of success and failure.

##### Query Operator (`?`)

The query operator checks if a `logic` value is `true` (see [Failure](12_BOOK_OF_VERSE_I_FOUNDATIONS.md#failable-expressions) for how `?` works with other types):

<!--versetest
StartGame():void={}
-->
<!-- 05-->
```verse
var IsReady:logic = true

if (IsReady?):
    StartGame()

# Equivalent to:
if (IsReady = true):
    StartGame()
```

##### Not Operator

The `not` operator negates the success or failure of an expression:

<!--versetest
ContinuePlaying()<computes>:void={}
IsGameOver:?int = option{1}
-->
<!-- 06-->
```verse
if (not IsGameOver?):
    ContinuePlaying()

# Effects are not committed with not
var X:int = 0
if (not (set X = 5, IsGameOver?)):
    # X is still 0 here, even though the assignment "tried" to happen
    Print("X is {X}")  # Prints "X is 0"
```

##### And Operator

The `and` operator succeeds only if both operands succeed:

<!--versetest
EnterRoom()<computes>:void={}
AllowQuestAccess()<computes>:void={}
ProcessResult()<computes>:void={}
HasKey:?int = option{1}
DoorUnlocked:?int = option{1}
player := struct{Level:int, HasItem:?int}
QuickCheck()<computes><decides>:void = {}
ExpensiveCheck()<computes><decides>:void = {}
-->
<!-- 07-->
```verse
Player:player = player{Level:=10, HasItem:=option{1}}
if (HasKey? and DoorUnlocked?):
    EnterRoom()

# Short-circuit evaluation - second operand not evaluated if first fails
if (QuickCheck[] and ExpensiveCheck[]):
    ProcessResult()
```

##### Or Operator

The `or` operator succeeds if at least one operand succeeds:

<!--versetest
OpenDoor()<computes>:void={}
ProcessResult()<computes>:void={}
HasKeyCard:?int = false
HasMasterKey:?int = option{1}
QuickCheck()<computes><decides>:void = {}
ExpensiveCheck()<computes><decides>:void = {}
-->
<!-- 08-->
```verse
if (HasKeyCard? or HasMasterKey?):
    OpenDoor()

# Short-circuit evaluation - second operand not evaluated if first succeeds
if (QuickCheck[] or ExpensiveCheck[]):
    ProcessResult()
```

##### Truth Table

Consider two expressions `P` and `Q` which may either succeed or fail, the following table shows the result of logical operators applied to them:

| Expression P | Expression Q | P and Q | P or Q | not P |
|--------------|--------------|---------|---------|-------|
| Succeeds | Succeeds | Succeeds (Q's value) | Succeeds (P's value) | Fails |
| Succeeds | Fails | Fails | Succeeds (P's value) | Fails |
| Fails | Succeeds | Fails | Succeeds (Q's value) | Succeeds |
| Fails | Fails | Fails | Fails | Succeeds |

#### Assignment and Initialization

When initializing constants and variables, both `=` and `:=` can be used if an explicit type is provided. For type inference (no type annotation), you must use `:=`.

<!--versetest-->
<!-- 09-->
```verse
# Constant initialization with explicit types - both = and := work
MaxHealth:int = 100
PlayerName:string := "Hero"

# Variable initialization with explicit types - both = and := work
var CurrentHealth:int = 100
var Score:int := 0

# Type inference requires := (no type annotation)
AutoTyped := 42  # Inferred as int

# Note: var requires explicit type - var X := value is not allowed
```

The `set =` operator updates variable values:

<!--versetest
vector3:=struct{X:float, Y:float, Z:float}
-->
<!-- 10-->
```verse
var Points:int = 0
set Points = 100

var Position:vector3 = vector3{X := 0.0, Y := 0.0, Z := 0.0}
set Position = vector3{X := 10.0, Y := 20.0, Z := 0.0}
```

#### Special Operators

##### Indexing

The square bracket operator is used for multiple purposes in Verse:

1. **Array/Map indexing** - Access elements in collections
2. **Function calls** - Call functions which may fail

<!--versetest
MyFunction1(X:int, Y:int)<decides>:void={}
MyFunction2(?X:int=0, ?Y:int=0)<decides>:void={}
Arg1:int = 0
Arg2:int = 0
<#
-->
<!-- 11-->
```verse
# Array indexing (failable)
MyArray := array{10, 20, 30}
if (Element := MyArray[1]):
    Print("Element at index 1: {Element}")  # Prints 20

# Map lookup (failable)
Scores:[string]int = map{"Alice" => 100, "Bob" => 85}
if (AliceScore := Scores["Alice"]):
    Print("Alice's score: {AliceScore}")

# String indexing (failable)
Name:string = "Verse"
if (FirstChar := Name[0]):
    Print("First character: {FirstChar}")  # Prints 'V'

# Function call that can fail
Result1 := MyFunction1[Arg1, Arg2]          # Can fail
Result2 := MyFunction2[?X:=Arg1, ?Y:=Arg2]  # Named arguments
EmptyCall := MyFunction2[]                  # and optional values
```
<!-- #>-->

##### Member Access

The dot operator accesses fields and methods of objects:

<!--versetest
player := class<computes>{Health:float = 100.0, GetName()<computes>:string = "Hero"}
vector3 := struct<computes>{X:float, Y:float, Z:float}
config_settings := struct<computes>{MaxPlayers:int = 10}
config := struct<computes>{Settings:config_settings = config_settings{}}
Player:player = player{}
MyVector:vector3 = vector3{X:=1.0, Y:=2.0, Z:=3.0}
Config:config = config{}
-->
<!-- 12-->
```verse
Player.Health
Player.GetName()
MyVector.X
Config.Settings.MaxPlayers
```


##### Range

The range operator creates ranges for iteration:

<!--versetest-->
<!-- 13-->
```verse
# Inclusive range
for (I := 0..4):
    Print("{I}")  # Prints 0, 1, 2, 3, 4
```

##### Object Construction

Verse provides multiple syntaxes for constructing objects. All of the following are equivalent:

<!--versetest
point:=struct{X:int = 0, Y:int = 0}
player_data:=struct{Name:string,Level:int,Health:float}
game_config:=struct{MaxPlayers:int,EnablePvP:logic}
-->
<!-- 14-->
```verse
# Curly braces with commas
Point1 := point{X:= 10, Y:= 20}

# Curly braces with semicolons
Point2 := point{X:= 10; Y:= 20}

# Colon syntax with newlines (no braces)
Point3 := point:
    X:= 10
    Y:= 20

# Colon syntax with commas and newlines
Point4 := point:
    X:= 10,
    Y:= 20

# Fields can be separated by newlines inside braces
Player := player_data {
    Name := "Hero"
    Level := 5
    Health := 100.0
}

# Trailing commas are not allowed
Config := game_config{
    MaxPlayers := 100,
    EnablePvP := true # ,  -- comma not allowed here
}

# Dot syntax for single field (requires defaults for other fields)
Point5 := point . X:=10  # Y gets default value 0
Point6 := point . Y:=20  # X gets default value 0
```

##### Tuple Access

Round braces when used with a single argument after a tuple expression, accesses tuple elements:

<!--versetest-->
<!-- 15-->
```verse
MyTuple := (10, 20, 30)
FirstElement := MyTuple(0)  # Access first element
SecondElement := MyTuple(1)  # Access second element
```

#### Type Conversions

Verse has limited implicit type conversion. Most conversions must be explicit:

<!--versetest-->
<!-- 16-->
```verse
# No implicit int to float conversion
MyInt:int = 42
# MyFloat:float = MyInt  # Error!
MyFloat:float = MyInt * 1.0  # OK: explicit conversion

# No implicit numeric to string conversion
Score:int = 100
# Message:string = "Score: " + Score  # Error!
Message:string = "Score: {Score}"  # OK: string interpolation
```

When operators work with mixed types, specific rules apply:

<!--versetest-->
<!-- 17-->
```verse
# int * float -> float
Result := 5 * 2.0  # Result is 10.0 (float)

# Comparisons must be same type
if (5 = 5):     # OK
if (5.0 = 5.0): # OK
# if (5 = 5.0):   # Fails
```

## Book of Verse Source Unit: 05_mutability.md

### Mutability

Immutability is the default in Verse. When you create a value, it stays that value forever — unchanging, predictable, and safe to share. This foundational principle makes programs easier to reason about, eliminates entire categories of bugs, and enables powerful optimizations. But games are dynamic worlds where state constantly evolves: health decreases, scores increase, inventories change. Verse embraces both paradigms, providing immutability by default while offering controlled, explicit mutation when you need it.

The distinction between immutable and mutable data in Verse goes deeper than just whether values can change. It fundamentally affects how data flows through your program, how values are shared between functions, and how the compiler reasons about your code. Understanding this distinction is crucial for writing efficient, correct Verse programs.

#### The Pure Foundation

In Verse's pure fragment, computation happens without side effects. Values are created but never modified. Functions transform inputs into outputs without changing anything along the way. This is not a limitation — it is a powerful foundation that makes code predictable and composable.

<!--versetest
point := struct{X:float, Y:float}
Distance(P1:point, P2:point)<reads>:float =
    DX := P2.X - P1.X
    DY := P2.Y - P1.Y
    Sqrt(DX * DX + DY * DY)

assert:
    Origin := point{X:=0.0, Y:=0.0}
    UnitX := point{X := 1.0, Y:=0.0}
    UnitY := point{X:=0.0, Y := 1.0}
<#
-->
<!-- 01 -->
```verse
# Immutable values and structures
point := struct<computes>:
    X:float = 0.0
    Y:float = 0.0

# These values are eternal - Origin will always be (0, 0)
Origin := point{}
UnitX := point{X := 1.0}
UnitY := point{Y := 1.0}

Distance(P1:point, P2:point)<reads>:float =
    DX := P2.X - P1.X
    DY := P2.Y - P1.Y
    Sqrt(DX * DX + DY * DY)
```
<!-- #> -->

In this pure world, equality means structural equality — two values are equal if they have the same shape and content. For primitive types and structs, this happens automatically. For classes, which have identity beyond their content, equality requires more careful consideration.

<!--versetest-->
<!-- 02 -->
```verse
# Recursive data structures using classes
linked_list := class:
    Value:int = 0
    Next:?linked_list = false

    # Custom equality check for structural comparison
    Equals(Other:linked_list)<computes><decides>:void =
        Self.Value = Other.Value
        # Both have no next, or both have next and those are equal
        if (Self.Next?):
            Tmp := Self.Next?
            OtherNext := Other.Next?
            Tmp.Equals[OtherNext]
        else:
            not Other.Next?

List1 := linked_list{Value := 1, Next := option{linked_list{Value := 2}}}
List2 := linked_list{Value := 1, Next := option{linked_list{Value := 2}}}

List1.Equals[List2] # This succeeds
```

Pure computation forms the backbone of functional programming in Verse. It's predictable, testable, and parallelizable. When a function is marked `<computes>`, you know it will always produce the same output for the same input, with no hidden dependencies or surprising behaviors.

#### Introducing Mutation

Mutation enters through two keywords: `var` and `set`. The `var` annotation declares that a variable can be reassigned. The `set` keyword performs that reassignment. Together, they provide controlled mutation with clear visibility.

<!--versetest-->
<!-- 03 -->
```verse
Score:int = 100   # Immutable variable - cannot be reassigned
                  # Mutable variable - can be reassigned 
var Health:float = 100.0       # type annotation is required
set Health = 75.0              # Allowed
```

Every use of `var` and `set` has implications for effects. Reading from a `var` variable requires the `<reads>` effect. Using `set` requires both `<reads>` and `<writes>` effects. This is not bureaucracy — it is transparency. The effects make mutation visible in function signatures, so callers know when functions might observe or modify state.

##### Requirements for var Declarations

Mutable variable declarations have strict requirements that prevent common errors:

**Must provide explicit type:**

<!--versetest-->
<!-- 04 -->
```verse
# Valid - explicit type
var X:int = 0

# Invalid - cannot use := syntax with var
# var X := 0  # Error
```

The type inference syntax `:=` cannot be used with `var`. You must explicitly declare the type.

**Must provide initial value (in local scope):**

<!--versetest-->
<!-- 05 -->
```verse
# Valid - initialized
var Health:float = 100.0

# Invalid - no initial value in local scope
# var Score:int  # Error
```

In local scopes (functions, control flow blocks), every `var` declaration requires an initial value. However, when declaring mutable fields in classes or interfaces, the initial value can be omitted and provided during instantiation (see the Classes and Interfaces chapter for details).

**Cannot be completely untyped:**

<!--versetest
assert_semantic_error(3502):
    F():void=
        var X
<#
-->
<!-- 06 -->
```verse
# Invalid - neither type nor value
# var X  
```
<!-- #> -->

##### var Declarations as Expressions

Variable declarations with `var` can be used as expressions, evaluating to their initial value:

<!--versetest-->
<!-- 07 -->
```verse
X := (var Y:int = 42)  # X = 42, Y declared and mutable
X = 42
```

However, `var` declarations **cannot be the target of `set`**:

<!--versetest
assert_semantic_error(3509):
    F():void=
        set (var Z:int = 0) = 1
<#
-->
<!-- 08 -->
```verse
# Invalid - var declarations evaluate to values, not variables
# set (var Z:int = 0) = 1  # Error: cannot use set on a value
```
<!-- #> -->

Since `var` declarations return their initial value as an expression result, you cannot use `set` on them - `set` requires a mutable variable, not a value.

##### set with Block Expressions

The `set` statement can use block expressions, which allows complex computations and side effects:

<!--versetest-->
<!-- 09 -->
```verse
var X:int = 0
var Y:int = 1

set X = block:
    set Y = X      # Side effect: Y becomes 0
    2              # Block result: X becomes 2

X = 2 and Y = 0
```

This pattern is useful when the new value requires intermediate computations or when you need multiple side effects during assignment.

**Important:** Verse evaluates the left-hand side of `set` before the block executes, and assigns the block's return value. This can lead to confusing behavior in certain cases:

<!--versetest-->
<!-- 10 -->
```verse
# Confusing: Setting the same variable inside the block
var X:int = 0
set X = block:
    set X = 5  # X temporarily becomes 5
    2          # But X will be set to 2 (the block result)
X = 2          # The inner set was overwritten!

# Confusing: Modifying index variables used in array access
var Xs:[]int = array{10, 20, 30}
var Index:int = 1
set Xs[Index] = block:
    set Index = 2  # Index changes, but does not affect which element is set
    99
Xs[1] = 99         # Element at original Index (1) was modified, not Xs[2]
Index = 2          # Index is now 2, but too late to affect the assignment
```

To avoid confusion, it is best to avoid modifying the target variable or any variables used in the target expression inside the block.

##### Scope and Redeclaration Restrictions

**No Variable Shadowing:**

Verse does not allow variable shadowing. Once an identifier is declared, you cannot redeclare it with `:=` anywhere in the same scope or any nested scope. This is more restrictive than many languages that allow inner scopes to shadow outer scope variables.

<!--versetest-->
<!-- 11 -->
```verse
var X:int = 0

# Invalid - X already exists in current scope
# X := 1  # Error
```

Unlike many languages, you cannot shadow variables even in nested blocks:

<!--versetest
SomeCondition:logic=false
-->
<!-- 12 -->
```verse
var A:int = 1

if (SomeCondition?):
    # Invalid - A already declared in outer scope
    # A := 2  # Error: cannot shadow A

block:
    # Also invalid - cannot shadow here either
    # var A:int = 2  # Error: ambiguous identifier
```

If you need multiple identifiers with similar purposes, use descriptive names (e.g., `InitialHealth`, `CurrentHealth`) or use qualified names to create separate scopes (see the [Modules and Paths](13_BOOK_OF_VERSE_II_PROGRAM_STRUCTURE_AND_TYPES.md#book-of-verse-source-unit-16modulesmd) chapter for details on qualified names and disambiguation).

**Cannot redeclare with assignment syntax:**

<!--versetest-->
<!-- 13 -->
```verse
var A:int = 1
var B:int = 2

# Invalid - looks like assignment but A already exists
# A := B  # ERROR
```

Use `set A = B` instead to assign to existing mutable variables.

**Cannot nest var declarations:**

<!--versetest
assert_semantic_error(3549):
    var (var X):int = 0
<#
-->
<!-- 14 -->
```verse
# Invalid
# var (var X):int = 0  # ERROR 3549
```
<!-- #> -->

The `var` keyword cannot be nested within itself.

#### Deep vs Shallow Mutability

Verse's approach to mutability differs significantly between structs and classes, reflecting their different roles in the language.

##### Struct Mutability: Deep and Structural

When you declare a struct variable with `var`, you are declaring the entire structure as mutable — the variable itself and all its nested fields, recursively. This deep mutability means you can modify any part of the structure tree.

<!--versetest
point:=struct<computes>{X:float, Y:float}
player_stats := struct<computes>:
    Level:int = 1
    Position:point = point{X:=0.0, Y:=0.0}
    Inventory:[]string = array{}

assert:
    Stats1:player_stats = player_stats{}
    var Stats2:player_stats = player_stats{}
    set Stats2.Level = 2
    Stats2.Level = 2
    set Stats2.Position.X = 100.0
    Stats2.Position.X = 100.0
    set Stats2.Inventory = Stats2.Inventory + array{"Sword"}
    Stats2.Inventory = array{"Sword"}
<#
-->
<!-- 15 -->
```verse
player_stats := struct<computes>:
    Level:int = 1
    Position:point = point{}
    Inventory:[]string = array{}

# Immutable struct variable - nothing can change
Stats1:player_stats = player_stats{}
# set Stats1.Level = 2  # ERROR: Cannot modify immutable struct

# Mutable struct variable - everything can change
var Stats2:player_stats = player_stats{}
set Stats2.Level = 2  # OK
set Stats2.Position.X = 100.0  # OK - nested fields are mutable
set Stats2.Inventory = Stats2.Inventory + array{"Sword"}  # OK
```
<!-- #> -->

When you assign one struct variable to another, Verse performs a deep copy. The two variables become independent, each with their own copy of the data. Changes to one do not affect the other.

<!--versetest
point:=struct<computes>{}
player_stats := struct<computes>:
    Level:int = 1
    Position:point = point{}
    Inventory:[]string = array{}

-->
<!-- 16 -->
```verse
var Original:player_stats = player_stats{Level := 5}
var Copy:player_stats = Original

set Copy.Level = 10
Original.Level = 5   # unchanged, they are independent copies
```

This deep-copy semantics extends to all value types: structs, arrays, maps, and tuples. When you pass a struct to a function, the function receives its own copy. When you store a struct in a container, the container holds a copy. This prevents aliasing and makes reasoning about struct mutations local and predictable.

<!--versetest-->
<!-- 16001 -->
```verse
# Arrays also have value semantics - assignments create copies
var Original:[]int = array{1, 2, 3}
var Copy:[]int = Original

set Copy[0] = 999
Original[0] = 1  # unchanged, they are independent copies
Copy[0] = 999
```

##### Class Mutability: Reference Semantics

Classes behave differently. They have reference semantics — when you assign a class instance, you are sharing a reference to the same object, not creating a copy. The `var` annotation on a class variable only affects whether that variable can be reassigned to reference a different object. It does not affect the mutability of the object's fields.

<!--versetest
game_character := class:
    Name:string = "Hero"
    var Health:float = 100.0
    MaxHealth:float = 100.0

assert:
    Player1:game_character = game_character{}
    set Player1.Health = 50.0
    Player1.Health = 50.0
    var Player2:game_character = Player1
    set Player2 = game_character{Name := "Villain"}
    Player2.Name = "Villain"
    set Player2.Health = 75.0
    Player2.Health = 75.0
<#
-->
<!-- 18 -->
```verse
game_character := class:
    Name:string = "Hero"
    var Health:float = 100.0  # This field is always mutable
    MaxHealth:float = 100.0   # This field is always immutable

# Immutable variable, but mutable fields can still change
Player1:game_character = game_character{}
# set Player1 = game_character{}  # ERROR: Cannot reassign non-var variable
set Player1.Health = 50.0  # OK: Health field is mutable

# Mutable variable allows reassignment
var Player2:game_character = Player1  # Same object
set Player2 = game_character{Name := "Villain"}  # OK: Can reassign
set Player2.Health = 75.0  # OK: Modifies the new object

# Player1 and the original Player2 reference were the same object
# After reassignment, Player2 references a different object
```
<!-- #> -->

The key insight: for classes, the class definition determines field mutability at definition time, not at variable declaration time. A `var` field is always mutable, regardless of how you access it. A non-`var` field is always immutable, even if accessed through a `var` variable.

<!--versetest
point:=struct<computes>{X:float}
container := class:
    ImmutableData:point= point{X:=1.0}
    var MutableData:int = 0
assert:
    Box:container = container{}
    set Box.MutableData = 42
    Box.MutableData = 42
<#
-->
<!-- 19 -->
```verse
container := class:
    ImmutableData:point= point{}  # Always immutable
    var MutableData:int = 0       # Always mutable

# Even through an immutable variable, mutable fields can change
Box:container = container{}
set Box.MutableData = 42         # Allowed
# set Box.ImmutableData = Point{X := 1.0}  # ERROR: Field is immutable
```
<!-- #> -->

##### Collection Mutability: Arrays and Maps

Arrays and maps follow struct semantics—they are values, not references. When you copy a collection, you get an independent copy. Mutations to one copy do not affect the other.

###### Basic Array Mutation

Mutable arrays allow element replacement:

<!--versetest-->
<!-- 20 -->
```verse
var Nums:[]int = array{0, 1}
Nums[0] = 0
Nums[1] = 1

set Nums[0] = 42
Nums[0] = 42
Nums[1] = 1  # Unchanged

set Nums[1] = 666
Nums[0] = 42
Nums[1] = 666
```

You cannot add elements beyond the array's current length:

<!--versetest-->
<!-- 21 -->
```verse
var A:[]int = array{0}
not (set A[1] = 1)  # Fails - index out of bounds
# Must use concatenation: set A = A + array{1}
```

###### Basic Map Mutation

Mutable maps allow both updating existing keys and adding new keys:

<!--versetest-->
<!-- 22 -->
```verse
var Scores:[int]int = map{0 => 1, 1 => 2}
set Scores[1] = 42
Scores[1] = 42

# Adding new keys
set Scores[2] = 100
Scores[2] = 100

# Map with string keys
var Config:[string]int = map{"volume" => 50}
set Config["brightness"] = 75
```

Looking up a non-existent key does not add it:

<!--versetest-->
<!-- 23 -->
```verse
M:[int]int := map{}
not (M[0] = 0)  # Key does not exist, comparison fails
# M is still empty - lookup didn't add the key
```

**Deleting keys from maps:**

Verse does not have a direct "delete" or "remove" operation for maps. To remove keys, create a new map that excludes the unwanted keys by iterating over the original map:

<!--versetest-->
<!-- 24 -->
```verse
var Scores:[string]int = map{"Alice" => 100, "Bob" => 85, "Charlie" => 92}

# Remove "Bob" by creating a new map without that key
var NewScores:[string]int = map{}
for (Name->Score:Scores):
    if (Name <> "Bob"):
        set NewScores[Name] = Score

set Scores = NewScores

# Scores now only contains Alice and Charlie
Scores["Alice"] = 100
Scores["Charlie"] = 92
```

This pattern can be wrapped in a helper function for reusability. See the [Control Flow](12_BOOK_OF_VERSE_I_FOUNDATIONS.md#book-of-verse-source-unit-07controlmd) chapter for more details on `for` loops.

###### Nested Collection Mutation

Collections can be nested, and `set` works through multiple levels:

**Map of arrays:**

<!--versetest-->
<!-- 25 -->
```verse
var Data:[int][]int = map{}
set Data[666] = array{42}
Data[666] = array{42}

# Mutate nested array element
set Data[666][0] = 1234
Data = map{666 => array{1234}}
Data[666] = array{1234}
```

**Array of maps:**

<!--versetest-->
<!-- 26 -->
```verse
var Grid:[][int]int = array{map{}}

# Replace entire map at index
set Grid[0] = map{42 => 666}
Grid[0] = map{42 => 666}
Grid[0][42] = 666

# Add new key to nested map
set Grid[0][1234] = 4321
Grid[0] = map{42 => 666, 1234 => 4321}
Grid[0][42] = 666
Grid[0][1234] = 4321

# Update existing key in nested map
set Grid[0][42] = 1122
Grid[0][42] = 1122
```

**Array of arrays:**

<!--versetest-->
<!-- 27 -->
```verse
var Matrix:[][]int = array{array{1234}}
set Matrix[0][0] = 42
Matrix = array{array{42}}
Matrix[0] = array{42}
Matrix[0][0] = 42

# Replace inner array
set Matrix[0] = array{666}
Matrix[0] = array{666}
Matrix[0][0] = 666
```

All nested levels should exist to use `set`, if any of the higher levels do not exist, the entire set will fail.

<!--versetest-->
<!-- 28 -->
```verse
var Grid:[string][]int = map{"apples"=>array{1,2,3,4}}

set Grid["bananas"] = array{}  # OK - no nesting, just adds new key
set Grid["apples"][2] = 7      # OK - changes nested array element "3" to "7"

# This would fail: set Grid["oranges"][0] = 10
# Error: "oranges" key does not exist, so Grid["oranges"] fails
```

###### Value Semantics for Collections

Extracting a value from a mutable collection creates an independent copy:

<!--versetest-->
<!-- 29 -->
```verse
var X:[][int]int = array{map{42 => 1122, 1234 => 4321}}

# Y gets a copy of the map, not a reference
Y := X[0]
Y = map{42 => 1122, 1234 => 4321}

# Mutating X does not affect Y
set X[0][0] = 111
X[0] = map{42 => 1122, 1234 => 4321, 0 => 111}
Y = map{42 => 1122, 1234 => 4321}  # Unchanged

# Replacing entire element does not affect Y
set X[0] = map{42 => 4242}
X[0] = map{42 => 4242}
Y = map{42 => 1122, 1234 => 4321}  # Still unchanged
```

This is different from class reference semantics—collections copy, classes share.

###### Collections with Mutable Values

When collections contain classes or structs with mutable fields, you can mutate through the collection:

<!--versetest
my_class := class:
    var X:[]int = array{0}
-->
<!-- 30 -->
```verse
C := my_class{}
set C.X[0] = 4266642
C.X[0] = 4266642
```

**Map values with mutable members:**

<!--versetest
my_class := class{  var X:int = 0 }
-->
<!-- 31 -->
```verse
var M:[int]my_class = map{0 => my_class{}}
M[0].X = 0

# Mutate class field through map
set M[0].X = 30
M[0].X = 30
```

The map constructed from a `var` does not track changes to the source variable:

<!--versetest-->
<!-- 32 -->
```verse
var I:int = 42
M:[int]int = map{0 => I}
M[0] = 42

set I = 0
M[0] = 42  # Still 42! Map has a copy of the value
```

##### Arrays of Structs: Independent Copies

When you store structs in an array, each element is an independent copy:

<!--versetest
my_struct := struct<computes>:
    I:int = 10
-->
<!-- 33 -->
```verse
S := my_struct{I := 88}
var A : []my_struct = array{S, S}

# All three have the value 88, but are independent
S.I = 88
A[0].I = 88
A[1].I = 88

# Mutating one does not affect the others
set A[0].I = 99
S.I = 88     # Unchanged
A[0].I = 99  # Changed
A[1].I = 88  # Unchanged
```

##### Arrays of Classes: Shared References

Arrays of classes behave very differently—all references to the same object share mutations:

<!--versetest
my_class := class:
    var I:int = 20
-->
<!-- 34 -->
```verse
C := my_class{}
var A:[]my_class = array{C, C, C}

# All three array elements reference the same object
A[0].I = 20
A[1].I = 20
A[2].I = 20

# Mutating through one affects all references
set A[0].I = 30
A[0].I = 30
A[1].I = 30  # Changed!
A[2].I = 30  # Changed!

set A[1].I = 40
A[0].I = 40  # All three see the change
A[1].I = 40
A[2].I = 40

# Replacing an element breaks the sharing for that element
set A[1] = my_class{}
A[0].I = 40  # Still references original
A[1].I = 20  # New object with default value
A[2].I = 40  # Still references original
```

This is a critical distinction: **structs in collections are copies, classes in collections are shared references**.

##### Compound Assignment Operators

Verse supports compound assignment operators that combine arithmetic with mutation:

<!--versetest
my_struct:= struct<computes>:
    A:int = 10
-->
<!-- 35 -->
```verse
var S:my_struct = my_struct{}

set S.A += 10
S.A = 20

set S.A -= 3
S.A = 17

set S.A *= 4
S.A = 68
```

Available compound operators:

- `set += ` - Addition assignment (int, float, string, array)
- `set -= ` - Subtraction assignment (int, float)
- `set *= ` - Multiplication assignment (int, float)
- `set /= ` - Division assignment (float only)

**Important**: `set /=` does not work with integers because integer division is failable.

Compound assignments work anywhere regular assignment does:

<!--versetest-->
<!-- 36 -->
```verse
var Score:int = 100
set Score += 50
set Score *= 2

var Data:[]int = array{1, 2, 3}
set Data += array{4, 5}  # Array concatenation
Data = array{1, 2, 3, 4, 5}

var Nums:[][]int = array{array{1}}
set Nums[0][0] *= 42
Nums[0][0] = 42
```

Array concatenation with `+=` works on struct fields, nested fields,
and collection values, just like regular `set` does:

<!--versetest-->
<!-- 35001 -->
```verse
my_struct := struct<computes>:
    X:[]int = array{}

my_nested := struct<computes>:
    Inner:my_struct = my_struct{}

# Append to a struct field
var S:my_struct = my_struct{}
set S.X += array{1, 2, 3}
S.X = array{1, 2, 3}

# Append to a nested struct field
var N:my_nested = my_nested{}
set N.Inner.X += array{10, 20}
N.Inner.X = array{10, 20}

# Append to a map value
var M:[int][]int = map{}
set M[42] = array{}
set M[42] += array{1}
set M[42] += array{2}
M[42] = array{1, 2}

# Append to a nested array value
var A:[][]int = array{array{}}
set A[0] += array{1}
set A[0] += array{2}
A[0] = array{1, 2}
```

##### Tuple Mutability: Replacement Only

Tuples can be replaced entirely but individual elements cannot be mutated:

<!--versetest-->
<!-- 37 -->
```verse
var T0:tuple(int, int) = (10, 20)
T0(0) = 10
T0(1) = 20

# Can replace entire tuple
set T0 = (30, 40)
T0(0) = 30
T0(1) = 40
```

**Cannot mutate elements:**

<!--versetest
assert_semantic_error(3509):
    TestTupleMutation()<transacts>:void =
        var T0:tuple(int, int) = (50, 60)
        set T0(0) = 70
<#
-->
<!-- 38 -->
```verse
var T0:tuple(int, int) = (50, 60)
set T0(0) = 70  # ERROR: Cannot mutate tuple elements
```
<!-- #> -->

This restriction applies even when the tuple is mutable. You must replace the entire tuple to change its contents.

##### Map Ordering and Mutation

Maps preserve **insertion order**, and this order is maintained through mutations:

###### New Keys Append to End

<!--versetest-->
<!-- 39 -->
```verse
var M:[int]int = map{2 => 2}

set M[1] = 1  # Appends to end
set M[0] = 0  # Appends to end

# Iteration order is insertion order: 2, 1, 0
Keys := array{2, 1, 0}
var Index:int = 0
for (Key->Value : M):
    Keys[Index] = Key
    set Index += 1

M = map{2 => 2, 1 => 1, 0 => 0}
```

###### Updating Existing Keys Preserves Position

<!--versetest-->
<!-- 40 -->
```verse
var M:[string]int = map{"a" => 3, "b" => 1, "c" => 2}

# Mutating value keeps key position
set M["a"] = 0
M = map{"a" => 0, "b" => 1, "c" => 2}  # Same order

# Another update
set M["c"] = 0
set M["a"] = 2
M = map{"a" => 2, "b" => 1, "c" => 0}  # Still same order
```

###### Order Matters for Equality

Map equality considers both keys/values **and order**:

<!--versetest-->
<!-- 41 -->
```verse
var M:[string]int = map{"a" => 3, "b" => 1, "c" => 2}
set M["a"] = 0

# Same keys and values, same order = equal
M = map{"a" => 0, "b" => 1, "c" => 2}

# Same keys and values, different order = not equal
M <> map{"b" => 1, "c" => 2, "a" => 0}
```

#### Critical Mutability Restrictions

Verse imposes several important restrictions on where and how mutation can occur. These are not arbitrary—they prevent unsound behaviors and maintain type safety.

##### Cannot Mutate Immutable Class Fields

Classes might contain unique pointers or other resources that cannot be safely cloned. Therefore, you cannot mutate immutable fields of a class instance:

<!--versetest
assert_semantic_error(3509):
    classX := class:
        AI:int = 20

    F()<transacts>:void=
        CX:classX = classX{}
        set CX.AI = 30
<#
-->
<!-- 42 -->
```verse
classX := class:
    X:int = 20  # Immutable field

C:= classX{}
C.X = 20
set C.X = 30  # Error: Cannot mutate immutable class field
```
<!-- #> -->

This restriction applies even when the class instance itself is mutable. Only `var` fields of classes can be mutated.

##### Only <computes> Structs Allow Field Mutation

Only structs marked `<computes>` (pure structs) allow field mutation through a variable:

<!--versetest-->
<!-- 43 -->
```verse
# OK: <computes> struct allows field mutation
my_mutable_struct := struct<computes>{M:int = 0, J:float = 3.0}

var S:my_mutable_struct = my_mutable_struct{}

Old := S # makes a copy of the struct

set S.M = 1 # makes a copy of the struct, but updates `M` in the process

S.M = 1 # Succeeds
not (Old = S) # Structs do not pass as references
```

When a new struct is constructed, Verse assigns it the updated value and copies other fields.
If there is other places referencing the old struct, they will not have the updated values (unlike classes)

This restriction ensures that only predictable, effect-free structs can be mutated.

##### Cannot Mutate Through Immutable Class Fields

When mutating nested structures, you cannot mutate through an immutable field of a class (a field not declared with `var`):

<!--versetest
assert_semantic_error(3509):
    inner_struct := struct<computes>{Value:int = 0}
    immutable_class := class:
        Field:inner_struct = inner_struct{}
    outer_struct := struct<computes>:
        C:immutable_class

    F()<transacts>:void=
        var S:outer_struct = outer_struct{C := immutable_class{}}
        set S.C.Field.Value = 10
<#
-->
<!-- 44 -->
```verse
struct0 := struct<computes>{A:int = 10}
struct1 := struct<computes>{S0:struct0 = struct0{}}
class0 := class{CI:struct1 = struct1{}}  # Class with immutable field CI
struct2 := struct<computes>{C0:class0 = class0{}}
struct3 := struct<computes>{S2:struct2 = struct2{}}

var S3:[]struct3 = array{struct3{}, struct3{}}
set S3[1].S2.C0.CI.S0.A = 7  # ERROR: Cannot mutate through immutable field CI
```
<!-- #> -->

The error occurs because `CI` is an immutable field (not declared with `var`). **However**, you CAN mutate through `var` fields of a class in the mutation path.

Even with a mutable index, you cannot mutate an immutable array:

<!--NoCompile-->
<!-- 45 -->
```verse
var I:int = 2  # Mutable index
A:[]int = array{5, 6, 7}  # Immutable array
set A[I] = 2  # ERROR: A is not var - mutability of I does not matter
```

The array itself must be declared `var` to allow element mutation:

<!--versetest-->
<!-- 46 -->
```verse
I:int = 2
var A:[]int = array{5, 6, 7}
set A[I] = 2  # OK: A is var
```

#### Identity and Uniqueness

The `<unique>` specifier gives classes identity-based equality. Without it, classes can't be compared for equality at all (you'd need to write custom comparison methods). With it, equality means identity — two references are equal only if they refer to the exact same object.

<!--versetest
unique_item := class<unique>:
    var Count:int = 0
assert:
    Item1:unique_item = unique_item{}
    Item2:unique_item = Item1
    Item3:unique_item = unique_item{}
    Item1 = Item2
    not (Item1 = Item3)
<#
-->
<!-- 47 -->
```verse
unique_item := class<unique>:
    var Count:int = 0

Item1:unique_item = unique_item{}
Item2:unique_item = Item1  # Same object
Item3:unique_item = unique_item{}  # Different object

if (Item1 = Item2):
    Print("Same object")  # This prints

if (Item1 = Item3):
    Print("Same object")  # This does not print - different objects
```
<!-- #> -->

This identity-based equality is crucial for game objects that need distinct identities even when their data is identical. Two monsters might have the same stats, but they are still different monsters.

## Book of Verse Source Unit: 06_functions.md

### Functions

Functions are reusable code blocks that perform actions and produce
outputs based on inputs. Think of them as abstractions for behaviors,
much like ordering food from a menu at a restaurant. When you order,
you tell the waiter what you want from the menu, such as
`OrderFood("Ramen")`. You do not need to know how the kitchen prepares
your dish, but you expect to receive food after ordering. This
abstraction is what makes functions powerful - you define the
instructions once and reuse them in different contexts throughout your
code.

#### Parameters

Functions can accept any number of parameters, from none at all to as
many as needed. The syntax follows a straightforward pattern where
each parameter has an identifier and a type, separated by commas:

<!--versetest-->
<!-- 01-->
```verse
ProcessData(Name:string, Age:int, Score:float):string =
    "{Name} is {Age} years old with a score of {Score}"
```

For functions with many parameters or optional configuration, Verse
supports named and default parameters.

##### Named Parameters

Named parameters with defaults make functions more flexible and
ergonomic. They allow you to:

- Specify arguments by name rather than position
- Provide default values for optional parameters
- Call functions with only the arguments you need
- Add new optional parameters without breaking existing code

Named parameters are declared with a `?` prefix and called with the
name and a `:=` followed by a value:

<!--versetest-->
<!-- 02-->
```verse
# A function with named parameters
Greet(?Name:string, ?Greeting:string):string = "{Greeting} {Name}!"

# A call with named arguments 
Greet(?Name := "Alice", ?Greeting := "Hello") 
```

Named parameters with default values are truly optional:

<!--versetest-->
<!-- 03-->
```verse
# Named parameters with defaults
Log(Message:string, ?Level:int=1, ?Color:string="white"):string =
    "[Level {Level}] {Message} ({Color})"

# Call with all defaults
Log("Starting")                          # Returns "[Level 1] Starting (white)"

# Call with some arguments
Log("Warning", ?Level:=2)                # Returns "[Level 2] Warning (white)"

# Call with arguments in any order
Log("Error", ?Color:="red", ?Level:= 3)  # Returns "[Level 3] Error (red)"
```

After the first named parameter, all subsequent parameters must also be named:

<!--versetest
assert_semantic_error(3629):
    Invalid(?Named:int, Positional:string):void = {}
<#
-->
<!-- 04-->
```verse
# Invalid: named followed by positional
Invalid(?Named:int, Positional:string):void = {}  # ERROR
```
<!-- #>-->

When calling functions with named parameters, you must use the
`?Name:=Value` syntax. All parameters without default must be specified.
Positional arguments come first:

<!--versetest
Configure(Required:int, ?Option1:string = "", ?Option2:logic = false):void = {}
<#
-->
<!-- 07-->
```verse
Configure(Required:int, ?Option1:string, ?Option2:logic):void = { }

# Valid
Configure(42, ?Option1:="test", ?Option2:=true)

# Invalid: named arg before positional
Configure(?Option1:="test", 42, ?Option2:=true)  # ERROR
```
<!-- #>-->

Verse evaluates default values in the function's defining scope; they
can reference:

  - Module-level definitions
  - Class or interface members
  - Earlier parameters

<!--versetest
ModuleTimeout:int = 30

Connect(?Host:string = "localhost", ?Timeout:int = ModuleTimeout):void = {}

game_config := class:
    DefaultLives:int = 3

    StartGame(?Lives:int = DefaultLives)<transacts>:void = {}

CreateRange(?Start:int = 0, ?End:int = Start + 10):[]int =
    array{Start, End}
<#
-->
<!-- 09-->
```verse
# Module-level definition
ModuleTimeout:int = 30

# Access module-level definition
Connect(?Host:string, ?Timeout:int = ModuleTimeout):void =...

# Access member definition
game_config := class:
    DefaultLives:int = 3

    StartGame(?Lives:int = DefaultLives):void =...

# Access earlier parameter
CreateRange(?Start:int, ?End:int = Start + 10):[]int =...
```
<!-- #>-->

Default values work with overridden members in class hierarchies:

<!--versetest
base_game := class:
    DefaultSpeed:float = 1.0

    Move(?Speed:float = DefaultSpeed)<transacts>:void = {}

fast_game := class(base_game):
    DefaultSpeed<override>:float = 2.0
<#
-->
<!-- 13-->
```verse
base_game := class:
    DefaultSpeed:float = 1.0

    Move(?Speed:float = DefaultSpeed):void =...
    # Uses DefaultSpeed from current instance

fast_game := class(base_game):
    DefaultSpeed<override>:float = 2.0

base_game{}.Move()         # Uses 1.0
fast_game{}.Move()         # Uses 2.0 (overridden value)
```
<!-- #>-->

Named and default parameters interact with the type system.  A
function with default parameters is a subtype of the same function
without those parameters:

<!--versetest-->
<!-- 14-->
```verse
Process(?Required:int, ?Optional:int = 0):int = Required + Optional

# Can assign to type without optional parameter
F1:type{_(?Required:int):int} = Process
F1(?Required := 5)                          # Returns 5 (uses default)

# Can assign to type with optional parameter
F2:type{_(?Required:int, ?Optional:int):int} = Process
F2(?Required := 5, ?Optional := 3)          # Returns 8

# Can even assign to type with no parameters (all have defaults)
DefaultAll(?A:int = 1, ?B:int = 2):int = A + B
F3:type{_():int} = DefaultAll
F3()                                        # Returns 3
```

Function types preserve named parameter names:

<!--versetest-->
<!-- 15-->
```verse
Calculate(?Amount:float, ?Rate:float):float = Amount * Rate

# Valid: names match
F1:type{_(?Amount:float, ?Rate:float):float} = Calculate

# Invalid: different names
# F2:type{_(?Value:float, ?Factor:float):float} = Calculate  # ERROR
```

Function types do not include default values:

<!--versetest-->
<!-- 16-->
```verse
F1(?X:int=1):int = X

F2:type{_(?X:int=99):int} = F1    # F1 and F2 are of the same type
```

Named parameters participate in function overload resolution:

<!--versetest-->
<!-- 17-->
```verse
Process(Value:int):string = "One parameter"
Process(Value:int, ?Option:string):string = "Two parameters"
Process(Value:int, ?Option1:string, ?Option2:logic):string = "Three parameters"

Process(42)                                        # Calls first overload
Process(42, ?Option := "test")                     # Calls second overload
Process(42, ?Option1 := "test", ?Option2 := true)  # Calls third overload
```

The compiler selects the overload that matches the provided
arguments. Named parameters make overload resolution more precise
since names must match exactly.

Named parameters have specific rules for *overload distinctness* that
differ from positional parameters. Two function signatures are
considered **indistinct** (cannot overload) if they could be called
with the same arguments.

**Order does not matter for named parameters:** Named parameters are
matched by name, not position, so reordering does not create
distinctness:

<!--versetest
assert_semantic_error(3532):
    F(?Y:int, ?X:int):int = X + Y
    F(?X:int, ?Y:int):int = X - Y
<#
-->
<!-- 18-->
```verse
# Not distinct - same parameters, different order
F(?Y:int, ?X:int):int = X + Y
F(?X:int, ?Y:int):int = X - Y  # ERROR
```
<!-- #>-->

**Defaults do not create distinctness:** The presence or absence of
default values does not make signatures distinct if the parameter names
are the same:

<!--versetest
assert_semantic_error(3532):
    F(?X:int=42):int = X
    F(?X:int):int = X
<#
-->
<!-- 19-->
```verse
# Same parameter name with/without default
F(?X:int=42):int = X
F(?X:int):int = X  # ERROR
```
<!-- #>-->

**The all-defaults rule:** If all parameters in both overloads have
default values, the signatures are indistinct because both can be
called with no arguments:

<!--versetest
assert_semantic_error(3532):
    F(?X:int=42):int = X
    F(?Y:int=42):int = Y
<#
-->
<!-- 20-->
```verse
# ERROR Both can be called as F()
# F(?X:int=42):int = X
# F(?Y:int=42):int = Y         # ERROR

# ERROR Both callable with no args
# F(?X:int=42):int = X
# F(?X:float=3.14):float = X  # ERROR
```
<!-- #>-->

**Different parameter names are distinct:** Functions with different
named parameter names can overload:

<!--versetest-->
<!-- 22-->
```verse
# Valid: Different names
F(?X:int):int = X
F(?Y:int):int = Y  # OK - distinct parameter names
```

**Named vs positional parameters are distinct:** A named parameter is
distinct from a positional parameter, even with the same name and
type:

<!--versetest-->
<!-- 23-->
```verse
# Valid: Named vs positional
F(?X:int):int = X
F(X:int):int = X  # OK
```

**At least one required parameter must differ:** If the set of
required (no default) named parameters differs, the overloads are
distinct:

<!--versetest-->
<!-- 24-->
```verse
# Valid: First requires ?Y, second does not
F(?Y:int, ?X:int=42):int = X
F(?X:int):int = X  # OK - different required parameter set
```

**Positional parameters create distinctness:** Different positional
parameter types make signatures distinct, even if named parameters are
the same:

<!--versetest-->
<!-- 25-->
```verse
# Valid: Different positional parameter types
F(Arg:float, ?X:int):int = X
F(Arg:int, ?X:int):int = X  # OK
```

**Superset of calls:** If one signature can handle all the calls that
another can, they are indistinct:

<!--versetest
assert_semantic_error(3532):
    F(?Y:int=42, ?X:int=42):int = X
    F(?X:int):int = X
<#
-->
<!-- 26-->
```verse
# ERROR 3532: First can handle all calls to second
# F(?Y:int=42, ?X:int=42):int = X
# F(?X:int):int = X  # ERROR - can call first as F(?X := 10)
```
<!-- #>-->

##### Tuple as Arguments

Tuples can be used to provide positional arguments. However, you
cannot mix a pre-constructed tuple variable with additional named
arguments:

<!--versetest-->
<!-- 28-->
```verse
Calculate(A:int, B:int, ?C:int = 0):int = A + B + C

# Valid: tuple provides positional arguments
Args:tuple(int, int) = (1, 2)
Calculate(Args)  # Returns 3

# Valid: all arguments provided directly
Calculate(1, 2, ?C := 5)  # Returns 8

# Invalid: cannot mix tuple variable with named arguments
# Calculate(Args, ?C := 5)  # ERROR
```

Functions can destructure tuple parameters directly in the parameter
list, allowing you to extract tuple elements inline without manual
indexing:

<!--versetest-->
<!-- 29-->
```verse
# Destructure tuple parameter in place
Func(A:int, (B:int, C:int), D:int):int =
    A + B + C + D

Func(1, (2, 3), 4)        # Direct tuple literal - returns 10
X := (2, 3)
Func(1, X, 4)             # Tuple variable - returns 10
Y := (1, (2, 3), 4)
Func(Y)                   # Entire argument list as tuple - returns 10
```

The parameter `(B:int, C:int)` destructures the tuple, giving direct
access to `B` and `C` instead of requiring `Tuple(0)` and `Tuple(1)`
indexing.

Tuples can be destructured to arbitrary depth:

<!--versetest-->
<!-- 30-->
```verse
# Simple nesting
H(A:int, (B:int, (C:int, D:int)), E:int):int =
    A + B + C + D + E

H(1, (2, (3, 4)), 5)              # Returns 15
T := (2, (3, 4))
H(1, T, 5)                        # Returns 15
T2 := (1, (2, (3, 4)), 5)
H(T2)                             # Returns 15
```

You can mix destructured tuple parameters with regular tuple
parameters that are not destructured:

<!--versetest-->
<!-- 31-->
```verse
# Destructured form - access elements directly
F(A:int, (B:int, C:int), D:int):int =
    A + B + C + D

# Non-destructured form - use tuple indexing
G(A:int, T:tuple(int, int), D:int):int =
    A + T(0) + T(1) + D

# Both work identically
F(1, (2, 3), 4)  # Returns 10
G(1, (2, 3), 4)  # Returns 10
```

Choose destructured form when you need direct access to individual
elements, and non-destructured when you need to pass the tuple as a
whole to other functions.

Tuple parameters can contain named/optional parameters, allowing for
flexible APIs that combine structural decomposition with optional
values:

<!--versetest-->
<!-- 32-->
```verse
# Named parameter inside nested tuple
SumValues(A:int, (X:int, (Y:int, ?Z:int = 0))):int =
    A + X + Y + Z

# Can provide Z explicitly
SumValues(1, (2, (3, ?Z := 4)))  # Returns 10

# Can omit Z to use default
SumValues((1, (2, 3)))           # Returns 6
```

A tuple can contain multiple named parameters, and they can be
specified in any order:

<!--versetest-->
<!-- 33-->
```verse
ProcessData(Base:int, (Items:[]int, ?Scale:int = 1, ?Offset:int = 0)):int =
    if (First := Items[0]):
        First * Scale + Offset + Base
    else:
        Base

Data := array{100, 200}

ProcessData(10, Data)                              # Uses defaults: 110
ProcessData(10, (Data, ?Scale := 2))               # 210
ProcessData(10, (Data, ?Offset := 5))              # 115
ProcessData(10, (Data, ?Scale := 2, ?Offset := 5)) # 215
ProcessData(10, (Data, ?Offset := 5, ?Scale := 2)) # 215 (order does not matter)
```

When a tuple parameter contains **only** named parameters (no
positional parameters), you must provide an empty tuple `()` even when
using all defaults:

<!--versetest-->
<!-- 34-->
```verse
# Tuple with only named parameters
Configure(Base:int, (?Width:int = 10, ?Height:int = 20)):int =
    Base + Width + Height

# Must provide empty tuple when using all defaults
Configure(5, ())  # Returns 35

# Cannot omit the tuple entirely
# Configure(5)  # ERROR - tuple parameter required
```

This is a known limitation in the current implementation. When the
tuple contains at least one positional parameter, this restriction
does not apply.

##### Flattening and Unflattening

Verse provides automatic conversion between tuples and multiple
arguments at function call sites, enabling flexible calling
conventions without explicit packing or unpacking.

*Flattening:* A function expecting multiple parameters can be called
with a single tuple. In the following, the tuple `Args` is
automatically unpacked into the `Add` function's parameters:

<!--versetest-->
<!-- 36-->
```verse
Add(X:int, Y:int):int= X + Y
Args:= (3, 5)
Add(Args)       # Returns 8 - tuple automatically flattened
```

*Unflattening:* A function expecting a single tuple parameter can be
called with flattened arguments.  The individual arguments of the call
to `F` are automatically packed into the tuple parameter:

<!--versetest-->
<!-- 37-->
```verse
F(P:tuple(int, int)):int = P(0) + P(1)

F(3, 5)  # Returns 8 - args automatically packed into tuple
```

The empty tuple has the same flattening behavior:

<!--versetest-->
<!-- 39-->
```verse
F(X:tuple()):int = 42

F(())   # Explicit empty tuple
F()     # No arguments - automatically creates empty tuple
```

**Overload restrictions:** Because of automatic flattening and
unflattening, you cannot define overloads that would be ambiguous. If
you define `F(P:tuple(int, int))`, you cannot also define `F(X:int,
Y:int)` because the call `F(3, 5)` could match either signature.
Similarly, `F(P:tuple(int, int))` and `F(Xs:[]int)` are indistinct
because arrays can also be called with the same syntax.

##### Evaluation Order

Verse evaluates arguments in a specific order to maintain predictable behavior:

1. *Positional arguments*: Left to right in the call
2. *Named arguments*: Left to right as encountered in the call
3. *Default values*: Filled in for omitted parameters, left to right
   in parameter order

If named arguments appear in a different order than parameters, the
compiler uses temporary variables to preserve the evaluation order you
specified:

<!--versetest-->
<!-- 40-->
```verse
Process(A:int, ?B:int, ?C:int, ?D:int):string =
    "{A}, {B}, {C}, {D}"

# Call with reordered named args
Process(1, ?D := 4, ?B := 2, ?C := 3)

# Evaluation order: 1, 4, 2, 3 (as written)
# But passed to function in parameter order: 1, 2, 3, 4
```

This ensures that side effects in argument expressions happen in the
order you write them, not in parameter order.

#### Extension Methods

Extension methods allow you to add new methods to existing types
without modifying their original definitions. This powerful feature
enables you to extend any type in Verse—including built-in types like
`int`, `string`, arrays, and maps—with custom functionality while
maintaining clean separation between different concerns.

Extension methods are particularly valuable when:

- You want to add domain-specific operations to built-in types
- You need to extend types from libraries you do not control
- You're building fluent or builder-style APIs
- You want to organize related functionality separately from type definitions

Extension methods use a special syntax where the extended type appears
in parentheses before the method name:

<!--versetest-->
<!-- 41-->
```verse
# Extend int with a custom method
(Value:int).Double()<computes>:int = Value * 2

# Call the extension method using dot notation
X := 5
Y := X.Double()  # Returns 10

# Can also call on literals
Z := 7.Double()  # Returns 14
```

The type in parentheses can be any Verse type: primitives, tuples,
classes, interfaces, arrays, maps, or structs.

Extending primitives:

<!--versetest-->
<!-- 42-->
```verse
(N:int).IsEven()<decides><computes>:void = Mod[N,2] = 0
(S:string).FirstChar()<decides><computes>:char = S[0]

42.IsEven[]           # Succeeds
"Hello".FirstChar[] = 'H' 
```

Extending tuples:

<!--versetest-->
<!-- 43-->
```verse
# Extend a specific tuple type (Note: Sqrt is <reads>)
(Point:tuple(int, int)).Distance()<reads>:float =
    Sqrt( (Point(0) * Point(0) + Point(1) * Point(1)) * 1.0)

(3, 4).Distance()  # Returns 5.0
```

When extending tuples, you must specify the tuple type
explicitly (e.g., `(Point:tuple(int, int))`). You cannot use
destructured parameter syntax (e.g., `(X:int, Y:int)`) for extension
method contexts.

The empty tuple `tuple()` represents the unit type and can have
extension methods:

<!--versetest-->
<!-- 49-->
```verse
(Unit:tuple()).GetMagicNumber():int = 42

().GetMagicNumber()  # Returns 42
```

Extending arrays:

<!--versetest-->
<!-- 44-->
```verse
(Vals:[]int).Sum()<transacts>:int =
    var Total:int = 0
    for (N:Vals):
        set Total += N
    Total

array{1, 2, 3, 4, 5}.Sum()  # Returns 15
```
Extending maps:

<!--versetest-->
<!-- 45-->
```verse
(M:[int]string).Keys()<computes>:[]int =
    for (Key->X:M):
        Key

map{1=>"a", 2=>"b", 3=>"c"}.Keys()  # Returns array{1, 2, 3}
```

Extending classes:

<!--NoCompile-->
<!--246-->
```verse
player := class:
    Name:string
    var Score:int
```

<!--versetest
player := class:
    Name:string
    var Score:int
-->
<!-- 46-->
```verse
# Add method to existing class
(P:player).AddScore(Points:int):void =
    set P.Score += Points

Player1 := player{Name := "Alice", Score := 100}
Player1.AddScore(50)  # Score becomes 150
```

Extension methods support all parameter features including named and
default parameters:


<!--versetest
<#
-->
<!-- 47-->
```verse
#(Text:string).Pad(?Left:int = 0, ?Right:int = 0):string = ...

"Hello".Pad(?Left:=5)               # "     Hello"
"Hello".Pad(?Right:=5)              # "Hello     "
"Hello".Pad(?Left:= 2, ?Right:=3)   # "  Hello   "
```
<!-- #>-->

##### Overloading

You can define multiple extension methods with the same name for
different types:

<!--versetest-->
<!-- 48-->
```verse
# Overloaded Extension method for different types
(N:int).Format():string = "int:{N}"
(B:logic).Format():string = if (B?) {"logic:true"} else {"logic:false"}

42.Format()      # Returns "int:42"
true.Format()    # Returns "logic:true"
```

The compiler selects the appropriate overload based on the receiver type.

##### Rules

**Must be called**: Extension methods cannot be referenced as
first-class values without calling them:

<!--versetest-->
<!-- 50-->
```verse
(N:int).Double():int = N * 2

# Valid: calling the method
X := 5.Double()

# Invalid: referencing without calling
# F := 5.Double  # ERROR
```

**Conflicts with Class Methods:** Extension methods cannot have the
same signature as methods defined directly in classes or interfaces:

<!--versetest
player := class:
    Health():int = 100

<#
-->
<!-- 51-->
```verse
player := class:
    Health():int = 100

# Invalid: Conflicts with class method
# (P:player).Health():int = 50  # ERROR
```
<!-- #>-->

This prevents ambiguity and ensures that class methods always take precedence.

**Scope and Visibility:** Extension methods are scoped like regular
functions. They're only visible where they are defined or imported:

<!--versetest
Utils := module:
    (S:string).Reverse<public>():string = S
<#
-->
<!-- 52-->
```verse
# In module A
Utils := module:
    (S:string).Reverse<public>():string =
        # Implementation

# In module B
using { Utils }

"Hello".Reverse()  # Available after importing
```
<!-- #>-->

**Extension Methods in Class Scope:** Extension methods can be defined
inside classes and access class members:

<!--versetest
game_manager := class:
    Multiplier:int = 10

    (Score:int).ScaledScore()<computes>:int =
        Score * Multiplier

    ProcessScore(Value:int)<computes>:int =
        Value.ScaledScore()

M()<transacts>:void={
GM := game_manager{}
GM.ProcessScore(5)
}
<# 
-->
<!-- 53-->
```verse
game_manager := class:
    Multiplier:int = 10

    (Score:int).ScaledScore()<computes>:int =
        Score * Multiplier  # Accesses class field

    ProcessScore(Value:int)<computes>:int =
        Value.ScaledScore()  # Uses extension method

GM := game_manager{}
GM.ProcessScore(5)  # Returns 50
```
<!-- #>-->

This creates a lexical closure where the extension method can
reference the enclosing class's members.

**Tuple Argument Conversion:** When an extension method has multiple 
parameters, you can pass a tuple to provide all arguments at once:

<!--versetest-->
<!-- 54 -->
```verse
point := class<computes>{ X:int; Y:int }

(P:point).Translate(DX:int, DY:int)<allocates>:point =
    point{X := P.X + DX, Y := P.Y + DY}

Origin := point{X := 0, Y := 0}
Delta := (5, 10)
NewPoint := Origin.Translate(Delta)  # Tuple expands to two arguments
```

This works when the tuple type matches the parameter list.

#### Lambdas

Lambda expressions with the `=>` operator are not
supported in the current version of Verse. For creating function
values and closures, use nested functions instead.

Functions are first-class values; they can be stored in variables,
passed as parameters, and returned from other functions. This enables
powerful functional programming patterns including higher-order
functions, callbacks, and composable operations. Currently, these
capabilities are provided through nested functions rather than lambda
expressions.

##### Types, Variance and Effects

Function types follow specific subtyping rules based on *variance*:

- *Parameters are contravariant*: A function accepting more general
  types can substitute for one accepting specific types.

- *Returns are covariant*: A function returning more specific types
  can substitute for one returning general types.


Consider the following three classes:

<!--NoCompile-->
<!--264-->
```verse
animal := class:
    Name:string

dog := class(animal):
    Breed:string

working_dog := class(dog):
    Work:string
```

And some use cases:

<!--versetest
animal := class:
    Name:string
dog := class(animal):
    Breed:string
working_dog := class(dog):
    Work:string

AnimalToDog(X:animal):dog = dog{Name := X.Name, Breed := "Unknown"}
DogToWorkingDog(X:dog):working_dog =
    working_dog{Name := X.Name, Breed := "Unknown", Work := "Guard"}
DogToAnimal(X:dog):animal = X
WorkingDogToDog(X:working_dog):dog = X

TestValid():void =
    var ProcessDog:type{_(:dog):dog} = AnimalToDog
    set ProcessDog = AnimalToDog  # OK: tuple(animal)->dog <: tuple(dog)->dog
    set ProcessDog = DogToWorkingDog  # OK: tuple(dog)->working_dog <: tuple(dog)->dog
<#
-->
<!-- 64 -->
```verse
# Some functions on animals
AnimalToDog(X:animal):dog = dog{Name := X.Name, Breed := "Unknown"}
DogToWorkingDog(X:dog):working_dog = working_dog{Name := X.Name, Breed := "Unknown", Work := "Guard"}
DogToAnimal(X:dog):animal = X
WorkingDogToDog(X:working_dog):dog = X

# Example of valid assignments
var ProcessDog:type{_(:dog):dog} = AnimalToDog

# Valid: Accepts more general (animal), returns exact (dog)
# Contravariant parameter: animal <: dog allows this
set ProcessDog = AnimalToDog  # OK: tuple(animal)->dog <: tuple(dog)->dog

# Valid: Accepts exact (dog), returns more specific (working_dog)
# Covariant return: working_dog <: dog allows this
set ProcessDog = DogToWorkingDog  # OK: tuple(dog)->working_dog <: tuple(dog)->dog


ProcessDog1 := AnimalToDog  # Inferred as type{_(:animal):dog}
set ProcessDog1 = DogToAnimal  # ERROR: incompatible assignment

ProcessDog2 := AnimalToDog  # Inferred as type{_(:animal):dog}
set ProcessDog2 = WorkingDogToDog  # ERROR: incompatible assignment
```
<!--  #> -->

Effects are part of the function type. A function with fewer effects
can be used where a function with more effects is expected - effects
are **covariant** (fewer effects = subtype):

<!--versetest
Pure()<computes>:int = 42
Transactional()<transacts>:int = 42
Suspendable()<suspends>:int = 42
UsePure(F()<computes>:int):int = F()
UseTransactional(F()<transacts>:int):int = F()
UseSuspendable(F()<suspends>:int):task(int) = spawn{ F() }
-->
<!-- 65-->
```verse
UsePure(Pure)                    # OK
UseTransactional(Transactional)  # OK
UseSuspendable(Suspendable)      # OK

# Covariance: fewer effects can substitute for more effects
UseTransactional(Pure)           # OK: ():int <: ()<transacts>:int

# Invalid: more effects cannot substitute for fewer
# UsePure(Transactional)         # ERROR: ()<transacts>:int </: ():int
```
A `<computes>` function can be passed where `<transacts>` is expected
because fewer effects means the function is more constrained.

When you assign different functions conditionally, Verse finds the
least upper bound (join) of their types:

<!--versetest
base := class:
    Value:int

derived := class(base):
    Extra:string
-->	
<!-- 66-->
```verse
# Assume the following:
# base := class{Value:int}
# derived := class(base){Extra:string}

F1():base = base{Value:=1}
F2():derived = derived{Value:=2, Extra:="test"}

# Join: ()->base (common supertype)
G := if(true?) {F1} else {F2}
G().Value  # Can access base members
```


##### Using `type{}`

The `type{_(...):...}` syntax declares function types with full
detail. This is the mechanism for creating function type signatures
that include parameter types, return types, and effects. Underscore
`_` is a placeholder for the function name, emphasizing that it
describes a signature, not a specific function:

<!--versetest-->
<!-- 72-->
```verse
# Function type variable
var Handler:?type{_(:string, :int)<decides>:void} = false

# Nested function matching the signature
MakeHandler(Name:string, Count:int)<decides>:void =
    Print("{Name}: {Count}")
    Count > 0  # Decides effect

set Handler = option{MakeHandler}

# Function accepting function parameter
Process(F:type{_(:int):int}, Value:int):int =
    F(Value)

# Nested function to pass
Double(X:int):int = X * 2
Process(Double, 5)  # Returns 10
```

The `type{}` construct *declares function type signatures*:

<!--versetest
m:= module:
    ValidType1 := type{_():int}
    ValidType2 := type{_(:string, :int):float}
    ValidType3 := type{_()<transacts><decides>:void}
<#    
-->
<!-- 73-->
```verse
# Type definitions for function signatures
ValidType1 := type{_():int}
ValidType2 := type{_(:string, :int):float}
ValidType3 := type{_()<transacts><decides>:void}
```
<!-- #>-->

Within `type{}`, function declarations must have return types but
*cannot have bodies*.

Function types work as field types in classes:

<!--versetest
calculator := class:
    Operation:type{_(:int,:int):int}
-->
<!-- 74-->
```verse
# Assume:
# calculator := class:
#    Operation:type{_(:int,:int):int}

Add(X:int, Y:int):int = X + Y
Multiply(X:int, Y:int):int = X * Y

# Create instances with different operations
Adder := calculator{Operation := Add}
Multiplier := calculator{Operation := Multiply}

Adder.Operation(5, 3)      # Returns 8
Multiplier.Operation(5, 3) # Returns 15
```

Function types can be used for local variables, enabling conditional
function selection:

<!--versetest-->
<!-- 75-->
```verse
ProcessA():int = 10
ProcessB():int = 20

SelectFunction(UseA:logic):int =
    # Choose function based on condition
    Fn:type{_():int} =
        if (UseA?):
            ProcessA
        else:
            ProcessB
    Fn()

SelectFunction(true)   # Returns 10
SelectFunction(false)  # Returns 20
```

Combine `type{}` with `?` to create optional function types:

<!--versetest-->
<!-- 76-->
```verse
DefaultHandler()<computes>:int = -1
CustomHandler()<computes>:int = 42

Process(Handler:?type{_()<computes>:int})<computes><decides>:int =
    # Use handler if provided, otherwise use default
    Handler?() or DefaultHandler()

Process[false]                   # Returns -1 (no handler)
Process[option{CustomHandler}]   # Returns 42 (custom handler)
```

Create arrays of functions sharing the same signature:

<!--versetest-->
<!-- 77-->
```verse
GetZero():int = 0
GetOne():int = 1
GetTwo():int = 2

SumFunctions(Functions:[]type{_():int}):int =
    var Result:int = 0
    for (Fn : Functions):
        set Result += Fn()
    Result

SumFunctions(array{GetZero, GetOne, GetTwo})  # Returns 3
```

##### Examples

**Map-Filter-Reduce**:

<!--versetest-->
<!-- 78-->
```verse
# Generic map
Map(Items:[]t, F(:t)<transacts>:u where t:type, u:type)<transacts>:[]u =
    for (Item:Items):
        F(Item)

# Generic filter
Filter(Items:[]t, Pred(:t)<computes><decides>:void where t:type)<computes>:[]t =
    for (Item:Items, Pred[Item]):
        Item

# Generic fold/reduce
Fold(Items:[]t, Initial:u, F(:u, :t)<transacts>:u where t:type, u:type)<transacts>:u =
    var Acc:u = Initial
    for (Item:Items):
        set Acc = F(Acc, Item)
    Acc

# Usage with nested functions
Values := array{1, 2, 3, 4, 5}

# Define nested functions for operations
Square(X:int)<computes>:int = X * X
IsEven(X:int)<computes><decides>:void = X = 0 or Mod[X,2] = 0
AddTo(Acc:int, X:int)<computes>:int = Acc + X

Squared := Map(Values, Square)
Evens := Filter(Values, IsEven)
Sum := Fold(Values, 0, AddTo)
```

**Function composition**:

<!--versetest-->
<!-- 79-->
```verse
Compose(F(:b):c, G(:a):b where a:type, b:type, c:type):type{_(:a):c} =
    # Return a nested function that composes F and G
    Composed(X:a):c = F(G(X))
    Composed

Add1(X:int):int = X + 1
Double(X:int):int = X * 2

# Compose: first doubles, then adds 1
DoubleThenIncrement := Compose(Add1, Double)
DoubleThenIncrement(5)  # Returns 11 (5*2 + 1)
```

**Partial application**:

<!--versetest-->
<!-- 80-->
```verse
Partial(F(:a, :b):c, X:a where a:type, b:type, c:type):type{_(:b):c} =
    # Return a nested function with X captured
    PartialFunc(Y:b):c = F(X, Y)
    PartialFunc

Add(X:int, Y:int):int = X + Y
Add5 := Partial(Add, 5)
Add5(3)  # Returns 8
```

#### Nested Functions

!!! warning "Unreleased Feature"
    Nested functions have not yet been released. This section documents planned functionality that is not currently available.

Nested functions (also called local functions) are functions defined
inside other functions. They provide encapsulation, enable closures
over local variables, and help organize complex logic within a
function's scope. Nested functions have names, can be recursive, and
are the primary way to create function values and closures in Verse.

A nested function is declared just like a top-level function, but
inside another function's body:

<!--versetest-->
<!-- 81-->
```verse
Outer(X:int):int =
    # Nested function definition
    Inner(Y:int):int = Y * 2

    # Call nested function
    Inner(X)

Outer(5)  # Returns 10
```

Nested functions are only visible within their enclosing function's
scope. They cannot be accessed from outside.

Nested functions capture (close over) variables from any enclosing
scope, creating powerful closures:

<!--versetest-->
<!-- 82-->
```verse
MakeGreeter(Name:string):type{_():string} =
    # Greeting captures Name from outer scope
    Greeting():string = "Hello, {Name}!"

    # Return the nested function
    Greeting

SayHello := MakeGreeter("Alice")
SayHello()  # Returns "Hello, Alice!"

SayHi := MakeGreeter("Bob")
SayHi()  # Returns "Hello, Bob!"
```

Each call to `MakeGreeter` creates a new closure with its own captured
`Name` value.

Nested functions support overloading by parameter types:

<!--versetest-->
<!-- 83-->
```verse
Process(X:int):string =
    # Overloaded nested functions
    Format(Value:int):string = "int: {Value}"
    Format(Value:float):string = "float: {Value}"

    # Calls appropriate overload
    IntResult := Format(42)       # Calls int version
    FloatResult := Format(3.14)   # Calls float version

    "{IntResult}, {FloatResult}"

Process(1)  # Returns "int: 42, float: 3.14"
```

Overload resolution works the same as for top-level functions.

##### Closures with State

Nested functions can capture `var` variables and mutate them, creating stateful closures:

<!--versetest-->
<!-- 84-->
```verse
MakeCounter(Initial:int):tuple(type{_():int}, type{_():void}) =
    var Count:int = Initial

    # Getter captures Count
    GetCount():int = Count

    # Incrementer mutates captured Count
    Increment():void = set Count = Count + 1

    (GetCount, Increment)

Counter := MakeCounter(0)
GetValue := Counter(0)
IncrementValue := Counter(1)

GetValue()        # Returns 0
IncrementValue()  # Increments count
GetValue()        # Returns 1
IncrementValue()  # Increments count
GetValue()        # Returns 2
```

This pattern creates a closure that maintains private mutable state.

##### Restrictions

Nested functions have several important restrictions that distinguish
them from top-level functions:

- Nested functions **cannot** have access specifiers like `<public>`,
  `<internal>`, or `<private>`:
- Nested functions are always private to their enclosing function.
- You cannot define classes inside functions (nested or otherwise):

<!--versetest
assert_semantic_error(3502):
    F():void =
        my_class := class {}
<#
-->
<!-- 86-->
```verse
# ERROR: Cannot define classes in local scope
F():void =
    my_class := class {}  # ERROR

# Correct: Define classes at module level
my_class := class {}

F():void =
    Instance := my_class{}  # OK - can use class
```
<!-- #>-->

- Nested functions cannot reference variables or other nested
  functions defined later in the same scope (this also means mutually
  recursive nested functions are not allowed):

<!--versetest
assert_semantic_error(3506):
    F():void =
        X := G()
        G():int = 42
<#
-->
<!-- 87-->
```verse
# ERROR 3506: G used before defined
F():void =
    X := G()     # ERROR: G not yet defined
    G():int = 42

# Correct: Define before use
F():void =
    G():int = 42
    X := G()     # OK: G is defined
```
<!-- #>-->

- The `(super:)` syntax for calling parent class methods **cannot** be used in nested functions:

<!--versetest
assert_semantic_error(3612):
    base_class := class:
        F(X:int):int = X

    derived_class := class(base_class):
        F<override>(X:int):int =
            G():int =
                (super:)F(X)
            G()
<#
-->
<!-- 88-->
```verse
# ERROR 3612: super not allowed in nested function
base_class := class:
    F(X:int):int = X

derived_class := class(base_class):
    F<override>(X:int):int =
        G():int =
            (super:)F(X)  # ERROR: super not allowed here
        G()

# Correct: Use super directly in the overriding method
derived_class := class(base_class):
    F<override>(X:int):int =
        BaseResult := (super:)F(X)  # OK
        G():int = BaseResult * 2
        G()
```
<!-- #>-->

#### Parametric Functions

Parametric functions (also called generic functions) allow you to
write code that works with multiple types while maintaining complete
type safety. Rather than writing separate functions for each type, you
define a single function with type parameters that adapt to whatever
types you use them with.

A parametric function declares type parameters using a `where` clause
that specifies constraints on those types:

<!--versetest-->
<!-- 89-->
```verse
# Simple identity function - works with any type
Identity(X:t where t:type):t = X
# Usage - type parameter inferred automatically
Identity(42)        # t inferred as int, returns 42
Identity("hello")   # t inferred as string, returns "hello"
```

The `where t:type` clause declares `t` as a type parameter with the constraint `type`,
meaning it can be any Verse type.
The function signature `(X:t):t` means "takes a value of type `t` and returns a value of that same type `t`."

The generic type parameter `t` captures the complete type information, not just the top-level type. This means containers passed to generic functions preserve their internal structure:

<!--versetest-->
<!-- 901-->
```verse
# The Identity function preserves exact container types
Identity(X:t where t:type):t = X

# Maps maintain their key and value types
IntToString:[int]string = map{1 => "one"}
Result1 := Identity(IntToString)  # Result1: [int]string

# Arrays maintain element types
IntArray:[]int = array{1, 2, 3}
Result2 := Identity(IntArray)  # Result2: []int

# Even nested containers preserve structure
NestedMap:[int][]string = map{1 => array{"a", "b"}}
Result3 := Identity(NestedMap)  # Result3: [int][]string
```

This is fundamentally different from using `any`, which would erase type information.

<!--NoCompile-->
<!-- 90-->
```verse
FunctionName(Parameters where TypeParameter:Constraint, ...):ReturnType = Body
```

- *Type parameters* appear in the `where` clause
- *Constraints* specify requirements (e.g., `type`, `subtype(comparable)`)
- *Multiple type parameters* are comma-separated in the `where` clause

Verse automatically infers type parameters from the arguments you
pass, eliminating the need for explicit type annotations in most
cases:

<!--versetest-->
<!-- 91-->
```verse
# Function with two type parameters
Pair(X:t, Y:u where t:type, u:type):tuple(t, u) = (X, Y)

# All type parameters inferred
Pair(1, "one")        # t = int, u = string, returns (1, "one")
Pair(true, 3.14)      # t = logic, u = float, returns (true, 3.14)
```

Inference with collections:

<!--versetest-->
<!-- 92-->
```verse
# Generic first element function
First(Items:[]t where t:type)<decides>:t = Items[0]

Values := array{1, 2, 3}
Result :int= First[Values]  # t inferred as int from []int
```
When you pass multiple values to a parametric function expecting a single type parameter, Verse can infer either a tuple or an array:

<!--versetest-->
<!-- 93-->
```verse
# Returns the argument unchanged
Identity(X:t where t:type):t = X

# Passing multiple values creates a tuple
Result1:tuple(int, int) = Identity(1, 2)  # t = tuple(int, int)

# Can also be treated as an array
Result2:[]int = Identity(1, 2)  # t = []int via conversion
```

##### Type Constraints

Type constraints restrict which types can be used with type parameters, enabling operations that require specific capabilities.

The most permissive constraint accepts any type:

<!--versetest-->
<!-- 94-->
```verse
# Works with absolutely any type
Store(Value:t where t:type):t = Value
```

Restricts to types that are subtypes of a specified type:

<!--versetest
vehicle := class:
    Speed:float = 0.0

car := class(vehicle):
    NumDoors:int = 4

ProcessVehicle(V:t where t:subtype(vehicle)):t =
    Print("Speed: {V.Speed}")
    V
<#
-->
<!-- 95-->
```verse
vehicle := class:
    Speed:float = 0.0

car := class(vehicle):
    NumDoors:int = 4

# Only accepts vehicle or its subtypes
ProcessVehicle(V:t where t:subtype(vehicle)):t =
    # Can access Speed because we know V is a vehicle
    Print("Speed: {V.Speed}")
    V
```
<!-- #>-->

<!--versetest
vehicle := class:
    Speed:float = 0.0

car := class(vehicle):
    NumDoors:int = 4

ProcessVehicle(V:t where t:subtype(vehicle)):t =
    Print("Speed: {V.Speed}")
    V
-->
<!-- 200-->
```verse
# Valid calls
ProcessVehicle(vehicle{})      # t = vehicle
ProcessVehicle(car{})          # t = car (subtype of vehicle)
```

The function returns type `t`, not the base type. This preserves the specific type:

<!--versetest
vehicle := class:
    Speed:float = 0.0

car := class(vehicle):
    NumDoors:int = 4

ProcessVehicle(V:t where t:subtype(vehicle))<transacts>:t =
    Print("Speed: {V.Speed}")
    V
-->
<!-- 96-->
```verse
# Type-preserving function with subtype constraint
MyCar := car{NumDoors:=4, Speed:=60.0}
Result:car= ProcessVehicle(MyCar)  # Result has type car, not vehicle
Result.NumDoors                  # Can access car-specific fields
```
The `subtype(comparable)` constraint enables equality comparisons:

<!--versetest-->
<!-- 97-->
```verse
# Can use = and <> operators on t
FindInArray(Items:[]t, Target:t where t:subtype(comparable))<decides>:[]int =
    for (Index -> Item : Items, Item = Target):
        Index
```

Type parameters can reference each other in constraints:

<!--versetest-->
<!-- 98-->
```verse
# u must be a subtype of t
Convert(Base:t, Derived:u where t:type, u:subtype(t)):t = Base
# This ensures type safety across related types
```

##### Member Access

When using subtype constraints, you can access members that exist on the base type:

<!--versetest
entity := class:
    Name:string = "Entity"
    Health:int = 100

player := class(entity):
    Score:int = 0
<#
-->
<!-- 99-->
```verse
entity := class:
    Name:string = "Entity"
    Health:int = 100

player := class(entity):
    Score:int = 0

```
<!-- #>-->

<!--versetest
entity := class:
    Name:string = "Entity"
    Health:int = 100

player := class(entity):
    Score:int = 0
-->
<!-- 299-->
```verse
# Can access entity members through type parameter
GetInfo(E:t where t:subtype(entity)):tuple(t, string, int) =
    (E, E.Name, E.Health)            # Can access Name and Health

P := player{Name := "Alice", Health := 100, Score := 1500}
Info := GetInfo(P)                   # Returns (player instance, "Alice", 100)
                                     # Info(0) has type player, not entity 
```

Method calls work too:

<!--versetest
entity := class:
    GetStatus():string = "Active"

CheckStatus(E:t where t:subtype(entity)):string =
    E.GetStatus()
<#
-->
<!-- 100-->
```verse
entity := class:
    GetStatus():string = "Active"

# Call methods on parametrically-typed values
CheckStatus(E:t where t:subtype(entity)):string =
    E.GetStatus()  # Method call through type parameter
```
<!-- #>-->

##### Polarity and Variance

Type parameters must be used consistently according to variance rules. 
This ensures type safety when functions are used as values or passed as arguments.

**Covariant positions** (safe for return types):

- Function return types
- Tuple/array element types (as return)
- Map key types (as return)
- Map value types (as return)

**Contravariant positions** (safe for parameter types):

- Function parameter types

**The polarity check:** Verse validates that type parameters appear
only in positions compatible with their intended use:

<!--versetest-->
<!-- 101-->
```verse
# Valid: t appears covariantly (return type)
GetValue(X:t where t:type):t = X

# Valid: t appears contravariantly (parameter)
Consume(X:t where t:type):void = {}

# Valid: t appears in both positions (through function parameter and return)
Apply(F:type{_(:t):t}, X:t where t:type):t = F(X)
```

**Invariant types cause errors:**

<!--versetest
assert_semantic_error(3552):
    c(t:type) := class{var X:t}
    MakeContainer(X:t where t:type):c(t) = c(t){X := X}
<#
-->
<!-- 102-->
```verse
# ERROR: Cannot return type that is invariant in t
c(t:type) := class{var X:t}  # Mutable field makes c invariant in t
MakeContainer(X:t where t:type):c(t) = c(t){X := X}
```
<!-- #>-->

The error occurs because `c(t)` contains a mutable field of type `t`,
making it invariant - neither covariant nor contravariant. Returning
such a type from a parametric function is unsafe.

**Map polarity:** Maps are covariant in both keys and values:

<!--versetest-->
<!-- 103-->
```verse
# Valid: covariant key and value
ProcessMap(M:[t]u where t:subtype(comparable), u:type):[t]u = M
```

#### Overloading

Function overloading allows you to define multiple functions with the
same name but different parameter types. The compiler selects the
correct version based on the types of the arguments provided at the
call site.

Define multiple functions with the same name but different parameter types:

<!--versetest-->
<!-- 104-->
```verse
# Overload by parameter type
Process(Value:int):string = "Integer: {Value}"
Process(Value:float):string = "Float: {Value}"
Process(Value:string):string = "String: {Value}"

# Calls select the appropriate overload
Process(42)        # Returns "Integer: 42"
Process(3.14)      # Returns "Float: 3.14"
Process("hello")   # Returns "String: hello"
```

The compiler determines which overload to call based on the argument
types. Each overload must have a distinct parameter type signature.

##### Capture

You cannot take a reference to an overloaded function name:

<!--versetest
assert_semantic_error(3502):
    f(x:int):void = {}
    f(x:float):void = {}
    g := f
<#
-->
<!-- 105-->
```verse
# ERROR: Cannot capture overloaded function
f(x:int):void = {}
f(x:float):void = {}

# Error: which f?
g:void = f
```
<!-- #>-->

This restriction exists because the compiler cannot determine which overload you mean without seeing the call site with arguments.

##### Effects

You can overload functions with different effects, but only if the
parameter types are also different:

**Valid: Different types, different effects:**

<!--versetest-->
<!-- 106-->
```verse
Process(x:float):float = x
Process(x:int)<transacts><decides>:int = x = 1

Process(3.0)   # Returns 3.0 (non-failable)
Process[1]     # Returns option{1} (failable)
```

**Invalid: Same types, different effects:**

<!--versetest
assert_semantic_error(3532):
    f(x:int):void = {}
    f(x:int)<transacts><decides>:void = {}
<#
-->
<!-- 107-->
```verse
# ERROR: Same parameter type
f(x:int):void = {}
f(x:int)<transacts><decides>:void = {}  # ERROR
```
<!-- #>-->

Effects alone do not create distinctness - you need different parameter types.

##### Overloads in Subclasses

Subclasses can add new overloads to methods:

<!--versetest
c0 := class:
    f(X:int):int = X

c1 := class(c0):
    f(X:float):float = X
<#
-->
<!-- 108-->
```verse
c0 := class:
    f(X:int):int = X

c1 := class(c0):
    # Add new overload for float
    f(X:float):float = X
```
<!-- #>-->

<!--versetest
c0 := class:
    f(X:int):int = X

c1 := class(c0):
    f(X:float):float = X
-->
<!-- 208-->
```verse
c0{}.f(5)     # OK - int overload
c1{}.f(5)     # OK - inherited int overload
c1{}.f(5.0)   # OK - new float overload
```

When a subclass defines a method that shares a name
with a parent method, it must either:

1. Provide a **distinct parameter type** (different from all parent overloads)
2. **Override exactly one** parent overload using `<override>`

<!--versetest
c := class<allocates>{}
d := class<allocates>(c){}

e := class<allocates>:
    func(C:c):c = C
    func(E:e):e = E

myf := class<allocates>(e):
    func<override>(C:c):d = d{}
<#
-->
<!-- 109-->
```verse
# Parent class with overloads
e := class:
     func(C:c):c = C
     func(E:e):e = E

# Valid: Overrides one parent overload
myf := class(e):
     func<override>(C:c):d = d{}

# ERROR: d is subtype of c, overlaps but does not override
# g := class(e):
#     func(D:d):d = D  # ERROR - ambiguous with func(C:c)
```
<!-- #>-->

##### Interfaces with Overloaded Methods

Interfaces can declare overloaded methods:

<!--versetest
formatter := interface:
    Format(X:int):string = "{X}"
    Format(X:float):string = "{X}"

entity := class(formatter):
    Format<override>(X:int):string = "Entity-{X}"
    Format<override>(X:float):string = "Entity-{X}"
<#
-->
<!-- 110-->
```verse
formatter := interface:
    Format(X:int):string = "{X}"
    Format(X:float):string = "{X}"

entity := class(formatter):
    Format<override>(X:int):string = "Entity-{X}"
    Format<override>(X:float):string = "Entity-{X}"
```
<!-- #>-->

##### Restrictions


**Cannot overload functions with non-functions:**

A name cannot be both a function and a non-function value:

<!--versetest
assert_semantic_error(3532):
    f:int = 0
    f():void = {}
<#
-->
<!-- 112-->
```verse
# ERROR: Cannot overload with variable
# f:int = 0
# f():void = {}
```
<!-- #>-->

**Bottom type cannot resolve overloads:**

The bottom type (from `return` without a value) cannot be used for overload resolution:

<!--versetest
assert_semantic_error(3518):
    F(X:int):int = X
    F(X:float):float = X
    G():void =
        F(@ignore_unreachable return)
        0
<#
-->
<!-- 114-->
```verse
# ERROR: Cannot determine which overload
F(X:int):int = X
F(X:float):float = X

# G():void =
#     F(@ignore_unreachable return)  # ERROR - which F?
#     0
```
<!-- #>-->

##### Overloading with `<suspends>`

You can mix suspending and non-suspending overloads if the parameter
types differ:

<!--versetest-->
<!-- 115-->
```verse
f(x:int)<suspends>:void =
    Sleep(1.0)

f(x:float):void =
    Print("Non-suspending")

# Call non-suspending directly
f(1.0)

# Call suspending with spawn
spawn{f(1)}
```

**Cannot call suspending overload without spawn:**

<!--versetest
assert_semantic_error(3512):
    f(x:int):void = {}
    f(x:float)<suspends>:void = {}
    g():void = f(1.0)
<#
-->
<!-- 116-->
```verse
# ERROR: suspends version needs spawn context
f(x:int):void = {}
f(x:float)<suspends>:void = {}

g():void = f(1.0)  # ERROR - float version is suspends
```
<!-- #>-->


##### Types 

Every function has a type that captures its parameters, effects, and
return value. The type syntax uses an underscore as a placeholder for
the function name:

<!--versetest-->
<!-- 118-->
```verse
type{_(:int,:string)<decides>:float}
```

This represents any function that takes an integer and a string, might
fail, and returns a float when successful.

Multiple functions may share a name through overloading, as long as
their signatures do not create ambiguity. The compiler can distinguish
between overloads based on the argument types:

<!--versetest-->
<!-- 119-->
```verse
Transform(X:int):string = "I:{X}"
Transform(X:float):string = "F:{X}"
Transform(X:string):string = "S:{X}"

Result1 := Transform(42)        # Calls int version
Result2 := Transform(3.14)      # Calls float version
Result3 := Transform("Hello")   # Calls string version
```

However, overloading has strict limitations based on **type
distinctness**. Two types are considered "distinct" for overload
purposes only if there is no possible value that could match both
types. This restriction prevents ambiguity and ensures that function
calls can always be resolved unambiguously at compile time.

Verse uses precise rules to determine whether two parameter types are
distinct enough to allow overloading. Understanding these rules is
critical for designing clear APIs.

The following type pairs are **not distinct** and cannot be used to
overload functions:

**1. Optional and Logic.** `?t` and `logic` are not distinct because
both types include `false` as a value, creating overload ambiguity when
`false` is passed as an argument:

<!--versetest
assert_semantic_error(3532):
    F(:?any):void = {}
    F(:logic):void = {}
<#
-->
<!-- 120-->
```verse
# ERROR: Not distinct
F(:?any):void = {}
F(:logic):void = {}
```
<!-- #>-->

Note that `?t` and `logic` are not equivalent types—`logic` contains
`true` and `false`, while `?t` contains `false` and option values like
`option{false}`. However, their shared `false` value means the compiler
cannot distinguish between them for overload resolution.

**2. Arrays and Maps.**  Arrays `[]t` and maps `[k]t` are not distinct:

<!--versetest
assert_semantic_error(3532):
    F(:[]int):void = {}
    F(:[string]int):void = {}
<#
-->
<!-- 121-->
```verse
# ERROR: Not distinct
F(:[]int):void = {}
F(:[string]int):void = {}
```
<!-- #>-->

**3. Functions and Maps.** Function types and maps are not distinct:

<!--versetest
assert_semantic_error(3532):
    F(:[string]int):void = {}
    F(G(:string)<transacts><decides>:int):void = {}
<#
-->
<!-- 122-->
```verse
# ERROR: Not distinct
F(:[string]int):void = {}
F(G(:string)<transacts><decides>:int):void = {}
```
<!-- #>-->

**4. Functions and Arrays.** Function types and arrays are not
distinct because an overloaded function could match both:

<!--versetest
assert_semantic_error(3532):
    F(:[]int):void = {}
    F(G(:string)<transacts><decides>:int):void = {}
<#
-->
<!-- 123-->
```verse
# ERROR: Not distinct
F(:[]int):void = {}
F(G(:string)<transacts><decides>:int):void = {}
```
<!-- #>-->

**5. Interfaces and Classes.** An interface and any class are never
distinct, even if the class does not implement the interface, because a
subtype of the class might:

<!--versetest
assert_semantic_error(3532):
    i := interface{}
    t := class{}
    f(:i):void = {}
    f(:t):void = {}
<#
-->
<!-- 124-->
```verse
i := interface{}
t := class{}

# ERROR: Not distinct (subtype of t might implement i)
f(:i):void = {}
f(:t):void = {}
```
<!-- #>-->

**6. Functions with Different Effects.** Functions are not distinct
based on effects alone. Changing or removing effects does not create a
distinct overload:

<!--versetest
assert_semantic_error(3532):
    a := class{}
    b := class{}
    F(G(:a)<transacts><decides>:b):void = {}
    F(G(:a):b):void = {}
<#
-->
<!-- 126-->
```verse
a := class{}
b := class{}

# ERROR: Not distinct
F(G(:a)<transacts><decides>:b):void = {}
F(G(:a):b):void = {}
```
<!-- #>-->

**7. Functions with Different Signatures.** Functions with different
parameter or return types are not distinct because of function
overloading:

<!--versetest
assert_semantic_error(3532):
    a := class{}
    b := class{}
    F(G(:b):b):void = {}
    F(G(:a):b):void = {}
<#
-->
<!-- 127-->
```verse
# ERROR: Not distinct
F(G(:b):b):void = {}
F(G(:a):b):void = {}
```
<!-- #>-->

**8. void as Top Type.** `void` is treated as equivalent to the top
type (accepts `any`), so it is not distinct from any other type:

<!--versetest
assert_semantic_error(3532):
    F(:int):void = {}
    F(:void):void = {}
<#
-->
<!-- 128-->
```verse
# ERROR: Not distinct
F(:int):void = {}
F(:void):void = {}
```
<!-- #>-->

**9. Subtype Relationships.** Classes with subtype relationships are
not distinct:

<!--versetest
assert_semantic_error(3532):
    a := class{}
    b := class(a){}
    F(:a):void = {}
    F(:b):void = {}
<#
-->
<!-- 129-->
```verse
a := class{}
b := class(a){}

# ERROR: Not distinct
F(:a):void = {}
F(:b):void = {}
```
<!-- #>-->

**10. Tuple Distinctness Rules.**  Tuples have complex distinctness rules:

**Empty tuples and arrays are not distinct:**

<!--versetest
assert_semantic_error(3532):
    a := class{}
    F(:tuple(), :a):void = {}
    F(:[]a, :a):void = {}
<#
-->
<!-- 130-->
```verse
a := class{}

# ERROR: Not distinct
F(:tuple(), :a):void = {}
F(:[]a, :a):void = {}
```
<!-- #>-->

**Tuples and arrays are distinct only if tuple element types are
completely distinct:**

<!--versetest
assert_semantic_error(3532):
    a := class{}
    b := class(a){}
    F(:tuple(a, b), :a):void = {}
    F(:[]a, :a):void = {}
<#
-->
<!-- 131-->
```verse
a := class{}
b := class(a){}

# ERROR: Not distinct (b is subtype of a)
F(:tuple(a, b), :a):void = {}
F(:[]a, :a):void = {}
```
<!-- #>-->

**Tuples and maps with `int` key are not distinct:**

<!--versetest
assert_semantic_error(3532):
    a := class{}
    F(:tuple(a), :a):void = {}
    F(:[int]a, :a):void = {}
<#
-->
<!-- 132-->
```verse
a := class{}

# ERROR: Not distinct
F(:tuple(a), :a):void = {}
F(:[int]a, :a):void = {}
```
<!-- #>-->

**Tuples and maps with non-`int` key ARE distinct:**

<!--versetest
a := class{}

F(:tuple(a), :a):void = {}
F(:[logic]a, :a):void = {}
<#
-->
<!-- 133-->
```verse
a := class{}

# Valid: Distinct types
F(:tuple(a), :a):void = {}
F(:[logic]a, :a):void = {}  # OK
```
<!-- #>-->

**Singleton tuples and optional for `int` are not distinct:**

<!--versetest
assert_semantic_error(3532):
    a := class{}
    F(:tuple(int), :a):void = {}
    F(:?int, :a):void = {}
<#
-->
<!-- 134-->
```verse
a := class{}

# ERROR: Not distinct
F(:tuple(int), :a):void = {}
F(:?int, :a):void = {}
```
<!-- #>-->

**Singleton tuples and optional for non-`int` ARE distinct:**

<!--versetest
a := class{}
-->
<!-- 135-->
```verse
# Valid: Distinct types
F(:tuple(a), :a):void = {}
F(:?a, :a):void = {}  # OK
```

#### Publishing Functions

Publishing a function is a promise of backwards compatibility between
the function and its clients. Consider this function:

<!--versetest-->
<!-- 139-->
```verse
F1<public>(X:int):int = X + 1
```

The type annotation (`X:int):int`) tells us that this function promises that
given any integer it will always return an integer. That contract cannot be
broken in future versions of the code. Because it has the default effect, which
includes the `<reads>` effect, the implementation could change in the future,
perhaps to perform additional operations or optimizations, as long as it
maintains its signature.

Functions that do not have the `<reads>` effect are less flexible. Consider
this function:

<!--versetest-->
<!-- 140-->
```verse
F2<public>(X:int)<computes>:int = X + 1
```

Because it has the `<computes>` effect specifier, it does not have the
`<reads>` effect. Within a given version, this guarantees referential
transparency: the function will always return the same result for the
same arguments. Across versions, this creates a stronger constraint:
since the compiler cannot verify that a modified body preserves the
same input-output mapping for all possible arguments, it
conservatively forbids any body changes. Thus, changing the body to
return `X + 2` in a future version would be rejected as backward
incompatible.

Functions such as `F1` and `F2` are sometimes called *opaque* as the return
type abstracts the function's body. Future version of Verse will support
*transparent* functions:

<!--NoCompile-->
<!-- 141-->
```verse
F2<public>(X:int) := X + 1
```

A transparent function does not declare its return type, instead the
function's type is inferred from its body. This implies a very
different promise: a forever guarantee that the function's body will
remain exactly the same throughout the lifetime of the module code.

## Book of Verse Source Unit: 07_control.md

### Control Flow

Every program has a natural rhythm to its execution, a sequence in
which instructions are processed and decisions are made. In Verse,
this flow is more than just a mechanical progression through lines of
code - it is a carefully orchestrated dance between different types of
expressions, each contributing to the overall behavior of your
program.

#### Blocks

A code block is a fundamental organizational unit, it groups related
expressions together and creates a new scope for variables and
constants. Unlike many languages where blocks are merely syntactic
conveniences, blocks are expressions themselves, meaning they produce
values just like any other expression.

The concept of scope is crucial to understanding code blocks. When you
create a variable or constant within a block, it exists only within
that block's context. This containment ensures that your code remains
organized and that names do not accidentally conflict across different
parts of your program. Consider this function, it is body is a code
block that contains one if-then-else expression, itself
composed of three different code blocks.

<!--versetest-->
<!-- 01 -->
```verse
CalculateReward(PlayerLevel:int)<reads>:int =
    if:
        PlayerLevel > 10
        Multiplier := 2.0  # Only exists within this if block
        Base := 100
        Result := Floor[(Base+PlayerLevel) * Multiplier] # Fails on infinity
    then:
        Result  # This block extends the scope of the if
    else:
        50      # Different branch, different scope
                # Multiplier and Result do not exist here
```
<!-- CalculateReward(11) = 222 -->

Verse has a flexible syntax with three equivalent formats for
writing blocks. The spaced format is the most common, using a colon to
introduce the block and indentation to show structure:

<!--versetest
IsPlayerReady()<decides><transacts>:void = {}
StartMatch()<transacts>:void = {}
BeginCountdown()<transacts>:void = {}
-->
<!-- 02 -->
```verse
if (IsPlayerReady[]):
    StartMatch()
    BeginCountdown()
```

The multi-line braced format offers familiarity for programmers coming
from C-style languages:

<!--versetest
IsPlayerReady()<decides><transacts>:void = {}
StartMatch()<transacts>:void = {}
BeginCountdown()<transacts>:void = {}
-->
<!-- 03 -->
```verse
if (IsPlayerReady[]) {
    StartMatch()
    BeginCountdown()
}
```

For simple operations, the single-line dot format keeps code concise:

<!--versetest-->
<!-- 04 -->
```verse
HasPowerup()<computes><decides>:void={}
ApplyBoost():void={}
F():void=
    if (HasPowerup[]). ApplyBoost()
```

Since everything is an expression, blocks themselves have values. The
value of a block is given by the last expression executed within
it. This enables elegant patterns where complex computations can be
encapsulated in blocks that seamlessly integrate with surrounding
code:

<!--versetest
CalculateScore()<computes>:int = 100
CalculateBonus(Time:float)<computes>:int = 50
CompletionTime:float = 10.0
AccuracyValue:float = 0.95
-->
<!-- 05 -->
```verse
FinalScore := block:              # The variable has the block's value
    Base := CalculateScore()
    Bonus := CalculateBonus(CompletionTime)
    Accuracy := Floor[AccuracyValue * 100.0]
    Base + Bonus + Accuracy       # This becomes the block's value
```


#### If Expressions

The `if` expression uses success and failure to drive decisions (see
[Failure](12_BOOK_OF_VERSE_I_FOUNDATIONS.md#book-of-verse-source-unit-08failuremd) for details). When an expression in the
condition succeeds, the corresponding branch executes:

<!--versetest
player := class:
   CanJump()<computes><decides>:void={}
   Jump()<computes>:void={}
   GetEquippedWeapon()<computes><decides>:weapon=weapon{}
   Idle()<computes>:void={}

weapon:=class<computes>{
   Fire():void={}
}
ConsumeAmmo():void={}
PlayJumpSound():void={}
-->
<!-- 07 -->
```verse
HandlePlayerAction(Player:player, Action:string):void =
    if (Action = "jump", Player.CanJump[]):
        Player.Jump()
        PlayJumpSound()
    else if (Action = "attack", Weapon := Player.GetEquippedWeapon[]):
        Weapon.Fire()
        ConsumeAmmo()
    else:
        # Default action
        Player.Idle()
```

This approach allows you to chain conditions that might fail without
explicit error handling at each step.

An alternative syntax uses `then:` and `else:` keywords to explicitly
label branches:

<!--versetest-->
<!-- 08 -->
```verse
ProcessValue(Value:int):string =
    if:
        Value > 0
        Value < 100
    then:
        "Valid"
    else:
        "Out of range"

ProcessValue(50) = "Valid"
```

This syntax can improve readability when you have multiple conditions
or want to emphasize the condition-action separation. 

The condition in an `if` must contain at least one expression that can
fail. This requirement ensures `if` is used for its intended
purpose—handling uncertain outcomes:

<!--NoCompile-->
<!-- 10 -->
```verse
# Error: condition cannot fail
if (1 + 1):  # Compile error - no fallible expression
    DoSomething()

# Valid: array access can fail
if (FirstItem := Items[0]):
    Process(FirstItem)
```

Empty conditions are also not allowed—every `if` must test something.

If any expression in the condition fails, control flow proceeds to the
`else` branch if present. Any effects performed while evaluating the
condition are automatically rolled back (see
[Failure](12_BOOK_OF_VERSE_I_FOUNDATIONS.md#speculative-execution) for details):

<!--versetest
GetPlayerScore()<decides><computes>:int=1
-->
<!-- 11 -->
```verse
var Counter:int = 0

if:
    set Counter = Counter + 1  # Provisional change
    Score := GetPlayerScore[]  # Might fail
    Score > 100
then:
    # Counter was incremented
else:
    # Counter rolled back to original value - increment undone!
```

This speculative execution makes conditional logic safer—you can
perform operations optimistically, knowing they'll be reversed if
subsequent conditions fail.

Variables defined in the condition are available in the `then` branch
but not in the `else` branch:

<!--NoCompile-->
<!-- 12 -->
```verse
if:
    Player := FindPlayer[Name]  # Define Player
then:
    AwardBonus(Player)  # OK - Player available
else:
    Penalize(Player)  # Compile error
```

This scoping reflects the logical flow: in the `else` branch, the
condition failed, so any variables bound during the condition might
not have meaningful values.

Since `if` is an expression, it produces a value. When all branches
return compatible types, the `if` can be used anywhere a value is
expected:

<!--versetest
IsCritical:logic= false
BaseDamage:int=0
Health:int=0
-->
<!-- 13 -->
```verse
Damage := if (IsCritical?):
    BaseDamage * 2
else:
    BaseDamage

# Ternary-style
Status := if (Health > 50). "Healthy" else. "Wounded"
```

When branches have incompatible types, the result is widened to `any`:

<!--versetest
UseNumber:logic=false
-->
<!-- 14 -->
```verse
# Different types in branches yields any
Result:any = if (UseNumber?) then 42 else "text"
```

All branches must produce a value for the `if` to be used as an
expression.

##### Handling Only Failures

A common pattern is to handle only the failure case of a `<decides>`
expression, letting success continue without additional logic. The
idiomatic way to express this is with `if (Condition): else:`:

<!--versetest-->
<!-- 14001 -->
```verse
ProcessData()<decides><transacts>:void = {}

HandleWithFailureCase()<transacts>:void =
    if (ProcessData[]):
        # Success - no additional logic needed
    else:
        # Handle the failure case
        Print("Processing failed")
```

When the condition succeeds, execution continues after the `if`
statement. When it fails, the `else` block handles the error. This
pattern is clearer than alternatives that might seem equivalent but
behave differently.

A tempting but sometimes incorrect
pattern is to use `not` to check for failure:

<!--NoCompile-->
```verse
# Causes unwanted rollback
if (not ProcessData[]):
    Print("Processing failed")
```

This fails because when `ProcessData[]` succeeds, `not true` fails,
causing the outer `if` to fail and roll back any transactional effects
from `ProcessData`. Safer patterns are:

<!--versetest-->
<!-- 14002 -->
```verse
var Counter:int = 0

IncrementCounter()<decides><transacts>:void =
    set Counter += 1

# Correct: if (Expr): else: for handling failures
TestIfElse():void =
    set Counter = 0
    if (IncrementCounter[]):
    else:
        Print("Failed")
    # Counter is 1 - effect preserved

# Correct: logic{} to convert to boolean
TestLogic():void =
    set Counter = 0
    Result := logic{IncrementCounter[]}
    if (not Result?):
        Print("Failed")
    # Counter is 1 - effect preserved
```

#### Case Expressions

When you need to make decisions based on multiple possible values, the
`case` expression provides clear, readable branching:

<!--versetest-->
<!-- 15 -->
```verse
GetWeaponDamage(WeaponType:string):float =
    case(WeaponType):
        "sword"  => 50.0
        "bow"    => 35.0
        "staff"  => 40.0
        "dagger" => 25.0
        _        => 10.0  # Default damage for unknown weapons

GetWeaponDamage("sword") = 50.0
```

The `case` expression is used when you have discrete values to match
against, making your intent clearer than a series of `if-else`
conditions.

Case expressions work with specific types that support direct value
comparison:

- **Primitives**: `int`, `logic`, `char`
- **Strings**: `string`
- **Enums**: Both open and closed enums
- **Refinement types**: Custom types with constraints

They do not work on `float`, objects and tuples due to implementation
limitations.

**Exhaustiveness Checking with Enums.** `case` with `enum` are checked
for exhaustiveness.  For closed enums where all values are known, the
compiler verifies you've handled all cases:

<!--versetest
direction := enum:
    North
    South
    East
    West
-->
<!-- 17 -->
```verse
# Exhaustive - no wildcard needed
GetVector(Dir:direction):tuple(int, int) =
    case (Dir):
        direction.North => (0, 1)
        direction.South => (0, -1)
        direction.East => (1, 0)
        direction.West => (-1, 0)

GetVector(direction.North) = (0, 1)
```

If you add a wildcard when all cases are covered, you'll get a warning
that the wildcard is unreachable:

<!--versetest
direction := enum:
    North
    South
    East
    West

GetVectorWithUnreachable(Dir:direction):tuple(int, int) =
    case (Dir):
        direction.North => (0, 1)
        direction.South => (0, -1)
        direction.East => (1, 0)
        direction.West => (-1, 0)
        _ => (0, 0)

assert:
    # Test that the function works despite unreachable wildcard
    GetVectorWithUnreachable(direction.North) = (0, 1)
<#
-->
<!-- 18 -->
```verse
    case (Dir):
        direction.North => (0, 1)
        direction.South => (0, -1)
        direction.East => (1, 0)
        direction.West => (-1, 0)
        _ => (0, 0)  # Warning: all cases already covered
```
<!-- #> -->

Incomplete case coverage is allowed in a `<decides>` context:

<!--versetest
direction := enum{  North, South, East, West}
-->
<!-- 19 -->
```verse
# Without wildcard in <decides> context - OK
GetPrimaryDirection2(Dir:direction)<decides>:string =
    case (Dir):
        direction.North => "Primary"
        # Other directions cause function to fail
```

Open enums can have values added after publication, so they can never
be exhaustive. They always require either a wildcard or a `<decides>`
context.

#### Loop Expressions

The `loop` expression creates an infinite loop that continues until
explicitly broken:

<!--versetest
UpdatePlayerPositions()<transacts>:void={}
CheckCollisions()<transacts>:void={}
RenderFrame()<transacts>:void={}
GameOver()<decides><transacts>:void={}
-->
<!-- 22 -->
```verse
GameLoop():void =
    loop:
        UpdatePlayerPositions()
        CheckCollisions()
        RenderFrame()
        if (GameOver[]). break
```

The `break` expression exits the loop entirely, terminating iteration.
`break` has "bottom" type—a type that represents a computation that
never returns normally. Since the bottom type is a subtype of all
other types, `break` can be used in any type context:

<!--versetest-->
<!-- 55 -->
```verse
NumberOfBits(X:int):int =
    var B:int = 1
    var C:int = 0
    loop:
        set B = if (B > X) { break } else { 2*B }
        set C = C+1
    C
```

This demonstrates bottom type: `break` unifies with `int` (from `2*B`)
in the if-expression. The assignment `set B = ...` uses the value of
the if-expression, showing that `break` is compatible in any type context.

**Loop Return Value:** The loop expression itself produces a value of type
`true`, regardless of what expressions appear in its body.
This return value is rarely useful in practice—loops are typically used for
their side effects.

When `break` appears in nested loops, it exits only the innermost
enclosing loop:

<!--versetest-->
<!-- 57 -->
```verse
var Outer:int = 0
loop:
    set Outer += 1
    var Inner:int = 0
    loop:
        set Inner += 1
        if (Inner = 5):
            break        # Exits inner loop
    if (Outer = 10):
        break            # Exits outer loop
```

The following restrictions apply. The `break` statement must appear in
a code block, not as part of a complex expression.  A loop must
contain at least one non-break statement. Finally, using `break`
outside a `loop` produces an error:

<!--versetest
ShouldStop()<decides>:void={}

assert_semantic_error(3506, 3581):
    ProcessData():void =
       if (ShouldStop[]):
               break      # Error
<#
-->
<!-- 58 -->
```verse
ProcessData():void =
   if (ShouldStop[]):
           break      # Error
```
<!-- #> -->
#### For Expressions

The `for` expression iterates over collections, ranges, and other
iterable types, providing a more structured approach to repetition:

<!--versetest
player:=class{}
GetScore(P:player):int=100
<#
-->
<!-- 23 -->
```verse
CalculateTotalScore(Players:[]player)<transacts>:int =
    var Total:int = 0
    for (Player : Players):
        PlayerScore := GetScore(Player)
        set Total += PlayerScore
    Total
```
<!-- #> -->

While it may look familiar from earlier imperative languages, `for` is
best thought of as a functional construct that combines iteration,
filtering with speculative execution, and construction of a collection
of results.

<!--versetest-->
<!-- 223 -->
```verse
Values:[]float= array{1.0, 10.1, 100.2}
Result := 
   for:
      V : Values
      V >= 10.0
      R := Floor[V]
   do:
      R*2.0

Result = array{20.0, 200.0}
```

The above is written with an alternative multi-clause syntax using the
`do:` keyword to  separate the iteration specification  from the body.
The `for` iterates  over the `Values` array,  discarding values smaller
than 10  and rounding down  numbers. It  returns an array  of floats.
The `Floor` function is defined as `decides` --if it were to fail that
iterate would be discarded.

There is another alternative syntax: the single-line dot syntax for
simple operations:

<!--versetest
Values:[]int = array{1, 2, 3}
DoSomething(V:int):void = {}
-->
<!-- 26 -->
```verse
# Single-line dot style
for (V : Values). DoSomething(V)
```

**Index and Value Pairs:**

When iterating arrays or maps, you can access both the index/key and the value
using the pair syntax `Index -> Value` or `Key -> Value`:

<!--versetest
player:=struct{ Name:string }
-->
<!-- 28 -->
```verse
PrintRoster(Players:[]player):void =
    for (Index -> Player : Players):
        Print("Player {Index}: {Player.Name}")
```

The index is zero-based, matching Verse's array indexing convention.

**Defining Variables in For Clauses:**

The for loop allows you to define intermediate variables that can be
used in subsequent filters or the loop body:

<!--versetest-->
<!-- 29 -->
```verse
# Define Y based on X
Doubled := for (X := 1..5, Y := X * 2):
    Y  # Returns array{2, 4, 6, 8, 10}

# Combine with filtering
SafeDivision := for (X := -3..3, X <> 0, Y := Floor[10.0 / (X*1.0)]):
    Y  # Skips X=0, returns array{-4, -5, -10, 10, 5, 3}
```

These intermediate variables are scoped to the iteration and can
reference earlier variables in the same clause.

**Multiple Filters:**

You can chain multiple filter conditions using comma-separated or
semicolon-separated expressions. Each filter must be failable, and if any fails, that
iteration is skipped:

<!--versetest-->
<!-- 30 -->
```verse
# Multiple independent filters
Filtered := for (X := 1..10, X <> 3, X <> 7):
    X  # Returns array{1, 2, 4, 5, 6, 8, 9, 10}

# Filters with intermediate variables
Complex := for (X := 1..5, X <> 2, Y := X * 2, Y < 10):
    Y  # Only includes values where X≠2 and Y<10
```

Each filter condition is evaluated in order, and iteration continues
only if all conditions succeed.

**Iterating Over Maps:**

Maps can be iterated over in two ways: values only, or key-value pairs
using the pair syntax:

<!--versetest-->
<!-- 31 -->
```verse
# Iterate over values only
Scores:[int]int = map{1 => 100, 2 => 200, 3 => 150}
TopScores := for (Score : Scores):
    Score  # Returns array{100, 200, 150}

# Iterate over key-value pairs
PlayerScores:[string]int = map{"Alice" => 100, "Bob" => 200}
for (PlayerName -> Score : PlayerScores):
    Print("{PlayerName} scored {Score}")
```

Maps preserve insertion order, so iteration order matches the order in
which keys were added to the map.

**String Iteration:**

Strings can be iterated character by character:

<!--versetest-->
<!-- 32 -->
```verse
CountVowels(Text:string):int =
    var Count:int = 0
    for (Char : Text, Char = 'a' or Char = 'e' or Char = 'i' or Char = 'o' or Char = 'u'):
        set Count += 1
    Count
```

**Nested Iteration (Cartesian Products):**

Multiple iteration sources create nested loops, producing the cartesian product:

<!--NoCompile-->
<!-- 33 -->
```verse
PrintGrid():void =
    for (X := 1..3, Y := 1..3):
        Print("({X}, {Y})")
    # Produces: (1,1), (1,2), (1,3), (2,1), (2,2), (2,3), (3,1), (3,2), (3,3)
```

**Filtering with Failure:**

Verse's `for` expressions are particularly powerful when they leverage
failure contexts, as they can naturally filter:

<!--versetest
player:=struct{ Name:string }
GetScore(P:player)<computes>:int=0
-->
<!-- 34 -->
```verse
GetHighScorers(Players:[]player):[]player =
    for (Player : Players, Score := GetScore(Player), Score > 1000):
        Player  # Only players with score > 1000 are included
```

When any expression in the iteration header fails, that iteration is
skipped. This allows elegant filtering without explicit `if`
statements:

<!--versetest
item:=struct{Price:float}
-->
<!-- 35 -->
```verse
# Filter items under budget and apply transformation
AffordableItems(Items:[]item, Budget:float):[]float =
    for (Item : Items, Item.Price <= Budget):
        Item.Price * 1.1  # Apply 10% markup
```

**For as an Expression:**

Like other control flow constructs, `for` is an expression. When the body produces values, `for` collects them into an array:

<!--versetest
player:=struct{Name:string}
-->
<!-- 36 -->
```verse
# Collect player names
GetNames(Players:[]player):[]string =
    for (Player : Players):
        Player.Name  # Each iteration produces a string
```

This makes `for` a powerful tool for transforming collections without
explicit accumulator variables.

**Breaking from For Loops:**

The `break` statement cannot exit `for` loops early. If you need only the
first matching result from an iteration, use `first` instead of `for`
(see [First Expressions](#first-expressions) below).

**Note on Continue:**

Unlike many languages, Verse does not currently support a `continue`
statement to skip to the next iteration. Instead, use conditional
logic or failure-based filtering to achieve similar results:

<!--versetest
item:=struct{IsValid:logic}
ProcessItem(I:item):void={}
-->
<!-- 38 -->
```verse
# Instead of continue, use conditional blocks
ProcessItems(Items:[]item):void =
    for (Item : Items):
        if (Item.IsValid?):
            ProcessItem(Item)
        # No continue needed - just structure with conditions

# Or use failure-based filtering in the header
ProcessValidItems(Items:[]item):void =
    for (Item : Items, Item.IsValid?):
        ProcessItem(Item)  # Only valid items reach here
```


**Range Iteration.** The range operator `..` provides numeric
iteration over integer sequences. Ranges are inclusive on both ends:

<!--versetest-->
<!-- 27 -->
```verse
# Iterates: 1, 2, 3, 4, 5 (both bounds included)
for (I := 1..5):
    Print("Count: {I}")

# Single element range
for (I := 42..42):
    Print("Answer: {I}")  # Prints once: "Answer: 42"

# Empty range (start > end produces no iterations)
for (I := 5..1):
    Print("Never executes")  # Loop body never runs
```

The `..` operator is always inclusive. There is no exclusive range
syntax.

Range bounds are evaluated in a specific order, and side effects occur
predictably:

1. **Left bound evaluated first**, then right bound
2. **Both bounds always evaluated**, even if the range is empty
3. **Side effects happen in order**, regardless of whether iterations occur

While you cannot store ranges as values, you can create arrays using
for expressions:

<!--versetest-->
<!-- 47 -->
```verse
# This works because for produces an array, not because ranges are storable
DoubledNumbers:[]int = for (I := 1..5){ I * 2 }

# Can then iterate over the array normally
for (N : DoubledNumbers):
    Print("{N}")
```

The range exists only during the for expression evaluation; the
resulting array is what gets stored.

**Restrictions.** The for loop has several important restrictions:

1. **Iteration source must be iterable:** Only ranges (`1..10`),
   arrays, maps, and strings can be iterated. 

2. **Filters must be failable:** Filter conditions must contain at
   least one expression that can fail. 

3. **Cannot redefine iteration variables:** You cannot redefine the
   iteration variable in the same clause.

4. **Cannot define mutable variables:** Using `var` to declare
   variables in the for clause is not allowed.

The range operator `..` has strict limitations that distinguish it
from other iterable types. Ranges are *not first-class values*—they
are expressions that iteratively yield each integer in the range as a
separate value. Ranges cannot be used in some contexts where you
might expect them to work:

<!--NoCompile-->
<!-- 40 -->
```verse
# ERROR: Cannot store range in variable
MyRange := 1..10
for (I := MyRange):

# ERROR: Cannot pass range to function
ProcessRange(1..10)

# ERROR: Cannot use range as standalone expression
Result := 1..10

# ERROR: Cannot put range in array
Ranges := array{1..10}

# ERROR: Cannot index range
Value := (1..10)(5)

# ERROR: Cannot access members on range
Length := (1..10).Length
```

Ranges work exclusively with the `int` type. Other numeric types,
booleans, types, or objects are not supported.

#### First Expressions

The `first` expression is similar to `for`, but instead of evaluating
the body for every iteration of the domain clause, it evaluates only
the **first** iteration of the domain clause that succeeds. Instead
of yielding an array as `for` does, it yields the value of the body
for that single iteration. If no iteration reaches the body, `first`
fails — so it requires a `<decides>` context.

<!--versetest
player:=struct{ Name:string }
GetScore(P:player)<computes><decides>:int=0
-->
<!-- 80 -->
```verse
# Find the first player with a score above the threshold
FindTopScorer(Players:[]player, Threshold:int)<decides>:player =
    first (Player : Players; GetScore[Player] > Threshold):
        Player
```

Like `for`, the `first` expression supports three syntax forms.
The block form uses `do:` to separate the iteration clauses from
the body:

<!--NoCompile-->
<!-- 81 -->
```verse
# Block form with do:
first:
    X : Collection
    Predicate[X]
do:
    Process(X)

# Braces form
first(X : Collection; Predicate[X]){ Process(X) }

# Dot form for single expressions
first(X : Collection; Predicate[X]). Process(X)
```

The `first` expression uses the same binding syntax as `for`. You
can iterate arrays, maps, strings, and ranges. You can use index-value
pairs with the `->` syntax, chain multiple filters, and nest multiple
iteration sources:

<!--versetest-->
<!-- 82 -->
```verse
# Find the index of an element using index -> value binding
IndexOf(Arr:[]int, Target:int)<decides>:int =
    first(I -> V : Arr, V = Target). I
```

Note that `first` yields the value of the **body** expression, not the
iteration variable. This is what makes it possible to search for one
thing and yield another — for example, finding an index by matching a
value.

**First vs For:**

| | `for` | `first` |
|-|-------|---------|
| Yields | Array of all results | First result only |
| On no match | Empty array | **Fails** (requires `<decides>`) |
| Stops | After all iterations | After the first iteration |

**Common Patterns:**

Since `first` requires `<decides>`, a common way to use it is to wrap
it in an `if` or an `option` to handle the case where no match is found:

<!--versetest-->
<!-- 83 -->
```verse
# Find with fallback using if
FindOrDefault(Arr:[]int, Target:int):int =
    if (Index := first(I -> V : Arr, V = Target). I):
        Index
    else:
        -1
```

Or:

<!--versetest-->
<!-- 84 -->
```verse
# Find with fallback using if
FindOptional(Arr:[]int, Target:int):?int =
    option:
        Index := first(I -> V : Arr, V = Target). I
            Index
```

#### Return Statements

The `return` statement provides explicit early exits from functions,
allowing you to terminate execution and return a value before reaching
the end of the function body:

<!--versetest-->
<!-- 48 -->
```verse
ValidateInput(Value:int):string =
    if (Value < 0):
        return "Error: Negative value"

    if (Value > 1000):
        return "Error: Value too large"

    "Valid"     # Implicit return
```

Return statements can only appear in specific positions within your
code—they must be in "tail position," meaning they must be the last
operation performed before control exits a scope. This restriction
ensures predictable control flow:

<!--versetest
GetOrder(:int)<transacts><decides>:order=order{}
order := class<allocates>{ IsValid()<decides><transacts>:logic=false }
-->
<!-- 49 -->
```verse
# Valid: return is last operation
ProcessOrder(OrderId:int)<transacts>:string =
    if (Order := GetOrder[OrderId]):
        if (Order.IsValid[]):
            return "Processed"
    "Invalid order"

# Valid: return in both branches
GetStatus(Value:int):string =
    if (Value > 0):
        return "Positive"
    else:
        return "Non-positive"
```

Verse functions implicitly return the value of their last expression,
so `return` is only needed for early exits:

<!--versetest
CalculateBonus(Score:int):int={
    if(Score<100)then{return 0}
    Score*10
}
-->
<!-- 51 -->
```verse
# Implicit return
GetValue():int = 42  # Returns 42

# Explicit early return
GetDiscount(Price:float):float =
    if (Price < 10.0):
        return 0.0  # Early exit with no discount

    Price * 0.1  # Implicit return with 10% discount
```

The `return` statement allows you to provide successful values from early
exits, while still allowing other paths to fail:

<!--versetest
config:=struct{MaxRetries:int}
GetConfig()<transacts><decides>:config=config{MaxRetries:=3}
AttemptOperation(Retry:int)<computes><decides>:string="success"
-->
<!-- 52 -->
```verse
RetryableOperation()<transacts>:string =
    if (Config := GetConfig[]):
        for (Retry := 1..Config.MaxRetries):
            if (Result := AttemptOperation[Retry]):
                return Result  # Success - exit immediately
    "Failed" # All retries exhausted
```

This pattern is common for search operations where you want to return
immediately upon finding a match, but fail if no match is found.

#### Defer Statements

The `defer` statement schedules code to run when the enclosing scope
exits. This makes it invaluable for cleanup operations like closing
files, releasing resources, or logging.

Defer is **scope-based**, not function-based. A `defer` block executes
when leaving the scope that directly contains it, including:

- **Function bodies** — runs when the function returns
- **`for` loops** — the `for` body runs each iteration in its own
  scope; the `for` domain also introduces a lexical scope
- **Each iteration of `loop` blocks** — runs at the end of each
  iteration (including on `break`)
- **`if`/`then`/`else` clauses** — runs when leaving the chosen branch
- **`block` scopes** — runs when leaving the block
- **`not` expressions** — `not e` evaluates `e` in a new lexical scope
- **`or` expressions** — `e0 or e1` evaluates `e0` in a new lexical
  scope
- **`and` expressions** — `e0 and e1` evaluates the entire expression
  in a new lexical scope
- **`option` and `logic` expressions** — `option{e}` and `logic{e}`
  evaluate `e` in a new lexical scope
- **`case` expressions** — `case(e0){e1=>e2, e3=>e4}` creates a
  lexical scope for the whole `case`, and then for each result
  expression (`e2`, `e4`)
- **Archetype instantiation** — `my_class{...}` introduces a lexical
  scope for the body
- **`defer` blocks themselves** — nested defers run when the outer
  defer completes
- **Structured concurrency macros** (`race`, `rush`, `branch`) —
  each arm runs in its own lexical scope
- **`spawn`, `await`, and `batch` expressions** — `spawn{e}`,
  `await{e}`, and `batch{e}` evaluate `e` in a new lexical scope
- **`live` bindings** — `live Name : e0 = e1` creates a new lexical
  scope for `e1`
- **Cancelled concurrent scopes** — runs during cancellation
  unwinding (see [Concurrency](14_BOOK_OF_VERSE_III_EFFECTS_CONCURRENCY_AND_EVOLUTION.md#cleanup-and-resource-management))

Here is a basic example:

<!--versetest
OpenFile(P:string)<computes>:?int=false
CloseFile(P:int)<computes>:void={}
ReadFile(P:int)<computes>:?string=false
ProcessContents(P:string)<computes><decides>:void={}
SaveResults()<computes><decides>:void={}
-->
<!-- 61 -->
```verse
ProcessFile(FileName:string)<transacts><decides>:void =
    File := OpenFile(FileName)?
    defer:
        CloseFile(File)  # Runs on success or early exit

    Contents := ReadFile(File)?
    ProcessContents[Contents]
    SaveResults[]
```

Deferred code executes when the scope exits successfully or through
explicit control flow like `return`:

<!--versetest
OpenConnection()<transacts>:int=0
CloseConnection(Id:int)<transacts>:void={}
Query(Id:int)<decides><transacts>:string="result"
ProcessResult(R:string)<transacts>:void={}

ProcessQuery()<transacts>:void =
    ConnId := OpenConnection()
    defer:
        CloseConnection(ConnId)  # Cleanup always needed

    for (Attempt := 1..5):
        if (Result := Query[ConnId]):
            ProcessResult(Result)
            return  # defer executes after return being called

    # defer executes before leaving the function scope on success

assert:
    # ProcessQuery is defined and demonstrates defer with return
<#
-->
<!-- 62 -->
```verse
ProcessQuery()<transacts>:void =
    ConnId := OpenConnection()
    defer:
        CloseConnection(ConnId)  # Cleanup always needed

    for (Attempt := 1..5):
        if (Result := Query[ConnId]):
            ProcessResult(Result)
            return  # defer executes after return being called

    # defer executes before leaving the function scope on success
```
<!-- #> -->

This is a subtle but crucial point: if a function fails due to
speculative execution, deferred code does **not** execute. This is
because failure triggers a rollback that undoes all effects, including
the scheduling of defer blocks:

<!--versetest
AcquireResource()<transacts><decides>:int=0
ReleaseResource(Id:int)<transacts>:void={}
RiskyOperation(Id:int)<transacts><decides>:void={}
-->
<!-- 63 -->
```verse
ExampleWithFailure()<transacts><decides>:void =
    ResourceId := AcquireResource[]
    defer:
        ReleaseResource(ResourceId) # Scheduled...

    RiskyOperation[ResourceId] # This fails!
    # defer does NOT run - entire scope was speculative and rolled back
```

When the `RiskyOperation` fails, the entire function also fails, and
speculative execution undoes everything—including the defer
registration. The resource cleanup never happens because the resource
acquisition itself is rolled back.

This behavior ensures consistency: if a function fails, it is as if it
never ran, including any cleanup code that was scheduled.

**Execution Order:**

When multiple `defer`s exist in the same scope, they execute in
reverse order of definition (last-in, first-out), mimicking the
stack-based cleanup of nested resources:

<!--versetest
OpenDatabase()<transacts>:int=0
CloseDatabase(Id:int)<transacts>:void={}
BeginTransaction(Id:int)<decides><transacts>:int=0
CommitTransaction(Id:int)<transacts>:void={}
DoWork()<transacts><decides>:void={}
-->
<!-- 64 -->
```verse
DatabaseTransaction()<transacts><decides>:void =
    DbId := OpenDatabase()
    defer:
        CloseDatabase(DbId)  # Executes second (outer resource)

    TxnId := BeginTransaction[DbId]
    defer:
        CommitTransaction(TxnId)  # Executes first (inner resource)

    DoWork[]  # Work happens with both resources active
    # Defers execute: CommitTransaction, then CloseDatabase
```

**Defers and Async Cancellation:**

Deferred code also executes when async operations are cancelled, such
as when a `race` completes or a `spawn` is interrupted:

<!--versetest
AcquireResource()<transacts>:int=0
ReleaseResource(Resource:int)<transacts>:void={}
LongRunningTask(Resource:int)<suspends><transacts>:void={}

ProcessWithTimeout()<suspends><transacts>:void =
    race:
        block:
            Resource := AcquireResource()
            defer:
                ReleaseResource(Resource)  # Runs if cancelled

            LongRunningTask(Resource)

        block:
            Sleep(10.0)  # Timeout
    # If timeout wins, first block is cancelled and defer runs

assert:
    # ProcessWithTimeout demonstrates defer with async cancellation
<#
-->
<!-- 65 -->
```verse
ProcessWithTimeout()<suspends><transacts>:void =
    race:
        block:
            Resource := AcquireResource()
            defer:
                ReleaseResource(Resource)  # Runs if cancelled

            LongRunningTask(Resource)

        block:
            Sleep(10.0)  # Timeout
    # If timeout wins, first block is cancelled and defer runs
```
<!-- #> -->

This ensures cleanup happens even when concurrency control interrupts your code.

**Nested Defers:**

Defer statements can be nested within other defer blocks, creating a
cascade of cleanup operations:

<!--versetest
Log(S:string)<transacts>:void={}
-->
<!-- 66 -->
```verse
ProcessWithCleanup():void =
    Log("A")
    defer:
        Log("B")
        defer:
            Log("inner")  # Runs after B
        Log("C")
    Log("D")
    # Output: A D B C inner
```

The execution order follows the LIFO principle at each nesting
level—inner defers execute after the outer defer's code, maintaining
the stack-like cleanup order.

**Defers in Control Flow:**

Defers work correctly within all control flow constructs:

<!--versetest
Log(S:string)<transacts>:void={}
-->
<!-- 67 -->
```verse
ProcessLoop():void =
    for (I := 0..2):
        Log("Start")
        defer:
            Log("Cleanup")  # Runs after each iteration
        Log("End")
    # Output: Start End Cleanup Start End Cleanup Start End Cleanup

ProcessWithIf(Condition:logic):void =
    if (Condition?):
        defer:
            Log("Then cleanup")
        Log("Then body")
    else:
        defer:
            Log("Else cleanup")
        Log("Else body")
```

Each control flow path executes its own defers independently.

**Defer Restrictions.** The defer statement has important restrictions
to ensure predictable behavior:

1. **Cannot be empty:** Defer blocks must contain at least one
   expression:

2. **Cannot be used as expression:** Defer cannot be used in positions
   where a value is expected.

3. **Cannot cross boundaries:** Defer blocks cannot contain `return`,
   `break`, or other control flow that would exit the defer's scope.

4. **Cannot fail:** Expressions in defer blocks cannot fail.

5. **Cannot suspend directly:** Defer blocks cannot contain suspend
   expressions, but they can use `spawn` for fire-and-forget async
   operations.

For how `defer` interacts with async cancellation and concurrency
constructs like `race` and `spawn`, see
[Cleanup and Resource Management](14_BOOK_OF_VERSE_III_EFFECTS_CONCURRENCY_AND_EVOLUTION.md#cleanup-and-resource-management).

#### Profiling

Understanding how your code performs is crucial for optimization, and
the `profile` expression measures execution time:

<!--versetest-->
<!-- 73 -->
```verse
OptimizedCalculation():float =
    profile("Complex Math"):
        var Result:float = 0.0
        for (I := 1..1000000):
            set Result += Sin(I*1.0) * Cos(I*1.0)
        Result
```

The profile expression wraps around the code you want to measure,
logging the execution time to the output. You can add descriptive tags
to organize your profiling output, making it easier to identify
bottlenecks in complex systems.

Profile expressions pass through their results transparently, meaning
you can wrap them around any expression without changing the program's
behavior:

<!--versetest
BaseDamage:float = 50.0
GetMultiplier()<computes>:float = 1.5
GetCriticalBonus()<computes>:float = 2.0
-->
<!-- 74 -->
```verse
PlayerDamage := profile("Damage Calculation"):
    BaseDamage * GetMultiplier() * GetCriticalBonus()
```

## Book of Verse Source Unit: 08_failure.md

### Failure

Most programming languages treat control flow as a matter of true or
false, yes or no, one or zero. They evaluate boolean conditions and
branch accordingly, creating a world of binary decisions that often
requires checking conditions twice - once to see if something is
possible, and again to actually do it. Verse takes a different
approach. Instead of asking "is this true?", Verse asks "does this
succeed?"

This distinction might seem subtle, but it changes how programs are
written and reasoned about. Failure is not an error or an
exception-it is a first-class concept that drives control flow. When an
expression fails, it does not crash your program or throw an exception
that needs to be caught. Instead, failure is a normal, expected
outcome that your code handles gracefully through the structure of the
language itself.

Consider the simple act of accessing an array element. In traditional languages, you might write:

<!--NoCompile-->
<!-- 01 -->
```verse
if (Index < Array.Length) {  # Traditional, non-Verse
    Value = Array[Index]
    Process(Value)
}
```

This checks validity separately from access, creating opportunities
for bugs if the check and access become separated or if the array
changes between them. In Verse, validation and access are unified:

<!--versetest
Array:[]int = array{1,2,3}
Index:int = 1
Process(V:int):void = {}
-->
<!-- 02 -->
```verse
if (Value := Array[Index]):
    Process(Value)
```

The array access either succeeds and binds the value, or it fails and
execution moves on. There is no separate validation step, so
the check and access cannot become inconsistent, and no undefined
behavior from accessing invalid indices.

#### Failable Expressions

A failable expression is one that can either succeed and produce a value, or fail and produce nothing. This is not the same as returning null or an error code - when an expression fails, it literally produces no value at all. The computation stops at that point in that particular path of execution.

Many operations are naturally failable. Array indexing fails when the index is out of bounds. Map lookups fail when the key does not exist. Comparisons fail when the values are not equal. Division fails when dividing by zero. Even simple literals can be made to fail:

<!--versetest
M()<decides>:void =
    42
    false?
    true?
<#
-->
<!-- 03 -->
```verse
42      # Always succeeds with value 42
false?  # Always fails - the query of false
true?   # Always succeeds - the query of true
```
<!-- #> -->

The query operator `?` turns any value into a failable expression. When applied to `false`, it always fails. When applied to any other value, it succeeds with that value. This simple mechanism provides immense power for controlling program flow.

You can create your own failable expressions through functions marked with the `<decides>` effect:

<!--versetest-->
<!-- 04 -->
```verse
ValidateAge(Age:int)<decides>:int =
    Age >= 0    # Fails if age is negative
    Age <= 150  # Fails if age is unrealistic
    Age         # Returns the age if both checks pass
```
<!-- ValidateAge[10] -->

This function does not just check conditions - it embodies them. If the age is invalid, the function fails. If it is valid, it succeeds with the age value. The validation and the value are inseparable.

#### Failure Contexts

Not every part of a program can execute failable expressions. They can only appear in failure contexts--places where the language knows how to handle both success and failure. Each failure context defines what happens when expressions within it fail.

The most common failure context is the condition of an `if` expression:

<!--versetest
Name:string="Joe"
GetPlayerByName(B:string)<decides><transacts>:int = 0
GetPlayerScore(B:int)<transacts><decides>:int = 0
-->
<!-- 05 -->
```verse
if (Player := GetPlayerByName[Name], Score := GetPlayerScore[Player], Score > 100):
    Print("High scorer: {Name} with {Score} points!")
```

This `if` condition contains three potentially failable expressions. All must succeed for the body to execute. If any fails, the entire condition fails, and control moves to the `else` branch (if present) or past the `if` entirely. The beauty is that each expression can use the results of previous ones - `Score` is only computed if we successfully found the `Player`.

The `for` expression creates a failure context for each iteration of the domain clause:

<!--versetest
Inventory:[]int= array{1}
IsWeapon:[]int= array{1}
GetDamage(:int)<computes><decides>:int=1
-->
<!-- 06 -->
```verse
for (Item : Inventory, IsWeapon[Item], Damage := GetDamage[Item], Damage > 50):
    Print("Powerful weapon: {Item} with {Damage} damage")
```

Each iteration attempts the failable expressions. If they all succeed, the body executes for that item. If any fails, that iteration is skipped, and the loop continues with the next item. This creates a natural filtering mechanism without explicit conditional logic.


Similar to `for`, the `first` expression creates a failure context for the domain clause:
<!--versetest
Inventory:[]int= array{1}
IsWeapon:[]int= array{1}
GetDamage(:int)<computes><decides>:int=1
-->
<!-- 100 -->
```verse
PowerfulWeapon := option. first(Item : Inventory, IsWeapon[Item], Damage := GetDamage[Item], Damage > 50). Item
```
Unlike `for`, if there are no successful iterations, `first` itself will fail, and so must be used in a failure context. In the above example, `option` is used to handle failure of the `first`.

Functions marked with `<decides>` create a failure context for their entire body:

<!--versetest
item:=struct{}
IsWeapon(i:item)<computes><decides>:void={}
GetDamage(i:item)<computes><decides>:int=0
-->
<!-- 07 -->
```verse
FindBestWeapon(Inventory:[]item)<decides>:item =
    var BestWeapon:?item = false
    var MaxDamage:int = 0

    for (Item : Inventory, IsWeapon[Item], Damage := GetDamage[Item]):
        if (Damage > MaxDamage):
            set BestWeapon = option{Item}
            set MaxDamage = Damage

    BestWeapon?  # Fails if no weapon was found
```

The function body is a failure context, allowing failable expressions throughout. The final line extracts the value from the option, failing if no weapon was found.

#### Speculative Execution

When you execute code in a failure context, changes to mutable variables are provisional—they only become permanent if the entire context succeeds. Functions that modify state in failure contexts must use the `<transacts>`  or the `<writes>` effect specifier (see [Effects](14_BOOK_OF_VERSE_III_EFFECTS_CONCURRENCY_AND_EVOLUTION.md#book-of-verse-source-unit-13effectsmd)):

<!--NoCompile-->
<!-- 08 -->
```verse
m:=module:
    buyer := class:
        var PlayerGold:int
        AttemptPurchase(Cost:int)<transacts><decides>:void =
           set PlayerGold = PlayerGold - Cost  # Provisional change
           PlayerGold >= 0                     # Check if still valid
           # If this fails, PlayerGold reverts to original value
```

If the check fails, the subtraction is automatically rolled back. You
do not need to manually restore the original value or check conditions
before modifying state.

This transactional behavior makes complex state updates safe and
predictable. Either everything succeeds and all changes are committed,
or something fails and nothing changes.

<!--versetest
game_state := struct{}
game := class:
    var State:game_state = game_state{}
    ModifyHealth()<transacts>:void = {}
    UpdateInventory()<transacts>:void = {}
    ChargeResources()<transacts>:void = {}
    ValidateFinalState()<transacts><decides>:void = {}
    ComplexOperation()<transacts><decides>:void =
       ModifyHealth()
       UpdateInventory()
       ChargeResources()
       ValidateFinalState[]
<#
-->
<!-- 09 -->
```verse
game := class:
    var State:game_state
    ComplexOperation()<transacts><decides>:void =
       ModifyHealth()       # All these operations
       UpdateInventory()    # are provisional
       ChargeResources()    # until all succeed
       ValidateFinalState[] # If this fails, everything rolls back
```
<!-- #> -->

The `game` class has multiple methods that update the `game_state`,
before returning `ComplexOperation` validates that the object is in a
valid state, if it is not, all changes performed in the method are
rolled back.

#### The Logic of Failure

Verse provides logical operators that work with failure, creating an
algebra for combining failable expressions.

The `and` operator ensures that both expression succeed.
The `not` operator inverts success and failure:

<!--versetest
Score:int=10
GetNearestEnemy()<decides><computes>:int=0
-->
<!-- 10 -->
```verse
if (not (Enemy := GetNearestEnemy[]) and Score > 0):
    Print("Coast is clear!")  # Executes when GetNearestEnemy fails
```

It is noteworthy that `Enemy` is not in scope within the `then` branch
because it is under a `not`.

The `or` operator provides alternatives:

<!--versetest
DefaultWeapon:?string=false
PrimaryWeapon()<decides><computes>:string="primary"
SecondaryWeapon()<decides><computes>:string="sword"
-->
<!-- 11 -->
```verse
Weapon := PrimaryWeapon[] or SecondaryWeapon[] or DefaultWeapon?
```

This tries each option in order, stopping at the first success. It's
not evaluating boolean conditions - it is attempting computations and
taking the first one that succeeds.

You can combine these operators to create sophisticated control flow:

<!--versetest
player := struct{}
IsAlive(P:player)<computes><decides>:void = {}
IsStunned(P:player)<computes><decides>:void = {}
HasAmmunition(P:player)<computes><decides>:void = {}
HasMeleeWeapon(P:player)<computes><decides>:void = {}
-->
<!-- 12 -->
```verse
ValidatePlayer(Player:player)<decides>:void =
    IsAlive[Player]
    not IsStunned[Player]
    HasAmmunition[Player] or HasMeleeWeapon[Player]
```

This function succeeds only if the player is alive, not stunned, and
has either ammunition or a melee weapon. Each line is a separate
failable expression that must succeed.

Another interesting use case is `not not Exp` -- it succeeds if `Exp`
succeeds but all effects of `Exp` are thrown away. This is a way to
try to see if a complex operation would succeed.

#### Expressions in Decides

A subtle feature is how relational expressions behave in decides
contexts. When a comparison appears in a context that can handle
failure, it does not just test a condition—it produces a value,
specifically it returns its left-hand side. So `X>0` returns `X` and
`0<=X` returns `0`.  This behavior applies to all comparison operators
in decides contexts:

<!--versetest-->
<!-- 14 -->
```verse
GetIfNotEqual(X:int, Y:int)<decides>:int =
    X <> Y  # Returns X when X ≠ Y, fails when X = Y

GetIfLessOrEqual(X:int, Limit:int)<decides>:int =
    X <= Limit  # Returns X when X ≤ Limit, fails otherwise

GetIfGreaterThan(X:int, Threshold:int)<decides>:int =
    X > Threshold  # Returns X when X > Threshold, fails otherwise
```
<!--
GetIfNotEqual[1,2]
GetIfGreaterThan[11,2]
GetIfLessOrEqual[1,2]
-->

Comparison expressions of the form `A op B` return `A` when the
comparison succeeds, and fail when the comparison is false.

This creates concise validation functions that either return `Value` or fail:

<!--versetest-->
<!-- 16 -->
```verse
ValidateInRange(Value:int, LwrBnd:int, UprBnd:int)<decides>:int =
    Value >= LwrBnd and Value <= UprBnd
```
<!-- ValidateInRange[5,0,10] -->

#### Option Types

The option type and failure are intimately connected. An option either
contains a value or is empty (represented by `false`). The query
operator `?` converts between options and failure:

<!--versetest-->
<!-- 18 -->
```verse
M()<decides>:void=
    MaybeValue:?int = option{42}  # An optional int
    Value := MaybeValue?          # Succeeds with 42

    Empty:?int = false            # An empty value
    Other := Empty?               # Failure
```

The `option{}` constructor works in reverse, converting failure to an empty option:

<!--versetest
RiskyComputation()<computes><decides>:int=1
-->
<!-- 19 -->
```verse
Result := option{RiskyComputation[]} # option{value} if computation succeeds
                                     # otherwise false
```
<!-- Result -->

This bidirectional conversion makes options and failure
interchangeable, allowing you to choose the most appropriate
representation for your specific use case.

The option type `?T` represents values that may or may not be present.
The question mark appears *before* the type, not after:

<!--versetest-->
<!-- 20 -->
```verse
ValidSyntax:?int = option{42}      # Correct
```
<!-- ValidSyntax? -->

The `?` prefix applies to any type:

<!--versetest
player := struct{}
-->
<!-- 21 -->
```verse
MaybeNumber:?int = option{42}
MaybeText:?string = option{"hello"}
MaybePlayer:?player = option{player{}}
```

Use the `option{}` constructor to wrap a value:

<!--versetest
RiskyComputation()<computes><decides>:int=1
-->
<!-- 22 -->
```verse
Filled:?int = option{42}
Empty:?int  = false
Result:?int = option{RiskyComputation[]}  # false if computation fails
```

Empty options and `false` are equivalent—an empty option *is* `false`:

<!--versetest-->
<!-- 23 -->
```verse
EmptyOption:?int = false
EmptyOption = false  # This comparison succeeds
```

Verse has a rich and flexible syntax which can also sometimes cause
subtle bugs. A comma gives rise to a tuple in an `option` whereas a
semicolon evaluates all values but retain only the last one:

<!--versetest-->
<!-- 24 -->
```verse
# Comma creates tuple
option{1, 2}? = (1, 2)

# Semicolon creates sequence - last value is used
option{1; 2}? = 2
```

##### Unwrapping

The query operator `?` extracts values from options, failing if the option is empty:

<!--versetest-->
<!-- 25 -->
```verse
M()<decides>:void=
    MaybeValue:?int = option{42}
    Value := MaybeValue?  # Succeeds with 42

    Empty:?int = false
    Other := Empty?  # Fails - cannot unwrap empty option
```

Unwrapping is only allowed in failure contexts:

<!--versetest
MaybeInt:?int = option{42}
UseItem(I:int):void={}
ProcessItem(I:int)<computes>:?int=option{3}
Items:[]int = array{1,2,3}
-->
<!-- 26 -->
```verse
# Valid: In if condition (failure context)
if (Value := MaybeInt?):
    Print("Got {Value}")

# Valid: In for loop (failure context)
for (Item : Items, ValidItem := ProcessItem(Item)?):
    UseItem(Item)

# Valid: In <decides> function body (failure context)
GetRequired(Maybe:?int)<decides>:int =
    Maybe?  # Fails if Maybe is empty
```

##### Nesting

Options can be nested to represent multiple layers of absence:

<!--versetest-->
<!-- 27 -->
```verse
# Double-nested option
Double:??int = option{option{42}}

# Single unwrap gets outer option
if (Inner := Double?):
    if (TheValue := Inner?):
        # TheValue has type int, equals 42

# Double unwrap gets the value directly
Value := Double??  # Fails if either layer is empty
```

Helper functions also work with nested options:

<!--versetest-->
<!-- 28 -->
```verse
UnpackNested(MaybeValue:??int):?int =
    if (Inner := MaybeValue?):
        Inner
    else:
        option{-1}  # Default for outer empty

DirectUnpack(MaybeValue:??int):int =
    if (Value := MaybeValue??):
        Value
    else:
        -1  # Default for any level empty
```
<!--
UnpackNested(option{option{1}})
DirectUnpack(option{option{2}})
-->

##### Chained Access

The `?.` operator provides safe member access on optional values:

<!--NoCompile-->
<!-- 29 -->
```verse
entity := class:
    Name:string = "Unknown"
    Health:int = 100

MaybeEntity:?entity = option{entity{}}

# Safe field access
if (Name := MaybeEntity?.Name):
    Print("Entity: {Name}")  # Succeeds

# Safe method call
MaybeEntity?.TakeDamage(10)  # Only calls if entity present

# Chaining through multiple optionals
linked_list := class:
    Value:int = 0
    Next:?linked_list = false

Head:?linked_list = option{linked_list{Value := 1}}
SecondValue := Head?.Next?.Value  # Fails if any link is empty
```

The `?.` operator short-circuits—if the option is empty, the entire
expression fails without evaluating the member access.

##### Defaulting

Use the `or` operator to provide fallback values for empty options:

<!--versetest-->
<!-- 30 -->
```verse
MaybeValue:?int = false
Value := MaybeValue? or 42  # Yields 42

# Chaining multiple options
Primary:?string = false
Secondary:?string = option{"backup"}
Default:string = "default"

Result := Primary? or Secondary? or Default
```
##### Comparison

Empty options equal `false`, and filled options equal their unwrapped values when compared properly:

<!--versetest-->
<!-- 40 -->
```verse
EmptyOption:?int = false
EmptyOption = false  # Succeeds

FilledOption:?int = option{1}
FilledOption? = 1  # Succeeds - unwrap then compare
```

However, you cannot directly compare optional and non-optional values without unwrapping:

<!--versetest-->
<!-- 41 -->
```verse
Opt:?int = option{42}
Regular:int = 42

# Must unwrap to compare
if (Opt? = Regular):
    Print("Equal")
```

#### Failure with Optionals

Combining decides functions with optional return types, creates a system with
multiple layers of failure. This pattern enables expressing complex conditions
concisely while maintaining clarity.

A function can fail at two levels:

- *Function-level failure*: The entire function fails using `<decides>`
- *Value-level failure*: The function succeeds but returns an empty option

<!--versetest
player := string
IsActive(S:string)<transacts><decides>:string=""
LookupPlayer(S:string)<transacts><decides>:string="player"
-->
<!-- 42 -->
```verse
FindEligiblePlayer(Name:string)<decides>:?player =
    Name <> ""           # Layer 1: Fail if name is empty
    Player := LookupPlayer[Name]  # Layer 1: Fail if player not found
    option{IsActive[Player]}      # Layer 2: Empty option if player inactive
```
<!-- FindEligiblePlayer["Someone"] -->

This function has three possible outcomes:

- *Function fails*: Empty name or player not found
- *Function succeeds with empty option*: Player found but inactive
- *Function succeeds with filled option*: Player found and active

Calling this function demonstrates the layered failure:

<!--versetest
FindEligiblePlayer(S:string)<transacts><decides>:?string=option{S}
-->
<!-- 43 -->
```verse
# Function-level failure
Result1 := FindEligiblePlayer[""]  # Fails, Result1 never assigned

# Function succeeds, returns empty option
if (Player := FindEligiblePlayer["InactiveUser"]?):
    # Won't execute - function succeeds but ? query fails
else:
    # Executes here

# Function succeeds, returns filled option
if (Player := FindEligiblePlayer["ActiveUser"]?):
    # Executes with Player bound to the active player
```

This pattern is particularly powerful for validation with different failure modes:

<!--versetest-->
<!-- 44 -->
```verse
ValidateScore(Score:int)<decides>:?int =
    Score >= 0           # Layer 1: Reject negative scores (invalid input)
    option{Score <= 100} # Layer 2: Reject high scores (out of range)
```

The distinction between function-level and value-level failure lets
you express different kinds of errors. Function-level failure
typically means "this operation couldn't complete" while value-level
failure means "the operation completed but the result does not meet the
expected criteria."

#### Casts as Decides

Type casting in Verse is integrated into the failure system. A dynamic cast
behaves just like a `<decides>` function call and similarly uses bracket
syntax. For example `Type[value]` attempts to cast `value`'s type to `Type` and
fails if unsuccessful.

This is also works with user defined types which must specify `<castable>`:

<!--versetest
component := class<castable>:
    Name:string = "Component"

physics_component := class<castable>(component):
    Velocity:float = 0.0

TryGetPhysics(Comp:component)<decides>:physics_component =
    physics_component[Comp]
<#
-->
<!-- 48 -->
```verse
component := class<castable>:
    Name:string = "Component"

physics_component := class<castable>(component):
    Velocity:float = 0.0

# Casting as a decides operation
TryGetPhysics(Comp:component)<decides>:physics_component =
    physics_component[Comp]  # Succeeds if Comp is actually a physics_component
```
<!-- #> -->

This makes type-based dispatch easily expressible:

<!--versetest
component := class<castable>:
    Name:string = "Component"
physics_component := class<castable>(component):
    Velocity:float = 0.0
render_component := class<castable>(component):
    Renderer:string = "RayTrace"
UpdatePhysics(P:physics_component):void={}
UpdateRendering(R:render_component):void={}
UpdateGeneric(G:component):void={}
-->
<!-- 49 -->
```verse
ProcessComponent(Comp:component):void =
    if (Physics := physics_component[Comp]):
        UpdatePhysics(Physics)
    else if (Render := render_component[Comp]):
        UpdateRendering(Render)
    else:
        # Unknown component type
        UpdateGeneric(Comp)
```
<!-- ProcessComponent(component{}) -->

The cast itself is the condition—no separate type checking needed. When the cast succeeds, you have both confirmed the type and obtained a properly-typed reference.

You can chain casts with other decides operations:

<!--versetest
component := class<castable>:
    Name:string = "Component"
physics_component := class<castable>(component):
    Velocity:float = 0.0
render_component := class<castable>(component):
    Renderer:string = "RayTrace"
UpdatePhysics(P:physics_component):void=return
UpdateRendering(R:render_component):void=return
UpdateGeneric(G:component):void=return
entity := class:
    GetComponent()<transacts><decides>:component=
        component{}
IsActive(c:component)<transacts><decides>:logic=true
-->
<!-- 50 -->
```verse
GetActivePhysicsComponent(Entity:entity)<decides>:physics_component =
    Comp := Entity.GetComponent[]  # Fails if no component
    Physics := physics_component[Comp]  # Fails if not physics
    IsActive[Physics]  # Fails if inactive
    Physics
```

Each step must succeed for the function to return a value. This creates self-documenting validation chains where type requirements are explicit.

Casts work with the `or` combinator for fallback types:

<!--versetest
component := class<castable>:
    Name:string = "Component"
physics_component := class<castable>(component):
    Velocity:float = 0.0
trigger_component := class<castable>(component):
    Trigger:float = 0.0
scripted_component := class<castable>(component):
    Scripted:string = "Something"
UpdatePhysics(P:physics_component):void=return
UpdateGeneric(G:component):void=return
entity := class:
    GetComponent()<transacts><decides>:component=
        component{}
IsActive(c:component)<transacts><decides>:logic=true
GetInteractable(Entity:entity)<decides><transacts>:component =
    physics_component[Entity] or
    trigger_component[Entity] or
    scripted_component[Entity]
<#
-->
<!-- 51 -->
```verse
GetInteractable(Entity:entity)<decides>:component =
    physics_component[Entity] or
    trigger_component[Entity] or
    scripted_component[Entity]
```
<!--
#>
GetInteractable[entity{}]
-->

This tries each cast in order, returning the first successful one. It's type-safe because all options share the common `component` base type.


#### Idioms and Patterns

As you work with failure, certain patterns emerge that solve common problems elegantly.

The validation chain pattern uses sequential failures to ensure all conditions are met:

<!--versetest
action := struct{}
player := struct{}
location := struct{}
GetActingPlayer(A:action)<transacts><decides>:player = player{}
IsValidTurn(P:player)<computes><decides>:void = {}
HasRequiredResources(P:player, A:action)<computes><decides>:void = {}
GetTargetLocation(A:action)<transacts><decides>:location = location{}
IsValidLocation(L:location)<computes><decides>:void = {}
ExecuteAction(A:action)<transacts><decides>:void = {}
-->
<!-- 62 -->
```verse
ProcessAction(Action:action)<decides>:void =
    Player := GetActingPlayer[Action]
    IsValidTurn[Player]
    HasRequiredResources[Player, Action]
    Location := GetTargetLocation[Action]
    IsValidLocation[Location]
    ExecuteAction[Action]
```

Each line must succeed for execution to continue. This creates self-documenting code where preconditions are explicit and checked in order.

The first-success pattern tries alternatives until one works:

<!--versetest
location := struct{}
path := struct{}
DirectPath(S:location, E:location)<transacts><decides>:path = path{}
PathAroundObstacles(S:location, E:location)<transacts><decides>:path = path{}
ComplexPathfinding(S:location, E:location)<transacts><decides>:path = path{}
-->
<!-- 63 -->
```verse
FindPath(Start:location, End:location)<decides>:path =
    DirectPath[Start, End] or
    PathAroundObstacles[Start, End] or
    ComplexPathfinding[Start, End]
```

This naturally expresses trying simple solutions before complex ones.

The filtering pattern uses failure to select items:

<!--versetest
enemy := struct{}
GetLevel(E:enemy)<computes><decides>:int = 10
-->
<!-- 64 -->
```verse
GetEliteEnemies(Enemies:[]enemy):[]enemy =
    for (Enemy : Enemies, Level := GetLevel[Enemy], Level >= 10):
        Enemy
```

Only enemies that have a level and whose level is at least 10 are included in the result.

The transaction pattern groups related changes:

<!--versetest
player := class:
    var Inventory:[]item = array{}
item := struct{}
RemoveItem(P:player, I:item)<transacts><decides>:void = {}
AddItem(P:player, I:item)<transacts>:void = {}
ValidateTrade(P1:player, P2:player)<computes><decides>:void = {}
-->
<!-- 65 -->
```verse
TradeItems(PlayerA:player, PlayerB:player, ItemA:item, ItemB:item)<transacts><decides>:void =
    RemoveItem[PlayerA, ItemA]
    RemoveItem[PlayerB, ItemB]
    AddItem(PlayerA, ItemB)
    AddItem(PlayerB, ItemA)
    ValidateTrade[PlayerA, PlayerB]
```

Either the entire trade succeeds, or nothing changes.

**Optional Indexing**

When working with optional containers, you can access their contents
using specialized query syntax that combines optional checking with
element access.  Optional tuples support direct element access through
the query operator:

<!--versetest-->
<!-- 58 -->
```verse
MaybePair:?tuple(int, string) = option{(42, "answer")}

# Access first element
if (FirstValue := MaybePair?(0)):
    # FirstValue is 42 (type: int)
    Print("First: {FirstValue}")

# Access second element
if (SecondValue := MaybePair?(1)):
    # SecondValue is "answer" (type: string)
    Print("Second: {SecondValue}")
```

The syntax `Option?(index)` simultaneously:

- Queries whether the option is non-empty
- Accesses the tuple element at the given index
- Binds the element value if both succeed

**Composition and Call Chains**

Decides functions compose naturally, allowing complex operations to be
built from simple, reusable pieces. When a decides function calls
another decides function, failures propagate automatically.

<!--versetest-->
<!-- 52 -->
```verse
ValidatePositive(X:int)<decides>:int =
    X > 0

Double(X:int)<decides>:int =
    Validated := ValidatePositive[X]  # Fails if X ≤ 0
    Validated * 2
```
<!-- Double[2] -->

If `ValidatePositive` fails, `Double` fails immediately. The validated value flows through the chain.

**Preserving failure context:**

When calling decides functions in non-decides contexts, you must handle failure explicitly:

<!--versetest
FindPlayer(Name:string)<transacts><decides>:string=Name
GetDefaultPlayer():string="Default"
UsePlayer(P:string):void=return
-->
<!-- 57 -->
```verse
# This will not compile - ProcessPlayer does not have <decides>
# BadProcessPlayer(Name:string):void =
#    Player := FindPlayer[Name]  # ERROR: Unhandled failure

# Handle with if
ProcessPlayerWithIf(Name:string):void =
    if (Player := FindPlayer[Name]):
        UsePlayer(Player)

# Handle with or
ProcessPlayerWithOr(Name:string):void =
    Player := FindPlayer[Name] or GetDefaultPlayer()
    UsePlayer(Player)
```
<!--
PlayerOne := "PlayerOne"
ProcessPlayerWithIf(PlayerOne)
ProcessPlayerWithOr(PlayerOne)
-->

Understanding composition helps you build complex validation logic
from simple, testable pieces.

#### Runtime Errors

While failure (`<decides>`) represents normal control flow with
transactional rollback, *runtime errors* represent unrecoverable
conditions that terminate execution. Runtime errors propagate up the
call stack, bypassing normal failure handling, and cannot be caught or
recovered within Verse code.

The `Err()` function explicitly triggers a runtime error with an optional message:

<!--versetest-->
<!-- 66 -->
```verse
ValidateInput(Value:int):int =
    if (Value < 0):
        Err("Negative values not allowed")
    Value
```

When a runtime error occurs, execution unwinds through the call stack,
terminating the current operation:

<!--versetest
Log(Message:string)<transacts>:void = {}
-->
<!-- 68 -->
```verse
DeepFunction()<transacts>:int =
    Log("C")
    Err("Fatal error")  # Runtime error here
    Log("D")            # Never executes
    return 1

MiddleFunction():int =
    Log("B")
    Result := DeepFunction()  # Error propagates through here
    Log("E")                  # Never executes
    return Result

TopFunction():void =
    Log("A")
    Value := MiddleFunction()  # Error propagates to here
    Log("F")                   # Never executes

# Execution order: A, B, C, then terminates
# Output: "ABC"
```

The runtime error propagates immediately, bypassing all subsequent code in the call chain.

Runtime errors propagate through asynchronous operations, terminating spawned tasks:

<!--versetest
Log(Message:string)<transacts>:void = {}
WaitTicks(Count:int)<suspends>:void = {}
-->
<!-- 69 -->
```verse
AsyncOperation()<suspends>:int =
    Log("Start")
    WaitTicks(1)
    Err("Async error")  # Runtime error during async execution
    WaitTicks(1)        # Never executes
    return 1

KickOff()<suspends>:void=
    # Error propagates out of spawned task
    spawn{ AsyncOperation() }

```

When a spawned task encounters a runtime error, that specific task
terminates. The runtime error does not automatically propagate to the
spawning context.

#### Living with Failure

Verse's approach to failure has roots in logic programming, where
computations search for solutions rather than executing steps. When a
path fails, the computation backtracks and tries alternatives. This
non-deterministic model, while powerful, can be hard to reason about
in its full generality.  Verse tames this power by making failure
contexts explicit and limiting backtracking to specific
constructs. You get the benefits of logic programming - declarative
code, automatic search, elegant handling of alternatives - without the
complexity of full unification and unbounded backtracking.

Consider a simple logic puzzle solver:

<!--versetest
constraint := struct{}
solution := struct{}
InitialState()<transacts>:solution = solution{}
ApplyConstraint(S:solution, C:constraint)<transacts>:void = {}
ValidateSolution(S:solution)<computes><decides>:void = {}
-->
<!-- 73 -->
```verse
SolvePuzzle(Constraints:[]constraint)<decides>:solution =
    var State:solution = InitialState()
    for (Constraint : Constraints):
        ApplyConstraint(State, Constraint)
    ValidateSolution[State]
    State
```

If any constraint can't be satisfied, the entire attempt fails. In a full logic programming language, this might trigger complex backtracking. In Verse, the failure model is simpler and more predictable while still being expressive enough for most problems.

Working effectively with failure in Verse requires a shift in mindset. Instead of thinking about error conditions that need to be avoided, think about success conditions that need to be met. Instead of defensive programming that checks everything before acting, write optimistic code that attempts operations and handles failure gracefully.

This perspective makes code more readable and intent more clear. When you see a function marked with `<decides>`, you know it represents a computation that might not have a result. When you see expressions in sequence within a failure context, you know they represent conditions that must all be met. When you see the `or` operator, you know it represents alternatives to try.

Failure in Verse is not something to be feared or avoided - it is a tool to be embraced. It makes programs safer by eliminating certain categories of bugs. It makes code clearer by unifying validation and action. It makes complex operations simpler by providing automatic rollback. Most importantly, it aligns the way we write programs with the way we think about actions and decisions in the real world.

As you write more Verse code, you'll find that failure becomes second nature. You'll reach for failable expressions naturally when expressing conditions. You'll structure your functions to fail early when preconditions are not met. You'll compose failures to create sophisticated control flow without nested conditionals. And you'll appreciate how this different way of thinking about control flow leads to code that is both more robust and more expressive than traditional approaches.

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
- [13_BOOK_OF_VERSE_II_PROGRAM_STRUCTURE_AND_TYPES.md](13_BOOK_OF_VERSE_II_PROGRAM_STRUCTURE_AND_TYPES.md)
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
