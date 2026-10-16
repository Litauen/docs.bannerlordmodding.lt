# BasicAreaIndicator

!!! info "v1.4.8"

An area marker with a name id and an icon id. The mission name-marker pass draws one for every active copy.

Find placed ones in the Scene panel with `s:BasicAreaIndicator`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to an empty entity at the spot you want marked. **AreaRadius** is the reach, in meters. **AreaIndex** becomes the tag `area_marker_` plus that number, same as [AreaMarker](/editor/scripts/areamarker/).

## What you set

The spawn prefab `basic_area_indicator` stores this order. **Type** is a public field that prefab does not write.

| Panel field | What to do with it |
|---|---|
| **IsActive** | The name-marker pass skips the point while this is off. The prefab stores `true`. |
| **NameStringId** | Game-text id for the name. Empty means no name until something sets one. |
| **Type** | Icon id. Empty becomes the common-area icon. The prefab does not write it. |
| **AreaRadius** | Meters around the entity. The prefab stores `3`. |
| **AreaIndex** | Number in the tag `area_marker_0`, `area_marker_1`, and so on. The prefab stores `0`. |
| **CheckToggle** | While checked, the editor draws helpers. Opening the entity in the editor clears it. The prefab stores `false`. |
| **NavMeshPrefabName** | Navmesh prefab. The prefab leaves it empty. |

## What it does

In a mission, each active indicator becomes a passage marker. The icon is **Type**, or `common_area` when **Type** is empty. The name is the text for **NameStringId**. A sabotage mission looks up the entity tagged `sabotage_mission_exit_area`, leaves the indicator inactive, and turns it on after every objective is used.

No vanilla scene file stores the script. The only authored placement is the spawn prefab.

## Where it lives

Managed script, on the area-marker base. Prefab `basic_area_indicator` is in `SandBox\Prefabs\sp_editor_spawnpoints.xml`.

## Examples

**Empty indicator.** That prefab. **IsActive** on, name id empty, radius `3`, index `0`, **CheckToggle** off. No mesh.

**Sabotage exit.** Not a scene override. The sabotage mission finds the entity tagged `sabotage_mission_exit_area`, reads this script, and enables it only after the objectives are done.
