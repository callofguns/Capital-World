# Performance

Target PC (the owner's): RTX 3070, Ryzen 5 5500, 16 GB RAM.

## Budgets (initial targets, not measured yet)
| Metric | Target |
|---|---|
| Frame rate | 60 FPS at 1080p on High |
| Game thread | ≤ 10 ms |
| Spawned full-AI NPCs (Level 2) | ≤ 40 near the player |
| Simple NPCs (Level 1) | ≤ 150 |
| Simulation step (all businesses, abstract) | ≤ 1 ms per step |
| Editor RAM | Keep the test maps light; 16 GB is tight |

## Approach
- The low-poly art style keeps triangle and texture costs low. Use shared material atlases and instanced static meshes for repeated props.
- Lumen is allowed, but test with the cheaper GI options too. UE 5.8 reportedly adds a lighter Lumen mode; check it once the engine is installed.
- NPC simulation levels: see `ARCHITECTURE.md §3`.
- Profile with Unreal Insights and `stat unit` at the end of every milestone. Record the numbers here.

## Measurements
_None yet._
