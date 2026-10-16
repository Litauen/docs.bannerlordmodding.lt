# ShipOarMachine

!!! info "v1.4.8"

One crewed oar. The description is **Oar** and the action is **Use**. The standing point is closed to the player. The deck angles come from [ShipOarDeck](/editor/scripts/shipoardeck/).

Find placed ones in the Scene panel with `s:ShipOarMachine`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the oar machine. It needs a child tagged `oar_entity` (or named `oar`), a child tagged `retracted_entity` (or named `retracted_frame`), and a `hand_position` on the oar. Children must have **Does not Affect Parent's Local Bounding Box**.

Loose prefabs in `NavalDLC\Prefabs\ship_machines.xml`: `ShipOarRight_6m`, `ShipOarLeft_6m`, `ShipOarRight_4m`, `ShipOarLeft_4m`, `ShipOarRight_8m`, `ShipOarLeft_8m`.

## What you set

`ShipOarRight_6m` stores this order. **_rowLoopBackwardAction** is a field that file does not write. The prefab also stores `_requiresAgent`, which this class does not declare.

| Panel field | What to do with it |
|---|---|
| **_rowIdleAction** | Seated idle. Right prefabs: `act_usage_row_idle_right`. Left prefabs: `act_usage_row_idle_left`. |
| **_rowLoopAction** | Stroke. Right: `act_usage_row_loop_right`. Left: `act_usage_row_loop_left`. |
| **_rowLoopBackwardAction** | Reverse stroke. Code default `act_usage_row_loop_right_backward`. The prefab does not write it. |
| **_rowDeathAction** | Death while seated. Right: `act_row_death_right`. Left: `act_row_death_left`. |
| **_rowSitDownAction** | Sit. Right: `act_row_sit_down_right`. Left: `act_row_sit_down_left`. |
| **_rowStandUpAction** | Stand. Right: `act_row_stand_up_right`. Left: `act_row_stand_up_left`. |
| **PilotStandingPointTag** | Pilot stand tag. Prefabs store `Pilot`. |
| **AmmoPickUpTag** | Ammo stand tag. Prefabs store `ammopickup`. The oar does not use one. |
| **WaitStandingPointTag** | Wait stand tag. Prefabs store `Wait`. |
| **NavMeshPrefabName** | Navmesh prefab. Prefabs leave it empty. |

## What it does

A crewman sits, rows, and stands up from the pilot point. The oar mesh moves between the extracted frame and the retracted frame. A connected boarding ramp deactivates the standing point until the ramp is gone. Destroying the oar stops the stroke. The crewman stops using it when an enemy is within `5` meters.

## Where it lives

Managed script on a usable machine. Loose prefabs in `NavalDLC\Prefabs\ship_machines.xml`. Packed copies in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`.

## Examples

No scene file stores this script.

**Right 6 m oar.** `ShipOarRight_6m`. The five stored clips use the `_right` names. Child `oar` and child `retracted_frame`.

**Left 6 m oar.** `ShipOarLeft_6m`. The five stored clips use the `_left` names.

**Shorter and longer.** `ShipOarRight_4m` and `ShipOarLeft_4m` use the same side clips with a `4m` oar mesh. `ShipOarRight_8m` and `ShipOarLeft_8m` use the `8m` mesh.
