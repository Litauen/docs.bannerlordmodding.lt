# AreaMarker

!!! info "v1.4.8"

A radius and an index. The index becomes the tag `area_marker_` plus that number. Usable machines inside the radius can be grouped under that tag.

Find placed ones in the Scene panel with `s:AreaMarker`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to an empty entity at the centre of the area. **AreaRadius** is the reach, in meters. Two markers with the same **AreaIndex** share one tag.

## What you set

The spawn prefab `area_marker` stores this order.

| Panel field | What to do with it |
|---|---|
| **AreaRadius** | Meters around the entity. The prefab stores `3`. |
| **AreaIndex** | Number in the tag `area_marker_0`, `area_marker_1`, and so on. The prefab stores `0`. |
| **CheckToggle** | While checked, the editor draws helpers. Opening the entity in the editor clears it. The prefab stores `true`. |
| **NavMeshPrefabName** | Navmesh prefab. The prefab leaves it empty. |

## What it does

**Tag** is `area_marker_` plus **AreaIndex**. Anything that asks for usable machines in range uses **AreaRadius**. The name shown for a track is the entity name.

## Where it lives

Managed script. The empty prefab is `area_marker` in `SandBox\Prefabs\sp_editor_spawnpoints.xml`. Dungeon scenes place that prefab and override the index.

## Examples

**Empty marker.** `SandBox\Prefabs\sp_editor_spawnpoints.xml`, entity `area_marker`. Radius `3`, index `0`, **CheckToggle** on. No mesh.

**Dungeon cell.** `SandBox\SceneObj\empire_dungeon_a`. One `area_marker` sets **AreaIndex** to `4` and **CheckToggle** off. Another sets **AreaRadius** to `4` and **AreaIndex** to `1`. The same prefab is overridden in the other culture dungeons.
