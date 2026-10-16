# WallSegment

!!! info "v1.4.8"

A castle wall or gatehouse section. **SideTag** picks the defense side. The navmesh ids are the faces swapped when the section is breached.

Find placed ones with `s:WallSegment`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the wall mesh entity. `aserai_castle_gatehouse_a_l3` in `Modules\Native\Prefabs\castle_aserai_l3.xml` is the current field list. Children tagged `solid_child` and `broken_child` are the intact and breached visuals. Slots under those are tagged `middle_pos`, `wait_pos`, and `attacker_wait_pos`.

## What you set

The gatehouse stores this order. `-1` means that face id is unused.

| Panel field | What to do with it |
|---|---|
| **_properGroundOutsideNavmeshID** | Outside ground while the wall stands. Gatehouse: `-1`. |
| **_properGroundInsideNavmeshID** | Inside ground while the wall stands. Gatehouse: `-1`. |
| **_underDebrisOutsideNavmeshID** | Outside ground under the rubble. Gatehouse: `-1`. |
| **_underDebrisInsideNavmeshID** | Inside ground under the rubble. Gatehouse: `-1`. |
| **_overDebrisOutsideNavmeshID** | Outside ground on the rubble. Gatehouse: `-1`. |
| **_overDebrisInsideNavmeshID** | Inside ground on the rubble. Gatehouse: `-1`. |
| **_underDebrisGenericNavmeshID** | Generic under-rubble faces. Gatehouse: `-1`. |
| **_overDebrisGenericNavmeshID** | Generic on-rubble faces. Gatehouse: `-1`. |
| **_onSolidWallGenericNavmeshID** | Faces on the intact wall. Gatehouse: `-1`. |
| **SideTag** | `left`, `middle`, or `right`. Empty leaves the side unset. Gatehouse: empty. |
| **NavMeshPrefabName** | Left empty on the gatehouse. |

`aserai_castle_wall_a_l1` still stores older row names (`OnSolidWallNavmeshID1`, `OnSolidWallNavmeshID2`, and the connection ids) plus **SideTag**. Those older names are not the current fields.

## What it does

**SideTag** `left`, `middle`, or `right` sets the defense side. On a breach the solid child is hidden, the broken child is shown, and each pair of face ids is separated so troops walk the rubble navmesh instead of the standing wall.

## Where it lives

Managed script. Castle wall prefabs under `Modules\Native\Prefabs\castle_*` and `Modules\NavalDLC\Prefabs\castle_nord_l1.xml` through `castle_nord_l3.xml`.

## Examples

**Current field list, unused ids.** `aserai_castle_gatehouse_a_l3`. Every navmesh id `-1`, **SideTag** empty.

**Same list in a siege scene.** `Native\SceneObj\mp_siege_map_010`. Entity `aserai_castle_wall_a_l2`. Every navmesh id `-1`, **SideTag** empty.

**Older rows.** `aserai_castle_wall_a_l1` stores `OnSolidWallNavmeshID1` `31` and the other old connection ids, with **SideTag** empty.
