# PatrolArea

!!! info "v1.4.8"

A patrol loop. The children are the stops. Hideouts sort these machines by **AreaIndex** when they place bandits.

Find placed ones in the Scene panel with `s:PatrolArea`. See [Filter](/editor/editor/#filter).

## Where you put it

Use `sp_guard_patrol` or `sp_guard_patrol_simple` from `Modules\SandBox\Prefabs\sp_editor_spawnpoints.xml`. Both are tagged `sp_guard_patrol` and are editor-only. Children are the standing points (animation points on the guard prefab). The machine starts with only the last child active.

## What you set

| Panel field | What to do with it |
|---|---|
| **AreaIndex** | Sort key. Prefab `0`. Hideouts order patrols by this number. A forest hideout copy uses `3`. |
| **PilotStandingPointTag** | Prefab `Pilot`. |
| **AmmoPickUpTag** | Prefab `ammopickup`. |
| **WaitStandingPointTag** | Prefab `Wait`. |
| **NavMeshPrefabName** | Left empty. |

## What it does

All child points start deactivated, then the last one is turned on. When an AI user is on the active point, that point turns off and the previous point turns on, so the guard walks back along the chain. A single child stays active. Hideout spawning collects these machines and sorts them by **AreaIndex**.

## Where it lives

Managed script. Prefabs in `Modules\SandBox\Prefabs\sp_editor_spawnpoints.xml`. Hideout, dungeon, town, and villa scenes place them.

## Examples

**Full guard loop.** `sp_guard_patrol`. **AreaIndex** `0`. Children are animation points.

**Short loop.** `sp_guard_patrol_simple`. Same fields.

**One loop sorted later.** `SandBox\SceneObj\bandit_forest`. A copy named `sp_guard_patrol_simple_1` sets **AreaIndex** to `3`. Other copies of the prefab in that scene leave the prefab value.
