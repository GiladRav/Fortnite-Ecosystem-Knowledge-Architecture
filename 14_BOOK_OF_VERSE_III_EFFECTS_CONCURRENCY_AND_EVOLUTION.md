# Book of Verse III: Effects, Concurrency, and Evolution

## Document Metadata

| Field | Value |
|---|---|
| Document ID | `14` |
| Domain | Verse language |
| Primary Environment | Verse; includes potentially unreleased language-main material |
| Language | English; Hebrew appears only in canonical terminology fields and exact source identifiers |
| Source Priority | Epic official documentation -> current Book of Verse -> verified local technical corpus -> research and teaching sources |
| Last Verified | 2026-08-02 |
| Stability Status | Mixed: stable concepts plus version-sensitive interface and platform details |

## Document Purpose

Preserve advanced Book of Verse material on effects, concurrency, Live Variables, persistable types, and code evolution.

## Scope and Exclusions

Live Variables and other main-branch features must not be assumed available in shipping UEFN; practical integration belongs to `09` and maintenance to `10`.

## When to Use This Document

- Use for effect checking, async/concurrency, cancellation, persistence schemas, compatibility, and code evolution.

## Authority and Availability Gate

Book of Verse can describe language-main or planned functionality before it is available in the current UEFN release. It is not the final authority for current compiler availability. Before giving executable guidance:

1. confirm the construct in Epic's current Verse Language Reference or Verse API Reference;
2. confirm that the active UEFN compiler accepts it;
3. label material that is absent from the official current reference as **experimental or unreleased**;
4. never present a Book of Verse example as guaranteed shipping UEFN behavior solely because it appears in this document.

`Live Variables` and any dependent reactive constructs are treated as unreleased until Epic's current UEFN documentation and compiler confirm otherwise.

## Quick Topic Index

