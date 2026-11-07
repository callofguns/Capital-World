# Decisions Log

Each entry records what was decided, why, and who decided it. Add new entries at the bottom.
Do not change a past decision silently: add a new entry that replaces it.

---

### D-001 — Game name, city, and module name
- **Decision:** The game is **Capital World**, the city is **Havenport**, and the Unreal project and C++ module are named `CapitalWorld`.
- **By:** Owner.

### D-002 — Engine: Unreal Engine 5.8
- **Decision:** Use the newest UE 5 release (5.8.x, released June 2026). Record the exact version in `PROJECT_STATE.md` once it is installed. Do not upgrade the engine mid-milestone.
- **Why:** The owner is installing the engine fresh, so the newest stable 5.x makes sense. Epic has announced UE6, but the spec targets UE5. We will look at UE6 only after the coffee-shop skeleton works.
- **By:** Owner chose "newest version". Claude pinned the version.

### D-003 — Development happens locally, not in the cloud
- **Decision:** The owner runs Claude Code on their own Windows PC (RTX 3070, Ryzen 5 5500, 16 GB RAM, over 200 GB free SSD).
- **Why:** The cloud sessions are Linux containers with no Unreal Engine and no GPU, so they cannot compile, run, or test the game.
- **By:** Owner.

### D-004 — Art style: low-poly cartoon
- **Decision:** The game uses a cartoony, low-poly, colorful, sunny West Coast look. This replaces the spec's "moderately realistic" style.
- **By:** Owner.

### D-005 — Characters are low-poly, not MetaHumans
- **Decision:** All characters are low-poly, matching the art style.
- **Why:** MetaHumans are photorealistic, so they clash with the style. They are also too heavy for crowds on an RTX 3070 with 16 GB RAM.
- **By:** Owner.

### D-006 — First business is a coffee shop
- **Decision:** The coffee shop is the first business. It replaces the spec's convenience store and becomes the reference implementation for the business framework.
- **By:** Owner.

### D-007 — Git workflow: main plus feature branches, owner merges (REPLACED by D-018)
- **Decision:** `main` is always stable. All work happens on feature branches and goes into `main` through pull requests. Claude writes a plain-English summary on each PR, and the **owner clicks Merge**. Claude never merges or pushes directly to `main`.
- **By:** Owner.

### D-008 — Money: $0 budget for assets, $0 for storage
- **Decision:** Asset budget is $0. Total extra spending is capped at about $10. Large files use GitHub's free Git LFS tier (currently 10 GiB storage and 10 GiB bandwidth per month on GitHub Free). The GitHub LFS budget should stay at $0 so it can never charge. If we outgrow it, we move to a **free** alternative rather than paying.
- **By:** Owner.

### D-009 — Asset sources
- **Decision:** Use, in this order of preference:
  1. Free low-poly packs whose license allows commercial use. Quaternius and Kenney publish CC0 packs; check each pack's license page at download time.
  2. Assets Claude makes with Blender, driven by Python scripts. The owner installs Blender.
  3. Unreal's built-in starter content and engine-provided assets.
- Every third-party asset gets an entry in `Content/ThirdParty/LICENSES.md` with its source URL, license and date. Assets with an unclear license are not used.
- **By:** Owner set the $0 budget and allowed Blender. Claude chose the sources.

### D-010 — Data in Unreal Data Tables
- **Decision:** Game content (products, prices, wages, recipes, furniture, properties) lives in Unreal Data Tables, with row structs defined in C++.
- **By:** Owner.

### D-011 — Building interiors are part of the open world
- **Decision:** Building interiors are part of the same streamed world as the city (World Partition plus Level Instances), not separate maps. There are no loading screens at doors.
- **Why:** The neighborhood is small, so seamless entry is affordable. It also suits the "physically inside the world" vision and keeps business actors in one world for simulation. Interiors stream in and out by distance.
- **By:** Claude (the owner said "you decide").

### D-012 — Simulation is separate from presentation (two C++ modules)
- **Decision:** The game has two C++ modules:
  - **`CapitalWorldSim`** holds pure game rules: money, ledger, businesses, inventory, time, needs, chapters. It has no dependency on actors, rendering or UI, and can be tested without a world.
  - **`CapitalWorld`** holds gameplay and presentation: player, interaction, actors, AI, UI, audio. It depends on `CapitalWorldSim`, never the other way around.
- **Why:** This enforces the spec's "separate authoritative state from presentation" rule. It makes simulation tests fast and makes multiplayer possible later.
- **By:** Claude.

### D-013 — Money is stored as integer cents
- **Decision:** All money is `int64` cents, wrapped in an `FCWMoney` struct. Floats are never used for money. UI formats it for display only.
- **By:** Claude.

### D-014 — Stable IDs
- **Decision:** Every persistent entity (business, property, employee, vehicle, item stack, transaction) has a stable `FGuid`. Actors reference simulation data by ID; they don't own it.
- **By:** Claude.

### D-015 — Updates for the owner
- **Decision:** Claude keeps working through milestones without stopping for approval, but gives the owner a **simple overview after every update**.
- **By:** Owner.

### D-016 — Documentation lives in `Documentation/`
- **Decision:** Use the layout from the spec: `ROADMAP`, `ARCHITECTURE`, `PROJECT_STATE`, `GAME_DESIGN`, `TESTING`, `DECISIONS`, `PERFORMANCE` and `SETUP_GUIDE`. `CLAUDE.md` at the repo root points to them.
- **By:** Claude (the owner said "you decide").

### D-017 — C++ naming prefix `CW`
- **Decision:** Game classes and structs use the prefix `CW` after Unreal's type letter, for example `ACWPlayerCharacter`, `UCWLedgerSubsystem`, `FCWMoney`. This avoids name clashes with engine and plugin types.
- **By:** Claude.

### D-018 — Push directly to `main` after every update (replaces D-007)
- **Decision:** Claude commits and pushes straight to `main` after every update. No feature branches or pull requests are required.
- **Safeguard:** only push code that compiles and passes its tests. If something is broken, say so in the commit message and the overview.
- **By:** Owner.
