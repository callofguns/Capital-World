# Testing

**Rule:** never write that a test passed unless it was actually run. Record real results below with the date.

## How tests will run (Unreal Automation)
Tests are Unreal automation tests named `CapitalWorld.<Area>.<Name>`.
Run them headless from PowerShell. Adjust the engine path to the real install path, which is recorded in `PROJECT_STATE.md`:
```powershell
& "C:\Program Files\Epic Games\UE_5.8\Engine\Binaries\Win64\UnrealEditor-Cmd.exe" `
  "C:\Dev\Capital-World\CapitalWorld.uproject" `
  -ExecCmds="Automation RunTests CapitalWorld; Quit" `
  -unattended -nullrhi -nosplash -log -testexit="Automation Test Queue Empty"
```
Results appear in the log, under `Saved/Logs/`. This command hasn't been verified on the owner's machine yet. Correct it here once it has been run.

## Planned test areas
| Area | Example tests | Milestone |
|---|---|---|
| Sim.Smoke | Module loads | 0 |
| Player | Walk speed, jump, crouch height | 1 |
| Interaction | Trace finds interactable, prompt text, door toggles | 1 |
| Carry | Pick up, place on grid, invalid placement blocked | 1 |
| Inventory | Add, remove, stack, overflow | 1–2 |
| Economy | Cents maths, transfers, ledger balances, no negative-amount transactions | 3 |
| Business | Register, open, close, sale updates revenue and stock | 3 |
| Checkout | Typed amount must equal total | 3 |
| Time | Day length = 36 min, hour, day and week events fire | 3 |
| Needs | Hunger drains energy, pass-out rules | 4 |
| Save | Round-trip, version migration, corrupt-save handling | 4 |
| End-to-end | New game → shop → one day → save → load → verify money and stock | 4 |
| Soak | Simulate 30 in-game days; check no drift, crashes or memory growth | 5+ |

## Latest verified results
_None. No tests have been run yet._
