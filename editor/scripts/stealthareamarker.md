# StealthAreaMarker

!!! info "v1.4.8"

A radius inside a hideout ambush. Enemy agents who spawn inside it are the sentries for that pocket. Ally reinforcements spawn at a child of the marker.

Find placed ones in the Scene panel with `s:StealthAreaMarker`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it as a child of a [StealthAreaUsePoint](/editor/scripts/stealthareausepoint/). Give it two editor-only children: one tagged `reinforcement_ally_group_spawn_point_tag`, and one tagged `wait_point_tag`. The prefab uses a green stand arrow on the spawn child.

## What you set

`SandBox\Prefabs\sp_editor_spawnpoints.xml` stores this order. **ReinforcementAllyGroupId** is saved on the marker. The hideout logic that was opened reads the two child tags, and does not read this string.

| Panel field | What to do with it |
|---|---|
| **ReinforcementAllyGroupId** | Stored id. The prefab and the opened scenes use `reinforcement_ally_group_1`. |
| **AreaRadius** | Meters around the marker. Prefab copies store `12`, `40`, and `50`. Scenes override it. |
| **AreaIndex** | Index on the area-marker base. The opened markers store `0`. |
| **CheckToggle** | Editor helper toggle. The opened markers store off. |
| **NavMeshPrefabName** | Navmesh prefab. The opened markers leave it empty. |

## What it does

Enemy agents inside **AreaRadius** are recorded on this marker. When the player kills the last of them, the parent use point turns on. Using that point spawns allies at the child tagged `reinforcement_ally_group_spawn_point_tag`, and sends them toward the child tagged `wait_point_tag`.

## Where it lives

Managed script, on the area-marker base. The prefab entity is `stealth_area_marker` in `SandBox\Prefabs\sp_editor_spawnpoints.xml`. Hideout scenes under `SandBox\SceneObj` place it inside the use point.

## Examples

**Prefab radii.** Three `stealth_area_marker` entities in that file. They store **AreaRadius** `12`, `40`, and `50`. Group id stays `reinforcement_ally_group_1` and **CheckToggle** stays off.

**Forest hideout.** `SandBox\SceneObj\bandit_forest_sv`. Markers under the use point store radius `45` and radius `40`. Group id stays `reinforcement_ally_group_1`.

**Sea hideout.** `SandBox\SceneObj\sea_bandit_a_sv`. One marker stores radius `35`, the same group id, **AreaIndex** `0`, and **CheckToggle** off. Further markers in that scene store `90` and `60`.
