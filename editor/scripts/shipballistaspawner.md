# ShipBallistaSpawner

!!! info "v1.4.8"

Spawns a ship ballista when the mission starts and copies the deploy tags and the yaw limit onto it. The fire weapon is [ShipFireBallistaSpawner](/editor/scripts/shipfireballistaspawner/).

Find placed ones in the Scene panel with `s:ShipBallistaSpawner`. See [Filter](/editor/editor/#filter).

## Where you put it

Place `ballista_a_spawnerShip` or `ballista_b_spawnerShip` from `NavalDLC\Prefabs\spawners.xml`. The spawned prefab name is this entity's name with the last `_` piece removed, unless **ToBeSpawnedOverrideName** is set. `ballista_a_spawnerShip` therefore spawns `ballista_a`.

## What you set

Both spawner prefabs store the same rows, in this order.

| Panel field | What to do with it |
|---|---|
| **AddOnDeployTag** | Tag copied onto the spawned ballista. Entities with that tag are kept when it deploys. The prefab stores empty. |
| **RemoveOnDeployTag** | Tag copied onto the ballista. Entities with that tag are removed when it deploys. The prefab stores empty. |
| **DirectionRestrictionDegree** | Yaw limit in degrees, copied onto the ballista as radians. The prefab stores `90`. |
| **ToBeSpawnedOverrideName** | Prefab to spawn instead of the name derived from this entity. The prefab stores empty. |
| **ToBeSpawnedOverrideNameForFireVersion** | Prefab name used when a fire version is spawned. The prefab stores empty. This spawner does not spawn a fire version. |

## What it does

On mission start it spawns the weapon and copies the three deploy and yaw fields. The spawned weapon gets a ship ballista crew. **DirectionRestrictionDegree** of `90` is a right angle of yaw.

## Where it lives

Managed script, on the ballista spawner. Prefabs are in `NavalDLC\Prefabs\spawners.xml`.

## Examples

No scene file stores this script.

**Attacker spawner.** `ballista_a_spawnerShip` in `NavalDLC\Prefabs\spawners.xml`. All five rows at the defaults above. Spawns `ballista_a`.

**Defender spawner.** `ballista_b_spawnerShip` in the same file. The same five rows. Spawns `ballista_b`.