- [Critical Availability Warning](#critical-availability-warning)
- [Book of Verse Source Unit: 13_effects.md](#book-of-verse-source-unit-13effectsmd)
- [Book of Verse Source Unit: 14_concurrency.md](#book-of-verse-source-unit-14concurrencymd)
- [Book of Verse Source Unit: 15_live_variables.md](#book-of-verse-source-unit-15livevariablesmd)
- [Book of Verse Source Unit: 17_persistable.md](#book-of-verse-source-unit-17persistablemd)
- [Book of Verse Source Unit: 18_evolution.md](#book-of-verse-source-unit-18evolutionmd)

## Common Question Router

- For environment selection and corpus-wide routing, start with [`00_MASTER_KNOWLEDGE_INDEX.md`](00_MASTER_KNOWLEDGE_INDEX.md).
- For current official URLs or version-sensitive claims, use [`01_EPIC_GAMES_DOCUMENTATION_INDEX.md`](01_EPIC_GAMES_DOCUMENTATION_INDEX.md).
- For a simple English-to-Hebrew name mapping, use [`02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md`](02_ENGLISH_HEBREW_TERMINOLOGY_INDEX.md).
- For a detailed definition, use the domain glossary listed under Related Glossary Sections.

---

## Critical Availability Warning

**Book of Verse tracks the head of the Verse language main branch and can document features before they ship in UEFN.** `Live Variables` is treated here as **Unreleased / language-main** unless the current UEFN compiler, Epic language reference, and release notes confirm availability. Apply the same verification rule to every advanced construct that is absent from current Epic UEFN documentation.

## Book of Verse Source Unit: 13_effects.md

### Effects

Every function tells two stories. The first story, told through types,
describes what data flows in and what data flows out. The second
story, told through effects, describes what the function does along
the way — whether it reads from memory, writes to storage, might fail,
or could suspend execution. While most languages leave this second
story implicit, Verse makes it explicit, turning side effects from
hidden surprises into documented contracts.

Think about a simple game function that updates a player's score. In
most languages, you'd see a signature like `UpdateScore(player,
points)` and have to guess what happens inside. Does it modify the
player object? Write to a database? Print to a log? Trigger
animations? Without reading the implementation, you can't know. In
Verse, effects are part of the signature itself, declaring upfront
exactly what kinds of operations the function might perform.

This explicitness might seem like extra work at first, but it
fundamentally changes how you reason about code. When you see
`<reads>` on a function, you know it observes mutable state. When you
see `<writes>`, you know it modifies that state. When you see
`<decides>`, you know it might fail. These are not comments or
documentation that might be wrong — they are compiler-enforced
contracts that must be accurate.

#### Understanding Effects

Effects represent observable interactions between your code and the
world around it. Reading a player's health, updating a score, spawning
a particle effect, waiting for an animation to complete — all these
operations have effects that ripple beyond simple computation. Verse's
effect system captures these interactions, making them visible and
verifiable.

Consider this simple function that greets a player:

<!--versetest
c:=class:
    var CurrentGreeting:string=""
    GreetPlayer()<transacts>:void =
        set CurrentGreeting = "Hello, adventurer!"
assert:
    C:=c{}
    C.GreetPlayer()
<#
-->
<!-- 01 -->
```verse
GreetPlayer()<transacts>:void =
    set CurrentGreeting = "Hello, adventurer!"
    Print(CurrentGreeting)
```
<!--
#>
-->

The `<transacts>` effect tells you immediately that this function
modifies mutable state. You do not need to read the implementation to
know that calling `GreetPlayer()` will change something in your
program's memory. The effect is a promise about behavior, checked and
enforced by the compiler.

Effects compose naturally through function calls. If function A calls
function B, and B has certain effects, then A must declare at least
those same effects (with some exceptions we'll explore). This
propagation ensures that effects can't be hidden or laundered through
intermediate functions — the true nature of an operation is always
visible at every level of the call stack.

**Why Effects Matter**

Making effects explicit serves both human understanding and compiler
optimization. For developers, effects act as documentation that can't
lie. When you are debugging why a value changed unexpectedly, you can
trace through the call chain looking only at functions with
`<writes>`. When you are trying to understand why a function might
fail, you look for `<decides>`. This is not guesswork — it is guaranteed
by the type system.

For the compiler, explicit effects enable powerful optimizations and
safety guarantees. Pure functions marked `<computes>` can be memoized,
their results cached because they'll always return the same output for
the same input. Functions without `<writes>` can be safely executed in
parallel without locks. Functions without `<decides>` can be called
without failure handling.

The effect system also enforces architectural decisions. Want to
ensure your math library remains pure? Mark its functions
`<computes>`. Building a predictive client system that must run on
players' machines? Use `<predicts>` to ensure no server-only
operations sneak in. These are not just conventions — they are
compiler-enforced guarantees.

#### Effect Families and Specifiers

Verse organizes effects into families, each tracking a specific aspect
of computation. Each family contains fundamental effects, and effect
specifiers declare which effects a function may perform.

The six effect families are:

* **Cardinality**: Whether and how a function returns
* **Heap**: Access to mutable memory
* **Suspension**: Whether a function may suspend execution
* **Divergence**: Whether a function may run forever
* **Prediction**: Where a function runs
* **Internal**: Reserved for internal use

Some effects have no specifier, while some specifiers imply multiple
effects. For instance, `<transacts>` implies `reads`, `writes` and
`allocates`, and belongs to the Heap family.

Effect specifiers can be further divided into *exclusive* specifiers
(`<converges>`, `<computes>`, `<transacts>`) and *additive* specifiers
(`<suspends>`, `<decides>`, `<reads>`, `<writes>`, `<allocates>`). A
function may have at most one exclusive specifier but can combine
multiple additive ones. For example, `<computes><decides>` is valid
(pure computation that may fail), but `<computes><transacts>` is an
error (cannot have two exclusive effects).

|Fundamental Effect|Effect Specifier|Effect Family|Effects implied by Specifier | Notes |
| -----        | -----------    | -------     | ----- | ---- |
| **succeeds** |                | Cardinality |                  | *No specifier; Must Succeed* |
| **fails**    |                | Cardinality |                  | *No specifier; Can Fail* |
|              | `<decides>`    | Cardinality | `{succeeds, fails}` | *Cannot combine with `<suspends>`* |
| **reads**    | `<reads>`      | Heap        | `{reads}`        | *Allows reading mutable states* |
| **writes**   | `<writes>`     | Heap        | `{writes}`       | *Allows writing mutable states* |
| **allocates**| `<allocates>`  | Heap        | `{allocates}`    | *Allows allocation of mutable memory* |
|              | `<transacts>`  | Heap        | `{reads, writes, allocates}` | *Exclusive; default* |
|              | `<computes>`   | Heap        | `{}`             | *Exclusive; Pure computation* |
| **suspends** | `<suspends>`   | Suspension  | `{suspends}`     | *Cannot combine with `<decides>`* |
| **diverges** |                | Divergence  | `{diverges}`     | *No specifier; May run forever* |
|              | `<converges>`  | Divergence  | `{}`             | *Exclusive; Native functions, abstract methods, type signatures* |
| **dictates** |                | Prediction  | `{dictates}`     | *No specifier; Server Authority* |
|              | `<predicts>`   | Prediction  | `{}`             | *Allows Client Prediction* |
| **no_rollback** |             | Internal    | `{no_rollback}`  | *To be deprecated; Transactions disallowed* |

The following restrictions are in effect:

- `<suspends>` and `<decides>` cannot be combined on the same function,
- `<converges>` is only allowed on `<native>` functions, abstract methods, and type signatures,
- duplicate specifiers (e.g., `<computes><computes>`) are errors.

#### How Effects Compose

Think of effect specifiers as setting bits in a bit vector: one bit
per fundamental effect. Without any annotation, a function such as
`GameUpdate` has the following effects:

<!--NoCompile-->
<!-- 02 -->
```verse
GameUpdate():void = ...  # No explicit effects specified
```

| dictates | suspends | reads | writes | allocates | succeeds | fails |
| :---:    | :---:    | :---: | :---:  | :---:     | :---:    | :---: |
| ✔️       | ❌      | ✔️    | ✔️    | ✔️        | ✔️      | ❌    |

This means it has effects `dictates`, `reads`, `writes`, `allocates`
and `succeeds`. It's almost like writing `<dictates><transacts>`
except we lack a way to say the function cannot fail.

As an aside: the absence of specifiers for `fails` and `succeeds` can
be explained by the fact that a specifier like `<fails>` means the
function always fails, never returns a value, and cannot have
observable side effects (they would be undone by failure).  The
`succeeds` effect is implicit.

Annotating a function only affects the bits in that specifier's
family. For example, function `CheckPlayerStatus` with the `<reads>`
and `<predicts>` specifier:

<!--NoCompile-->
<!-- 03 -->
```verse
CheckPlayerStatus()<reads><predicts>:string = ...
```

has the following effects:

| dictates | suspends | reads | writes | allocates | succeeds | fails |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| ❌ | ❌ | ✔️ | ❌ | ❌ | ✔️ | ❌ |

Specifying `<reads>` clears the `writes` and `allocates` bits, and
`<predicts>` clears the `dictates` bit, everything else is unchanged.

#### Effect Families in Detail

##### Cardinality effects

The cardinality family deals with whether functions return values
successfully. Every function either succeeds (returning its declared
type) or fails (producing no value). Most functions always succeed —
they are deterministic transformations that always produce output. But
functions marked with `<decides>` can fail, turning failure into a
control flow mechanism.

<!--versetest
ValidateHealth(Health:float)<transacts><decides>:void =
    Health > 0.0
    Health <= 100.0

StartCombat():void={}
player:=struct{Health:float}

assert:
    Player:=player{Health:=50.0}
    if (ValidateHealth[Player.Health]):
        StartCombat()
<#
-->
<!-- 04 -->
```verse
ValidateHealth(Health:float)<transacts><decides>:void =
    Health > 0.0      # Fails if health is zero or negative
    Health <= 100.0   # Fails if health exceeds maximum

# Usage
if (ValidateHealth[Player.Health]):
    # Health is valid, continue processing
    StartCombat()
```
<!--
#>
-->

The beauty of the decides effect is that it unifies validation with
control flow. You do not check conditions and then act on them — the
check itself drives the program's path.

##### Heap effects

The heap family governs access to mutable memory. This is perhaps the
most important family for understanding program behavior, as it
determines whether functions can observe or modify state.

The `<computes>` specifier marks pure functions — those that neither
read nor write mutable state. These functions are deterministic: given
the same inputs, they always produce the same outputs. They're the
mathematical ideal of computation, transforming data without side
effects.

<!--versetest-->
<!-- 05 -->
```verse
CalculateDamage(BaseDamage:float, Multiplier:float)<computes>:float =
    BaseDamage * Multiplier
```

The `<reads>` effect allows functions to observe mutable state. They
can see the current values of variables and mutable fields, but cannot
modify them. This is useful for queries and calculations based on
current game state.

<!--versetest
player := class:
    Name:string
    var Health:float = 100.0
    var Score:int = 0

GetPlayerStatus(P:player)<reads>:string =
    if (P.Health > 50.0):
        "Healthy"
    else if (P.Health > 0.0):
        "Injured"
    else:
        "Defeated"

assert:
    P:=player{Name:="Test"}
    Status:=GetPlayerStatus(P)
<#
-->
<!-- 06 -->
```verse
player := class:
    Name:string
    var Health:float = 100.0
    var Score:int = 0

GetPlayerStatus(P:player)<reads>:string =
    if (P.Health > 50.0):
        "Healthy"
    else if (P.Health > 0.0):
        "Injured"
    else:
        "Defeated"
```
<!--
#>
-->

The `<writes>` effect permits modification of mutable state. Functions
with this effect can use `set` to update variables and mutable
fields. `<writes>` often requires `<reads>` as well, for instance when
modification involves reading the current value.

In fact, the `set` instruction is by default `<transacts>` due to the
addition of *live variables* to the language. A live variable is
variable whose value depends on other variables; when one of those
variables is updated by a `set` the live variable will be evaluated
with potentially some `reads` and `allocates`.

<!--versetest
player := class:
    Name:string
    var Health:float = 100.0

HealPlayer(P:player, Amount:float)<transacts>:void =
    NewHealth := P.Health + Amount
    set P.Health = Min(NewHealth, 100.0)

assert:
    P:=player{Name:="Test", Health:=50.0}
    HealPlayer(P, 30.0)
<#
-->
<!-- 07 -->
```verse
HealPlayer(P:player, Amount:float)<transacts>:void =
    NewHealth := P.Health + Amount
    set P.Health = Min(NewHealth, 100.0)
```
<!--
#>
-->

The `<allocates>` effect indicates functions that create observably
unique values — either objects marked `<unique>` or values containing
mutable fields. Each call to such a function returns a distinct value,
even if the inputs are identical.

<!--NoCompile-->
<!-- 08 -->
```verse
game_entity := class<allocates>:
    ID:id
    var Position:vector3

CreateEntity(Pos:vector3)<allocates>:game_entity =
    game_entity{ID := GenerateID(), Position := Pos}
```

The `<transacts>` is the default for functions. 

##### Suspension effects

The suspension family contains a single effect:
`<suspends>`. Functions with this effect can pause their execution and
resume later, potentially across multiple game frames. This is
essential for operations that take time: animations, cooldowns,
waiting for player input, or any multi-frame behavior.

<!--NoCompile-->
<!-- 09 -->
```verse
PlayVictorySequence()<suspends>:void =
    PlayAnimation(VictoryDance)
    Sleep(2.0)  # Wait 2 seconds
    PlaySound(VictoryFanfare)
    Sleep(1.0)
    ShowRewardsScreen()
```

The `suspends` effect is viral — any function that calls a suspending
function must itself be marked `<suspends>`. This ensures you always
know which functions might take time to complete.

While `<suspends>` and `<decides>` cannot be combined on the same
function, they have specific rules for how they interact across
function calls. A `<suspends>` function can call a `<decides>`
function, but *only within a failure context* using the square bracket
`[]` syntax -- this ensures that the failure is handled locally and
does not propagate as a failure effect:

<!--versetest
DoAsyncWork():void={}
-->
<!-- 10 -->
```verse
ValidateInput(Value:int)<decides><computes>:void =
    Value > 0
    Value < 100

ProcessAsync(Value:int)<suspends>:void =
    # Valid: calling decides function in failure context
    if (ValidateInput[Value]):
        # Process valid input
        DoAsyncWork()

# Invalid: calling decides function outside failure context
# ProcessAsync(Value:int)<suspends>:void =
#     ValidateInput(Value)  # ERROR: must use [] syntax
```

A `<suspends>` function can call another `<suspends>` function, but *must not use failure-handling syntax* like `?`:

<!--versetest-->
<!-- 11 -->
```verse
AsyncOp()<suspends>:?int = false

CallAsync()<suspends>:void =
    # Valid: calling suspends function normally
    X := AsyncOp()

    # Invalid: cannot use ? with suspends in suspends context
    # if (Value := AsyncOp()?):
```

The asymmetry exists because `<suspends>` and `<decides>` represent
fundamentally different control flow mechanisms—suspension is about
time, while failure is about success/failure. Mixing their syntactic
forms creates ambiguity about what's being handled.

##### Internal effects

**[Pre-release]**: The `<no_rollback>` effect is deprecated.

###### Prediction effects

!!! note "Unreleased Feature"
    The `<predicts>` effect is not yet released.

The prediction family determines where code runs in a client-server
architecture. By default, functions have the `dictates` effect,
meaning they run authoritatively on the server. The `<predicts>`
specifier allows functions to run predictively on clients for
responsiveness, with the server later validating and potentially
correcting the results.

<!--NoCompile-->
<!-- 12 -->
```verse
HandleJumpInput()<predicts>:void =
    # Runs immediately on the client for responsiveness
    StartJumpAnimation()
    PlayJumpSound()

    # Server will validate and correct if needed
    PerformJump()
```

This enables responsive gameplay even with network latency, as players
see immediate feedback for their actions while the server maintains
authoritative state.

###### Divergence effects

Currently in planning, the divergence family will track whether
functions are guaranteed to terminate. The `<converges>` specifier
marks functions that provably complete in finite time, while
functions without it might run forever. This is particularly important
for constructors and initialization code.

The `<converges>` specifier can be used on:

- `<native>` functions that are guaranteed to terminate
- Abstract method signatures in classes and interfaces
- Function signatures in type expressions

Regular function implementations cannot use `<converges>` — only their declarations in abstract contexts or as native functions.


<!-- TODO: write more -->

#### Effect Composition

Effects generally propagate up the call chain — a function must
declare all the effects of the functions it calls. However, certain
language constructs can hide specific effects, preventing them from
propagating further.

An `if` expression hides `fails` effects in its failure context, thus
failure failure in a condition does not propagate to the enclosing
function:

<!--versetest-->
<!-- 13 -->
```verse
SafeMod(A:int, B:int)<computes>:int =
    if (V:= Mod[A,B])  then V else 0
```

The `spawn` expression hides the `suspends` effect, allowing immediate
functions to start asynchronous operations that continue
independently:

<!--versetest
GetNextTrack():int=0
PlayTrack(:int)<suspends>:void={}
-->
<!-- 14 -->
```verse
Play()<suspends>:void =
        loop:
            PlayTrack(GetNextTrack())
            Sleep(180.0)  

StartBackgroundMusic():void =  # No <suspends>
    spawn:
        Play() # Suspends effect hidden by spawn
```

As mentioned above failure is not allowed within `<suspends>` code
including `spawn`. One way around this restriction is to use the
`option` expression to convert failure into an optional value,
transforming the `fails` effect into a regular value that can be
handled without `<decides>`:

<!--versetest
item:=struct{}
-->
<!-- 16 -->
```verse
TryGetItem(Items:[]item, Index:int):?item =
    option{Items[Index]}  # Array access might fail, option catches it
```

The `defer` expression provides cleanup code that runs when exiting a
scope, but has strict effect limitations:

- Cannot contain `<suspends>` operations—deferred code must execute synchronously
- Cannot contain `<decides>` operations—deferred code must always succeed

<!--versetest
resource:=class{}
DoAsyncWork():void={}
GetResource()<transacts>:resource=resource{}
-->
<!-- 17 -->
```verse
AcquireResource()<transacts>:resource = GetResource()
ReleaseResource(R:resource)<transacts>:void = {}

ProcessResource()<suspends>:void =
    R := AcquireResource()
    defer:
        ReleaseResource(R)  # Valid: transacts allowed in defer

    # Process resource with async operations
    DoAsyncWork()
```

These constraints ensure that cleanup code executes predictably and
completely, without the possibility of suspension or failure that
could leave resources in an inconsistent state.

#### Subtyping and Type Compatibility

Effect annotations create a subtyping relationship between function
types. Understanding how effects interact with type compatibility is
essential when storing functions in variables, passing them as
parameters, or choosing between different implementations.

A function with **fewer effects** can be used where a function with
**more effects** is expected. This is effect subtyping—a function that
does less is compatible with a context that allows more:

<!--versetest-->
<!-- 18 -->
```verse
# Pure function with only computes
PureAdd(X:int)<computes>:int = X + 1

# Variable that expects computes and decides
F:type{_(:int)<computes><decides>:int} = PureAdd

# Calling through the variable
Result := F[5]  # Must use [] syntax since type has <decides>
# Returns 6 since PureAdd never fails
```

In this example, `PureAdd` has only `<computes>`, but it can be
assigned to a variable expecting `<computes><decides>`. The pure
function is a valid implementation of the failable interface—it simply
never exercises the failure capability.

This principle applies to all effects:

<!--versetest-->
<!-- 19 -->
```verse
# Function with <computes>
Compute(X:int)<computes>:int = X * 2

# Can assign to types expecting more effects
F1:type{_(:int)<computes><decides>:int} = Compute
F2:type{_(:int)<transacts>:int} = Compute
F3:type{_(:int)<reads>:int} = Compute

# All valid - Compute does less than what's allowed
```

When deciding subtyping, effects have the following impact:

- `<computes>` is a subtype of `<reads>`, `<transacts>`, and any combination with `<decides>`
- `<reads>` is a subtype of `<transacts>`
- Functions without `<decides>` are subtypes of functions with `<decides>`
- Functions without `<suspends>` are subtypes of functions with `<suspends>` (when compatible)

While you can add effects through subtyping, you **cannot remove**
effects that a function actually has:

<!--versetest-->
<!-- 20 -->
```verse
Validate(X:int)<computes><decides>:int =
    X > 0
    X

# ERROR: Cannot assign to type without <decides>
# F:type{_(:int)<computes>:int} = Validate
# The function CAN fail, but the type does not allow it
```

Similarly, functions with heap effects cannot be assigned to pure types:

<!--NoCompile-->
<!-- 21 -->
```verse
counter := class:
    var Count:int = 0

Increment(C:counter)<transacts>:int =
    set C.Count = C.Count + 1
    C.Count

# ERROR: Cannot assign transacts function to computes type
# F:type{_(:counter)<computes>:int} = Increment
# The function writes state, type does not permit it
```

This restriction ensures type safety—the type signature is a promise
about what effects the function might perform, and the actual function
must honor that promise.

When you conditionally select between functions with different
effects, the resulting expression has the union of all possible
effects. This is *effect joining*—the compiler conservatively assumes
the result might perform any effect that any branch could perform:

<!--versetest-->
<!-- 22 -->
```verse
# Functions with different effects
PureFunction(X:int)<computes>:int = X + 1
FailableFunction(X:int)<computes><decides>:int =
    X > 0
    X + 1

# Conditional selection joins effects
SelectFunction(UseFailable:logic):type{_(:int)<computes><decides>:int} =
    if (UseFailable?):
        FailableFunction  # Has <computes><decides>
    else:
        PureFunction      # Has <computes>
    # Result type must account for both: <computes><decides>

# The returned function might fail (from FailableFunction)
# or might not (from PureFunction), so type must include <decides>
F := SelectFunction(true)
Result := F[5]  # Must use [] because result type has <decides>
```

Effect joining applies to all control flow that selects between functions:

<!--versetest-->
<!-- 23 -->
```verse
Identity(X:int)<computes>:int = X

DecidesIdentity(X:int)<computes><decides>:int =
    X > 0
    X

TransactsIdentity(X:int)<transacts>:int = X

# Joining <computes> and <computes><decides>
F1:type{_(:int)<computes><decides>:int} =
    if (true?):
        Identity
    else:
        DecidesIdentity
# Result: <computes><decides> (union of effects)

# Joining <computes><decides> and <transacts>
F2:type{_(:int)<decides><transacts>:int} =
    if (true?):
        DecidesIdentity  # <computes><decides>
    else:
        TransactsIdentity  # <transacts>
# Result: <decides><transacts> (union of effects)
```


Effect subtyping enables flexible function parameters:

<!--versetest
PureAdd(:int)<computes>:int=1
Validate(:int)<computes><decides>:int=1
Increment(:int)<transacts>:int=1
-->
<!-- 25 -->
```verse
# Accepts any function that does not exceed <transacts><decides>
ProcessValues(
    Data:[]int,
    Transform(:int)<transacts><decides>:int
):[]int =
    for (Value:Data, Result := Transform[Value]):
        Result

# Can pass pure functions
ProcessValues(array{1, 2, 3}, PureAdd)

# Can pass failable functions
ProcessValues(array{1, 2, 3}, Validate)

# Can pass transactional functions
ProcessValues(array{1, 2, 3}, Increment)
```

Effect subtyping makes function composition work naturally:

<!--versetest
PureFunction(:int)<computes>:int=1
FailableFunction(:int)<computes><decides>:int=1
-->
<!-- 26 -->
```verse
Compose(
    F(:int)<computes>:int,
    G(:int)<computes>:int
):type{_(:int)<computes>:int} =
    Local(X:int)<computes>:int = G(F(X))
    Local

# If we want to allow more effects:
ComposeFlexible(
    F(:int)<transacts><decides>:int,
    G(:int)<transacts><decides>:int
):type{_(:int)<transacts><decides>:int} =
    Local(X:int)<transacts><decides>:int =
        if (IntermediateResult := F[X]):
            G[IntermediateResult]
        else:
            1=2; 0
    Local

# Can pass functions with fewer effects
ComposeFlexible(PureFunction, PureFunction)
ComposeFlexible(PureFunction, FailableFunction)
```

The following table summarize the interaction of effects and types:

| Scenario | Valid? | Explanation |
|----------|--------|-------------|
| Assign `<computes>` to `<computes><decides>` type | ✓ | Adding effects via subtyping |
| Assign `<computes>` to `<transacts>` type | ✓ | Pure is subtype of transactional |
| Assign `<reads>` to `<transacts>` type | ✓  | Reads is subtype of transactional |
| Assign `<computes><decides>` to `<computes>` type | ✗  | Cannot remove `<decides>` |
| Assign `<transacts>` to `<computes>` type | ✗  | Cannot remove heap effects |
| Select between `<computes>` and `<decides>` | Result: `<computes><decides>` | Effect joining |
| Select between `<reads>` and `<transacts>` | Result: `<transacts>` | Effect joining |

These rules ensure that effect annotations remain trustworthy
contracts—functions can do less than declared (subtyping), but never
more, and conditional selection conservatively accounts for all
possibilities (joining).

#### Effects on Data Types

Classes, structs, and interfaces can be annotated with effect
specifiers, which apply to their constructors. This is particularly
useful for ensuring that creating certain objects remains pure or has
limited effects:

<!--versetest-->
<!-- 28 -->
```verse
# Pure data structure - constructor has no effects
vector3 := struct<computes>:
    X:float = 0.0
    Y:float = 0.0
    Z:float = 0.0

# Entity that requires allocation due to unique identity
monster := class<unique><allocates>:
    Name:string
    var Health:float = 100.0
```

Classes, interfaces, and structs **cannot** be marked with `<suspends>` or `<decides>`:

<!--versetest-->
<!-- 29 -->
```verse
# Valid effect specifiers for classes/interfaces/structs:
valid_class := class<computes>{}
valid_interface := interface<computes>{}
valid_struct := struct<transacts>{}

# Invalid: async and failable effects not allowed
# invalid_class := class<suspends>{}      # ERROR
# invalid_interface := interface<decides>{}  # ERROR
# invalid_struct := struct<decides>{}     # ERROR
```

This restriction applies to the class/struct **declaration** itself —
the archetype constructor `my_class{...}` cannot be failable or
suspending. However, **constructor functions** can use `<decides>`:

<!--NoCompile-->
<!-- 29a -->
```verse
# The class declaration cannot be <decides>
my_class := class:
    Value:int

# But a constructor function CAN be <decides>
MakeMyClass<constructor>(V:int)<transacts><decides> := my_class:
    Value := block:
        V > 0      # Fails if V <= 0
        V < 100    # Fails if V >= 100
        V
```

This provides failable construction when needed — the object either
exists fully formed or the constructor function fails and no object
is created.

Field default values and block clauses in classes have strict effect requirements:

<!--versetest-->
<!-- 30 -->
```verse
# Field initializers must use pure functions
HelperFunction()<transacts>:int = 42

# Invalid: field initializers cannot call transacts functions
# bad_class := class:
#     Value:int = HelperFunction()  # ERROR

# Block clauses must respect class effects
valid_class := class<transacts>:
    var Counter:int = 0
    block:
        set Counter = 1  # Valid: class has transacts

# Invalid: block effect exceeds class effect
# bad_class := class<computes>:
#     var Counter:int = 0
#     block:
#         set Counter = 1  # ERROR: computes class cannot write
```

Class member initializers and block clauses are implicitly restricted
to have no more effects than what the class declares. This ensures
that constructing an instance of the class respects the class's effect
contract.

Limiting constructor effects helps maintain architectural
boundaries. Data transfer objects can be kept pure with `<computes>`,
ensuring they are just data carriers. Game entities might require
`<allocates>` for unique identity, while service objects might need
full `<transacts>` to initialize their state.

##### Interface Construction Effect Constraints

When classes or interfaces inherit from interfaces with construction effects, they must declare at least the same construction effects:

<!--versetest-->
<!-- 31 -->
```verse
# Interface with transacts effect
transacting_interface := interface<transacts>{}

# Valid: class has at least transacts
valid_class := class<transacts>(transacting_interface){}

# Invalid: class has less effects than interface requires
# invalid_class := class<computes>(transacting_interface){}  # ERROR
```

Interface field initializers must also respect the interface's declared construction effects:

<!--versetest-->
<!-- 32 -->
```verse
transacting_class := class<transacts>{}

# Valid: interface has transacts, field initializer has transacts
valid_interface := interface<transacts>:
    Instance:transacting_class = transacting_class{}

# Invalid: interface has computes, but field initializer has transacts
# invalid_interface := interface<computes>:
#     Instance:transacting_class = transacting_class{}  # ERROR
```

These constraints ensure that construction effects flow correctly through inheritance hierarchies. A class inheriting an interface must be able to construct all interface fields, which requires having at least the same construction effects.

#### Working with Effects

When designing functions, start with the minimal effects needed and
expand only when necessary. Pure functions with `<computes>` are the
easiest to test, reason about, and compose. Add `<reads>` when you
need to observe state, `<writes>` when you need to modify it, and
`<decides>` when you need failure-based control flow.

Effects are part of your API contract. Once published, removing
effects is a backwards-compatible change (your function does less than
before), but adding effects is breaking (your function now does more
than callers might expect). Design your effect signatures
thoughtfully, as they become promises to your users.

Remember that over-specifying effects is allowed and sometimes
beneficial. A function marked `<reads>` can be implemented as pure
`<computes>` internally. This provides flexibility for future changes
without breaking existing callers.

<!--versetest
weapon:=struct<computes>{Type:weapon_type,Dammage:int}
weapon_type:=enum:
    Sword
-->
<!-- 321 -->
```verse
# API promises it might read state
GetDefaultWeapon<public>()<reads>:weapon =
    # But current implementation is pure
    weapon{Type := weapon_type.Sword, Dammage := 10}
```

Effect over-specification can future-proof APIs and avoid breaking
changes later. For example, marking a currently pure function as
`<reads>` allows you to add state observation in the future without
breaking compatibility.

#### Backwards Compatibility

The effects of a function are part of what is checked for backwards
compatibility. When updating a function that is part of a published
API, the new version can have "fewer bits" but not more. So, a
function that was marked as `<reads>` in a previous version cannot be
changed to `<transacts>`, but it can be refined to `<computes>`.

Effects transform side effects from hidden gotchas into visible,
verifiable contracts. By making the implicit explicit, Verse helps you
write more predictable, maintainable, and correct code. The effect
system is not a burden — it is a tool that helps you express your intent
clearly and have the compiler verify that your implementation matches
that intent.

## Book of Verse Source Unit: 14_concurrency.md

### Concurrency

Concurrency is a fundamental aspect of Verse, allowing you to control
time flow as naturally as you control program flow. Unlike traditional
programming languages that bolt on concurrency as an afterthought,
Verse integrates time flow control directly into the language through
dedicated expressions and effects.

Game development inherently requires managing multiple simultaneous
activities. Think about a typical game scene: NPCs patrol their routes
while particle effects play, UI elements animate as cooldown timers
count down, and background music fades between tracks. All these
activities happen concurrently, overlapping in time. Verse recognizes
this reality and provides first-class language constructs to express
these parallel behaviors naturally.

The language achieves this through a combination of structured and
unstructured concurrency primitives, all built on the concept of async
expressions that can suspend and resume across multiple simulation
updates. This approach makes concurrent programming feel as natural as
writing sequential code, while avoiding the traditional pitfalls of
thread-based concurrency like data races and deadlocks.

#### Core Concepts

##### Immediate vs Async Expressions

Every expression falls into one of two categories: immediate or
async. Understanding this distinction is crucial for working with
Verse's concurrency model.

Immediate expressions evaluate with no delay, completing entirely
within the current simulation update or frame. These include most
basic operations you'd expect to happen instantly: arithmetic
calculations, variable access, simple function calls, and data
structure manipulation. When you write `X := 5 + 3`, the addition
happens immediately, the assignment completes instantly, and execution
moves to the next statement without any possibility of interruption.

Async expressions, on the other hand, have the possibility of taking
time to evaluate, potentially spanning multiple simulation
updates. They represent operations that inherently take time in the
game world: animations playing out, timers counting down, network
requests completing, or simply waiting for the next frame. An async
expression might complete immediately if its conditions are already
met, or it might suspend execution, allowing other code to run while
it waits for the right moment to resume.

##### Simulation Updates

A simulation update (or tick) represents one step of the game's
simulation. Simulation and rendering are **independent** — they run
at separate rates and are decoupled from each other in modern engines.

Each tick processes input, updates game logic, runs physics, and
advances the game state. Verse's concurrency model lets you think
in terms of logical time flow — async expressions suspend at tick
boundaries and resume in future ticks when their conditions are met.

Async expressions naturally align with this update cycle. When an
async expression suspends, it yields control back to the game engine,
which continues processing other tasks and rendering frames. The
suspended expression resumes in a future update when its conditions
are met, seamlessly continuing from where it left off. This
cooperative model ensures that long-running operations do not block the
game's responsiveness.

##### The `suspends` Effect

Concurrent operations require the `<suspends>` effect specifier (see
[Effects](14_BOOK_OF_VERSE_III_EFFECTS_CONCURRENCY_AND_EVOLUTION.md#book-of-verse-source-unit-13effectsmd)). Functions marked with `<suspends>` can use
concurrency expressions, call other suspending functions, and
cooperatively yield execution:

<!--versetest-->
<!-- 01 -->
```verse
# Function marked with suspends can use async expressions
MyAsyncFunction()<suspends>:void =
    Sleep(1.0)  # Pause execution
    Print("One second later!")

# Regular functions cannot use async expressions
MyImmediateFunction():void =
    # Sleep(1.0)  # ERROR: Cannot use Sleep without suspends
    Print("This happens immediately")
```

The `<suspends>` effect propagates through the call chain—any function
calling a suspending function must itself be marked `<suspends>`.

#### Structured Concurrency

Structured concurrency represents one of Verse's most elegant design
decisions. Rather than spawning threads or tasks that live
independently and require manual lifecycle management, structured
concurrency expressions have lifespans naturally bound to their
enclosing scope. When you enter a structured concurrency block, you
know that all concurrent operations within it will be properly managed
and cleaned up when the block exits, preventing resource leaks and
making code easier to reason about.

This approach mirrors how we think about sequential code. Just as a
block of sequential statements has a clear beginning and end,
structured concurrent operations have a defined lifetime. You can nest
them, compose them, and reason about them using the same mental model
you use for regular code blocks.

##### Effect Requirements

All structured concurrency expressions (`sync`, `race`, `rush`, and
`branch`) require the `<suspends>` effect. You cannot use these
constructs in immediate (non-suspending) functions:

<!--versetest
Operation1<public>()<suspends>:void = {}
Operation2<public>()<suspends>:void = {}
-->
<!-- 02 -->
```verse
# Valid: structured concurrency in suspends function
ProcessConcurrently()<suspends>:void =
    sync:
        Operation1()
        Operation2()

# Invalid: cannot use sync without suspends
# ProcessImmediate():void =
#     sync:  # ERROR: sync requires suspends
#         Operation1()
```

##### The sync Expression

The `sync` expression embodies the simplest concurrent pattern: doing
multiple things at once and waiting for all of them to finish. When
you have independent operations that can benefit from parallel
execution, `sync` provides a clean way to express this parallelism
while maintaining deterministic behavior.

<!--versetest
AsyncOperation1()<suspends>:int=1
AsyncOperation2()<suspends>:int=1
AsyncOperation3()<suspends>:int=1
F()<suspends>:void={
Results := sync:
    AsyncOperation1()
    AsyncOperation2()
    AsyncOperation3()
Print("All operations complete with results: {Results(0)} {Results(1)} {Results(2)}")
}
<#
-->
<!-- 04 -->
```verse
# All expressions start simultaneously and must all complete
Results := sync:
    AsyncOperation1()  # Returns value1
    AsyncOperation2()  # Returns value2
    AsyncOperation3()  # Returns value3

Print("All operations complete with results: {Results(0)} {Results(1)} {Results(2)}")
```
<!-- #> -->

Inside a `sync` block, all subexpressions begin execution at
essentially the same moment. The sync expression then waits patiently
for every single subexpression to complete, regardless of how long
each takes individually. If one operation finishes in milliseconds
while another takes several seconds, sync continues waiting until that
last operation completes. Only then does execution continue past the
sync block.

The beauty of sync lies in its predictability. You always get results
from all subexpressions, always in the same order you wrote them,
packaged neatly in a tuple. This makes sync perfect for scenarios
where you need multiple pieces of data or need to ensure multiple
systems are ready before proceeding. Loading game assets in parallel,
initializing multiple subsystems simultaneously, or gathering data
from multiple sources all benefit from sync's all-or-nothing approach.

Consider a more sophisticated example that demonstrates sync's composability:

<!--versetest
LoadTexture()<suspends>:void={}
ApplyTexture()<suspends>:void={}
LoadSound()<suspends>:void={}
PlaySound()<suspends>:void={}
LoadModel():void={}
ProcessData(:int,:int,:int):void={}
FetchDataA()<suspends>:int=1
FetchDataB()<suspends>:int=1
FetchDataC():int=1
F()<suspends>:void={
sync:
    block:  # Task 1 - sequential operations
        LoadTexture()
        ApplyTexture()
    block:  # Task 2 - parallel to task 1
        LoadSound()
        PlaySound()
    LoadModel()  # Task 3 - parallel to tasks 1 and 2
ProcessData(sync:
    FetchDataA()
    FetchDataB()
    FetchDataC()
)
}
<#
-->
<!-- 05 -->
```verse
# Nested blocks for complex operations
sync:
    block:  # Task 1 - sequential operations
        LoadTexture()
        ApplyTexture()
    block:  # Task 2 - parallel to task 1
        LoadSound()
        PlaySound()
    LoadModel()  # Task 3 - parallel to tasks 1 and 2

# Using sync results directly as function arguments
ProcessData(sync:
    FetchDataA()
    FetchDataB()
    FetchDataC()
)
```
<!--versetest
#>
-->

##### The race Expression

Where `sync` embodies cooperation, `race` represents competition. The
race expression starts multiple async operations simultaneously, but
only cares about the first one to cross the finish line. As soon as
one subexpression completes, race immediately cancels all the others
and continues with the winner's result. This winner-takes-all
semantics makes race perfect for timeout patterns, fallback
mechanisms, and any situation where you want the fastest possible
response.

<!--versetest
SlowOperation()<suspends>:int=0
FastOperation()<suspends>   :int=0
MediumOperation()<suspends>   :int=0

TestRace()<suspends>:void =
    # First to complete wins, others are canceled
    Winner := race:
        SlowOperation()     # Takes 5 seconds
        FastOperation()     # Takes 1 second - wins!
        MediumOperation()   # Takes 3 seconds

    Print("Winner result: {Winner}")  # Prints FastOperation's result 
<#
-->
<!-- 06 -->
```verse
# First to complete wins, others are canceled
Winner := race:
    SlowOperation()     # Takes 5 seconds
    FastOperation()     # Takes 1 second - wins!
    MediumOperation()   # Takes 3 seconds

Print("Winner result: {Winner}")  # Prints FastOperation's result
```
<!-- #> -->

The power of race becomes apparent when you consider real game
scenarios. Imagine querying multiple servers for data, where you want
to use whichever responds first. Or implementing a player action with
a timeout, where either the player completes the action or time runs
out. Race elegantly expresses these patterns without complex state
management or manual cancellation logic.

Cancellation in race is immediate and thorough. The moment a winner
emerges, all losing subexpressions receive a cancellation signal and
begin cleanup. This is not just an optimization; it is crucial for
resource management and preventing unwanted side effects from
operations that are no longer needed.

**Type handling in race:**

The type system handles race elegantly. Since only one subexpression's
result will be returned, the result type of a race is the most
specific common supertype of all the subexpressions. This ensures type
safety while maintaining flexibility in what kinds of operations you
can race against each other:

<!--versetest
base_class := class:
    Value:int

derived_a := class(base_class):
    Name:string = "A"

derived_b := class(base_class):
    Name:string = "B"

GetA()<suspends>:derived_a = derived_a{Value := 1}
GetB()<suspends>:derived_b = derived_b{Value := 2}

F()<suspends>:void={
Result:base_class = race:
    GetA()
    GetB()
SameTypeResult:int = race:
    block:
        Sleep(1.0)
        42
    block:
        Sleep(2.0)
        100
}
<#
-->
<!-- 07 -->
```verse
base_class := class:
    Value:int

derived_a := class(base_class):
    Name:string = "A"

derived_b := class(base_class):
    Name:string = "B"

GetA()<suspends>:derived_a = derived_a{Value := 1}
GetB()<suspends>:derived_b = derived_b{Value := 2}

# Result type is base_class (common supertype)
Result:base_class = race:
    GetA()  # Returns derived_a
    GetB()  # Returns derived_b
# Result is base_class, can hold either derived type

# If all expressions return the same type, that is the result type
SameTypeResult:int = race:
    block:
        Sleep(1.0)
        42
    block:
        Sleep(2.0)
        100
# Result type is int
```
<!-- #> -->

A pattern involves adding identifiers to determine which subexpression won:

<!--versetest
SlowOperation()<suspends>:int=0
FastOperation()  <suspends> :int=0
InfiniteOperation()  <suspends> :int=0
F()<suspends>:void={
WinnerID := race:
    block:
        SlowOperation()
        1
    block:
        FastOperation()
        2
    block:
        loop:
            InfiniteOperation()
        3

case(WinnerID):
    1 => Print("Slow operation won somehow!")
    2 => Print("Fast operation won as expected")
    _ => Print("Impossible!")
}
<#
-->
<!-- 08 -->
```verse
# Adding identifiers to determine which expression won
WinnerID := race:
    block:
        SlowOperation()
        1  # Return 1 if this wins
    block:
        FastOperation()
        2  # Return 2 if this wins
    block:
        loop:
            InfiniteOperation()
        3  # Never returns

case(WinnerID):
    1 => Print("Slow operation won somehow!")
    2 => Print("Fast operation won as expected")
    _ => Print("Impossible!")
```
<!-- #> -->

##### The rush Expression

The `rush` expression occupies a unique middle ground between `sync`
and `race`. Like race, it completes as soon as the first subexpression
finishes. Unlike race, it does not cancel the losers. This creates an
interesting pattern where you can start multiple operations, proceed
as soon as one provides a result, while allowing the others to
continue their work in the background.

<!--versetest
LongBackgroundTask()<suspends>:int=0
QuickCheck() <suspends>  :int=0
MediumTask() <suspends>  :int=0
F()<suspends>:void={
FirstResult := rush:
    LongBackgroundTask()
    QuickCheck()
    MediumTask()

Print("First result: {FirstResult}")
}
<#
-->
<!-- 09 -->
```verse
# First to complete allows continuation, others keep running
FirstResult := rush:
    LongBackgroundTask()   # Continues after rush completes
    QuickCheck()          # Finishes first
    MediumTask()          # Also continues after rush

Print("First result: {FirstResult}")
# LongBackgroundTask and MediumTask are still running!
```
<!-- #> -->

Rush shines in scenarios where you want to be responsive while still
completing all operations eventually. Consider preloading game assets:
you might start loading multiple levels simultaneously, begin gameplay
as soon as the current level loads, while continuing to cache the
other levels in the background. Or think about achievement checking,
where you want to notify the player as soon as one achievement unlocks
while continuing to check for others.

The non-canceling nature of rush requires careful consideration. Those
background tasks continue consuming resources and performing their
operations even after rush completes. They'll keep running until they
naturally complete or until their enclosing async context ends. This
makes rush powerful but also potentially dangerous if misused with
operations that might never complete or that consume significant
resources.

There's an important technical restriction to be aware of: rush cannot
be used directly in the body of iteration expressions like `loop` or
`for`. The interaction between rush's background tasks and iteration
could lead to resource accumulation. If you need rush-like behavior in
a loop, wrap it in an async function and call that function from your
iteration.

##### Returning from Concurrent Arms

A `return` statement inside a `sync`, `race`, or `rush` arm causes
the enclosing *function* to return, not just the arm. The structured
concurrency expression is abandoned, defers in arms that have already
started execute, and arms that have not yet started are simply
skipped.

<!--versetest
CoroUtils := module:
    LogEvent(Msg:string):void = {}
    GetEventLogString()<computes>:string = ""
    WaitTicks(N:int)<suspends>:void = {}
    Tick(N:int):void = {}
-->
<!-- 09b -->
```verse
Log(Msg:string):void = CoroUtils.LogEvent(Msg)

MaybeReturn(Delay:int, Value:?string)<suspends>:string =
    defer { Log("a") }
    CoroUtils.WaitTicks(Delay)
    if (V := Value?):
        return V         # Returns from MaybeReturn
    Log("done")
    "no-return"

Wrapper(Value:?string)<suspends>:string =
    defer { Log("z") }
    R := sync:
        block:
            MaybeReturn(0, Value)   # Arm 1
        block:
            defer { Log("b") }
            CoroUtils.WaitTicks(1)
            Log("2")
            2
    "{R(0)}"
```

When `Value` is set, arm 1 executes `return V` inside
`MaybeReturn`. This exits `Wrapper` entirely — the `sync` is
abandoned, arm 2 never completes, and defers run during unwinding.
When `Value` is not set, arm 1 completes normally and `sync` waits
for both arms to finish.

##### The branch Expression

The `branch` expression represents fire-and-forget concurrency within
a structured context. When you encounter a branch, it immediately
starts executing its body as a background task and then, without any
pause or hesitation, continues with the next expression. There's no
waiting, no result collection, just a task spinning off to do its work
while the main flow proceeds unimpeded.

<!--versetest
AsyncOperation1()<computes><suspends>:int=0
ImmediateOperation()<computes> :int=0
AsyncOperation2() <suspends><computes>  :int=0
F()<suspends>:void={
branch:
    AsyncOperation1()
    ImmediateOperation()
    AsyncOperation2()
}
<#
-->
<!-- 10 -->
```verse
branch:
    # This block runs independently
    AsyncOperation1()
    ImmediateOperation()
    AsyncOperation2()

# Execution continues immediately here
Print("Branch started, continuing main flow")
# Branch block is still running in background
```
<!-- #> -->


Branch excels at handling side effects that shouldn't interrupt the
main game flow but that are acceptable to lose if the enclosing scope
ends. Think about triggering particle effects that play out over time,
starting background music that fades in gradually, or pre-loading
assets that might be needed soon. These operations need to happen, but
there's no reason to make the player wait for them to complete. Branch
lets you express this "start it and move on" pattern directly.

The critical semantic of branch is its **cancellation behavior**: a
branch task is automatically canceled when execution leaves the
enclosing function scope, whether that happens through normal
completion, failure, or cancellation from above. This is the
structured concurrency guarantee at work—branches cannot outlive their
parent context, which prevents orphaned tasks from accumulating. But
it also means branch is the wrong choice for work that *must*
complete, like logging analytics events or saving player progress. For
those tasks, use `spawn` instead, which runs independently of its
creating scope.

Like rush, branch faces restrictions with iteration expressions. You
cannot use branch directly inside a loop or for body, as this could
lead to an unbounded number of background tasks. The workaround
remains the same: encapsulate the branch in an async function and call
that function from your iteration.

#### Unstructured Concurrency

##### The spawn Expression

While structured concurrency handles most concurrent programming needs
elegantly, sometimes you need to break free from the hierarchical task
structure. The `spawn` expression is Verse's single concession to
unstructured concurrency, allowing you to start an async operation
that lives independently of its creating scope. Think of spawn as an
emergency escape hatch—powerful when needed, but not your first choice
for typical concurrent patterns.

<!--versetest
LongRunningTask()  <suspends> :int=0
-->
<!-- 11 -->
```verse
# spawn returns a task(t) object you can control
BackgroundTask:task(int) = spawn{LongRunningTask()}

# Or fire-and-forget without capturing the task
spawn{LongRunningTask()}
Print("Spawned task continues even after this scope exits")
```

What makes spawn unique is its ability to work anywhere. Unlike all
the structured concurrency expressions that require an async context,
spawn works in immediate functions, class constructors, module
initialization—anywhere you can write code. This universality comes
with responsibility. The task you spawn becomes a free agent,
continuing its work regardless of what happens to the code that
created it. There's no automatic cleanup, no parent-child
relationship, just an independent task pursuing its goal.

The spawned function must have the `<suspends>` effect. You **cannot**
spawn functions with the `<decides>` effect:

<!--versetest-->
<!-- 12 -->
```verse
AsyncWork()<suspends>:void =
    Sleep(1.0)
    Print("Background work complete")

FailableWork()<decides>:void =
    false?  # Might fail

# Valid: spawning suspends function
spawn{AsyncWork()}

# Invalid: cannot spawn decides function
# spawn{FailableWork()}  # ERROR: spawn requires suspends, not decides
```

This restriction exists because spawned tasks run independently
without a parent to handle their failure. Since `<suspends>` and
`<decides>` cannot be combined on the same function, and spawn needs
`<suspends>`, functions with `<decides>` cannot be spawned. If you
need to spawn failable work, wrap it in a suspends function that
handles the failure internally:

<!--versetest
FailableWork<public>()<computes><decides>:void = {}
-->
<!-- 13 -->
```verse
SafeFailableWork()<suspends>:void =
    if (FailableWork[]):
        Print("Work succeeded")
    else:
        Print("Work failed, but handled gracefully")

spawn{SafeFailableWork()}  # Valid - failure handled inside
```

Spawn finds its place in specific architectural patterns. Global
background services that monitor game state throughout the entire
session, cleanup tasks that must complete even if the triggering
context ends, or integration points where immediate code needs to
trigger async operations—these scenarios justify reaching for spawn
over the structured alternatives.

The contrast with branch illuminates the design philosophy. Branch
gives you structured fire-and-forget concurrency, but its tasks are
canceled when the enclosing scope exits. Spawn gives you tasks that
outlive their creating scope—use it when the work *must* complete
regardless of what happens to the code that started it. Choose branch
when cancellation is acceptable; choose spawn when it is not.

**Working with spawned tasks:**

The `spawn` expression returns a `task(t)` object where `t` is the
return type of the spawned function. This task object provides methods
to control and query the spawned operation—you can cancel it, wait for
it to complete, or check its current state. While spawn creates
independent tasks that do not require management, having access to the
task object gives you the power to intervene when needed. See the "The
task(t) Type" section below for complete details on task objects and
their capabilities.

#### The task(t) Type

The `task(t)` type represents a handle to an executing async
operation, where `t` is the return type of the operation. While Verse
creates tasks automatically behind the scenes for all async
expressions, only `spawn` gives you direct access to a task object
that you can control and query.

<!--versetest-->
<!-- 14 -->
```verse
# spawn returns task(t) where t is the return type
BackgroundWork()<suspends>:int =
    Sleep(2.0)
    42

MyTask:task(int) = spawn{BackgroundWork()}
# MyTask is a handle to the spawned operation
```

Task objects provide a rich interface for managing async operations:
you can cancel them, wait for their completion, and query their
current state. This control is essential for implementing robust
concurrent systems where you need to coordinate multiple independent
operations.


A task moves through several distinct states during its lifetime:

**Active**: The task is currently running or suspended, but has not
yet finished. It's still doing work or waiting to resume.

**Completed**: The task finished successfully and returned a
result. Once completed, a task never changes state again. (Terminal state)

**Canceled**: The task was canceled before it could complete. This is
a terminal state — canceled tasks cannot resume.

**Settled**: A task is settled if it has reached either the Completed
or Canceled state. Settled tasks are no longer executing. (Terminal state)

**Uninterrupted**: A task is uninterrupted if it completed
successfully without being canceled. This is equivalent to the
Completed state. (alias)

**Interrupted**: A task is interrupted if it was canceled. This is
equivalent to the Canceled state. (alias)

##### Task.Cancel()

!!! note "Unreleased Feature"
    The Cancel() method has not been released at this time.
	
The `Cancel()` method requests cancellation of a task. This is a safe
operation that can be called on any task in any state:

<!--versetest
BackgroundWork()<transacts><suspends>:void={Sleep(1.0)}
F()<suspends>:void= {
LongTask:task(void) = spawn{BackgroundWork()}
LongTask.Cancel()
LongTask.Cancel()
}
<#
-->
<!-- 16 -->
```verse
LongTask:task(void) = spawn{BackgroundWork()}

# Request cancellation
LongTask.Cancel()

# Safe to call multiple times
LongTask.Cancel()  # No error

# Safe to call on completed tasks (has no effect)
```
<!-- #> -->

Cancellation is cooperative—the task does not stop
immediately. Instead, it receives a cancellation signal that is
checked at the next suspension point. The task then unwinds
gracefully, allowing cleanup code to run. See "Suspension Points and
Cancellation" below for details on when cancellation takes effect.

Calling `Cancel()` on an already completed task is safe and has no
effect. This means you can cancel tasks without worrying about race
conditions between completion and cancellation.

##### Task.Await()

The `Await()` method suspends the calling context until the task
completes, then returns the task's result:

<!--versetest
BackgroundWork()<computes><suspends>:int=42
F()<suspends>:void={
ComputeTask:task(int) = spawn{BackgroundWork()}
Result:int = ComputeTask.Await()
Print("Task returned: {Result}")
}
<#
-->
<!-- 17 -->
```verse
ComputeTask:task(int) = spawn{BackgroundWork()}

# Wait for task to complete and get result
Result:int = ComputeTask.Await()
Print("Task returned: {Result}")
```
<!-- #> -->

**Key behaviors of Await():**

- **Blocks until completion**: If the task is still running, `Await()`
  suspends until it finishes
- **Returns immediately if complete**: If the task already finished,
  `Await()` returns the cached result instantly (Sticky)
- **Can be called multiple times**: You can await the same task
  repeatedly, always getting the same result
- **Propagates cancellation**: If the awaited task was canceled,
  `Await()` propagates the cancellation to the caller

<!--versetest
ComputeValue<public>()<suspends>:int = 42
F()<suspends>:void={
MyTask:task(int) = spawn{ComputeValue()}
FirstResult := MyTask.Await()
SecondResult := MyTask.Await()
}
<#
-->
<!-- 18 -->
```verse
MyTask:task(int) = spawn{ComputeValue()}

# First await - waits for completion
FirstResult := MyTask.Await()

# Second await - returns cached result immediately
SecondResult := MyTask.Await()

# FirstResult = SecondResult
```
<!-- #> -->


##### Common Task Patterns

**Canceling a task after timeout:**

<!--versetest
ProcessData()<suspends>:void={}
-->
<!-- 27 -->
```verse
StartTask()<suspends>:void =
    DataTask:task(void) = spawn{ProcessData()}

    race:
        block:
            DataTask.Await()
            Print("Task completed")
        block:
            Sleep(5.0)
            DataTask.Cancel()
            Print("Task timed out and was canceled")
```

**Waiting for multiple spawned tasks:**

<!--versetest
Task1()<suspends>:int=1
Task2()<suspends>:int=2
Task3()<suspends>:int=3
-->
<!-- 28 -->
```verse
RunMultipleTasks()<suspends>:void =
    T1 := spawn{Task1()}
    T2 := spawn{Task2()}
    T3 := spawn{Task3()}

    # Wait for all to complete
    Results := sync:
        T1.Await()
        T2.Await()
        T3.Await()

    Print("All tasks complete: {Results(0)}, {Results(1)}, {Results(2)}")
```


##### Suspension Points and Cancellation

Task cancellation in Verse follows a cooperative model. Rather than
forcefully terminating tasks, which could leave resources in
inconsistent states, Verse sends cancellation signals that tasks check
at **suspension points**. When a task receives a cancellation signal,
it has the opportunity to clean up resources before terminating. This
cooperative approach prevents data corruption while ensuring
responsive cancellation.

Suspension points are the specific locations where async tasks can
pause and resume. These are the only places where:

- A task can be suspended to allow other tasks to run
- Cancellation signals are checked and processed
- The runtime can switch between concurrent tasks

Common suspension points include:

**Timing operations:**

<!--versetest
F()<suspends>:void=
    Sleep(1.0)
    NextTick() 
<#
-->
<!-- 30 -->
```verse
Sleep(1.0)  # Suspends for duration, checks cancellation when resuming
NextTick()  # Waits one simulation update, checks cancellation
```
<!-- #> -->

**Calling suspends functions:**

<!--versetest
SomeAsyncFunction<public>()<suspends>:void = {}
F()<suspends>:void={
Result := SomeAsyncFunction()
}
<#
-->
<!-- 32 -->
```verse
Result := SomeAsyncFunction()  # Suspension point at the call
```
<!-- #> -->

**Structured concurrency expressions:**

<!--versetest
Op1()<suspends>:void = {}
Op2()<suspends>:void = {}
M()<suspends>:void =
    sync:
        Op1()
        Op2()
<#
-->
<!-- 33 -->
```verse
sync:  # Suspension point when entering sync
    Op1()
    Op2()
# Suspension point when sync completes
```
<!-- #> -->

**Task operations:**

<!--versetest
ComputeValue<public>()<suspends>:int = 42
F()<suspends>:void={
MyTask:task(int) = spawn{ComputeValue()}
Result := MyTask.Await()
}
<#
-->
<!-- 34 -->
```verse
Result := MyTask.Await()  # Suspension point while waiting
```
<!-- #> -->

**Important:** Immediate code between suspension points runs without
interruption. If you write a long computation loop without any
suspension points, that task cannot be canceled until it reaches the
next suspension point:

<!--versetest
ComputeExpensiveOperation(:int):void={}
-->
<!-- 35  -->
```verse
# Cannot be canceled during the loop
LongComputation()<suspends>:void =
    for (I := 0..1000000):
        # No suspension points - runs to completion
        ComputeExpensiveOperation(I)
    Sleep(0.0)  # First cancellation check happens here!

# Can be canceled every iteration
ResponsiveComputation()<suspends>:void =
    for (I := 0..1000000):
        ComputeExpensiveOperation(I)
        Sleep(0.0)  # Cancellation checked every iteration
```

If you need to make long-running computations cancellable, insert
periodic suspension points using `Sleep(0.0)` or `NextTick()`, which
yield control without actual delay but allow cancellation checking.

Cancellation cascades through the task hierarchy. When a parent task
is canceled, all its child tasks receive cancellation signals
too. This cascading behavior maintains the invariant that child tasks
do not outlive their parents in structured concurrency, preventing
resource leaks and ensuring predictable cleanup. In a race expression,
for example, when the winner completes, the race task sends
cancellation signals to all losing subtasks, which then cascade to any
tasks those losers might have created.

#### Cleanup and Resource Management

##### The defer: Block

The `defer:` block provides guaranteed cleanup code that executes when
its enclosing scope exits — whether through normal completion, failure,
or cancellation. For the full description of `defer` semantics,
including execution order, scope rules, and restrictions, see
[Defer Statements](12_BOOK_OF_VERSE_I_FOUNDATIONS.md#defer-statements).

This section focuses on how `defer` interacts with concurrency.

**defer: with cancellation:**

When a concurrent task is canceled (e.g., a losing `race` arm or a
cancelled `spawn`), defer blocks execute as the stack unwinds from the
cancellation point. This makes `defer` essential for resource cleanup
in concurrent code:

<!--versetest
AcquireResource():int=42
ReleaseResource(:int):void={}
LongRunningTask(:int)<suspends>:void={loop{NextTick()}}
-->
<!-- 36 -->
```verse
ProcessWithTimeout()<suspends>:void =
    race:
        block:
            Resource := AcquireResource()
            defer:
                ReleaseResource(Resource)  # Runs when this arm is cancelled
            LongRunningTask(Resource)
        block:
            Sleep(10.0)  # Timeout
    # If timeout wins, first block is cancelled and defer runs
```

<!--versetest
Setup():void={}
Teardown():void={}
LongOperation()<suspends>:void={loop{NextTick()}}
-->
<!-- 42 -->
```verse
CancellableWork()<suspends>:void =
    Setup()

    defer:
        Teardown()
        Print("Cleanup after cancellation")

    # If this task is canceled, defer runs during unwinding
    LongOperation()
```

**No suspending in defer:**

defer blocks **cannot** contain suspending operations. This ensures
cleanup happens immediately without delay:

<!--versetest
ValidDefer()<suspends>:void =
    defer:
        Print("Cleanup happens immediately")
    Sleep(1.0)
<#
-->
<!-- 44 -->
```verse
# ERROR: Cannot use suspending operations in defer
BadDefer()<suspends>:void =
    defer:
        Sleep(1.0)  # ERROR: defer blocks cannot suspend
        NextTick()  # ERROR: defer blocks cannot suspend
```
<!-- #> -->

This restriction is essential — if defer blocks could suspend, cleanup
could be delayed indefinitely, defeating their purpose as guaranteed
finalization. However, defer blocks *can* use `spawn` for
fire-and-forget async operations.

#### Timing Functions

The fundamental timing function that suspends execution for a specified duration:

<!--versetest
M()<suspends>:void =
    Sleep(1.0)

    Sleep(0.0)
<#
-->
<!-- 46 -->
```verse
# Suspend for 1 second
Sleep(1.0)

# Suspend for one frame (smallest possible delay)
Sleep(0.0)
```
<!-- #> -->

The `Sleep(0.0)` pattern deserves special attention. While it does not
add actual delay, it serves two critical purposes:

1. **Creates a suspension point** for cancellation checking
2. **Yields control** to other concurrent tasks, preventing one task from monopolizing execution

This makes `Sleep(0.0)` essential for responsive concurrent code:

<!--versetest
ProcessFrame():void={}
ExpensiveOperation(:int):void={}
-->
<!-- 47 -->
```verse
# Without Sleep(0.0) - cannot be cancelled during loop
UnresponsiveLoop()<suspends>:void =
    for (I := 0..10000):
        ExpensiveOperation(I)
    # Cancellation only checked after all iterations

# With Sleep(0.0) - responsive to cancellation
ResponsiveLoop()<suspends>:void =
    for (I := 0..10000):
        ExpensiveOperation(I)
        Sleep(0.0)  # Yields and checks cancellation each iteration
```

**Best practice:** Insert `Sleep(0.0)` in long-running loops to ensure
tasks remain responsive to cancellation and share execution time
fairly with other concurrent operations.

##### NextTick()

!!! note "Unreleased Feature"
    NextTick() have not yet been released. 

The `NextTick()` function suspends execution until the next simulation
update (tick). Unlike `Sleep(0.0)` which yields control and may resume
in the same tick if no other work is pending, `NextTick()` guarantees
that at least one simulation update will occur before resuming:

<!--versetest
M()<suspends>:void =
    NextTick()

    NextTick()
    NextTick()
    NextTick()
<#
-->
<!-- 48 -->
```verse
# Wait for exactly one simulation tick
NextTick()

# Multiple ticks
NextTick()  # Wait 1 tick
NextTick()  # Wait another tick
NextTick()  # Wait a third tick
```
<!-- #> -->

`NextTick()` is essential for game logic that needs to be synchronized with simulation updates:

<!--versetest
ProcessGameLogic():void={}
UpdatePhysics():void={}
CheckCollisions():void={}
PerformAction():void={}

GameLoop()<suspends>:void =
    loop:
        ProcessGameLogic()
        UpdatePhysics()
        CheckCollisions()
        NextTick()

DelayByTicks(TickCount:int)<suspends>:void =
    for (I := 1..TickCount):
        NextTick()

### Test the delay function
TestDelay()<suspends>:void =
    DelayByTicks(5)
    PerformAction()
<#
-->
<!-- 49 -->
```verse
# Process game logic every tick
GameLoop()<suspends>:void =
    loop:
        ProcessGameLogic()
        UpdatePhysics()
        CheckCollisions()
        NextTick()  # Wait for next simulation update

# Delay action by specific number of ticks
DelayByTicks(TickCount:int)<suspends>:void =
    for (I := 1..TickCount):
        NextTick()

# Wait 5 ticks before executing action
DelayByTicks(5)
PerformAction()
```
<!-- #> -->

**Sleep(0.0) vs NextTick():**

| Feature   | Sleep(0.0)              | NextTick() |
|---------  |------------             |------------|
| Timing    | May resume in same tick | Always waits for next tick |
| Use case  | Yield for cancellation checks | Synchronize with simulation updates |
| Guarantee | Creates suspension point | Guarantees tick boundary |

Both create suspension points for cancellation, but `NextTick()`
provides stronger timing guarantees when you need to align with the
simulation clock.

<!--versetest
ProcessFrame()<computes>:logic=false
-->
<!-- 50 -->
```verse
# Common patterns
LoopWithDelay()<suspends>:void =
    loop:
        ProcessFrame()
        Sleep(0.033)  # ~30 FPS

TickBasedLoop()<suspends>:void =
    loop:
        if (ProcessFrame()=false): 
             break
        NextTick()  # Once per simulation tick	
```

Timing Patterns are:

<!--versetest
DoAction():void={}
UpdateLogic()<computes>:void={}
Float(:int)<computes>:float=0.0
SetPosition(:float):void={}
-->
<!-- 51 -->
```verse
# Delayed action
PerformDelayedAction()<suspends>:void =
    Sleep(2.0)  # Wait 2 seconds
    DoAction()

# Periodic execution
PeriodicUpdate()<suspends>:void =
    loop:
        UpdateLogic()
        Sleep(1.0)  # Update every second

# Animation timing
AnimateMovement(Start:float,End:float)<suspends>:void =
    for (T := 0..10):
        SetPosition(Lerp(Start, End, Float(T)/10.0))
        Sleep(0.0)  # One frame
```

##### Getting Current Time: GetSecondsSinceEpoch

The `GetSecondsSinceEpoch()` function returns the current Unix
timestamp—the number of seconds elapsed since January 1, 1970,
00:00:00 UTC. This function is essential for timestamping events,
measuring durations, and synchronizing with external systems that use
Unix time.

<!--versetest
LogEvent(Message:string):void =
    Timestamp := GetSecondsSinceEpoch()
    Print("[{Timestamp}] {Message}")
<#
-->
<!-- 52 -->
```verse
# Get current timestamp
CurrentTime := GetSecondsSinceEpoch()
# Returns something like 1716411409.0 (May 22, 2024)

# Log an event with timestamp
LogEvent(Message:string):void =
    Timestamp := GetSecondsSinceEpoch()
    Print("[{Timestamp}] {Message}")
```
<!-- #> -->

**Critical transactional behavior:**

Within a single transaction, `GetSecondsSinceEpoch()` returns the
**same value** every time it is called. This ensures deterministic
behavior and prevents time-related race conditions:

<!--versetest
DoExpensiveWork()<transacts>:void = {}
PerformDatabaseUpdates()<transacts>:void = {}

MeasureTransactionTime()<transacts>:void =
    StartTime := GetSecondsSinceEpoch()

    DoExpensiveWork()
    PerformDatabaseUpdates()

    EndTime := GetSecondsSinceEpoch()

    Duration := EndTime - StartTime
<#
-->
<!-- 53 -->
```verse
MeasureTransactionTime()<transacts>:void =
    StartTime := GetSecondsSinceEpoch()

    # Perform complex operations
    DoExpensiveWork()
    PerformDatabaseUpdates()

    EndTime := GetSecondsSinceEpoch()

    # StartTime = EndTime!
    # Time is "frozen" within the transaction
    Duration := EndTime - StartTime  # Always 0.0
```
<!-- #> -->

This transactional consistency is intentional—it prevents
non-deterministic behavior where transaction retry could produce
different results due to time progression. If the transaction fails
and is retried, all calls to `GetSecondsSinceEpoch()` in the retried
attempt will return a new consistent timestamp.

**Use cases:**

**Event logging and debugging:**

<!--versetest
logger := class:
    var EventLog:[]tuple(float, string) = array{}

    Log(Message:string)<transacts>:void =
        Timestamp := GetSecondsSinceEpoch()
        set EventLog = EventLog + array{(Timestamp, Message)}

    GetRecentEvents(LastSeconds:float)<transacts>:[]string =
        Now := GetSecondsSinceEpoch()
        Cutoff := Now - LastSeconds
        for (Entry : EventLog, Entry(0) >= Cutoff):
            Entry(1)
<#
-->
<!-- 55 -->
```verse
logger := class:
    var EventLog:[]tuple(float, string) = array{}

    Log(Message:string)<transacts>:void =
        Timestamp := GetSecondsSinceEpoch()
        set EventLog = EventLog + array{(Timestamp, Message)}

    GetRecentEvents(LastSeconds:float)<transacts>:[]string =
        Now := GetSecondsSinceEpoch()
        Cutoff := Now - LastSeconds
        for ((Time, Message) : EventLog, Time >= Cutoff):
            Message
```
<!-- #> -->

**Session tracking:**
<!--versetest-->
<!-- 56 -->
```verse
player_session := class:
    LoginTime:float

MakeSession()<transacts>:player_session =
    player_session{LoginTime := GetSecondsSinceEpoch()}

GetSessionDuration(S:player_session)<transacts>:float =
    GetSecondsSinceEpoch() - S.LoginTime
```

**Rate limiting:**

<!--versetest
PerformAction():void={}
ShowCooldownMessage():void={}
rate_limiter := class:
    var LastAction:float = 0.0
    Cooldown:float = 5.0

    CanAct()<transacts><decides>:void =
        Now := GetSecondsSinceEpoch()
        TimeSinceLastAction := Now - LastAction
        TimeSinceLastAction >= Cooldown
        set LastAction = Now

assert:
   Limiter := rate_limiter{}
   if (Limiter.CanAct[]):
       PerformAction()
   else:
       ShowCooldownMessage()
<#
-->
<!-- 57 -->
```verse
rate_limiter := class:
    var LastAction:float = 0.0
    Cooldown:float = 5.0  # 5 second cooldown

    CanAct()<transacts><decides>:void =
        Now := GetSecondsSinceEpoch()
        TimeSinceLastAction := Now - LastAction
        TimeSinceLastAction >= Cooldown
        set LastAction = Now

Limiter := rate_limiter{}

if (Limiter.CanAct[]):
    PerformAction()
else:
    ShowCooldownMessage()
```
<!-- #> -->

**Absolute timestamps for external systems:**

When interfacing with external systems, databases, or APIs that use Unix timestamps:

<!--versetest
MyPlayerID:string = "player123"
SendToAnalytics<public>(EventType:string, Timestamp:float, PlayerID:string):void = {}
FetchServerTime():float = 1716411409.0

M():void =
    SendToAnalytics("player_action", GetSecondsSinceEpoch(), MyPlayerID)

    ServerTime := FetchServerTime()
    LocalTime := GetSecondsSinceEpoch()
    ClockSkew := LocalTime - ServerTime
<#
-->
<!-- 58 -->
```verse
# Timestamp for external analytics
AnalyticsEvent := map{
    "event_type" => "player_action",
    "timestamp" => GetSecondsSinceEpoch(),
    "player_id" => MyPlayerID
}
SendToAnalytics(AnalyticsEvent)

# Comparing with server timestamps
ServerTime := FetchServerTime()
LocalTime := GetSecondsSinceEpoch()
ClockSkew := LocalTime - ServerTime
```
<!-- #> -->

**Important notes:**

- Returns `float` representing seconds (may have fractional parts for millisecond precision)
- Located in `/Verse.org/Verse` module—use `using { /Verse.org/Verse }` to access
- Not affected by `Sleep()` or other suspension—measures real-world time
- Consistent within transactions for determinism
- Each new transaction gets a fresh timestamp

**Combining with Sleep for time-based logic:**

<!--versetest
PerformAction<public>()<suspends>:void = {}
-->
<!-- 59 -->
```verse
# Wait until a specific time
WaitUntil(TargetTime:float)<suspends>:void =
    loop:
        if (GetSecondsSinceEpoch() >= TargetTime) then:
            break
        Sleep(0.1)  # Check every 100ms

# Schedule an action for the future
ScheduleDelayedAction(DelaySeconds:float)<suspends>:void =
    TargetTime := GetSecondsSinceEpoch() + DelaySeconds
    WaitUntil(TargetTime)
    PerformAction()
```

Note that the transactional consistency means you cannot use
`GetSecondsSinceEpoch()` to measure time within a single
transaction. For measuring execution time of operations that do not
span transactions, use profiling tools or external timing mechanisms.

#### Events and Synchronization

Events provide synchronization primitives for coordinating between
concurrent tasks. They implement producer-consumer and observer
patterns, allowing tasks to signal each other and wait for specific
conditions. Events bridge the gap between independent concurrent
operations, enabling communication without shared mutable state.

##### Basic Events

The `event(t)` type creates a communication channel where producers
signal values and consumers await them. Each signal delivers one value
to each awaiting task:

<!--versetest
ProcessValue(:int):void={}
F()<suspends>:void={
GameEvent := event(int){}

ProducerTask()<suspends>:void =
    Sleep(1.0)
    GameEvent.Signal(42)

ConsumerTask()<suspends>:void =
    Value := GameEvent.Await()
    ProcessValue(Value)

sync:
    ProducerTask()
    ConsumerTask()
}
<#
-->
<!-- 60 -->
```verse
# Create an event channel for integers
GameEvent := event(int){}

# Producer: signals values to the event
ProducerTask()<suspends>:void =
    Sleep(1.0)
    GameEvent.Signal(42)

# Consumer: awaits values from the event
ConsumerTask()<suspends>:void =
    Value := GameEvent.Await()
    ProcessValue(Value)

sync:
    ProducerTask()
    ConsumerTask()
```
<!-- #> -->

When `Await()` is called on an event, the calling task suspends until
another task calls `Signal()` with a value. The signaled value is
delivered to one waiting task, and execution resumes. If multiple
tasks await the same event, each `Signal()` wakes exactly one
awaiter—signals and awaits pair up one-to-one.

This one-to-one matching makes events perfect for task
coordination. Think of a player action system: the input handler
signals button presses while the gameplay system awaits them. Or
consider an AI pathfinding request: the game logic signals destination
requests while the pathfinding system awaits and processes them.

Events work naturally with structured concurrency. You can use them
within `sync` blocks to coordinate parallel operations, or combine
them with `race` to implement timeouts on event waiting:

<!--versetest
F()<suspends>:void={
GameEvent:event(int)=event(int){}
Result := race:
    block:
        Value := GameEvent.Await()
        option{Value}
    block:
        Sleep(5.0)
        false
}
<#
-->
<!-- 61 -->
```verse
# Wait for event with timeout
Result := race:
    block:
        Value := GameEvent.Await()
        option{Value}
    block:
        Sleep(5.0)
        false  # Timeout - no value received
```
<!-- #> -->

##### Sticky Events

!!! note "Unreleased Feature"
    Sticky Events have not yet been released and is not currently available.

While basic events deliver each signal to exactly one awaiter,
`sticky_event(t)` remembers the last signaled value and delivers it to
all subsequent awaits until a new value is signaled:

<!--NoCompile-->
<!-- 62 -->
```verse
StateEvent := sticky_event(int){}

# Signal once
StateEvent.Signal(100)

# Multiple awaits all receive the same value
Value1 := StateEvent.Await()  # Gets 100
Value2 := StateEvent.Await()  # Gets 100 again
Value3 := StateEvent.Await()  # Still gets 100

# New signal updates the sticky value
StateEvent.Signal(200)
Value4 := StateEvent.Await()  # Gets 200
Value5 := StateEvent.Await()  # Also gets 200
```

Sticky events excel at representing state changes that multiple
consumers need to observe. Unlike basic events where each signal
disappears after one await, sticky events maintain the current
state. Consider a game phase system: when the phase changes from
"Lobby" to "Playing", every system that checks the phase should see
"Playing", not have one system consume the signal while others miss
it.

The sticky behavior creates a form of eventually consistent state. If
a task awaits a sticky event, it is guaranteed to see the most recent
signal, even if that signal occurred before the await. This makes
sticky events ideal for configuration updates, mode switches, or any
scenario where "what's the current state?" matters more than "what
just changed?".

##### Subscribable Events

!!! note "Unreleased Feature"
    Subscribable Events have not yet been released and is not currently available.

The `subscribable_event` type implements the observer pattern,
allowing multiple handlers to react to each signal. Unlike events
where awaiting tasks explicitly wait, subscribable events let you
register callback functions that execute automatically when values are
signaled:

<!--NoCompile-->
<!-- 63 -->
```verse
LogScore(:int):void={}
UpdateUI(:int):void={}
CheckAchievements(:int):void={}

ScoreEvent := subscribable_event(int){}

# Subscribe multiple handlers
Logger := ScoreEvent.Subscribe(LogScore)
UIUpdater := ScoreEvent.Subscribe(UpdateUI)
AchievementChecker := ScoreEvent.Subscribe(CheckAchievements)

# Signal invokes all subscribed handlers
ScoreEvent.Signal(1000)  # Calls LogScore(1000), UpdateUI(1000), CheckAchievements(1000)

# Unsubscribe to stop receiving signals
Logger.Cancel()
ScoreEvent.Signal(2000)  # Only calls UpdateUI and CheckAchievements
```

Each subscription returns a `cancelable` object that lets you
unsubscribe by calling `Cancel()`. Once canceled, that handler stops
receiving signals. This provides fine-grained control over handler
lifetimes, essential for systems that come and go during gameplay.

Subscribable events shine in broadcast scenarios where multiple
independent systems need to react to the same occurrence. When a
player scores points, the UI needs to update, the audio system needs
to play a sound, the achievement system needs to check for unlocks,
and the analytics system needs to log the event. With subscribable
events, each system registers its handler independently, and every
signal reaches all interested parties.

##### The awaitable and signalable Interfaces

Events are built on two fundamental interfaces that you can use to create custom synchronization types:

<!--NoCompile-->
<!-- 64 -->
```verse
awaitable(t:type) := interface:
    Await()<suspends>:t

signalable(t:type) := interface:
    Signal(Value:t):void
```

The `awaitable` interface represents anything that can be waited on,
while `signalable` represents anything that can send signals. By
separating these capabilities, Verse enables precise control over who
can produce values versus who can consume them.

You can pass `awaitable` parameters to functions that should only read
from an event, preventing accidental signals:

<!--versetest
ProcessValue(:int):void={}
-->
<!-- 65 -->
```verse
# This function can only await, not signal
ConsumerFunction(Source:awaitable(int))<suspends>:void =
    Value := Source.Await()
    ProcessValue(Value)
    # Source.Signal(123)  # ERROR: awaitable does not have Signal

# This function can only signal, not await
ProducerFunction(Target:signalable(int)):void =
    Target.Signal(42)
    # Value := Target.Await()  # ERROR: signalable does not have Await
```

This separation creates clear interfaces for producer-consumer
relationships. A queue implementation might expose an `awaitable`
interface to consumers for reading and a `signalable` interface to
producers for writing, ensuring neither side can accidentally use the
wrong operation.

##### Transactional Behavior

Event subscriptions participate in Verse's transactional system. If a
transaction containing a `Subscribe()` call fails and rolls back, the
subscription never takes effect:

<!--NoCompile-->
<!-- 66 -->
```verse
Handler(:int):void={}

MyEvent := subscribable_event(int){}

# Subscription in a failing transaction
if:
    Sub := MyEvent.Subscribe(Handler)
    false?  # Transaction fails and rolls back

# Subscription was rolled back - handler not called
MyEvent.Signal(100)
```

Similarly, `Cancel()` operations are transactional. If you cancel a subscription within a transaction that later fails, the subscription remains active:

<!--versetest
subscription := class:
    Cancel()<transacts>:void = {}

subscribable_event(t:type) := class:
    Subscribe(Handler:t->void)<transacts>:subscription = subscription{}
    Signal(Value:t)<transacts>:void = {}
-->
<!-- 67 -->
```verse
Handler(:int):void={}

MyEvent := subscribable_event(int){}
Sub := MyEvent.Subscribe(Handler)

# Cancel in a failing transaction
if:
    Sub.Cancel()
    false?  # Transaction fails

# Cancel was rolled back - subscription still active
MyEvent.Signal(100)  # Handler still gets called
```

This transactional integration ensures that event subscriptions
maintain consistency with other transactional operations. If you are
setting up a complex system where subscribing to events is part of a
larger initialization that might fail, the transaction system
guarantees that either all initialization succeeds or none of it does,
preventing partial setups that could cause subtle bugs.

##### Event Patterns and Use Cases

**Request-Response:** Use basic events to implement request-response patterns between systems:

<!--versetest
FindPath(Start:int, Goal:int):void = {}

pathfinding_system := class:
    PathRequest:event(tuple(int, int)) = event(tuple(int, int)){}
    PathResponse:event(int) = event(int){}

    PathfindingService()<suspends>:void =
        loop:
            Request := PathRequest.Await()
            Start := Request(0)
            Goal := Request(1)
            FindPath(Start, Goal)
            PathResponse.Signal(42)

    RequestPath(Start:int, Goal:int)<suspends>:int =
        PathRequest.Signal((Start, Goal))
        PathResponse.Await()
<#
-->
<!-- 68 -->
```verse
PathRequest := event(tuple(int, int)){}  # (start, goal)
PathResponse := event(int){}             # path result

PathfindingService()<suspends>:void =
    loop:
        (Start, Goal) := PathRequest.Await()
        FindPath(Start, Goal)
        PathResponse.Signal(42)

RequestPath(Start:int, Goal:int)<suspends>:int =
    PathRequest.Signal((Start, Goal))
    PathResponse.Await()
```
<!-- #> -->

**State Broadcasting:** Use sticky events for state that multiple systems need to observe:

<!--versetest
game_phase := enum{Menu, Playing, Paused, GameOver}
UIUpdate(P:game_phase)<transacts>:void={}
AIUpdate(P:game_phase)<transacts>:void={}
AudioUpdate(P:game_phase)<transacts>:void={}

sticky_event(t:type) := class:
    var CurrentValue:?t = false
    Signal(Value:t)<transacts>:void = set CurrentValue = option{Value}
    Await()<suspends><transacts>:t =
        loop:
            if (V := CurrentValue?):
                return V
-->
<!-- 69 -->
```verse
PhaseChange := sticky_event(game_phase){}

# Systems await current phase without missing updates
UISystem()<suspends>:void =
    loop:
        Phase := PhaseChange.Await()
        UIUpdate(Phase)

AISystem()<suspends>:void =
    loop:
        Phase := PhaseChange.Await()
        AIUpdate(Phase)

AudioSystem()<suspends>:void =
    loop:
        Phase := PhaseChange.Await()
        AudioUpdate(Phase)
```

**Multi-System Notifications:** Use subscribable events when many
systems need to react to the same events:

<!--versetest
subscription := class:
    Cancel()<transacts>:void = {}

subscribable_event(t:type) := class:
    Subscribe(Handler:t->void)<transacts>:subscription = subscription{}
    Signal(Value:t)<transacts>:void = {}
-->
<!-- 70 -->
```verse
UpdateInventoryUI(:int):void={}
PlayPickupSound(:int):void={}
CheckCollectionAchievement(:int):void={}
LogItemPickup(:int):void={}

ItemPickedUp := subscribable_event(int){}

# Each system subscribes independently
InitializeSystems():void =
    ItemPickedUp.Subscribe(UpdateInventoryUI)
    ItemPickedUp.Subscribe(PlayPickupSound)
    ItemPickedUp.Subscribe(CheckCollectionAchievement)
    ItemPickedUp.Subscribe(LogItemPickup)

# Single signal reaches all systems
OnPlayerPickupItem(ItemID:int):void =
    ItemPickedUp.Signal(ItemID)
```

Events complement structured concurrency by providing communication
channels that outlive individual concurrent operations. While `sync`,
`race`, `rush`, and `branch` organize how tasks execute relative to
each other, events organize how tasks share information and coordinate
their actions.

#### Common Patterns and Best Practices

Implement operations with timeouts using `race`:

<!--versetest
ActualOperation()<suspends>:void={}
-->
<!-- 71 -->
```verse
PerformWithTimeout()<suspends>:logic =
    race:
        block:
            ActualOperation()
            true  # Success
        block:
            Sleep(5.0)  # 5 second timeout
            false  # Timeout
```

Initialize multiple systems concurrently:

<!--versetest
LoadAssets()<suspends>:void={}
ConnectToServer()<suspends>:void={}
InitializeUI()<suspends>:void={}
PrepareAudio()<suspends>:void={}

InitializeGame()<suspends>:void =
    sync:
        LoadAssets()
        ConnectToServer()
        InitializeUI()
        PrepareAudio()
    Print("Game ready!")
<#
-->
<!-- 72 -->
```verse
InitializeGame()<suspends>:void =
    sync:
        LoadAssets()
        ConnectToServer()
        InitializeUI()
        PrepareAudio()
    Print("Game ready!")
```
<!-- #>-->

Start background tasks that do not block gameplay:

<!--versetest
MonitorPlayerStats()<suspends>:void={}
UpdateLeaderboards()<suspends>:void={}
ProcessAchievements()<suspends>:void={}
-->
<!-- 73 -->
```verse
StartBackgroundSystems()<suspends>:void =
    branch:
        MonitorPlayerStats()
    branch:
        UpdateLeaderboards()
    branch:
        ProcessAchievements()
    # Main game continues while background tasks run
```

Spawn entities with delays:

<!--versetest
enemy_class := class {     Spawn()<suspends>:void={} }
-->
<!-- 74 -->
```verse
SpawnWave(Enemies:[]enemy_class)<suspends>:void =
    for (Enemy : Enemies):
        spawn{Enemy.Spawn()}
        Sleep(0.5)  # Half second between spawns
```

#### Limitations and Considerations

##### Iteration Restrictions

The interaction between iteration and certain concurrency expressions
requires careful consideration. Rush and branch cannot be used
directly inside loop or for bodies, a restriction that prevents
unbounded task accumulation. When you write a loop that might execute
hundreds or thousands of times, allowing rush or branch directly would
create that many background tasks, potentially overwhelming the
system.

<!--versetest
Operation1()<suspends>:void = {}
Operation2()<suspends>:void = {}

ProcessWithRush(I:int)<suspends>:void =
    rush:
        Operation1()
        Operation2()

M()<suspends>:void =
    for (I := 0..10):
        ProcessWithRush(I)
<#
-->
<!-- 76 -->
```verse
# Not allowed
for (I := 0..10):
    rush:  # ERROR: Cannot use rush in loop
        Operation1()
        Operation2()

# Workaround - wrap in function
ProcessWithRush(I:int)<suspends>:void =
    rush:
        Operation1()
        Operation2()

for (I := 0..10):
    ProcessWithRush(I)  # OK
```
<!-- #> -->

This restriction forces you to be intentional about creating
background tasks in iterations. By wrapping the concurrent operation
in a function, you acknowledge the task creation and make it explicit
in your code structure. This small friction prevents accidental
resource exhaustion while maintaining the flexibility to use these
patterns when genuinely needed.

##### Abstraction Over Implementation

Verse deliberately abstracts away the underlying threading and
scheduling mechanisms. You will not find thread creation APIs,
thread-local storage, or explicit synchronization primitives like
mutexes or semaphores. This is not a limitation but a design
philosophy. By working with higher-level task abstractions, Verse
eliminates entire categories of bugs—no data races, no deadlocks from
incorrect lock ordering, no forgotten unlock calls.

The concurrency model is cooperative rather than preemptive. Tasks
voluntarily yield control at suspension points rather than being
forcibly interrupted by a scheduler. This cooperative nature makes
reasoning about concurrent code easier since you know exactly where
task switches can occur. It also integrates naturally with game
engines' frame-based execution models, where predictable timing is
crucial.

##### Effect Interactions

The effect system that makes Verse's concurrency safe also introduces
some restrictions. The `decides` effect, which marks functions that
can fail, cannot be combined with the `suspends` effect. This
separation keeps the failure model and the concurrency model
orthogonal, preventing complex interactions that would be difficult to
reason about. Transactional operations and certain device-specific
operations may also have restrictions when used in concurrent
contexts, ensuring that operations that must be atomic remain so.

## Book of Verse Source Unit: 15_live_variables.md

### Live Variables

!!! note "Unreleased Feature"
    Live variables have not yet been released. This chapter documents planned functionality that is not currently available.

Live variables represent a reactive programming paradigm in Verse,
enabling variables to automatically recompute their values when
dependencies change. Rather than requiring explicit callbacks or event
handlers, live variables establish dynamic relationships between data,
creating a declarative system where changes propagate naturally
through your code.

Traditional programming requires manual tracking of dependencies and
explicit updates when values change. If variable `A` depends on
variable `B`, you must remember to update `A` whenever `B` changes,
often through callback functions or observer patterns. Live variables
eliminate this bookkeeping by automatically tracking which variables
are read during evaluation and re-evaluating when those dependencies
change. This creates more maintainable code where the intent—that `A`
should always reflect some function of `B` — is expressed directly in
the code itself.

Live variables build a foundation for reactive programming constructs,
including `await`, `upon`, and `when`. Understanding live variables is
essential for working with Verse's event-driven programming model,
particularly for game development scenarios where many values must
stay synchronized.

#### Live Expressions

A *live expression* establishes a dynamic relationship between a
variable and a guard. Once established, the target is automatically
re-evaluated whenever any of the guard's dependencies change, keeping
the variable in sync.

<!--versetest-->
<!-- 01-->
```verse
var X:int = 0
var Y:int = 0
set live X = Y+1  # X now tracks Y
set Y = 5         # X automatically becomes 6
```
<!--
X = 6
-->

In the above, `set live X = Y+1` is a live expression, the target is
the previously declared variable `X` and the guard is the expression
`Y+1` with a dependency on variable `Y`.

Live variables extend mutable variables (see
[Mutability](12_BOOK_OF_VERSE_I_FOUNDATIONS.md#book-of-verse-source-unit-05mutabilitymd)) with automated dependency tracking:
any variable read during the evaluation of the guard expression is
tracked. When any of those variables change, the guard is
re-evaluated, and the target variable updates automatically.

##### Declaration Forms

Live variables can be declared in several ways, each suited to different use cases:

<!--NoCompile-->
<!-- 02-->
```verse
# Live variable declaration
var live X:int = Exp

# Live assignment to existing variable
var X:int = 0
# ... later ...
set live X = Exp

# Immutable live variable
live Y:int = Exp

# Variable with a function type (with <reads> effect)
var X: F = Exp            # Initial value computed normally
var live X: F = Exp       # Initial value tracked for dependencies

# Immutable variable with a function type (with <reads> effect)
X: F = Exp                # Initial value computed normally
live X: F = Exp           # Initial value tracked for dependencies

# Input-output variable pairs
var In->Out: F = Exp      # Initial value computed normally
var live In->Out: F = Exp # Initial value tracked for dependencies

In->Out: F = Exp          # Initial value computed normally
live In->Out: F = Exp     # Initial value tracked for dependencies
```

The most common form, `var live X = Exp`, creates a mutable variable
whose initial value comes from evaluating the guard and subsequently
updates whenever dependencies change. The guard expression can read
other variables, and those reads are tracked to establish the
dependency relationship.

The assignment form, `set live X = Exp`, converts an existing variable
into a live variable by attaching a guard. This is useful when you
need to make a variable reactive after initialization or conditionally
based on program state.

Immutable live variables, declared with just `live Y = Exp`, cannot be
directly written but still update automatically when their guard's
dependencies change. This provides a read-only reactive value, useful
for derived computations that should never be manually overridden.

When a variable's type is a function with the `<reads>` effect, the
variable becomes live through its type (assignments are filtered
through the function, and changes to the function's dependencies
trigger recalculation). The `live` keyword in the declaration
determines whether the initial expression `Exp` is also tracked for
dependencies. Without `live`, `Exp` is evaluated once; with `live`,
dependencies in `Exp` are tracked and can trigger updates before the
first assignment.

Input-output pairs create two variables where one captures raw values
and the other holds transformed values. Again, the `live` keyword
controls whether the initial expression `Exp` is tracked for
dependencies.

The following sections detail these more complex forms.

##### Functions as Types

Verse allows functions to be used as types for variables. When a
function with the `<reads>` effect is used as a type, the variable
automatically becomes live, updating whenever the function's
dependencies change.

<!--versetest-->
<!-- 03 FAILURE
  Line 8: Script Error 3547: Expected a type, got function identifier instead.
  Line 8: Script Error 3601: Data definitions at this scope must be initialized with a value.
-->
```verse
var Mult:int = 2

Multiply(Arg: int)<reads>:int = Arg * Mult

var X : Multiply

set X = 10        # X gets 20
set Mult = 1      # X gets 10
```
<!--
X = 10
-->

In this example, `Multiply` serves dual roles: it is both a function
and a type for variable `X`.

**As a type:** When you declare `var X : Multiply`, several things happen:

- The storage type of `X` becomes `int` (the function's return type)
- Values assigned to `X` must be `int` (the function's parameter type)
- Each assignment passes through the function: `set X = 10` calls `Multiply(10)` and stores the result

**As a live expression:** Because `Multiply` has a `<reads>` effect
(it reads mutable variable `Mult`), the variable declaration becomes a
live expression with `Multiply` as its guard. This creates two ways
the value changes:

1. **Direct assignment:** `set X = 10` filters the value through `Multiply`, storing 20
2. **Dependency updates:** `set Mult = 1` triggers recalculation, updating `X` to 10

This pattern elegantly combines transformation (every write is
filtered) with reactivity (changes to dependencies trigger updates).

##### Input-Output Variables

Input-output variable pairs capture both raw input values and their
transformed outputs. The syntax `var In->Out:F=Exp` creates two
related variables where `Out` is the writable variable and `In`
automatically stores the untransformed value before it passes through
function `F`.

This pattern elegantly handles common game scenarios where values must
stay within dynamic constraints. Consider health that must remain
within bounds:

<!--NoCompile-->
<!-- 04-->
```verse
clamp := class:
    var Lower:int = 0
    var Upper:int = 100
    Evaluate(Value:int)<reads>:int =
        if (Value < Upper) then:
           if (Value > Lower) then Value else Lower
        else:
           Upper

Clamp := clamp{}
var BaseHealth->Health: Clamp.Evaluate = 50

# Health = 50 (clamped to [0, 100])
set Health = 75      # BaseHealth = 75, Health = 75
set Health = 120     # BaseHealth = 120, Health = 100 (clamped)
set Clamp.Upper = 60 # BaseHealth = 120, Health = 60 (reclamped)
```

When you write to `Health`, two things happen:

 1. The raw value is stored in `BaseHealth`
 2. The value is passed through `Clamp.Evaluate`, and the result is stored in `Health`

Because `Clamp.Evaluate` has a `<reads>` effect (it reads the mutable
variables `Lower` and `Upper`), this becomes a live expression. When
the constraints change, `Health` is automatically recalculated from
`BaseHealth`.

**How It Works**

The declaration `var BaseHealth->Health: Clamp.Evaluate = 50` creates a live expression where:

- `BaseHealth` stores the raw input value (read-only from external perspective)
- `Health` stores the clamped value (read-write)
- `Clamp.Evaluate` is the transformation function with a `<reads>` effect

The object `Clamp` is an instance of class `clamp` with mutable bounds `Lower` and `Upper`. Because `Evaluate` reads these mutable variables, changes to them trigger recalculation:

- `set Health=75` — The value passes through unchanged, so both `BaseHealth` and `Health` become 75
- `set Health=120` — Exceeds `Upper`, so `BaseHealth` becomes 120 but `Health` becomes 100
- `set Clamp.Upper=60` — The constraint changes, triggering recalculation: `Health` updates to 60 while `BaseHealth` remains 120

Using an instance method like `Clamp.Evaluate` allows multiple
independent clamps in the same context, each with its own dynamic
bounds.

**Access Control**

The scope of input and output variables can be controlled
independently by adding access specifiers: for example `var
In<private>->Out<public>:t` makes the base value private while
exposing the constrained value publicly.

##### Restricted Effects and Stability

Live variable guards cannot have the `<writes>` effect. This
fundamental restriction prevents side effects during guard
evaluation, which Verse must be able to perform freely whenever
dependencies change.

<!--NoCompile-->
<!-- 05-->
```verse
# ERROR: guard cannot write
var X:int = 0
var GlobalCounter:int = 0
set live X = block:
    set GlobalCounter += 1  # Not allowed!
    GlobalCounter
```

Live variables with interdependencies can form cycles. When target
expressions use idempotent operations and values are comparable, these
cycles can naturally converge to fixed points.

<!--versetest-->
<!-- 06-->
```verse
var X:int = 2
var Y:int = 2

set live X = if (Y < 0) then 0 else Y - 1
set live Y = if (X < 0) then 0 else X - 1

# Evaluates as: X=1, Y=0, X=-1, Y=0 (stable)
```
<!--
X=-1
Y=0
-->

If the type of the variable is comparable, the guards are re-evaluated
until values stabilize. In this example, `X` decrements to -1, `Y`
clamps to 0, and `X` would recompute but produces -1 again, so the
system stabilizes.

However, cycles without proper termination conditions can
diverge. Verse cannot prevent all divergence—care must be taken when
designing interdependent live variables.

This has a subtle implication: since any variable might become live
after creation, reading any variable must be assumed to potentially
trigger guard evaluation and, in the worst case, trigger a cycle. The
effect system accounts for this: the `<writes>` effect implies
`<diverges>` because any write might trigger cyclic live variable
evaluation. The following illustrates a cyclic definition when `X` is
larger than 0:

<!-- 07-->
```verse
var X:int = 0
var live Y:int = if (X>0) then X+1 else 0

set live X = Y
set X = 1  # Error! Cyclic evaluation
```

##### Tracking Dependencies

Live variables track dependencies dynamically at runtime, not statically from source code. 
A variable becomes a dependency only when it is actually read during evaluation, not merely when it appears in the guard expression:

1. *Runtime tracking:* Dependencies are determined by which variables are actually accessed during each evaluation
2. *Transitive tracking:* Dependencies include variables read in called functions
3. *Dynamic changes:* The dependency set can change from one evaluation to the next

Consider this example:

<!--NoCompile-->
<!-- 08-->
```verse
var X:int = 1
var Y:int = 2
var Z:int = 3

SomeFun(Value:int):int =
   if(Value > 0) then X else Y

var live W:int = SomeFun(Z)   # W is 1, Dependencies: {Z, X}
set Z = 0                     # W is 2, Dependencies: {Z, Y}
```

Initially, `SomeFun(Z)` reads `Z` (which is 3) and evaluates the `then` branch, reading `X`, yielding `W=1` with dependencies `{Z, X}`.

After `set Z=0`, the change to `Z` triggers re-evaluation. Now
`SomeFun(Z)` reads `Z` (which is 0) and evaluates the `else` branch,
reading `Y`. This results in `W=2` with new dependencies `{Z, Y}`.

Notice how `Y` became a dependency only when the execution path
changed. If `X` is subsequently modified, `W` will *not* update
because `X` is no longer in the dependency set. This dynamic tracking
ensures that live variables only react to changes that actually affect
their current value.

##### Turning Off Liveness

A live variable established through its guard (not its type) can be
turned off by a subsequent regular assignment.

<!--versetest-->
<!-- 09-->
```verse
var X:int = 0
var Y:int = 5
set live X = Y  # X is now live, tracking Y

set Y = 10      # X becomes 10
set X = 20      # X is now a regular variable again
set Y = 15      # X remains 20 (no longer tracking Y)
```
<!--
X=20
-->

This allows temporary reactive behavior that can be disabled when no
longer needed. However, variables that are live through their type
expression remain live permanently—their reactive behavior is
intrinsic to their type.

#### Reactive Constructs

Live variables form the foundation for three reactive constructs that
handle asynchronous events without explicit callbacks: `await`,
`upon`, and `when`.

##### The await Expression

The `await` expression suspends execution until a target expression
succeeds, providing a synchronization primitive for asynchronous
programming.

<!--versetest
-->
<!-- 10-->
```verse
F()<suspends>:void =
    var X:int = 0

    OldX := X # copy the old value

    # Suspend until X changes from OldX (0)
    await{X <> OldX}
    Print("X changed to: {X}")
```

The target expression is evaluated immediately. If it fails, the
task suspends. Verse tracks which variables were read during
evaluation. Whenever those variables change, the guard is re-evaluated.
If it succeeds, execution resumes immediately.

The practical implications are that you can write code that naturally
expresses "wait for this condition" without manually managing event
handlers or callback registration. The code suspends at the await
point and resumes exactly when the condition becomes true.

<!--versetest
int_ref := class:
    var Contents:int = 0

TestAwait()<transacts><suspends>:void =
    X:int_ref=int_ref{}
    Y:int_ref=int_ref{}
    # Wait for a specific condition
    await{X.Contents > 10}
    set Y.Contents = X.Contents * 2
<#
-->
<!-- 11 -->
```verse
# Wait for a specific condition
await{X.Contents > 10}
set Y.Contents = X.Contents * 2
```
<!-- #>-->

The guard expression must have effects `<reads><computes><decides>`
(see [Effects](14_BOOK_OF_VERSE_III_EFFECTS_CONCURRENCY_AND_EVOLUTION.md#book-of-verse-source-unit-13effectsmd))—it can read and compute but cannot
write. This ensures re-evaluation is side-effect free.
The body of `await` also cannot contain `branch` expressions, since
`branch` requires a `<suspends>` context and the guard must remain
side-effect free.

##### The upon Expression

The `upon` expression provides one-shot reactive behavior: when a
condition becomes true, execute some code once. Unlike `await`, which
resumes the current task, `upon` creates a new concurrent task that
runs when triggered.

<!--versetest-->
<!-- 12-->
```verse
var Health:int = 100
var IsDead:logic = false

upon(Health <= 0):
    set IsDead = true
    Print("Player died!")

set Health = 50  # Nothing happens
set Health = 0   # Triggers: prints "Player died!"
set Health = -10 # Nothing happens (already triggered once)
```

The `upon` expression evaluates its guard immediately and records the variables read. It then yields a `task(t)` where `t` is the result type of the body, representing the pending reactive behavior. When dependencies change, the guard is re-evaluated. If it succeeds, the body executes once in a new concurrent task, and the upon completes.

This one-shot behavior makes `upon` perfect for state transitions and event notifications. When a threshold is crossed, when a resource becomes available, when a timer expires—these scenarios naturally map to `upon`'s "fire once when condition becomes true" semantics.

The body must have the `<transacts>` effect (see [Effects](14_BOOK_OF_VERSE_III_EFFECTS_CONCURRENCY_AND_EVOLUTION.md#book-of-verse-source-unit-13effectsmd)), allowing it to read and write variables (including other live variables), with execution guaranteed to be atomic with respect to notifications.

##### The when Expression

The `when` expression provides continuous reactive behavior: every time a condition is true, execute some code. This creates a persistent observer that runs whenever its guard succeeds.

<!--versetest-->
<!-- 13 FAILURE
  Line 6: Verse compiler error V3560: Expected definition but found macro invocation.
  Line 10: Verse compiler error V3560: Expected definition but found assignment.
  Line 11: Verse compiler error V3560: Expected definition but found assignment.
  Line 12: Verse compiler error V3560: Expected definition but found assignment.
  Line 3: Verse compiler error V3502: Module-scoped `var` must have `weak_map` type.
  Line 4: Verse compiler error V3502: Module-scoped `var` must have `weak_map` type.
-->
```verse
var Score:int = 0
var DisplayedScore:int = 0

when(Score):
    set DisplayedScore = Score
    Print("Score updated to: {Score}")

set Score = 100  # Triggers: prints "Score updated to: 100"
set Score = 100  # No trigger (value unchanged)
set Score = 200  # Triggers: prints "Score updated to: 200"
```

The `when` expression evaluates its guard immediately. If the guard succeeds, the body executes. Then it records the variables read by the guard and yields a `task(void)`. Whenever dependencies change and the guard succeeds, the body executes again, creating a continuous observation loop.

This makes `when` ideal for maintaining derived state and responding to ongoing changes. Synchronizing UI with game state, updating AI behavior based on player actions, or maintaining consistency between related variables all benefit from `when`'s persistent reactivity.

<!--versetest-->
<!-- 14-->
```verse
var X:int = 2
var Y:int = 2

when(Y):
    Z := if (Y < 0) then 0 else Y - 1
    if (Z <> X):
        set X = Z

when(X):
    Z := if (X < 0) then 0 else X - 1
    if (Z <> Y):
        set Y = Z

# These when expressions will stabilize at X = -1, Y = 0
```

The body executes with the `<transacts>` effect, and the when immediately re-registers after each execution, creating the continuous observation pattern.

##### Cancellation

All three reactive constructs—`await`, `upon`, and `when`—return a `task` that can be canceled, allowing dynamic control over reactive behavior.

<!--versetest-->
<!-- 15 FAILURE
  Line 10: Script Error 3512: This invocation calls a function that has the 'suspends' effect, which is not allowed by its context.
-->
```verse
var X:int = 0
var Y:int = 0

Task := upon(X > 5):
    set Y = X

Task.Cancel()  # Cancels the reactive behavior
set X = 10     # Y remains 0
```

Canceling a task immediately removes all dependency tracking and prevents the associated code from running. This provides fine-grained control over the lifecycle of reactive behaviors, allowing you to enable and disable observations based on game state or user actions.

#### The batch Expression

The `batch` expression groups multiple variable updates together, delaying notifications until the entire group completes. This prevents intermediate states from triggering reactive behaviors and ensures observers see consistent snapshots of related changes.

<!--versetest-->
<!-- 16-->
```verse
var X:int = 0
var Y:int = 0

when(X > 1 and Y < 10):
    Print("Fired!") # Never prints

when(X):
    Print("X Changed to {X}!") # Prints once

batch:
    set X = 2   
    set Y = 10
    set X += 5
    Print("Inside batch")

Print("After batch")

# Output order:
# -"Inside batch"
# -"X Changed to 7!"
# -"After batch"
```

Inside a `batch` block, variable updates occur immediately but notifications to awaiting tasks and reactive constructs are deferred. When the batch completes, all pending notifications fire in the order their triggers occurred, but observers see the final consistent state rather than intermediate values.

If the same notification occurs twice, only the first of them will be delivered.

Batch expressions nest: notifications are delayed until all enclosing batches complete. This composability ensures that no matter how deeply nested your code, you can guarantee atomic updates of related variables.

The body of a batch must not have the `<suspends>` effect—all operations must complete immediately. This ensures batch blocks have well-defined boundaries and can't leave the system in an inconsistent state by suspending mid-update.

#### Issues and Patterns

##### API Design

Any variable appearing in the public interface of a class or module
can be made live by external code, potentially violating class
invariants. To avoid this, one could limit the exposure of mutable
variables or at least use access modifiers to control this:

<!--versetest-->
<!-- 17 FAILURE
  Line 4: Script Error 3509: This variable expects to be initialized with a value of type int, but this initializer is an incompatible value of type type{_(:float)<reads>:float}.
  Line 4: Script Error 3509: `live` requires a `comparable` right-hand side.  This right-hand side is of type type{_(:float)<reads>:float}.
  Line 4: Script Error 3641: Attributes on var only allowed inside a module or a class
  Line 4: Script Error 3594: Access level private is only allowed inside classes and interfaces.
-->
```verse
var<private> live X<public>:int = Exp
```

Here `X` is publicly visible for reading but can only be updated by
the class itself. This prevents external code from attaching arbitrary guards that might break the class's
invariants.

##### Failures and Liveness

Live variable updates and reactive construct triggers are integrated
in the failure semantics of Verse.  When there is a failure, live
variable updates are rolled back and their notifications are
suppressed.

<!--versetest-->
<!-- 18-->
```verse
var X:int = 0
var Y:int = 0

if:
    set live X = Y + 5  # Establishes live relationship
    false?          # Transaction fails

upon(X):
    Print("{X}") # Does not print when Y changes

# Live relationship was not established
set Y = 10  # X remains 0
```

This ensures that reactive behaviors only observe committed changes,
maintaining consistency even in the presence of speculative execution
and failure.

##### Derived Synchronization

A common pattern is for multiple UI elements to reflect the same 
game state, `when` provides automatic synchronization:

<!--versetest-->
<!-- 19-->
```verse
var PlayerScore:int = 0
var DisplayedScore:int = 0
var ScoreText:string = ""

when(PlayerScore):
    set DisplayedScore = PlayerScore
    set ScoreText = "Score: {PlayerScore}"
```

Every change to `PlayerScore` automatically updates both the numeric
display value and the formatted text, keeping the UI consistent
without manual coordination.

##### Conditional Reactivity

Live variables can track different sources based on conditions:

<!--versetest-->
<!-- 20 FAILURE
  Line 10: Script Error 3513: Expected an expression that can fail in the 'if' condition clause
-->
```verse
var UseAlternate:logic = false
var PrimaryValue:int = 10
var AlternateValue:int = 20
var CurrentValue:int = 0

set live CurrentValue =
    if (UseAlternate?) then AlternateValue else PrimaryValue

# CurrentValue = 10
set UseAlternate = true
# CurrentValue = 20
set AlternateValue = 30
# CurrentValue = 30
set PrimaryValue = 15
# CurrentValue = 30 (still tracking AlternateValue)
```

The dependency tracking is dynamic: when the condition changes, the
set of tracked variables changes accordingly, allowing flexible
reactive routing.

##### Resource Loading

Use `upon` for one-time initialization when resources become available:

<!--versetest-->
<!-- 21 FAILURE
resource_manager := class:
    var TextureLoaded:logic = false
    var ModelLoaded:logic = false

    Initialize()<suspends>:void = {}
-->
<!-- 21 FAILURE
  Line 8: Verse compiler error V3502: Type definitions are not yet implemented outside of a module scope.
-->
```verse
resource_manager := class:
    var TextureLoaded:logic = false
    var ModelLoaded:logic = false

    Initialize()<suspends>:void =
        upon(TextureLoaded? and ModelLoaded?):
            Print("All resources loaded, starting game")
            StartGame()
```

This pattern eliminates manual tracking of loading state. When both
resources finish loading, the game starts automatically.

##### Modifier Stack (Under Consideration)

**The design of modifier_stack has not been finalized; material presented here is likely to change.**

Game development often requires applying multiple modifiers to a single value. For instance, a player's health might need to be
clamped to a valid range, temporarily boosted by a health potion and automatically recomputed when dependencies change.

The `modifier_stack` pattern provides a composable solution using live variables and function-as-type, allowing ordered transformations that automatically update when any modifier's dependencies change.

The modifier stack consists of three components:

1. **`modifier_iterface(t)`** - An interface for modifiers that transform values of type `t`
2. **`modifier_stack(t)`** - A container that orders and composes modifiers
3. **Live variable** - Uses `modifier_stack.Evaluate` as its type for automatic reactivity

When you assign to a live variable with a modifier stack type, the value flows through each modifier in position order, and the final result is stored. Because `modifier_stack.Evaluate` has the `<reads>` effect, changes to any modifier's dependencies (or adding/removing modifiers) trigger automatic recalculation.

The public API is as follows:

<!--NoCompile-->
<!-- 22-->
```verse
modifier_iterface(t : type) := interface:
   Evaluate(Value:t)<reads> : t

modifier_stack(t:type) := class:
   # Insert a Modifier at Position; return a cancelable used to remove the Modifier.
   AddModifier<final>(Modifier:modifier_iterface(t), Position:rational)<transacts>: cancelable

   # Returns the input Value evaluated against each modifier in the stack in position order.
   Evaluate<final>(Value:t)<reads> : t
```

The `AddModifier` method returns a `cancelable` which can be used to remove the inserted modifier.
Removing a modifier triggers recalculation of any live variable associated with this stack.

For example, consider the following which creates a live variable `Health` filtered through a
modifier stack containing a magic potion modifier that doubles the input value:

<!--NoCompile-->
<!-- 23-->
```verse
HealthStack := modifier_stack(float){}
HealthStack.AddModifier(magic_potion{Value:=2.0})
var RawHealth -> Health : HealthStack.Evaluate = 10.0
# RawHealth = 10.0, Health = 20.0
```

The variable automatically recomputes when the multiplier changes or when modifiers are added to the stack.

In more detail, this example demonstrates two modifiers working together: a `magic_potion` that multiplies health, and a `clamp` that bounds values within a range.

<!--NoCompile-->
<!-- 24-->
```verse
# Define modifier implementations
magic_potion := class(modifier_iterface(float)):
   var Value:float
   Evaluate<override>(Arg:float)<reads>:float = Arg * Value

clamp := class(modifier_iterface(float)):
   var Low:float
   var High:float
   Evaluate<override>(Arg:float)<reads>:float =
       if (Arg<Low) then Low else { if (Arg>High) then High else Arg }

# Create instances
Potion := magic_potion{ Value:= 2.0 }
Clamp := clamp{Low:=1.0, High:= 12.0 }

# Build the modifier stack
HealthStack := modifier_stack(float){}
RevokePotion := HealthStack.AddModifier(Potion, 0.0)  # Apply first (position 0.0)
HealthStack.AddModifier(Clamp, 1.0)                   # Apply second (position 1.0)

# Create live variable with modifier stack
var Health : HealthStack.Evaluate = 5.0  # 5.0 * 2.0 = 10.0 (then clamped to [1.0, 12.0])
set Potion.Value = 3.0                   # 5.0 * 3.0 = 15.0 (clamped to 12.0)
RevokePotion.Cancel()                    # 5.0 (no potion, just clamp to [1.0, 12.0])
```

The value flows through modifiers in position order:

1. **Initial:** 5.0 → Potion (×2.0) → 10.0 → Clamp → 10.0
2. **After changing `Potion.Value`:** 5.0 → Potion (×3.0) → 15.0 → Clamp → 12.0
3. **After removing potion:** 5.0 → Clamp → 5.0

There are plans to enforce via the compiler that: each modifier instance can only be added to one stack, and 
each stack instance can be associated with one variable.  This will enable future features
where modifier stacks maintain state specific to their associated live variable.

##### Common Errors

**Unnecessary Live Declarations**

Defining a live variable with no dependencies that can change is unnecessary and misleading:

<!--NoCompile-->
<!-- 25-->
```verse
var live X:int = 10    # X is 10 and will never change
set live X = 20        # X is 20 and will never change
```

In both cases, `X` does not update automatically, so the program
behaves identically without the `live` keyword. The `live` annotation
falsely suggests reactive behavior where none exists.

**Missing Mutable Dependencies**

Similarly, a live variable that only depends on immutable values will never update:

<!--NoCompile-->
<!-- 26-->
```verse
X:int = 10
var live Y = X+1    # Y is 11 and will never change
```

Since `X` is immutable, `Y` has no mutable dependencies and will
remain at 11 forever. The `live` declaration is pointless.

**Function-as-Type Confusion**

A subtle error occurs when trying to make a variable live through a
function type:

<!--NoCompile-->
<!-- 27-->
```verse
var Mult:int = 10

Multiply(Value:int):type{_(:int):int} =
    Fun(Arg:int):int = Value * Arg
    Fun

var X:Multiply(Mult) = 10    # X = 100

set Mult = 20                 # X is still 100 (not live!)
```

This code is mistaken. The programmer likely thought that
`Multiply(Mult)` would make `X` live because the expression has a
`<reads>` effect (it reads `Mult`) and returns a function type
`int->int`.

**The error:** For a variable to be live through its type, the
*returned function itself* must have the `<reads>` effect, not the
expression that produces the function.

To see why, consider this equivalent transformation:

<!--NoCompile-->
<!-- 28-->
```verse
MFun = Multiply(Mult)
var X:MFun = 10
```

Now it is clear that `X` is not live—`MFun` is just a function value
with type `int->int`, and that function does not have a `<reads>`
effect.

**The correct approach:** Use the pattern where the function used as a
type directly has the `<reads>` effect:

<!--NoCompile-->
<!-- 29-->
```verse
var Mult:int = 10

Multiply(Arg:int)<reads>:int = Arg * Mult

var X:Multiply = 10    # X = 100
set Mult = 20          # X = 200 (now live!)
```

Here `Multiply` itself has `<reads>`, so using it as a type makes `X` live.

If the same function has to be reused with different variables as
dependent, one can package it in an object as shown earlier.

#### Evolution 

When publishing a new version of a system, it is allowed to remove
`live` from a variable definition. This forward compatibility
guarantee means that reactive behavior is an implementation detail
that can be optimized away without breaking client code.

Converting a regular variable to a live variable in a new version is
generally safe if the computed value matches what the previous version
maintained manually. However, if external code depends on being able
to set arbitrary values, this could break expectations.

The ability to cancel reactive constructs provides an important
upgrade path: code that creates `when` or `upon` observers can later
be modified to cancel them under different conditions without breaking
existing behavior.

## Book of Verse Source Unit: 17_persistable.md

### Persistable Types

Persistable types allow you to store data that persists beyond the
current game session. This is essential for saving player progress,
preferences, and other game state that should be maintained across
multiple play sessions.

Persistable data is stored using module-scoped `weak_map(player, t)`
variables, where `t` is any persistable type. When a player joins a
game, their previously saved data is automatically loaded into all
module-scoped variables of type `weak_map(player, t)`.

<!--NoCompile-->
<!-- 01 -->
```verse
using { /Fortnite.com/Devices }
using { /UnrealEngine.com/Temporary/Diagnostics }
using { /Verse.org/Simulation }

# Global persistable variable storing player data
MySavedPlayerData : weak_map(player, int) = map{}

# Initialize data for a player if not already present
InitializePlayerData(Player : player) : void =
    if (not MySavedPlayerData[Player]):
        if (set MySavedPlayerData[Player] = 0) {}
```

#### Built-in Persistable Types

The following primitive types are persistable by default:

- Numeric Types:

   - **`logic`** - Boolean values (true/false)
   - **`int`** - Integer values (must fit in 64-bit signed range for persistence)
   - **`float`** - Floating-point numbers

- Character Types:

   - **`string`** - Text values
   - **`char`** - Single UTF-8 character
   - **`char32`** - Single UTF-32 character

- Container Types:

   - **`array`** - Persistable if element type is persistable
   - **`map`** - Persistable if both key and value types are persistable
   - **`option`** - Persistable if the wrapped type is persistable
   - **`tuple`** - Persistable if all element types are persistable

#### Custom Persistable Types

You can create custom persistable types using the `<persistable>`
specifier with classes, structs, and enums.

Classes must meet specific requirements to be persistable:

<!--versetest-->
<!-- 02 -->
```verse
player_class := enum<persistable>:
    Villager

player_profile_data := class<final><persistable>:
    Version:int = 1
    Class:player_class = player_class.Villager
    XP:int = 0
    Rank:int = 0
    CompletedQuestCount:int = 0
```

Requirements for persistable classes:

- Must have the `<persistable>` specifier
- Must be `<final>` (no subclasses allowed)
- Cannot be `<unique>` 
- Cannot have a superclass (including interfaces) 
- Cannot be parametric (generic) 
- Can only contain persistable field types 
- Cannot have variable members (`var` fields) 
- Field initializers must be effect-free (cannot use `<transacts>`, `<decides>`, etc.) 

Structs are ideal for simple data structures that will not change after
publication:

<!--versetest-->
<!-- 03 -->
```verse
coordinates := struct<persistable>:
    X:float = 0.0
    Y:float = 0.0
```

Requirements for persistable structs:

- Must have the `<persistable>` specifier
- Cannot be parametric (generic) 
- Can only contain persistable field types (see Prohibited Field Types below) 
- Field initializers must be effect-free (cannot use `<transacts>`, `<decides>`, etc.)
- Cannot be modified after island publication

Enums represent a fixed set of named values:

<!--versetest-->
<!-- 04 -->
```verse
day := enum<persistable>:
    Monday
    Tuesday
    Wednesday
    Thursday
    Friday
    Saturday
    Sunday
```

Important notes:

- `<closed>` persistable enums cannot be changed to open after publication
- Only `<open>` persistable enums can have new values added after publication

#### Prohibited Field Types

Persistable types have strict restrictions on what field types they
can contain. The following types **cannot** be used as fields in
persistable classes or structs:

- Abstract and Dynamic Types:

   - **`any`** - Cannot be persisted (too dynamic)
   - **`comparable`** - Abstract interface type
   - **`type`** - Type values cannot be persisted

- Non-Serializable Types:

   - **`rational`** - Exact rational numbers (not persistable)
   - **Function types** (e.g., `int -> int`) - Functions cannot be serialized
   - **`weak_map`** - Weak references are not persistable
   - **Interface types** - Abstract interfaces cannot be persisted

- Non-Persistable User Types

   - **Non-persistable enums** - Enums without `<persistable>` specifier cannot be used
   - **Non-persistable classes** - Classes without `<persistable>` specifier cannot be used
   - **Non-persistable structs** - Structs without `<persistable>` specifier cannot be used


#### Example

Initializing Player Data:

<!--versetest
player := class<unique><persistent><module_scoped_var_weak_map_key>{}
player_stats := struct<persistable>:
    Level:int = 1
    Experience:int = 0
    GamesPlayed:int = 0

var PlayerData : weak_map(player, player_stats) = map{}

GetOrCreatePlayerStats(Player : player) : player_stats =
    if (ExistingStats := PlayerData[Player]):
        ExistingStats
    else:
        NewStats := player_stats{}
        if (set PlayerData[Player] = NewStats):
            NewStats
        else:
            player_stats{}
<#
-->
<!-- 06 -->
```verse
# Define a persistable player stats structure
player_stats := struct<persistable>:
    Level:int = 1
    Experience:int = 0
    GamesPlayed:int = 0

# Global persistent storage
PlayerData : weak_map(player, player_stats) = map{}

# Initialize or retrieve player data
GetOrCreatePlayerStats(Player : player) : player_stats =
    if (ExistingStats := PlayerData[Player]):
        ExistingStats
    else:
        NewStats := player_stats{}
        if (set PlayerData[Player] = NewStats):
            NewStats
        else:
            player_stats{}  # Fallback
```
<!-- #> -->


#### JSON Serialization

!!! note "Unreleased Feature"
    JSON Serialization have not yet been released and is not publicly available.

Verse provides JSON serialization functions for persistable types,
enabling manual serialization and deserialization of data. While the
primary persistence mechanism uses `weak_map(player, t)` for automatic
player data, JSON serialization can be useful for debugging, data
migration, or integration with external systems.

Converts a persistable value to JSON string:

<!--versetest
player := class<unique>{}
player_data := class<final><persistable>:
    Level:int = 1
    Score:int = 100
PersistenceModule := module{
    ToJson<public>(Data:player_data)<decides>:string = ""
}
-->
<!-- 08 -->
```verse
# Serialize persistable data to JSON
Data := player_data{Level := 5, Score := 250}
JsonString := PersistenceModule.ToJson[Data]
# Produces: {"$package_name":"/...", "$class_name":"player_data", "x_Level":5, "x_Score":250}
```

Deserializes JSON string to typed value:

<!--versetest
player := class<unique>{}
player_data := class<final><persistable>:
    Level:int = 1
    Score:int = 100
PersistenceModule := module{
    FromJson<public>(JsonStr:string, T:type)<transacts><decides>:player_data =
        false?
        player_data{Level := 1, Score := 100}
}
-->
<!-- 09 -->
```verse
# Deserialize JSON to typed value
JsonString := ""
if (Restored := PersistenceModule.FromJson[JsonString, player_data]):
    # Restored.Level = 10
    # Restored.Score = 500
```

All serialized persistable objects include metadata fields:

```json
{
  "$package_name": "/SolIdeDataSources/_Verse",
  "$class_name": "player_data",
  "x_Level": 5,
  "x_Score": 250
}
```

**Metadata fields:**

- `$package_name` - Package path of the type
- `$class_name` - Qualified class/struct name

**Field names:**

- Prefixed with `x_` in current format
- Old format used mangled names like `i___verse_0x123_FieldName`

##### Type-Specific Serialization

**Primitives:**

<!--versetest
player := class<unique>{}
int_ref := class<final><persistable>:
    Value:int
PersistenceModule := module{
    ToJson<public>(Data:int_ref)<decides>:string = ""
}
-->
<!-- 11 -->
```verse
# Serialized as JSON number
JsonString := PersistenceModule.ToJson[int_ref{Value := 42}]
# {"$package_name":"...", "$class_name":"int_ref", "x_Value":42}
```

**Optional types:**

<!--versetest
player := class<unique>{}
optional_ref := class<final><persistable>:
    Value:?int
PersistenceModule := module{
    ToJson<public>(Data:optional_ref)<decides>:string = ""
}
-->
<!-- 12 -->
```verse
# None serialized as false
PersistenceModule.ToJson[optional_ref{Value := false}]
# {..., "x_Value":false}

# Some serialized as object with empty key
PersistenceModule.ToJson[optional_ref{Value := option{42}}]
# {..., "x_Value":{"":42}}
```

**Tuples:**

<!--versetest
player := class<unique>{}
tuple_ref := class<final><persistable>:
    Pair:tuple(int, int)
empty_tuple_ref := class<final><persistable>:
    Empty:tuple()
PersistenceModule := module{
    ToJson<public>(Data:tuple_ref):string = ""
    ToJson<public>(Data:empty_tuple_ref):string = ""
}
-->
<!-- 13 -->
```verse
# Serialized as JSON array
PersistenceModule.ToJson(tuple_ref{Pair := (4, 5)})
# {..., "x_Pair":[4,5]}

# Empty tuple
PersistenceModule.ToJson(empty_tuple_ref{Empty := ()})
# {..., "x_Empty":[]}
```

**Arrays:**
<!--versetest
player := class<unique>{}
array_ref := class<final><persistable>:
    Values:[]int
PersistenceModule := module{
    ToJson<public>(Data:array_ref)<decides>:string = ""
}
-->
<!-- 14 -->
```verse
PersistenceModule.ToJson[array_ref{Values := array{1, 2, 3}}]
# {..., "x_Values":[1,2,3]}
```

**Maps:**

<!--versetest
player := class<unique>{}
map_ref := class<final><persistable>:
    Lookup:[string]int
PersistenceModule := module{
    ToJson<public>(Data:map_ref)<decides>:string = ""
}
-->
<!-- 15 -->
```verse
PersistenceModule.ToJson[map_ref{Lookup := map{"a" => 1, "b" => 2}}]
# {..., "x_Lookup":[{"k":{"":"a"},"v":{"":1}}, {"k":{"":"b"},"v":{"":2}}]}
```

**Enums:**

<!--versetest
player := class<unique>{}
day := enum<persistable>:
    Monday
    Tuesday
enum_ref := class<final><persistable>:
    Day:day
PersistenceModule := module{
    ToJson<public>(Data:enum_ref)<decides>:string = ""
}
-->
<!-- 16 -->
```verse
PersistenceModule.ToJson[enum_ref{Day := day.Monday}]
# {..., "x_Day":"day::Monday"}
```

##### Default Value Handling

When deserializing, missing fields are automatically filled with their default values:

<!--versetest
player := class<unique>{}
versioned_data := class<final><persistable>:
    Version:int = 1
    NewField:int = 0
PersistenceModule := module{
    FromJson<public>(JsonStr:string, T:type)<transacts><decides>:versioned_data =
        false?
        versioned_data{Version := 1, NewField := 0}
}
-->
<!-- 17 -->
```verse
# Old JSON without NewField
OldJson := ""

# Deserializes successfully with default for NewField
if (Data := PersistenceModule.FromJson[OldJson, versioned_data]):
    Data.Version = 1
    Data.NewField = 0  # Uses default value
```

This enables forward-compatible schema evolution - new fields with
defaults can be added without breaking old saved data.

##### Block Clauses During Deserialization

Block clauses do not execute when deserializing from JSON:

<!--versetest
player := class<unique>{}
logged_class := class<final><persistable>:
    Value:int
PersistenceModule := module{
    ToJson<public>(Data:logged_class):string = ""
    FromJson<public>(JsonStr:string, T:type)<transacts>:logged_class = logged_class{Value := 1}
}
-->
<!-- 18 -->
```verse
# Normal construction triggers block
Instance1 := logged_class{Value := 1}

# Deserialization does NOT trigger block
Json := PersistenceModule.ToJson(Instance1)
Instance2 := PersistenceModule.FromJson(Json, logged_class)  # No print
```

Block clauses are only executed during normal construction, not during
deserialization. This means initialization logic in blocks will not run
for loaded data.

##### Integer Range Limitations

Verse protects against integer overflow during serialization. Integers
that exceed the safe serialization range cause runtime errors:

<!--versetest
player := class<unique>{}
int_ref := class<final><persistable>:
    Value:int
PersistenceModule := module{
    ToJson<public>(Data:int_ref)<decides>:string = ""
}
-->
<!-- 19 -->
```verse
# Safe range integers work fine
SafeData := int_ref{Value := 1000000000000000000}
PersistenceModule.ToJson[SafeData]  # OK

# Very large integers may cause runtime errors during serialization
# to prevent silent precision loss
```

This prevents silent precision loss that could occur with
floating-point representation of large integers.


#### Best Practices

- **Schema Stability:** Design your persistable types carefully, as
they cannot be easily changed after publication. Consider versioning
strategies for future updates.

- **Use Structs for Simple Data:** For data that will not need
inheritance or complex behavior, prefer persistable structs over
classes.

- **Handle Missing Data:** Always check if data exists for a player
before accessing it, and provide appropriate defaults.

- **Atomic Updates:** When updating persistent data, create new
instances rather than trying to modify existing ones (Verse uses
immutable data structures).

- **Consider Memory Usage:** Persistent data is loaded for all players
when they join, so be mindful of the amount of data stored per player.

## Book of Verse Source Unit: 18_evolution.md

### Verse Code Evolution and Compatibility

Verse takes a unique approach to code evolution, designed with the ambitious goal of creating software that could remain functional and valuable for decades or even centuries. This vision stems from Verse's role as the programming language for a persistent, global metaverse where code must coexist, evolve, and maintain compatibility across vast timescales.

At its core, Verse embraces three fundamental principles that shape how code evolves: future-proof design that avoids being rooted in past artifacts of other languages, a metaverse-first approach where code persistence and compatibility are critical, and strong static verification that catches runtime problems at compile time. These principles create a foundation for a language that can grow and adapt while maintaining the stability required for a global, persistent codebase.

#### The Nature of Code Publication

When developers publish code to the Verse metaverse, they enter into a social contract with all future users of that code. This contract is more than just a convention—it is enforced by the language itself. Consider what happens when you publish a simple value:

<!--versetest-->
<!-- 01 -->
```verse
Thing<public>:int = 777
```

This seemingly straightforward declaration carries profound implications. By marking `Thing` as public, you are making a commitment that extends indefinitely into the future. Users can depend on `Thing` always existing and always being an integer. While you retain the freedom to change its actual value, the existence and type of `Thing` become permanent fixtures in the metaverse's landscape.

This permanence extends beyond simple values to encompass the entire structure of published code. Persistable structs, once published to an island, become immutable schemas that cannot be altered. Closed enums remain closed forever, unable to accept new values after publication. When a class or interface is marked with the `<castable>` attribute, that decision becomes irreversible, as changing it could introduce unsafe casting behaviors that break existing code.

The publication model distinguishes between two contexts: the live metaverse and islands. In the envisioned live metaverse, publishing an update that attempts to change an immutable variable's value has no effect—the variable already exists with its original value. However, in the current island-based implementation, new instances of an island will adopt the updated value, providing a practical migration path while maintaining conceptual consistency.

#### The Architecture of Backward Compatibility

Backward compatibility in Verse goes beyond simple syntactic preservation—it encompasses semantic guarantees about how code behaves. The language enforces these guarantees through multiple mechanisms that work together to create a robust compatibility framework.

Function effects exemplify this approach. When a function is published with specific effects like `<reads>`, indicating it may read mutable heap data, this becomes part of its contract. Future versions of the function can have fewer effects—evolving from `<reads>` to `<computes>`—but never more. This restriction ensures that code depending on the function's effect profile continues to work correctly, as the function only becomes more pure, never less.

Type evolution follows similar principles. Types can become more specific over time, such as changing from `rational` to `int`, as this represents a refinement rather than a fundamental change. Structures must maintain all existing fields, though new fields can be added. Classes marked with `<final_super>` commit to their inheritance hierarchy permanently, ensuring that code relying on specific inheritance relationships remains valid.

The enforcement of these rules happens at publication time, not just at compile time. Verse actively prevents developers from publishing updates that would violate compatibility guarantees, turning what might be runtime failures in other systems into publication-time errors that must be resolved before code can be deployed.

#### Managing Breaking Changes

Despite the strong emphasis on compatibility, Verse recognizes that some breaking changes are occasionally necessary. The language provides two mechanisms for managing such changes: a deprecation system for gradual migration and special privileges for essential breaking changes.

The deprecation system operates as a multi-phase process that gives developers ample time to adapt. When code patterns become deprecated, they first generate warnings rather than errors. These warnings appear when saving code, alerting developers to practices that will not be supported in future versions. The code continues to compile and run, allowing projects to function while migration plans are developed. Only when developers explicitly upgrade to a new language version do deprecations become errors, and even then, the option to remain on older versions provides an escape hatch.

Version 1 introduced several significant deprecations that illustrate this process. The prohibition of failure in set expressions, which previously allowed with warnings, now requires explicit handling of failable expressions. Mixed separator syntax, which created implicit blocks and confusing scoping rules, must now use consistent separation. The introduction of local qualifiers provides a new tool for disambiguating identifiers while deprecating the use of 'local' as a regular identifier name.

For truly exceptional circumstances, Epic Games and potentially other future authorities retain "superpowers" to make breaking changes outside the normal compatibility framework. These powers include the ability to delete published entities, change types in non-backward-compatible ways, and rewrite modules for legal or safety reasons. These capabilities acknowledge that being good stewards of the metaverse namespace sometimes requires violating the usual compatibility rules, though such actions should remain rare and justified by compelling reasons.

#### Catalog of Compatibility Rules

When you publish code in Verse, many aspects of your definitions become permanent commitments. Understanding exactly what can and cannot change is essential for designing APIs that can evolve gracefully. This catalog documents both the changes that Verse prohibits and the changes it allows to ensure backward compatibility.

The rules follow a general principle: changes that make types more specific (narrowing), add new capabilities, or relax restrictions are often allowed, while changes that make types more general (widening), remove capabilities, or impose new restrictions are typically forbidden. However, the rules vary significantly between final/non-instance members and non-final instance members, with the latter having much stricter requirements.

##### Definitions

**Cannot remove public definitions.** Once a public variable, function, class, or other definition is published, it must remain available. Removing it would break any code that depends on it.

**Cannot change the kind of a definition.** A class cannot become a struct, interface, enum, or function. A function cannot become a class or type alias. These fundamental changes alter how code interacts with your definitions.

**Cannot rename definitions.** Renaming is equivalent to deletion plus addition, which breaks existing references.

##### Enums

**Cannot add or remove enumerators from closed enums.** Closed enums (the default) commit to a fixed set of values forever. Code can exhaustively match all cases without a wildcard, so adding cases would break such matches.

**Closed enums cannot become open.** An enum published as closed cannot become open. This affects whether exhaustive matching is possible.

**Cannot reorder enumerators.** The order of enum values is part of the public contract.

**Cannot rename enumerators.** Each enumerator name is a permanent identifier.

**Open enums can add new enumerators.** This is the sole evolution path for enums—open enums trade the guarantee of exhaustive matching for the ability to grow.

##### Classes and Inheritance

**Class Finality:**

- **Can make non-final class final.** Adding the `<final>` specifier prevents future inheritance, which is a safe addition that strengthens guarantees.
- **Cannot make final class non-final.** Once a class is marked `<final>`, removing this restriction would allow unexpected subclasses that could break assumptions in existing code.

**Class Uniqueness:**

- **Can make non-unique class unique.** Adding the `<unique>` specifier enables identity-based equality, which does not break existing code.
- **Cannot make unique class non-unique.** Removing `<unique>` would change the equality semantics from identity to undefined, breaking code that relies on identity comparison.

**Class Abstractness:**

- **Can make abstract class non-abstract.** Allowing instantiation of a previously abstract class is a safe expansion of capabilities.
- **Cannot make non-abstract class abstract.** Preventing instantiation of a previously concrete class breaks code that creates instances.

**Class Concreteness:**

- **Can make non-concrete final class concrete.** Allowing default instantiation of a final class is safe since no subclasses exist to be affected.
- **Cannot change concreteness in other cases.** Making concrete classes non-concrete or changing concreteness of non-final classes could break instantiation code or subclass behavior.

**Inheritance Changes:**

- **Can add inheritance to non-abstract classes.** Adding a parent class or interface extends capabilities without breaking existing functionality.
- **Cannot remove or change inheritance from non-abstract classes.** Removing a parent breaks code that depends on the inheritance relationship.
- **Cannot add, remove, or change inheritance on abstract classes.** Abstract class hierarchies must remain fixed to prevent conflicts and maintain subtype relationships.
- **Cannot add, remove, or change inheritance on interfaces.** Interface hierarchies must remain stable for the same reasons.

**Special Attributes:**

- **Cannot add or remove the `<castable>` attribute.** Runtime type checks depend on this property. Adding it after publication would enable casts that were not previously safe, while removing it would break existing casts.
- **Cannot remove `<final_super>` once added.** Derived types marked with `<final_super>` must continue inheriting from the same parent to maintain the type hierarchy that `GetCastableFinalSuperClass` depends on.
- **Derived types with `<final_super>` must remain derived from the same parent.** Changing the parent type would break runtime type queries.

**Special Transformations:**

- **Can change final class with no inheritance to struct.** This is a safe transformation since both are value-like and the class cannot have subclasses.
- **Can potentially change abstract class with no class inheritance to interface.** This transformation maintains the same contract but is not yet fully implemented (marked as TODO in tests).

##### Structs

**Cannot add any fields to structs.** Structs are immutable value types with a fixed memory layout. Adding fields would change this layout and break binary compatibility.

**Cannot change between class and struct.** These are fundamentally different types with different semantics—classes are references, structs are values.

##### Fields and Data Members

**Adding Fields:**

- **Can add fields with default values to classes.** New fields with defaults do not break existing construction code since the defaults are used automatically.
- **Cannot add fields without default values to classes.** Existing code that constructs instances would break since it does not provide values for the new fields.
- **Cannot add any fields to structs.** Structs have fixed memory layout and adding fields breaks binary compatibility.

**Removing Fields:**

- **Cannot remove fields from classes or structs.** All published fields must remain available since code may reference them.

**Field Mutability:**

- **Can change final instance field to non-final.** Allowing mutation where it was previously prohibited is a safe expansion of capabilities.
- **Cannot change non-final instance field to final.** Once code depends on being able to mutate a field, making it immutable breaks that code.

**Field Type Changes:**

For **final or non-instance data** (fields that can't be overridden):
- **Can narrow the type (make more specific).** For example, changing from `any` to `int` strengthens the guarantee about what values the field holds.
- **Cannot widen the type (make more general).** For example, changing from `int` to `any` violates the guarantee that callers could read a specific type.

For **non-final instance data** (fields that can be overridden in subclasses):
- **Cannot narrow or widen the type.** These must maintain their exact type to prevent breaking overrides in subclasses or calling code.

**Default Initializers:**

- **Can add a default initializer to a class field.** This makes construction easier without breaking existing code.
- **Cannot remove a default initializer from a class field.** Removing a default breaks construction code that relied on it.

**Overrides:**

- **Can add an override to a field.** Providing a more specific implementation in a subclass is allowed.
- **Can remove an override if it didn't narrow the type.** If the override maintained the same type, removing it is safe.

##### Functions and Methods

Function signature changes follow different rules depending on whether the function is **final/non-instance** (can't be overridden) or a **non-final instance method** (can be overridden). The rules reflect fundamental principles of type safety: functions can become more flexible about what they accept (contravariance) and more specific about what they return (covariance), but only when overriding is not involved.

**Overload Management:**

- **Cannot remove function overloads.** Each published function signature must remain available since code may call it.
- **Function overloads are matched by signature.** When checking compatibility, functions with the same parameters are compared to ensure compatible types and effects.

**Return Types (Covariance):**

For **final or non-instance functions** (module functions, static methods, final methods):
- **Can narrow the return type (make more specific).** Changing from `any` to `int` means the function now guarantees a more specific return value, which is always safe for callers.
- **Cannot widen the return type (make more general).** Changing from `int` to `any` means the function might return different types, breaking code that expects an integer.

For **non-final instance methods** (overridable methods):
- **Cannot narrow or widen the return type.** These must maintain their exact return type to ensure subclass overrides remain compatible.

**Parameter Types (Contravariance):**

For **final or non-instance functions**:
- **Can widen parameter types (make more general).** Changing from `int` to `any` means the function accepts more inputs, which never breaks existing calls with integers.
- **Cannot narrow parameter types (make more specific).** Changing from `any` to `int` means the function rejects some previously valid arguments.
- **Can relax type parameter constraints.** Changing from `t:subtype(comparable)` to `t:type` allows more type arguments.
- **Cannot strengthen type parameter constraints.** The reverse change restricts valid type arguments.

For **non-final instance methods**:
- **Cannot narrow or widen parameter types.** These must maintain their exact parameter types to ensure subclass overrides and calls remain compatible.

**Optional Parameters:**

- **Can add optional named parameters with defaults.** This does not break existing calls since the parameters are optional.
- **Can change default values of optional parameters.** New callers get the new defaults while existing compiled code continues using the values it was compiled with.

**Effects (Covariance):**

For **final or non-instance functions**:
- **Can narrow effects (reduce).** Changing from `<transacts>` to `<reads>` to `<computes>` makes the function more pure, which is always safe since code expecting more effects can handle fewer.
- **Cannot widen effects (increase).** Changing from `<computes>` to `<reads>` violates the contract that the function has limited effects.

For **non-final instance methods**:
- **Cannot narrow or widen effects.** These must maintain their exact effects to ensure overrides work correctly.

**Conversions Between Callable Forms:**

- **Cannot convert between normal functions and constructors.** These are fundamentally different callable entities with different calling conventions.
- **Cannot convert between functions and parametric types.** A function cannot become a type parameter or vice versa.

**Function body:**

- **Cannot change the body of transparent functions.** Verification of callers might depend on the function body of transparent functions, so changes could break callers.
- **Cannot change the body of opaque functions without the `<reads>` effect.** A function without `<reads>` guarantees referential transparency — the same inputs always produce the same outputs. Although the body is not visible to callers, the compiler cannot verify that a modified body preserves this mapping for all inputs, so it conservatively forbids body changes to ensure that `NonReadsFunction() = NonReadsFunction()` holds across versions.
- **Can change the body of opaque functions that have the `<reads>` effect.** Code evolution can be observed by the `<reads>` effect.

**Understanding Variance:**

The asymmetry in these rules reflects **variance** in type theory:
- **Parameters are contravariant**: Accepting more general types (widening) is safe
- **Returns are covariant**: Returning more specific types (narrowing) is safe
- **Effects are covariant**: Having fewer effects is safe

These rules only apply to final/non-instance functions because overridable methods must maintain exact signatures to preserve the Liskov Substitution Principle—any subclass override must be callable wherever the base method is called.

##### Access Specifiers

**Increasing Accessibility (Allowed):**

- **Can increase accessibility of definitions.** Making a private definition public, or protected to public, expands access without breaking existing code.
- **Can make constructors more accessible.** Allowing more code to construct instances is a safe capability expansion.

**Reducing Accessibility (Forbidden):**

- **Cannot reduce accessibility of definitions.** Making a public definition protected, internal, or private breaks all external code using it. Making a protected definition private breaks subclass access.
- **Cannot make constructors less accessible.** A class constructor that was public cannot become private or protected.

##### Persistable Types

Persistable types require stricter compatibility rules because they define the format of saved player data that must remain loadable indefinitely. Changes to persistable types risk corrupting or losing saved data.

**Persistable Attribute Changes:**

- **Cannot add the `<persistable>` attribute after publication.** Making a type persistable changes its serialization behavior and imposes new restrictions.
- **Cannot remove the `<persistable>` attribute.** Saved data depends on the persistence format of these types.

**Persistable Class Fields:**

- **Can add fields with default values to persistable classes.** Saved data without the new field will load successfully using the default.
- **Cannot add fields without defaults to persistable classes.** Old saved data wouldn't have values for these fields.
- **Cannot remove any fields from persistable classes.** Saved data may contain these field values and must be able to load them.

**Persistable Struct Fields:**

- **Cannot add any fields to persistable structs.** Structs have fixed layouts and saved data expects the exact structure.
- **Cannot remove any fields from persistable structs.** All fields in saved data must be loadable.

**Persistable Enum Changes:**

For **closed persistable enums**:
- **Cannot add enumerators.** Case statements may exhaustively match all values, and saved data deserialization depends on the fixed set.
- **Cannot remove enumerators.** Saved data may contain removed values, making it unloadable.

For **open persistable enums**:
- **Can add new enumerators.** Open enums are designed to grow, and the persistence system handles unknown values.
- **Cannot remove enumerators.** Saved data may still reference removed values.

**Type Lifecycle:**

- **Can add new persistable types.** Publishing new types for data persistence is always allowed.
- **Cannot remove persistable types once published.** They must remain available to deserialize old saved data.
- **Cannot change the structure of persistable types.** Field types, inheritance relationships, and other structural changes break deserialization.

**Module/Type Aliases:**

- **Can add module or type aliases to persistable types.** This provides additional ways to reference existing types without changing them.
- **Can remove module or type aliases to persistable types.** Removing an alias does not affect the underlying type's persistence.
- **Module aliases must reference the same path.** Changing the target breaks all code using the alias.
- **Type aliases must reference the same type.** Changing the aliased type breaks all code using the alias.

**Non-Persistable Changes:**

- **Can freely add or remove non-persistable types.** Types without `<persistable>` do not affect saved data and can be added or removed as needed.

**Persistent Variables:**

Verse supports persistent variables (`var` declarations in module scope) that maintain values across sessions:

- **Can add new persistent variables.** New variables are initialized with their default values.
- **Cannot remove persistent variables.** The metaverse expects these variables to exist persistently.
- **Cannot change persistent variable types.** Saved values must match the expected type.
- **Non-persistent variables can be freely changed.** Local or instance variables do not persist and can be modified.

##### Parametric Types

Parametric types (generic types with type parameters) have additional compatibility considerations:

**Type Parameter Constraints:**

- **Can widen type parameter constraints in parametric type domains.** Making constraints more permissive (e.g., from `t:subtype(comparable)` to `t:type`) allows more type arguments.
- **Cannot narrow type parameter constraints.** Restricting valid type arguments breaks existing code using the parametric type.
- **Type parameters are treated as rigid when checking functions inside parametric types.** This ensures type safety within the generic context.

**Parametric vs Non-Parametric:**

- **Cannot convert between parametric and non-parametric forms.** A parametric type cannot lose its type parameters, and a regular type cannot gain them.
- **Cannot convert between functions and parametric types.** These are fundamentally different constructs.

**Variance:**

- **Variance is inferred from usage, not declared.** How type parameters are used (in input positions, output positions, or both) determines their variance.
- **Cannot change inferred variance.** Once a type parameter's usage pattern is published, it establishes a variance contract that must be maintained.

##### Summary

This catalog represents the core compatibility guarantees that Verse enforces. While these restrictions may seem extensive, they ensure that published code remains a stable foundation for the metaverse ecosystem.

Key principles to remember:

1. **Additions are usually safe**: New optional fields, new overloads, new enumerators in open enums
2. **Removals are usually forbidden**: Removing public definitions breaks dependent code
3. **Narrowing is often safe**: More specific return types, fewer effects
4. **Widening is selectively safe**: More general parameter types (contravariance)
5. **Final/non-instance members are more flexible**: They can evolve types and effects
6. **Non-final instance members are rigid**: They must maintain exact signatures
7. **Persistable types are strictest**: Saved data imposes permanent constraints

The key to working within these constraints is thoughtful initial design—choosing the right visibility, finality, effects, and type properties from the start. Consider future evolution needs when making these irreversible decisions.

#### Design Philosophy for Longevity

Creating code that remains viable across extended timescales requires a different approach to software design. Developers must think beyond immediate functionality to consider how their code will evolve and interact with future systems. This forward-thinking approach influences every aspect of development, from initial design to ongoing maintenance.

Schema planning becomes critical when working with persistable types. Since these cannot be changed after publication, developers must carefully consider not just current requirements but potential future needs. This might mean including optional fields that are not immediately necessary or choosing open enums over closed ones when future expansion seems likely. The cost of getting these decisions wrong—being locked into inflexible schemas—encourages thorough upfront design.

Effect specification offers an interesting trade-off. While Verse allows and sometimes encourages over-specification of effects, marking a function as having effects it does not currently use, this provides flexibility for future implementation changes. A function marked as `<reads>` can later be optimized to `<computes>` without breaking compatibility, but the reverse is not true. This asymmetry encourages conservative effect declarations that leave room for future modifications.

The choice between open and closed constructs represents another long-term decision. Open enums allow new values to be added after publication, providing extensibility at the cost of preventing exhaustive pattern matching. Closed enums offer the opposite trade-off. Understanding when flexibility or completeness is more valuable requires thinking about how the code will be used not just today, but years into the future.

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
- [13_BOOK_OF_VERSE_II_PROGRAM_STRUCTURE_AND_TYPES.md](13_BOOK_OF_VERSE_II_PROGRAM_STRUCTURE_AND_TYPES.md)
- [15_GLOSSARY_FORTNITE_CREATIVE.md](15_GLOSSARY_FORTNITE_CREATIVE.md)
- [16_GLOSSARY_UEFN.md](16_GLOSSARY_UEFN.md)
- [17_GLOSSARY_VERSE.md](17_GLOSSARY_VERSE.md)

## Related Glossary Sections

- [17_GLOSSARY_VERSE.md](17_GLOSSARY_VERSE.md)

## Official Sources

- [Book of Verse](https://verselang.github.io/book/)
- [Verse persistence](https://dev.epicgames.com/documentation/en-us/fortnite/using-persistable-data-in-verse)

## Version and Stability Notes

- Treat UI labels, device option names, experimental features, publishing rules, memory limits, and eligibility requirements as version-sensitive.
- Prefer the official index and release notes when this document conflicts with a newer Epic page.
- Preserve exact API identifiers and Verse syntax; do not translate them.

## Source Coverage Notes

- Local sources were merged by topic and assigned one primary owner document; cross-links replace duplicate body text where practical.
- Administrative program plans, schedules, marketing material, fundraising text, and production-only QA files are not part of agent memory.
- Unique transferable technical, design, research, ethical, or instructional knowledge found inside excluded wrappers was distilled into the appropriate owner document.
