# Roadmap

Milestones are built in order. Each one ends with tests run on the owner's PC, updated docs, and a simple overview for the owner.

**The skeleton goal** (what the owner asked for first) is Milestones 1–4: a basic but complete loop that shows the idea of the game.

Legend: ✅ done · 🔄 in progress · ⬜ not started

---

## Milestone 0 — Foundation 🔄
- ✅ Gather the owner's decisions (art, first business, workflow, budget, story structure)
- ✅ Write design, architecture, decision and setup docs
- ✅ Add Git config for Unreal (`.gitignore`, `.gitattributes` with LFS)
- ⬜ Owner installs the tools (see `SETUP_GUIDE.md`)
- ⬜ Create the `CapitalWorld` C++ project (UE 5.8) with the `CapitalWorldSim` and `CapitalWorld` modules
- ⬜ First compile on the owner's PC; record the exact engine and compiler versions
- ⬜ Run an empty automation test to prove the test pipeline works

## Milestone 1 — First-person foundation ⬜
Tasks, in order:
1. Player character: walk, run, jump and crouch, using Enhanced Input
2. First-person camera with a visible body (placeholder low-poly body)
3. Interaction framework: `ICWInteractable`, focus trace and an on-screen prompt
4. Doors (open and close) as the first interactable
5. Carry: pick up a small object and drop it
6. Placement: place furniture on a grid with a ghost preview and valid/invalid tint
7. Personal inventory: a few slots, with a pickup and drop UI
8. Basic HUD: crosshair, prompt, clock, money (placeholder)
9. Test map `L_TestRange` with ramps, stairs, doors and props
10. Automation tests: movement, interaction trace, carry and place, inventory add and remove

## Milestone 2 — Coffee shop interior ⬜
- Low-poly coffee shop interior (Blender-made or CC0): counter, register, coffee machine, fridge, shelves, tables, stockroom
- Entrance doors, staff door, stockroom storage containers
- Item Data Table (beans, milk, cups, muffins, …) and a stock inventory per container
- Coffee machine interaction: make a drink from an order
- Furniture placement inside the shop on the grid

## Milestone 3 — First working business ⬜
- `UCWTimeSubsystem` (1 day = 36 min), day and night lighting
- `UCWLedgerSubsystem`: accounts, transactions, cents, history
- Registering a business and owning a property (simple, before full property system)
- Prices, opening hours, open and closed state
- Customers (simple): walk in, queue, order, wait, pay, leave
- **Checkout:** the register shows the total, and the player types the amount to confirm
- Revenue, ingredient cost, daily report on the shop computer
- Business keeps running off-screen through abstract demand

## Milestone 4 — Player life (skeleton complete) ⬜
- Apartment rental, bed (sleep), fridge (eat)
- Hunger and energy rules, passing out, hospital
- Phone v1: clock, bank balance, ordering supplies, text chat with reply choices
- Taxi v1: call, ride along or skip
- **Starter job v1 at the supermarket** (NPC-owned): clock in, run the register, restock shelves, get paid per hour
- Character creator v1: name, gender, presets
- Save and load: slots, autosave, versioned data
- Chapter 1 shell: goals list and debt warning at $0
- **End-to-end test:** new game → rent → open shop → run a day → save → load → verify

## Milestone 5 — NPCs and employees ⬜
- Customer AI with simulation levels (abstract, simple, full)
- Hiring baristas: wages, schedules, skill, happiness, payroll
- Employees making drinks and running the register
- Pedestrians

## Milestone 6 — City foundation ⬜
- First Havenport neighborhood (World Partition), roads, buildings in three tiers
- Traffic framework, arcade driving, parking
- Weather framework

## Milestone 7 — Property, banking and story ⬜
- Property listings, buying and leasing, utilities
- Loans, interest, debt limits plus grace period plus property seizure
- Chapter system with the owner's story; end-of-chapter events
- Easy and Hard difficulty tables
- Phone haggling with vendors

## Milestone 8 — Expansion ⬜
- More business categories (retail first, using the same framework)
- Suppliers, delivery vehicles, logistics
- New districts unlocked by chapters
- Sandbox mode

## Milestone 9 — Polish ⬜
- Art pass, audio, animation, UI polish, performance profiling, long-run regression tests

## Milestone 10 — Multiplayer (future) ⬜
- Networking review, server-authoritative economy, shared ownership
