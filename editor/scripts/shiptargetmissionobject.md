# ShipTargetMissionObject

!!! info "v1.4.8"

A point on the ship that siege weapons can aim at. The score rises with crew on deck and falls as the hull is hurt. A sinking ship is marked as no threat. The class adds no fields of its own.

Find placed ones in the Scene panel with `s:ShipTargetMissionObject`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to a small entity on the ship. The packed prefab is `ship_target_point`. The target position is this entity. The aim box is one meter around that position on each axis.

## What you set

The only stored row is the mission-object navmesh field.

| Panel field | What to do with it |
|---|---|
| **NavMeshPrefabName** | Navmesh prefab. The packed point and the conversation scene leave it empty. |

## What it does

The target is flagged as a moving ship. Its value starts at `500` and is scaled by how full the main deck is and by how much health the hull has lost. A deck that is almost empty scores `0`. The side is the ship's side. The velocity is the ship's velocity at this point.

## Where it lives

Managed script. Packed prefab `ship_target_point` in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`.

## Examples

**Conversation longship.** `NavalDLC\SceneObj\conversation_scene_sea_multi_agent`. Entity `ship_target_point` under `longship_ship_nested`. **NavMeshPrefabName** empty.

**Packed point.** Prefab `ship_target_point`, entity `ship_target_point`. The same empty navmesh row.
