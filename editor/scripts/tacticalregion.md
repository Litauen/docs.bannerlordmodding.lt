# TacticalRegion

!!! info "v1.4.8"

A circular region of ground: forest, difficult terrain, or an opening. Child tactical positions join it.

Find placed ones with `s:TacticalRegion`. See [Filter](/editor/editor/#filter).

## Where you put it

Use the editor-only prefab `TacticalRegion` in `Modules\SandBox\Prefabs\sp_editor_barriers.xml`. It shows `animation_stand_arrow_red` and is visible only while editing. Put `TacticalPosition` children under it when those slots belong to the region.

## What you set

The prefab stores this order. **NavMeshPrefabName** is a panel field the prefab does not write.

| Panel field | What to do with it |
|---|---|
| **IsEditorDebugRingVisible** | Editor helper while the entity is ticked. Prefab: off. |
| **radius** | Radius of the region. Prefab: `1`. |
| **tacticalRegionType** | `Forest`, `DifficultTerrain`, or `Opening`. Prefab: `Forest`. |
| **NavMeshPrefabName** | Left empty. The prefab does not write this row. |

## What it does

The region center is the entity. **radius** and **tacticalRegionType** describe the ground. After the mission starts, child entities that have `TacticalPosition` are the linked slots. **IsEditorDebugRingVisible** keeps the editor helper on while you tick this entity.

## Where it lives

Managed script. Helper: `Modules\SandBox\Prefabs\sp_editor_barriers.xml`.

## Examples

**Forest marker.** Prefab `TacticalRegion`. Radius `1`, type `Forest`, debug ring off.

**Battlefields.** `SandBoxCore\SceneObj\battle_terrain_006` and `NavalDLC\SceneObj\nord_battle_terrain_c` store the script. Village scenes under `SandBox\SceneObj` store it too.
