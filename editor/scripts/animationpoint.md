# AnimationPoint

!!! info "v1.4.8"

A standing point that plays arrive, loop, and leave actions. Pair fields start a second loop once enough neighbours are in use.

Find placed ones in the Scene panel with `s:AnimationPoint`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the stand entity, not on the prop. The entity forward is the direction the user faces. Vanilla stands are editor-only and use a stand-arrow mesh. A chair sit point uses [ChairUsePoint](/editor/scripts/chairusepoint/) instead of this script.

## What you set

`NavalDLC\Prefabs\architecture_nord.xml` stores this order on a shop stand. **PairEntity** is a public field that stand does not write. The two position fields at the bottom are on the standing-point base; that stand does not write them, and a throne sit point does.

| Panel field | What to do with it |
|---|---|
| **ArriveAction** | Action played on arrival. Empty skips it. The shop stand leaves it empty. |
| **LoopStartAction** | Action held while the point is in use. The shop stand stores `act_stand_1`. |
| **PairLoopStartAction** | Loop used once enough paired users are present. Empty means no pair loop. |
| **LeaveAction** | Action played when the user steps off. Empty skips it. |
| **GroupId** | Points with the same id are alternatives. `-1` means this point has none. |
| **RightHandItem** | Item id, or a prefab name, shown in the right hand after the arrive action. |
| **RightHandItemBone** | Bone for that item. The shop stand stores `ItemR`. |
| **LeftHandItem** | Same for the left hand. |
| **LeftHandItemBone** | Bone for the left item. The shop stand stores `ItemL`. |
| **PairEntity** | Entity whose animation points count as the pair. Empty on the shop stand. |
| **MinUserToStartInteraction** | How many paired users are required before the pair loop. Default `1`. |
| **ActivatePairs** | On: using this point turns the paired points on. The shop stand stores off. |
| **MinWaitinSeconds** | Shortest AI stay, in seconds. The shop stand stores `30`. A negative value means no timed wait. |
| **MaxWaitInSeconds** | Longest AI stay. The shop stand stores `120`. |
| **ForwardDistanceToPivotPoint** | Shifts the user along the point forward as agent scale changes. The shop stand stores `0`. |
| **SideDistanceToPivotPoint** | Same shift to the side. The shop stand stores `0`. |
| **KeepOldVisibility** | On: conversation hides the hand items and puts them back afterward. The shop stand stores off. |
| **AutoSheathWeapons** | Sheathe on use. Code default is on. The shop stand stores off. A [Chair](/editor/scripts/chair/) turns this on for its points at mission start. |
| **AutoEquipWeaponsOnUseStopped** | Equip when the user leaves. The shop stand stores off. |
| **AutoWieldWeapons** | Wield when the user leaves. The shop stand stores off. |
| **TranslateUser** | Moves the user onto the point. Stored `true`. |
| **HasRecentlyBeenRechecked** | Runtime flag. The shop stand stores `false`. |
| **_useOwnPositionInsteadOfWorldPosition** | Standing-point base. Off uses the world frame. Not written on the shop stand. |
| **_customPlayerInteractionDistance** | Extra player use distance. `0` on sit points that write it. |
| **NavMeshPrefabName** | Navmesh prefab for this point. The shop stand leaves it empty. |

## What it does

The user walks to the point, plays **ArriveAction**, then holds **LoopStartAction**. Hand items are attached after the arrive action finishes. If **PairEntity** has enough users, the point switches to **PairLoopStartAction**. **GroupId** of `0` or higher marks alternative points the parent machine can pick among. A loop the player cannot play disables the point for the player.

## Where it lives

Managed script, on the standing-point base. Shop, furniture, and spawn-point prefabs under SandBox and NavalDLC store it. Hundreds of town, village, and shipyard scenes store it too.

## Examples

**Armory clerk stand.** `NavalDLC\Prefabs\architecture_nord.xml`, a child of `shop_prop_merchantarmory_a`. **LoopStartAction** is `act_stand_1`. Arrive, leave, and pair actions are empty. **GroupId** is `-1`. Waits are `30` to `120` seconds.

**Longer idle.** `SandBox\Prefabs\sp_editor_spawnpoints.xml` stores the same `act_stand_1` loop with **MaxWaitInSeconds** `320` and **AutoWieldWeapons** on.
