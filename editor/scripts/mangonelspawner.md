# MangonelSpawner

!!! info "v1.4.8"

A siege-deployment marker that spawns a mangonel, and a fire mangonel, when the battle starts. The marker also turns individual ammo slots on or off.

Find placed ones with `s:MangonelSpawner`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on an empty marker, usually the prefab `mangonel_a_spawner` (large) or `mangonel_b_spawner` (small). Both prefabs also carry `TrajectoryVisualizer`. The marker is what you move in the town or castle scene. The machine itself is spawned as a child and is not saved into the scene.

Ammo slots are children of the spawned machine named `ammo_pos_a` through `ammo_pos_h`. Clearing a checkbox removes that child.

## What you set

| Panel field | What to do with it |
|---|---|
| **AddOnDeployTag** | Copied onto the spawned mangonel. Entities with this tag are shown when the engine deploys. Prefab: empty. |
| **RemoveOnDeployTag** | Copied onto the spawned mangonel. Entities with this tag are removed when the engine deploys. Prefab: empty. |
| **ammo_pos_a_enabled** | Keep the `ammo_pos_a` child. Prefab: on. |
| **ammo_pos_b_enabled** | Keep `ammo_pos_b`. Prefab: on. |
| **ammo_pos_c_enabled** | Keep `ammo_pos_c`. Prefab: on. |
| **ammo_pos_d_enabled** | Keep `ammo_pos_d`. Prefab: on. |
| **ammo_pos_e_enabled** | Keep `ammo_pos_e`. Prefab: on. |
| **ammo_pos_f_enabled** | Keep `ammo_pos_f`. Prefab: on. |
| **ammo_pos_g_enabled** | Keep `ammo_pos_g`. Prefab: on. |
| **ammo_pos_h_enabled** | Keep `ammo_pos_h`. Prefab: on. |
| **ToBeSpawnedOverrideName** | Prefab spawned for the stone engine. `mangonel_a_spawner` stores `mangonel_a_grapeshot`. `mangonel_b_spawner` stores `mangonel_b_grapeshot`. Empty means the entity name with the last `_spawner` piece removed. |
| **ToBeSpawnedOverrideNameForFireVersion** | Prefab spawned when the engine is on fire. Large: `mangonel_a_grapeshot_fire`. Small: `mangonel_b_grapeshot_fire`. Empty means the stone name plus `_fire`. |

## What it does

On mission start the marker spawns the stone prefab, and a second copy for the fire version. Tags on the marker are copied onto the spawned machine. Each `ammo_pos_*` child whose checkbox is off is deleted. **AddOnDeployTag** and **RemoveOnDeployTag** are written onto the mangonel script of that machine.

## Where it lives

`Modules\Native\Prefabs\spawners.xml`.

## Examples

**Grapeshot defaults.** `mangonel_a_spawner` spawns `mangonel_a_grapeshot` and `mangonel_a_grapeshot_fire`, all eight ammo slots on. `mangonel_b_spawner` spawns `mangonel_b_grapeshot` and `mangonel_b_grapeshot_fire`.

**Overrides cleared.** `SandBoxCore\SceneObj\empire_castle_004` places `mangonel_a_spawner` and sets both override names to empty. With those empty, the stone prefab is the marker name minus the final `_spawner` piece, and the fire prefab is that name plus `_fire`.

**Fewer ammo slots.** `SandBox\SceneObj\empire_town_f` places `mangonel_b_spawner` on the siege level and turns **ammo_pos_a_enabled** through **ammo_pos_d_enabled** off. The grapeshot override names stay on the prefab.

**Baked into a multiplayer siege.** `Native\SceneObj\mp_siege_map_006` stores two entities named `mangonel_b_spawner` with the script on the scene, **AddOnDeployTag** and **RemoveOnDeployTag** empty.

**Nord town.** `NavalDLC\SceneObj\nord_town_a` places both `mangonel_a_spawner` and `mangonel_b_spawner`. One small spawner, on the siege level, turns **ammo_pos_b_enabled**, **ammo_pos_c_enabled**, and **ammo_pos_d_enabled** off.
