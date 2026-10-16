# ShipPullingMachine

!!! info "v1.4.8"

A usable machine that hauls another ship toward this one while the pilot holds the use key. The description is **Ship Pulling machine** and the action is **Use**. The class adds no fields of its own.

Find placed ones in the Scene panel with `s:ShipPullingMachine`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an entity that can yaw. The loose prefab is `pulling_machine` in `NavalDLC\Prefabs\ship_machines.xml`. The pilot child is tagged `Pilot`.

The ship root needs a child named `pull_point_holder`. Other ships need entities tagged `ShipPullPoint`. The pull looks along this entity's forward axis and picks the point the pilot is looking at.

## What you set

The prefab stores the usable-machine rows, in this order.

| Panel field | What to do with it |
|---|---|
| **PilotStandingPointTag** | Pilot stand tag. The prefab stores `Pilot`. |
| **AmmoPickUpTag** | Ammo stand tag. The prefab stores `ammopickup`. This machine does not use one. |
| **WaitStandingPointTag** | Wait stand tag. The prefab stores `Wait`. |
| **NavMeshPrefabName** | Navmesh prefab. The prefab leaves it empty. |

## What it does

Mouse movement yaws the entity. While the use key is held, both ships get a force of `25` along the line between this machine and the chosen `ShipPullPoint`. The point has to sit in front of the machine, and its forward axis has to face back toward it.

## Where it lives

Managed script on a usable machine. Prefab `pulling_machine` in `NavalDLC\Prefabs\ship_machines.xml`.

## Examples

No scene file stores this script.

**Editor machine.** `NavalDLC\Prefabs\ship_machines.xml`, entity `pulling_machine`. The four rows above. The pilot point is the child `chair_sit_position_front`, tagged `Pilot`.
