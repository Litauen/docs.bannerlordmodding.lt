# DynamicObjectAnimationPoint

!!! info "v1.4.8"

A standing point that plays an arrive action, a loop, and a leave action, and can put an item in each hand while the point is in use.

Find placed ones in the Scene panel with `s:DynamicObjectAnimationPoint`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the standing-point entity the agent should walk onto. Pair points that share a **GroupId** of `0` or higher are alternatives. A paired entity can be tagged `alternative`.

## What you set

The only vanilla copy stores the rows in this order. Inherited standing-point rows are included because that file stores them.

| Panel field | What to do with it |
|---|---|
| **ForwardDistanceToPivotPoint** | Meters forward from the point to the pivot the user faces. Default `0`. |
| **LeftHandItemBone** | Bone for the left-hand item. The copy stores `ItemL`. |
| **LeaveAction** | Action played when the user lets go. Empty on the copy. |
| **AutoWieldWeapons** | Standing-point flag. The copy leaves it off. |
| **NavMeshPrefabName** | Navmesh prefab. Empty on the copy. |
| **ActionSpeed** | Playback speed of the actions. Default `1`. |
| **GroupId** | `0` or higher marks this point as one of a set of alternatives. Default `-1` (no set). |
| **LoopStartAction** | Action looped while the point is in use. |
| **LeftHandItem** | Item id put in the left hand while in use. Empty means none. |
| **ArriveAction** | Action played as the user arrives. Empty on the copy. |
| **AutoSheathWeapons** | Standing-point flag. The copy leaves it on. |
| **KeepOldVisibility** | On: do not hide the agent's own items when they start using the point. |
| **RightHandItem** | Item id for the right hand. |
| **HasRecentlyBeenRechecked** | Standing-point flag. The copy leaves it off. |
| **TranslateUser** | On: the user is moved onto the point. The copy leaves it on. |
| **_useOwnPositionInsteadOfWorldPosition** | On: the use frame uses the entity position instead of the world frame. Off on the copy. |
| **_customPlayerInteractionDistance** | Player use distance. `0` keeps the normal distance. |
| **SideDistanceToPivotPoint** | Meters to the side from the point to the pivot. Default `0`. |
| **AutoEquipWeaponsOnUseStopped** | Re-equip when use stops. Off on the copy. |
| **RightHandItemBone** | Bone for the right-hand item. The copy stores `ItemR`. |

## What it does

On use, the agent plays **ArriveAction** if it is set, then loops **LoopStartAction**. **RightHandItem** and **LeftHandItem** are attached to the bones while the point is active. Letting go plays **LeaveAction** when that name is set. **GroupId** of `-1` means this point has no alternative.

## Where it lives

Managed script on the standing-point base. The only copy is in `NavalDLC\AssetPackages\nested_prefabs_packed.xml`. No loose prefab and no `scene.xscene` stores it.

## Examples

**Tinkering point on the incinerate prop.** Packed entity `use_point` under `gangradir_incinerate_1`. **LoopStartAction** is `act_agent_binds_machine_tinkering`. **ArriveAction** and **LeaveAction** are empty. Both item ids are empty. **GroupId** is `-1`. **ActionSpeed** is `1`. Hand bones are `ItemR` and `ItemL`. The packed file contains three of these scripts. The second opened copy uses the same loop action.
