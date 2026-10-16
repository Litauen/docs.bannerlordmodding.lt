# MultiplayerFireBallistaSpawner

!!! info "v1.4.8"

Spawns the fire multiplayer ballista. Same panel as the normal ballista spawner. This one spawns only the fire prefab.

Find placed ones in the Scene panel with `s:MultiplayerFireBallistaSpawner`. See [Filter](/editor/editor/#filter).

## Where you put it

Use `ballista_a_spawnerFireMP` or `ballista_b_spawnerFireMP` from `Modules\Native\Prefabs\spawners.xml`. Empty **ToBeSpawnedOverrideName** drops the last `_piece` of the entity name, then this spawner appends `_fire` unless **ToBeSpawnedOverrideNameForFireVersion** is set. `ballista_a_spawnerFireMP` spawns `ballista_a_fire`.

## What you set

| Panel field | What to do with it |
|---|---|
| **AddOnDeployTag** | Tag added on deploy. Empty in vanilla. |
| **RemoveOnDeployTag** | Tag removed on deploy. Empty in vanilla. |
| **DirectionRestrictionDegree** | Yaw limit, in degrees. Default `90`. |
| **ToBeSpawnedOverrideName** | Base prefab name before `_fire` is added. Empty uses the entity-name rule. |
| **ToBeSpawnedOverrideNameForFireVersion** | Full fire prefab name. Empty means the base name plus `_fire`. |

The prefab also stores a `wait_pos_ground` matrix. This script has no such field.

## What it does

At mission start it spawns the fire ballista only, and copies the deploy tags and the yaw limit onto it.

## Where it lives

Managed script. Prefabs in `Modules\Native\Prefabs\spawners.xml`.

## Examples

**Fire pair of the two bodies.** `spawners.xml`: `ballista_a_spawnerFireMP` and `ballista_b_spawnerFireMP`. Tags empty.

**Fire ballista on the wall.** `Native\SceneObj\mp_siege_map_003`. A `ballista_b_spawnerFireMP` writes restriction `90` and leaves both override names empty, so it spawns `ballista_b_fire`.
