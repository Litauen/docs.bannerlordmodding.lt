# CheckpointUsePoint

!!! info "v1.4.8"

A use point that saves a checkpoint id when the player stands on it. The return position is a child tagged `sp_checkpoint`.

Find placed ones in the Scene panel with `s:CheckpointUsePoint`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the machine entity. It collects standing points on itself and on its children. Add a child tagged `sp_checkpoint` for the return position. The use prompt is **Checkpoint**, action **Use**.

## What you set

| Panel field | What to do with it |
|---|---|
| **UniqueId** | Number saved when the player is on a standing point. Default `0`. |
| **PilotStandingPointTag** | Tag of the pilot standing point. Default `Pilot`. |
| **AmmoPickUpTag** | Ammo standing-point tag. Default `ammopickup`. The checkpoint does not use one. |
| **WaitStandingPointTag** | Wait-point tag. Default `Wait`. The checkpoint does not use one. |
| **NavMeshPrefabName** | Navmesh prefab for this entity. The prefab leaves it empty. |

## What it does

While the player occupies any standing point on this machine, the checkpoint mission stores **UniqueId**, unless an enemy is cautious, patrolling-cautious, or alarmed. A later visit moves the player to the `sp_checkpoint` child when the saved id matches and no `CheckpointArea` claimed that id first.

## Where it lives

Managed script on the usable-machine base. Only the prefab below stores it. No scene file contains the script.

## Examples

**Use point under the checkpoint machine.** `SandBox\Prefabs\sp_editor_spawnpoints.xml`. The parent entity `checkpoint_usable_machine` has `EventTriggeringUsableMachine`. The child `use_point` has this script and a `StandingPoint`. **UniqueId** is `0`. The four machine tags are the defaults. `use_point` is tagged `Pilot` and `sp_checkpoint`. Its child `sp_checkpoint_player` is tagged `sp_checkpoint`.
