# CLAUDE.md — Capital World

Capital World is a first-person 3D business and life simulator in Unreal Engine 5.8 (C++ plus Blueprints), set in the fictional sunny West Coast city of **Havenport**. It uses a cartoony low-poly art style. The first business is a **coffee shop**.

## Read first
1. `Documentation/PROJECT_STATE.md`: current status and the **exact next steps**
2. `Documentation/GAME_DESIGN.md`: owner decisions (DECIDED / PROPOSED / OPEN)
3. `Documentation/DECISIONS.md`: technical and design decisions, with reasons
4. `Documentation/ARCHITECTURE.md`: modules, subsystems, rules
5. `Documentation/ROADMAP.md`, `TESTING.md`, `PERFORMANCE.md`, `SETUP_GUIDE.md`

## The owner
- Solo, about 10 h per week, **beginner with no Unreal experience**. Use plain language. When something must be done in the Unreal Editor, give short numbered click-by-click steps.
- After **every update**, give a **simple overview**: what changed, what was tested and the real result, what's next, and anything they need to do.
- Keep working through milestones without asking permission for routine technical choices. Ask only about design choices marked OPEN, or anything that costs money.

## Hard rules
- **Git:** never commit to `main`. Use a feature branch (`feature/<milestone>-<topic>`), push it, and open a PR with a plain-English summary. **The owner merges.**
- **Never claim a test passed or code compiled unless you actually ran it.** Record real results in `TESTING.md`.
- **Budget is $0.** Use only free assets whose license allows commercial use. Record every third-party asset in `Content/ThirdParty/LICENSES.md` with its source, license and date. Never add anything that costs money. Warn the owner before Git LFS usage nears GitHub's free 10 GiB.
- **Don't write the final story or lore** without the owner's ideas. Placeholders are fine.
- Keep the docs updated (`PROJECT_STATE.md` and `ROADMAP.md` at minimum) at the end of every work session.

## Code conventions
- Follow the Unreal coding standard. Game types use the prefix `CW` (`ACWPlayerCharacter`, `UCWLedgerSubsystem`, `FCWMoney`).
- Two modules: `CapitalWorldSim` (pure rules, no actors or UI) and `CapitalWorld` (gameplay and presentation). The dependency goes `CapitalWorld` → `CapitalWorldSim` only.
- Money is `int64` cents (`FCWMoney`). Never use floats for money. Every balance change goes through `UCWLedgerSubsystem`.
- Persistent entities have `FGuid` IDs. Actors reference simulation data by ID.
- Game content and numbers live in Data Tables, with row structs in C++.
- Blueprints are for presentation and level setup. Game rules live in C++.
