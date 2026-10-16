# DynamicPatrolAreaParent

!!! info "v1.4.8"

Parent of a patrol path. Its children are the patrol points. The checkpoint mission can disable a point on that path after a corpse was saved there.

Find placed ones in the Scene panel with `s:DynamicPatrolAreaParent`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an empty parent. Tag it `dynamic_patrol_area_tag`. Under it, add a child per stop. The villa path uses a child named `patrol_point_1` with a `UsablePlace`, and under that a `patrol_point` with a `PatrolPoint`.

## What you set

| Panel field | What to do with it |
|---|---|
| **DrawPath** | Stored flag. Code default on. Vanilla scenes store it off. The managed editor tick is empty, so this page does not claim it draws a line. |
| **UniqueId** | Stored integer. Code default `-1`. Scenes that were opened store `-1`. The checkpoint reload does not read this id. |
| **NavMeshPrefabName** | Navmesh prefab. Vanilla leaves it empty. |

## What it does

The stealth patrol logic and the agent handler collect every entity with this script. After a checkpoint reload, the checkpoint mission walks each parent's children. If the first grandchild's tags match a saved corpse's spawn tags, that grandchild's `PatrolPoint` is disabled.

## Where it lives

Managed script. The editor prefab is `dynamic_patrol_area` in `SandBox\Prefabs\sp_editor_spawnpoints.xml`. That prefab stores **DrawPath** off and an empty navmesh name, and it omits **UniqueId**.

## Examples

**Villa stealth paths.** `StoryMode\SceneObj\villa_singular_c`. The scene has 44 of these parents. The two that were opened are tagged `dynamic_patrol_area_tag`, with **DrawPath** off and **UniqueId** `-1`. The first path's child `patrol_point_1` holds a `UsablePlace`. Its child `patrol_point` is a `PatrolPoint` with **SpawnGroupTag** `stealth_agent`, **Index** `0`, **WaitDuration** `1`.

**Dungeon paths.** `SandBox\SceneObj\empire_dungeon_stealth`. 20 parents. The one that was opened has the same tag and the same three values.
