# SiegeLadderSpawner

!!! info "v1.4.8"

Spawns a [SiegeLadder](/editor/scripts/siegeladder/) and tells it which wall, which side, and which navmesh to use. The scene entity is the spawner.

Find placed ones in the Scene panel with `s:SiegeLadderSpawner`. See [Filter](/editor/editor/#filter).

## Where you put it

Place `siege_ladder_6m_spawner`, `siege_ladder_7m_spawner`, `siege_ladder_8m_spawner`, `siege_ladder_11m_spawner`, or `siege_ladder_14m_spawner` from `Native\Prefabs\spawners.xml`. Multiplayer copies are `siege_ladder_*_mp_spawner` in that file.

The spawned prefab name is this entity's name with the last `_` piece removed, unless **ToBeSpawnedOverrideName** is set. `siege_ladder_7m_spawner` therefore spawns `siege_ladder_7m`. A fire version would append `_fire`. These spawners leave the fire name empty and do not spawn one.

Aim the spawner at the wall. The ghost frames (`fork_holder`, `initial_wait_pos`, `use_push`, `stand_position_wall_push`, `distance_holder`, `stand_position_ground_wait`) are hidden from the panel. The multiplayer prefab and siege scenes still store them.

## What you set

The campaign spawner prefab writes only **UpperStateRotationDegree**. The multiplayer prefab stores the visible rows after the ghost frames, in this order.

| Panel field | What to do with it |
|---|---|
| **SideTag** | Side passed to the ladder (`left`, `right`, or another wall tag). Default empty. `aserai_castle_a` stores `left`. |
| **TargetWallSegmentTag** | Wall segment the ladder should reach. Default empty. `aserai_castle_a` stores `left_wall_lvl1`. |
| **OnWallNavMeshId** | Navmesh id on the wall. Default `-1`. Give each ladder its own id. The editor warns when the id is `-1`, `0`, or ends in `1`, and when two ladders share an id. |
| **AddOnDeployTag** | Copied onto the ladder. Default empty. |
| **RemoveOnDeployTag** | Copied onto the ladder. Default empty. |
| **UpperStateRotationDegree** | Ladder angle against the wall, in degrees. The spawner prefab stores `-19`. A value above about `-11.5` degrees is clamped back to that limit. `aserai_castle_a` stores `-23.730`. |
| **DownStateRotationDegree** | Ladder angle on the ground, in degrees. Default `90`. |
| **TacticalPositionWidth** | Width written onto the `initial_wait_pos` tactical position. Default `1`. `aserai_castle_a` stores `4`. |
| **BarrierTagToRemove** | Barrier removed when the ladder is up. Default empty. Scenes use a side-specific tag such as `left_ladder_barrier_a`. |
| **IndestructibleMerlonsTag** | Merlons that stay. Default empty. `aserai_castle_a` stores `merlon_solid_left`. |
| **ToBeSpawnedOverrideName** | Prefab to spawn instead of the name derived from this entity. Default empty. |
| **ToBeSpawnedOverrideNameForFireVersion** | Fire-version prefab. Default empty. |

## What it does

On mission start it spawns the ladder and copies the side, the wall tag, the navmesh id, both angles, the barrier tag, and the merlon tag. **TacticalPositionWidth** is written onto the ladder's `initial_wait_pos` child. Ghost child frames are copied onto the matching children of the spawned ladder.

## Where it lives

Managed script, on the siege spawner. Prefabs in `Native\Prefabs\spawners.xml`. Siege scenes under `SandBoxCore\SceneObj` and `NavalDLC\SceneObj` place them.

## Examples

**Campaign prefab, one row.** `siege_ladder_7m_spawner` in `Native\Prefabs\spawners.xml`. Only **UpperStateRotationDegree** `-19`. The `6m`, `8m`, `11m`, and `14m` campaign spawners match that.

**Multiplayer prefab, full visible rows.** `siege_ladder_11m_mp_spawner` in the same file. Side, wall, deploy tags, and merlon tag empty. **OnWallNavMeshId** `-1`. **UpperStateRotationDegree** `-19`. **DownStateRotationDegree** `90`. **TacticalPositionWidth** `1`. The ghost frames are stored above those rows.

**Left wall in an Aserai castle.** `SandBoxCore\SceneObj\aserai_castle_a`. Prefab `siege_ladder_7m_spawner` at about `530.7, 523.4, 52.2`. **SideTag** `left`, **TargetWallSegmentTag** `left_wall_lvl1`, **OnWallNavMeshId** `203`, **UpperStateRotationDegree** `-23.730`, **TacticalPositionWidth** `4`, **BarrierTagToRemove** `left_ladder_barrier_a`, **IndestructibleMerlonsTag** `merlon_solid_left`. Levels `level_1` and `siege`.

**Right wall in a Nord town.** `NavalDLC\SceneObj\nord_town_d`. Prefab `siege_ladder_7m_spawner` at about `1088.6, 1326.7, 293.2`. **SideTag** `right`, **TargetWallSegmentTag** `right_wall_l1`, **OnWallNavMeshId** `303`, **UpperStateRotationDegree** `-42.240`, **DownStateRotationDegree** `91.250`, **TacticalPositionWidth** `5`, **BarrierTagToRemove** `right_ladder_barrier_b`, **IndestructibleMerlonsTag** `merlon_solid_right`.
