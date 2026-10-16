# ChairUsePoint

!!! info "v1.4.8"

The sit point on a [Chair](/editor/scripts/chair/). It plays the sit, idle, and stand clips, and can randomly lean, drink, or eat.

Find placed ones in the Scene panel with `s:ChairUsePoint`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the sit child, not on the chair mesh. Face the arrow the way the sitter should face. Vanilla points are editor-only and use `animation_stand_arrow_blue`.

## What you set

`nord_throne` stores this order on `chair_sit_position_front`. Pair, hand, and wait fields are the [AnimationPoint](/editor/scripts/animationpoint/) rows.

| Panel field | What to do with it |
|---|---|
| **NearTable** | On: the sitter may lean on the table. The throne stores off. |
| **NearTableLoopAction** | Lean loop. Empty on the throne. |
| **NearTablePairLoopAction** | Lean loop used with a partner. Empty on the throne. |
| **Drink** | On: the sitter may drink. The throne stores off. |
| **DrinkLoopAction** | Drink loop. Empty on the throne. |
| **DrinkPairLoopAction** | Drink loop used with a partner. Empty on the throne. |
| **DrinkRightHandItem** | Item in the right hand while drinking. Empty on the throne. |
| **DrinkLeftHandItem** | Item in the left hand while drinking. Empty on the throne. |
| **Eat** | On: the sitter may eat. The throne stores off. |
| **EatLoopAction** | Eat loop. Empty on the throne. |
| **EatPairLoopAction** | Eat loop used with a partner. Empty on the throne. |
| **EatRightHandItem** | Item in the right hand while eating. Empty on the throne. |
| **EatLeftHandItem** | Item in the left hand while eating. Empty on the throne. |
| **ArriveAction** | Sit-down clip. The throne stores `act_throne_sit_down_from_front1`. |
| **LoopStartAction** | Idle while seated. The throne stores `act_throne_sit_idle1`. |
| **PairLoopStartAction** | Paired idle. Empty on the throne. |
| **LeaveAction** | Stand-up clip. The throne stores `act_throne_stand_up_to_front1`. |
| **GroupId** | Shared id for other seats on the same chair. The throne stores `-1`. |
| **RightHandItem** | Item for the plain idle. Empty on the throne. |
| **RightHandItemBone** | Bone for that item. The throne stores `ItemR`. |
| **LeftHandItem** | Left-hand item for the plain idle. Empty on the throne. |
| **LeftHandItemBone** | Bone for the left item. The throne stores `ItemL`. |
| **MinUserToStartInteraction** | Paired users needed before the pair loop. The throne stores `1`. |
| **ActivatePairs** | On: sitting here enables the paired points. The throne stores off. |
| **MinWaitinSeconds** | Shortest AI stay, in seconds. The throne stores `50`. |
| **MaxWaitInSeconds** | Longest AI stay. The throne stores `320`. |
| **ForwardDistanceToPivotPoint** | Forward shift as agent scale changes. The throne stores `0`. |
| **SideDistanceToPivotPoint** | Side shift. The throne stores `0`. |
| **KeepOldVisibility** | Restore hand items after conversation. The throne stores off. |
| **AutoSheathWeapons** | The throne stores off. The chair machine turns this on for every point at mission start. |
| **AutoEquipWeaponsOnUseStopped** | The throne stores off. |
| **AutoWieldWeapons** | The throne stores off. |
| **TranslateUser** | The throne stores `true`. |
| **HasRecentlyBeenRechecked** | The throne stores `false`. |
| **_useOwnPositionInsteadOfWorldPosition** | The throne stores `false`. |
| **_customPlayerInteractionDistance** | The throne stores `0`. |
| **NavMeshPrefabName** | The throne leaves it empty. |

## What it does

The agent sits with **ArriveAction**, idles on **LoopStartAction**, and stands with **LeaveAction**. If **NearTable**, **Drink**, or **Eat** is on and that loop is set, the sitter picks one of those at random, including the plain idle. Crouch or stand from the player leaves the seat.

## Where it lives

Managed script, on the animation-point base. Furniture prefabs are in `SandBox\Prefabs\sp_props_furniture.xml`. Town, village, and shipyard scene files store it as well.

## Examples

**Throne, front seat.** `SandBox\Prefabs\sp_props_furniture.xml`, child `chair_sit_position_front` of `nord_throne`. Sit `act_throne_sit_down_from_front1`, idle `act_throne_sit_idle1`, stand `act_throne_stand_up_to_front1`. Lean, drink, and eat are off. Waits are `50` to `320` seconds.

**Empire chair seat.** The same file, child `chair_sit_position_front_left` of `bd_chair_empire_d`. The script is a **ChairUsePoint**. Lean, drink, and eat start off, same as the throne.
