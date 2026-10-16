# SiegeTowerSpawner

!!! info "v1.4.8"

Spawns a [SiegeTower](/editor/scripts/siegetower/) and tells it which wall, which side, and which path to use.

Find placed ones in the Scene panel with `s:SiegeTowerSpawner`. See [Filter](/editor/editor/#filter).

## Where you put it

Place `siege_tower_5m_spawner`, `siege_tower_9m_spawner`, or `siege_tower_12m_spawner` from `Native\Prefabs\spawners.xml`. Aim it down the path the tower should follow. **PathEntityName** has to match an entity in the scene. **TargetWallSegmentTag** has to match the wall segment.

## What you set

The spawner prefabs store only **RampRotationDegree** and **BarrierLength**. The rest are the code defaults. Scenes also store `wait_pos_ground`, `ai_barrier_l`, and `ai_barrier_r`. Those three are hidden frames, not panel rows.

| Panel field | What to do with it |
|---|---|
| **SideTag** | Side passed to the tower (`left`, `right`, `middle`). Default empty. |
| **TargetWallSegmentTag** | Wall segment the ramp should meet. Default empty. |
| **PathEntityName** | Name of the path entity. Default `Path`. |
| **SoilNavMeshID1** | Soil face. Default `-1`. The scenes opened below leave this unset. |
| **SoilNavMeshID2** | Second soil face. Default `-1`. |
| **DitchNavMeshID1** | Ditch face. Default `-1`. |
| **DitchNavMeshID2** | Second ditch face. Default `-1`. |
| **GroundToSoilNavMeshID1** | Ground-to-soil face. Default `-1`. |
| **GroundToSoilNavMeshID2** | Second ground-to-soil face. Default `-1`. |
| **SoilGenericNavMeshID** | Generic soil face. Default `-1`. |
| **GroundGenericNavMeshID** | Generic ground face. Default `-1`. |
| **AddOnDeployTag** | Copied onto the tower. Default empty. |
| **RemoveOnDeployTag** | Copied onto the tower. Default empty. |
| **RampRotationDegree** | Ramp pitch, in degrees. Prefabs store `80`. Changing it rotates the ghost child `ramp`. |
| **BarrierLength** | Length of the AI barriers. 5 m prefab stores `2.5`. 9 m and 12 m prefabs store `4`. |
| **SpeedModifierFactor** | Multiplies the tower's min and max speed. Default `1`. The editor clamps a change to `0.8`–`1.2`. |
| **EnableAutoGhostMovement** | On: the editor ghost walks **PathEntityName**. Default off. |
| **BarrierTagToRemove** | Tag removed when the tower deploys. Default empty. Scenes set a barrier entity tag. |
| **ToBeSpawnedOverrideName** | Prefab to spawn instead of the name derived from the entity. Default empty. Empty means the entity name with the last `_piece` removed, so `siege_tower_9m_spawner` spawns `siege_tower_9m`. |
| **ToBeSpawnedOverrideNameForFireVersion** | Fire-version prefab. Default empty. This spawner does not spawn a fire version. |

## What it does

On mission start it spawns the tower and copies the path name, the wall tag, the side, the navmesh ids, the ramp rotation, and the barrier tag. **SpeedModifierFactor** scales both **MinSpeed** and **MaxSpeed** on the tower. Changing **PathEntityName** retargets the editor ghost.

## Where it lives

Managed script, on the siege-spawner base. The three prefabs are in `Native\Prefabs\spawners.xml`. Castle and town siege scenes place them on the `siege` level and on the matching wall level.

## Examples

**Prefab defaults.** `siege_tower_5m_spawner` stores ramp `80` and barrier length `2.5`. `siege_tower_9m_spawner` and `siege_tower_12m_spawner` store ramp `80` and barrier length `4`.

**5 m, middle wall.** `SandBoxCore\SceneObj\battania_town_e`. A `siege_tower_5m_spawner` sets **SideTag** `middle`, **TargetWallSegmentTag** `left_wall_l3`, **PathEntityName** `tower_path_l3_left`, **BarrierLength** `2.82`, and **BarrierTagToRemove** `tower_barrier_l3_left`. Levels `level_3` and `siege`.

**9 m, right wall.** The same scene places `siege_tower_9m_spawner` with **SideTag** `right`, wall `right_tower_wall_l2`, path `tower_path_l2_right`, ramp `126.46`, barrier length `2.94`, tag `tower_barrier_l2_right`. Levels `level_2` and `siege`.

**9 m on a nord castle.** `NavalDLC\SceneObj\nord_castle_a`. One `siege_tower_9m_spawner` uses **SideTag** `left`, wall `left_wall_l3`, path `tower_path_l3_left`, ramp `97.44`, tag `tower_barrier_l3_left`. Another uses **SideTag** `right`, wall `right_wall_l2`, path `tower_path_l2_right`, ramp `78.86`, barrier length `4.25`.

**12 m.** `SandBoxCore\SceneObj\battania_castle_a`. A `siege_tower_12m_spawner` sets **SideTag** `right`, wall `right_wall_lvl3`, path `siege_tower_path_right_lvl3`, ramp `87.21`, tag `tower_barrier_lvl3`. Levels `level_3` and `siege`.
