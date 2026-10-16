# MultiplayerBallistaSpawner

!!! info "v1.4.8"

Spawns the non-fire multiplayer ballista when the mission starts. The entity name picks the prefab.

Find placed ones in the Scene panel with `s:MultiplayerBallistaSpawner`. See [Filter](/editor/editor/#filter).

## Where you put it

Drop a spawner prefab from `Modules\Native\Prefabs\spawners.xml`: `ballista_a_spawnerMP` or `ballista_b_spawnerMP`. The spawned machine is a child. Tags on the spawner are copied onto it.

With **ToBeSpawnedOverrideName** empty, the prefab name is this entity's name minus the last `_piece`. `ballista_a_spawnerMP` spawns `ballista_a`. This spawner does not spawn the fire machine.

## What you set

`mp_siege_map_003` writes the visible rows in this order. The prefab leaves the two tags empty and does not write the last three, so they stay at the defaults below.

| Panel field | What to do with it |
|---|---|
| **AddOnDeployTag** | Tag added on the ballista when it deploys. Empty in vanilla. |
| **RemoveOnDeployTag** | Tag removed on deploy. Empty in vanilla. |
| **DirectionRestrictionDegree** | Yaw limit, in degrees. Default `90`. |
| **ToBeSpawnedOverrideName** | Prefab to spawn. Empty uses the entity-name rule above. |
| **ToBeSpawnedOverrideNameForFireVersion** | Fire prefab name. Empty would mean the normal name plus `_fire`. This spawner does not spawn that one. |

The prefab file also stores a `wait_pos_ground` matrix. This script has no such field.

## What it does

At mission start it spawns the ballista prefab as a child and copies the deploy tags and the yaw limit onto it.

## Where it lives

Managed script. Prefabs are in `Modules\Native\Prefabs\spawners.xml`. Placements are multiplayer siege scenes under `Modules\Native\SceneObj`.

## Examples

**Both ballista bodies.** `spawners.xml`: `ballista_a_spawnerMP` and `ballista_b_spawnerMP`. Tags empty.

**Wall ballista, restriction left at 90.** `Native\SceneObj\mp_siege_map_003`. A `ballista_b_spawnerMP` writes all five rows, restriction `90`, both override names empty.
