# TrebuchetSpawner

!!! info "v1.4.8"

A siege-deployment marker that spawns a trebuchet, and a fire trebuchet, when the battle starts. The marker also turns individual ammo slots on or off.

Find placed ones with `s:TrebuchetSpawner`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on `trebuchet_a_spawner` from `Modules\Native\Prefabs\spawners.xml`. That prefab also carries `TrajectoryVisualizer`. Ammo slots are children of the spawned machine. A checkbox whose name is the child name plus `_enabled` removes that child when it is off.

## What you set

The prefab stores no variables, so every row stays at the code default. `projectile_pile` is a hidden frame. `SandBox\SceneObj\empire_town_f` stores that frame and nothing else.

| Panel field | What to do with it |
|---|---|
| **AddOnDeployTag** | Copied onto the spawned trebuchet. Entities with this tag are shown when the engine deploys. Default: empty. |
| **RemoveOnDeployTag** | Copied onto the spawned trebuchet. Entities with this tag are removed when the engine deploys. Default: empty. |
| **ammo_pos_a_enabled** | Keep the `ammo_pos_a` child. Default: on. |
| **ammo_pos_b_enabled** | Keep `ammo_pos_b`. Default: on. |
| **ammo_pos_c_enabled** | Keep `ammo_pos_c`. Default: on. |
| **ammo_pos_d_enabled** | Keep `ammo_pos_d`. Default: on. |
| **ammo_pos_e_enabled** | Keep `ammo_pos_e`. Default: on. |
| **ammo_pos_f_enabled** | Keep `ammo_pos_f`. Default: on. |
| **ammo_pos_g_enabled** | Keep `ammo_pos_g`. Default: on. |
| **ammo_pos_h_enabled** | Keep `ammo_pos_h`. Default: on. |
| **ToBeSpawnedOverrideName** | Prefab spawned for the stone engine. Empty means the entity name minus the final underscore piece. `trebuchet_a_spawner` therefore spawns `trebuchet_a`. |
| **ToBeSpawnedOverrideNameForFireVersion** | Prefab spawned when the engine is on fire. Empty means the stone name plus `_fire`, so `trebuchet_a_fire`. |

## What it does

On mission start the marker spawns the stone prefab, and a second copy for the fire version. A spawned child is deleted when the spawner has a checkbox named with that child's name plus `_enabled` and the checkbox is off. A child whose name matches a frame field on the spawner, such as `projectile_pile`, receives that frame. **AddOnDeployTag** and **RemoveOnDeployTag** are written onto the trebuchet script of the spawned machine.

## Where it lives

Managed script. `Modules\Native\Prefabs\spawners.xml`.

## Examples

**Default spawner.** `trebuchet_a_spawner`. No variables stored. Stone prefab `trebuchet_a`, fire prefab `trebuchet_a_fire`, all eight ammo slots on.

**Town siege line.** `SandBox\SceneObj\empire_town_f` places `trebuchet_a_spawner` on the siege levels and stores only the hidden `projectile_pile` frame.

`SandBoxCore` towns and `NavalDLC\SceneObj\nord_castle_b` store the script as well.
