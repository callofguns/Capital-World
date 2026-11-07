# Architecture

**Status:** proposed. Nothing here has been compiled yet. Update this file as each system is actually built.

## 1. Guiding rules

1. **Game rules live in C++. Blueprints handle presentation and level setup.**
2. **Simulation state is plain data, not actors.** Businesses, money, inventory and employees are structs owned by subsystems and keyed by `FGuid`. Actors are views onto that data.
3. **One way in for money.** Every change to any balance is a transaction recorded by the ledger. No code edits a balance directly.
4. **No god classes.** Each subsystem owns one area. Subsystems talk through clear function calls and delegates, never by reaching into each other's data.
5. **Don't assume a single local player.** Owners are `FCWOwnerId` values, not "the player". Anything that changes state goes through a function that the server will own once multiplayer exists.
6. **Use engine features first:** Enhanced Input, Character Movement, AI Controllers, NavMesh, State Trees or Behavior Trees, Chaos Vehicles, World Partition, Data Tables, `USaveGame`, the Automation framework, Common UI and UMG.

## 2. Modules

```
CapitalWorld.uproject
Source/
  CapitalWorldSim/        <- pure rules, no actors (Runtime module)
  CapitalWorld/           <- gameplay + presentation (primary game module)
  CapitalWorld.Target.cs
  CapitalWorldEditor.Target.cs
```

Dependencies go one way: `CapitalWorld` → `CapitalWorldSim` → engine `Core`, `CoreUObject` and `Engine` (only for subsystems, Data Tables and `USaveGame`).
`CapitalWorldSim` must never include gameplay, actor or UI headers from `CapitalWorld`.

Each module keeps its automation tests in a `Tests/` folder, compiled only when `WITH_DEV_AUTOMATION_TESTS` is set.

## 3. `CapitalWorldSim` — simulation layer

| Folder | Main types | Responsibility |
|---|---|---|
| `Core/` | `FCWMoney`, `FCWOwnerId`, `FCWEntityId`, `CWLog` | Shared basic types. Money is `int64` cents. |
| `Time/` | `UCWTimeSubsystem`, `FCWGameTime` | Game clock (1 day = 36 real minutes), pausing, time speed, events for new hour, day and week. Every other system reads time from here. |
| `Economy/` | `UCWLedgerSubsystem`, `FCWAccount`, `FCWTransaction` | Bank accounts (personal and per business), cash, transactions, history, debt-limit checks. |
| `Inventory/` | `FCWItemDef` (Data Table row), `FCWItemStack`, `FCWInventory`, `UCWInventorySubsystem` | Item definitions, stock held in containers by ID, moving and using stock. |
| `Business/` | `FCWBusinessDef` (Data Table row), `FCWBusinessState`, `UCWBusinessSubsystem`, `UCWBusinessModule` | Generic business data and lifecycle. Category-specific behavior goes in small module objects such as `UCWRecipeModule` (coffee shop) and `UCWShelfRetailModule` (retail). |
| `Customers/` | `UCWDemandModel` | Demand: how many customers want what, based on time, price, reputation and difficulty. Abstract, so it works without spawned NPCs. |
| `Employees/` | `FCWEmployeeState`, `UCWEmployeeSubsystem` | Hiring, wages, schedules, skill, happiness, payroll (paid through the ledger). |
| `Property/` | `FCWPropertyDef`, `FCWPropertyState`, `UCWPropertySubsystem` | Renting, buying, rent collection, utilities, seizure for debt. |
| `Needs/` | `FCWNeedsState`, `UCWNeedsSubsystem` | Hunger and energy rules, passing out. |
| `Progression/` | `FCWChapterDef`, `UCWChapterSubsystem` | Chapters, goals, unlocks, debt limit per chapter, difficulty settings. |
| `Save/` | `UCWSaveGame`, `UCWSaveSubsystem` | Versioned save data, slots, autosave, validation and migration. |

Most simulation subsystems are `UGameInstanceSubsystem`s, so they survive level changes. Systems that need the world are `UWorldSubsystem`s.

