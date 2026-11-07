# Project State

_Last updated: 2026-11-07, in a cloud planning session._

## Where things stand
- **No Unreal project exists yet.** The repo has documentation and Git config only.
- **No code has been compiled or tested.** The planning session ran in a Linux cloud container without Unreal Engine.
- All design decisions from the owner are recorded in `GAME_DESIGN.md` and `DECISIONS.md`.

## About the owner
- Solo developer, about 10 hours per week, **no prior Unreal or game-dev experience**. Explain things simply and give exact click-by-click steps when editor work is needed.
- Has Visual Studio installed. Is installing UE 5.8, Git, Git LFS, Blender and Claude Code using `SETUP_GUIDE.md`.
- PC: RTX 3070, Ryzen 5 5500, 16 GB RAM, over 200 GB SSD.
- Wants a **simple overview after every update**. Merges PRs themselves.

## Exact next steps (Milestone 0, on the owner's PC)
1. Verify tools: `git --version`, `git lfs version`, the UE 5.8 install path, the Visual Studio version and workloads, Blender's path, `claude --version`. Record the versions below.
2. Create a feature branch, for example `feature/m0-project-setup`.
3. Create the C++ project `CapitalWorld` at the repo root (`CapitalWorld.uproject`, `Source/CapitalWorld`, `Source/CapitalWorldSim`, `Config/`), following `ARCHITECTURE.md`. Use UnrealBuildTool from the command line where possible.
4. Compile the Development Editor target. Fix errors until it is clean.
5. Add one trivial automation test (`CapitalWorld.Sim.Smoke`) and run it headless (see `TESTING.md`). Record the real result.
6. Commit, push, open a PR to `main` with a plain-English summary, and give the owner a simple overview.
7. Start Milestone 1.

## Recorded tool versions
| Tool | Version |
|---|---|
| Unreal Engine | _not installed yet_ |
| Visual Studio / MSVC | _unknown_ |
| Git / Git LFS | _not installed yet_ |
| Blender | _not installed yet_ |

## Open design questions (ask the owner when relevant)
- Story and lore: the owner will share their ideas later. Don't write final story content without them.
- Confirm the hunger and energy interpretation in `GAME_DESIGN.md §6`, and where a tired player wakes up.
- How the player earns money before the coffee shop opens: starting cash only, or a starter job.
- What exactly happens at each chapter's big event, and which chapter unlocks which business category.
- Approve or change all **PROPOSED** numbers in `GAME_DESIGN.md`.

## Known issues
- None yet. There is no code.
