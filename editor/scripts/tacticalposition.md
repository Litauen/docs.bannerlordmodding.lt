# TacticalPosition

!!! info "v1.4.8"

A formation slot: width, slope, type, and which side of the battle it belongs to.

Find placed ones with `s:TacticalPosition`. See [Filter](/editor/editor/#filter).

## Where you put it

Use the editor-only prefab `TacticalPosition` in `Modules\SandBox\Prefabs\sp_editor_barriers.xml`. It shows `animation_stand_arrow_green` and is visible only while editing. Child entities with the same script become the linked slots. Changing **_width** rescales the entity on X.

## What you set

The barrier prefab stores this order. **NavMeshPrefabName** is a panel field that prefab does not write.

| Panel field | What to do with it |
|---|---|
| **_width** | Width. The barrier stores `1`. The editor rescales the entity to match. |
| **_slope** | Slope. The barrier stores `0`. |
| **_isInsurmountable** | Men cannot cross it. The barrier stores off. |
| **_isOuterEdge** | Outer edge of a line. The barrier stores off. |
| **_tacticalPositionType** | Slot kind. The barrier stores `Regional`. Also `HighGround`, `ChokePoint`, `Cliff`, `SpecialMissionPosition`. |
| **_tacticalRegionMembership** | Region this slot sits in. The barrier stores `Forest`. Also `DifficultTerrain`, `Opening`. |
| **_tacticalPositionSide** | Formation side. The barrier stores `BehaviorSideNotSet`. |
| **NavMeshPrefabName** | Left empty. The barrier does not write this row. |

## What it does

On mission start the position and facing come from the entity. **_width** is the frontage. **_tacticalPositionType** and **_tacticalRegionMembership** tell the battle AI what the ground is. **_tacticalPositionSide** assigns it to a side. Child tactical positions are the linked slots. Wall scripts look for children tagged `middle_pos`, `wait_pos`, and `attacker_wait_pos`.

## Where it lives

Managed script. Helper: `Modules\SandBox\Prefabs\sp_editor_barriers.xml`.

## Examples

**Generic marker.** Prefab `TacticalPosition`. Width `1`, type `Regional`, membership `Forest`, side `BehaviorSideNotSet`.

**Caravan slot.** `CaravanTacticalPosition` in `Modules\Native\Prefabs\editor_helpers_&_tests.xml` stores the short names `width` `20`, `slope` `0`, `isInsurmountable` on, `isOuterEdge` off, `tacticalPositionType` `SpecialMissionPosition`, `tacticalRegionMembership` `DifficultTerrain`.

**Field battles.** `SandBoxCore\SceneObj\battle_terrain_006` and `NavalDLC\SceneObj\nord_battle_terrain_c` store the script. Village scenes under `SandBox\SceneObj` store it too.
