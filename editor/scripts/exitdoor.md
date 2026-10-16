# ExitDoor

!!! info "v1.4.8"

A use point that ends the mission. The prompt is **Leave Area**.

Find placed ones in the Scene panel with `s:ExitDoor`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the door entity. Give it a standing-point child the player can use. The script adds no fields of its own.

## What you set

The training-field door stores the usable-machine defaults.

| Panel field | What to do with it |
|---|---|
| **PilotStandingPointTag** | Pilot tag. Default `Pilot`. |
| **AmmoPickUpTag** | Ammo tag. Default `ammopickup`. The door does not use one. |
| **WaitStandingPointTag** | Wait tag. Default `Wait`. |
| **NavMeshPrefabName** | Navmesh prefab. The door leaves it empty. |

## What it does

A user on a standing point plays the middle pickup action (`act_pickup_middle_begin`, or the left-stance version). When that action reaches `act_pickup_middle_end`, the user lets go and the mission ends. If the pickup cannot start, the user is released and the mission stays open. The focus description is empty.

## Where it lives

Managed script on the usable-machine base. One scene stores it.

## Examples

**Training-field garden door.** `StoryMode\SceneObj\training_field_2`. Entity `exit_gate`, mesh `empire_garden_door_e_close`, at about `326, 385`. The four rows are the defaults. The child is `icon_man` (editor-only icon) with a `StandingPoint`. That child is not tagged `Pilot`.
