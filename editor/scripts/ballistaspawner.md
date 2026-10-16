# BallistaSpawner

!!! info "v1.4.8"

Spawns a [Ballista](/editor/scripts/ballista/) when the siege engine deploys, and a fire version beside it. The scene entity is the spawner, not the weapon.

Find placed ones in the Scene panel with `s:BallistaSpawner`. See [Filter](/editor/editor/#filter).

## Where you put it

Place `ballista_a_spawner` or `ballista_b_spawner` from `Native\Prefabs\spawners.xml`. Those prefabs also carry **TrajectoryVisualizer**. Put the spawner where the weapon should stand, and turn it to face the shot.

## What you set

The spawner prefab stores no variables, so these are the code defaults. A scene writes only the rows it changes.

| Panel field | What to do with it |
|---|---|
| **AddOnDeployTag** | Tag copied onto the spawned ballista. Entities with that tag are kept when it deploys. Default empty. |
| **RemoveOnDeployTag** | Tag copied onto the ballista. Entities with that tag are removed when it deploys. Default empty. |
| **DirectionRestrictionDegree** | Yaw limit in degrees, copied onto the ballista as radians. Default `90`. |
| **ToBeSpawnedOverrideName** | Prefab to spawn instead of the default weapon. Default empty. |
| **ToBeSpawnedOverrideNameForFireVersion** | Prefab for the fire version. Default empty. |

## What it does

On mission start the spawner creates the ballista and its fire version, then copies the three fields above. **DirectionRestrictionDegree** of `90` is a right angle of yaw. Cover meshes in a town use **AddOnDeployTag** so the cover stays with that weapon.

## Where it lives

Managed script, on the siege-spawner base. Prefabs `ballista_a_spawner` and `ballista_b_spawner` are in `Native\Prefabs\spawners.xml`. Town and castle siege scenes place them.

## Examples

**Bare spawner.** `Native\Prefabs\spawners.xml`. Both `ballista_a_spawner` and `ballista_b_spawner` have the script and **TrajectoryVisualizer**, and no variables.

**Defender cover.** `SandBox\SceneObj\sturgia_castle_siege_001`. One `ballista_b_spawner` sets **AddOnDeployTag** to `ballista_cover_defender_l1_a`. Another sets it to `ballista_cover_defender_l3_a`. The other fields stay at the defaults.
