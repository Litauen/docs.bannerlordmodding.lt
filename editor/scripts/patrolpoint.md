# PatrolPoint

!!! info "v1.4.8"

One stop on a stealth patrol. Wait time, an optional loop animation, and the spawn-group tag live here.

Find placed ones in the Scene panel with `s:PatrolPoint`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the standing point, under the patrol parent. The stealth prefab in `Modules\SandBox\Prefabs\sp_editor_spawnpoints.xml` nests it under a usable place named `patrol_point_1` and up. The point itself is `patrol_point`. Tag the entity with the same id you put in **SpawnGroupTag**.

## What you set

`empire_dungeon_stealth` writes the rows in this order. Code defaults, when the prefab leaves a row off, are wait `0`, deviation `0`, index `0`, infinite wait off, speed `-1`, and an empty loop.

| Panel field | What to do with it |
|---|---|
| **WaitDuration** | Seconds to wait at this stop. The dungeon uses `5` or `3`. |
| **WaitDeviation** | Random extra wait. Dungeon `0`. |
| **Index** | Stop number. Dungeon `0` on the points opened. |
| **SpawnGroupTag** | Which agents use this chain. Stealth prefab `stealth_agent`. The dungeon also uses `prison_break_reinforcement_point`. |
| **IsInfiniteWaitPoint** | Dungeon stores false. |
| **PatrollingSpeed** | Movement speed. `-1` leaves it unset. Dungeon `-1`. |
| **LoopAction** | Animation while waiting. Empty, or a clip such as `act_npc_villager_working_while_stand`. |
| **RightHandItem** | Item prefab in the right hand. Empty in the dungeon. |
| **RightHandItemBone** | Bone for that item. `ItemR`. |
| **LeftHandItem** | Left-hand item. Empty. |
| **LeftHandItemBone** | Bone. `ItemL`. |
| **AutoSheathWeapons** | Dungeon and the stealth prefab store false. Code default is on. |
| **AutoEquipWeaponsOnUseStopped** | Off. |
| **AutoWieldWeapons** | Off. |
| **TranslateUser** | On. |
| **HasRecentlyBeenRechecked** | Prefab writes false. Leave it. |
| **_useOwnPositionInsteadOfWorldPosition** | Off. |
| **_customPlayerInteractionDistance** | `0`. |
| **NavMeshPrefabName** | Empty. |

## What it does

On use, the agent plays **LoopAction** and shows the hand items. When they leave, the action and the items clear. **SpawnGroupTag** is how stealth and prison-break missions pick which points belong to which agents.

## Where it lives

Managed script. Prefab points in `Modules\SandBox\Prefabs\sp_editor_spawnpoints.xml`. Stealth dungeons and hideouts under SandBox and NavalDLC place them.

## Examples

**Stealth chain, tag only.** `sp_editor_spawnpoints.xml`, child `patrol_point`. **SpawnGroupTag** `stealth_agent`. Wait left at `0`.

**Working guard.** `SandBox\SceneObj\empire_dungeon_stealth`. Wait `5`, tag `stealth_agent`, loop `act_npc_villager_working_while_stand`.

**Reinforcement stop.** Same scene. Wait `5`, tag `prison_break_reinforcement_point`, loop empty.

**Short wait.** Same scene. Wait `3`, tag `stealth_agent`.
