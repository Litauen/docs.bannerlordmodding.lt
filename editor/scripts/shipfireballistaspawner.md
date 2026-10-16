# ShipFireBallistaSpawner

!!! info "v1.4.8"

Spawns the fire version of a ship ballista and copies the deploy tags and the yaw limit onto it. The plain weapon is [ShipBallistaSpawner](/editor/scripts/shipballistaspawner/).

Find placed ones in the Scene panel with `s:ShipFireBallistaSpawner`. See [Filter](/editor/editor/#filter).

## Where you put it

Place `ballista_a_spawnerfireShip` or `ballista_b_spawnerfireShip` from `NavalDLC\Prefabs\spawners.xml`. The spawned name is this entity's name with the last `_` piece removed, then `_fire` appended, unless **ToBeSpawnedOverrideNameForFireVersion** is set. `ballista_a_spawnerfireShip` therefore spawns `ballista_a_fire`.

## What you set

Both spawner prefabs store the same rows, in this order.

| Panel field | What to do with it |
|---|---|
| **AddOnDeployTag** | Tag copied onto the spawned weapon. The prefab stores empty. |
| **RemoveOnDeployTag** | Tag copied onto the weapon. The prefab stores empty. |
| **DirectionRestrictionDegree** | Yaw limit in degrees, copied onto the weapon as radians. The prefab stores `90`. |
| **ToBeSpawnedOverrideName** | Base prefab name before the fire suffix. The prefab stores empty, so the entity name is used. |
| **ToBeSpawnedOverrideNameForFireVersion** | Prefab to spawn instead of the derived `_fire` name. The prefab stores empty. |

## What it does

On mission start it spawns the fire weapon and copies the deploy tags and the yaw limit. **DirectionRestrictionDegree** of `90` is a right angle of yaw.

## Where it lives

Managed script, on the ship ballista spawner. Prefabs are in `NavalDLC\Prefabs\spawners.xml`.

## Examples

No scene file stores this script.

**Attacker fire spawner.** `ballista_a_spawnerfireShip` in `NavalDLC\Prefabs\spawners.xml`. All five rows at the defaults above. Spawns `ballista_a_fire`.

**Defender fire spawner.** `ballista_b_spawnerfireShip` in the same file. The same five rows. Spawns `ballista_b_fire`.
