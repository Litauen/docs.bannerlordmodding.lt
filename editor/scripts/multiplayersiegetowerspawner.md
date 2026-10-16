# MultiplayerSiegeTowerSpawner

!!! info "v1.4.8"

Spawns a multiplayer siege tower and sends it at a wall segment. The spawned tower is forced to `15000` hit points, min speed `0.5`, and max speed `1`. **SpeedModifierFactor** is applied and then those two speeds replace it.

Find placed ones in the Scene panel with `s:MultiplayerSiegeTowerSpawner`. See [Filter](/editor/editor/#filter).

## Where you put it

Use `siege_tower_5m_spawnerMP`, `siege_tower_9m_spawnerMP`, or `siege_tower_12m_spawnerMP` from `Modules\Native\Prefabs\spawners.xml`. Empty **ToBeSpawnedOverrideName** spawns `siege_tower_5m`, `siege_tower_9m`, or `siege_tower_12m`. **PathEntityName** is the path. **TargetWallSegmentTag** and **SideTag** name the wall this tower belongs to.

## What you set

Prefab order. Navmesh ids stay `-1` unless you fill them. The editor clamps **SpeedModifierFactor** to `0.8`–`1.2`.

| Panel field | What to do with it |
|---|---|
| **SideTag** | Wall side. Prefab empty. The 9 m tower on the siege map uses `left`. |
| **TargetWallSegmentTag** | Wall segment. Prefab empty. That same tower uses `left`. |
| **PathEntityName** | Path name. Prefab `Path`. The siege map uses `tower`. |
| **SoilNavMeshID1**, **SoilNavMeshID2** | Soil navmesh ids. Prefab `-1`. |
| **DitchNavMeshID1**, **DitchNavMeshID2** | Ditch ids. Prefab `-1`. |
| **GroundToSoilNavMeshID1**, **GroundToSoilNavMeshID2** | Ground-to-soil ids. Prefab `-1`. |
| **SoilGenericNavMeshID** | Generic soil id. Prefab `-1`. |
| **GroundGenericNavMeshID** | Generic ground id. Prefab `-1`. |
| **AddOnDeployTag** | Tag added on deploy. Empty. |
| **RemoveOnDeployTag** | Tag removed on deploy. Empty. |
| **RampRotationDegree** | Ramp pitch, in degrees. Prefab `80`. The 9 m map tower uses about `73.4`. |
| **BarrierLength** | Length of the AI barrier. Prefab `2.5` on the 5 m tower, `4` on the 9 m and 12 m towers. The map tower uses about `2.05`. |
| **SpeedModifierFactor** | Written on the panel. The multiplayer tower then sets min speed `0.5` and max speed `1`. |
| **EnableAutoGhostMovement** | Editor ghost walks the path. Prefab off. |
| **BarrierTagToRemove** | Tag stripped when the tower deploys. Default empty. The prefab does not store it. |
| **ToBeSpawnedOverrideName** | Prefab override. Empty uses the entity-name rule. |
| **ToBeSpawnedOverrideNameForFireVersion** | No fire tower is spawned by this script. |

**wait_pos_ground**, **ai_barrier_l**, and **ai_barrier_r** are hidden. Move those ghosts. The 9 m map tower also writes **ai_barrier_l**.

## What it does

Spawns the tower, sets hit points to `15000` and speeds to min `0.5` / max `1`, and passes the path, wall tags, navmesh ids, ramp, and barrier tag through.

## Where it lives

Managed script. Three prefabs in `Modules\Native\Prefabs\spawners.xml`.

## Examples

**Three heights.** `spawners.xml`. Ramp `80` on all three. Barrier length `2.5` (5 m) or `4` (9 m and 12 m). Navmesh ids `-1`.

**Left wall.** `Native\SceneObj\mp_siege_map_003`. `siege_tower_9m_spawnerMP`, tag `mp_siege_objective_siege_tower`. **SideTag** and **TargetWallSegmentTag** `left`, **PathEntityName** `tower`, ramp about `73.4`, barrier about `2.05`.