### Simulation tick
`UCWTimeSubsystem` drives a fixed **simulation step of 1 in-game minute** (1.5 real seconds). Each step:
1. Needs update.
2. Each open business resolves abstract demand into customers. Spawned customers take a business "slot" so they aren't double-counted.
3. Employees work their tasks.
4. On the hour, day or week boundary: wages, rent, utilities and reports.

Off-screen businesses run entirely on these abstract steps. This is how the spec's "businesses keep running when you leave" works.

### Simulation levels for NPCs (later milestones)
- **Level 0, abstract:** numbers only, no actor. Far away or not loaded.
- **Level 1, simple:** an actor that moves along splines with no full AI and a cheap animation.
- **Level 2, full:** AI Controller, NavMesh and full animation. Near the player.

## 4. `CapitalWorld` — gameplay and presentation

| Folder | Main types | Responsibility |
|---|---|---|
| `Player/` | `ACWPlayerCharacter`, `ACWPlayerController`, `UCWFirstPersonBodyComponent` | Movement (walk, run, jump, crouch), first-person camera with a visible body, Enhanced Input. |
| `Interaction/` | `ICWInteractable`, `UCWInteractionComponent`, `UCWInteractableComponent` | Line trace from the camera, focus, prompt text, primary and secondary actions, hold-to-interact. |
| `Interaction/Carry/` | `UCWCarryComponent`, `UCWPlaceableComponent` | Picking up and placing objects on a **grid**, with a ghost preview and valid/invalid tint. |
| `World/` | `ACWDoor`, `ACWContainer`, `ACWDayNightController` | World objects. |
| `Business/` | `ACWBusinessSite`, `ACWRegister`, `ACWCoffeeMachine`, `ACWShelf` | Physical business objects bound to a `BusinessId`. |
| `NPC/` | `ACWNPCCharacter`, `ACWCustomerController`, `ACWEmployeeController` | AI (State Tree) built on top of simulation data. |
| `Vehicles/` | `ACWVehicle` (Chaos Vehicles), `UCWTaxiService` | Arcade handling, taxi ride or skip. |
| `UI/` | `UCWHUD`, phone apps, computer apps, menus | UMG plus Common UI. Reads simulation data and calls simulation functions, never stores authoritative state. |
| `Characters/` | `UCWCharacterAppearance` | Character creator presets (name, gender, skin, hair, outfit colors). |

## 5. Data (Data Tables)

`Content/Data/` holds: `DT_Items`, `DT_Recipes`, `DT_Furniture`, `DT_Businesses`, `DT_Properties`, `DT_Chapters`, `DT_Difficulty`, `DT_Jobs`.
Row structs are defined in `CapitalWorldSim`. Designers (the owner) can edit numbers in the editor without touching code.

## 6. World layout

- One persistent World Partition map: `Content/Maps/Havenport/L_Havenport`.
- Interiors are **Level Instances** inside it, streamed by distance. No loading screens at doors (D-011).
- `Content/Maps/Test/` holds small test maps for automation tests and Milestone 1.

## 7. Saving

- `UCWSaveGame` holds a `SaveVersion` integer and serialized structs from each subsystem. Each subsystem implements `ICWSaveable` with `Save` and `Load`.
- Loading runs migrations when the version is older, then validates the result (for example, the ledger must balance and no ID can be duplicated). Bad data is fixed where possible, or the load is refused with a clear message.
- Autosave runs at the start of each in-game day. There are several manual slots.
- Not saved: anything transient, such as spawned Level 1–2 NPCs, which are rebuilt from simulation state.

## 8. Multiplayer-readiness (not built yet)

- State-changing calls go through subsystem functions that take an `FCWOwnerId`. Later, these become server-only.
- Actors keep only display state, rebuilt from IDs.
- Write down every new piece of shared state in `ARCHITECTURE.md §8` under "Needs replication later".

**Needs replication later:** (empty)

## 9. Testing hooks

- Simulation tests run on `CapitalWorldSim` with no world: ledger maths, inventory, business day, payroll, debt limits, save round-trip.
- Gameplay tests use test maps: movement, interaction, carry and place.
- See `TESTING.md`.
