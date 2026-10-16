# EventTriggeringUsableMachine

!!! info "v1.4.8"

A use point that fires every generic mission event script on the same entity while someone stands on it. The prompt text comes from two game-text ids.

Find placed ones in the Scene panel with `s:EventTriggeringUsableMachine`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the machine entity, with a standing-point child. Put one or more `GenericMissionEventScript` components on that same entity. Those scripts supply **EventId** and **Parameter**. Sabotage missions look the machine up by the entity tag `sabotage_objective`.

## What you set

| Panel field | What to do with it |
|---|---|
| **ActivatorAgentTags** | Stored string. The prefab leaves it empty. The use check does not read it. |
| **ActionTextId** | Game-text id shown as the action. Empty means an empty action. |
| **DescriptionTextId** | Game-text id shown as the description. |
| **PilotStandingPointTag** | Pilot standing-point tag. Default `Pilot`. |
| **AmmoPickUpTag** | Ammo tag. Default `ammopickup`. |
| **WaitStandingPointTag** | Wait tag. Default `Wait`. |
| **NavMeshPrefabName** | Navmesh prefab. The prefab leaves it empty. |

## What it does

While any standing point has a user, each `GenericMissionEventScript` on this entity that is not disabled fires its event id and parameter. The prompt is the key plus the text from **ActionTextId**. The focus description is the text from **DescriptionTextId**.

Sabotage listens for event id `sabotage_objective_used_event`. The parameter is the entity tag, then a space, then a description text id. Eavesdropping collects every machine of this type in the scene and starts when its event fires.

## Where it lives

Managed script on the usable-machine base. The only vanilla copy is the prefab below. No scene file contains the script, and no scene references that prefab name.

## Examples

**Empty checkpoint machine.** `SandBox\Prefabs\sp_editor_spawnpoints.xml`, entity `checkpoint_usable_machine`. This script is on the parent. **ActivatorAgentTags**, **ActionTextId**, and **DescriptionTextId** are empty. The four machine rows are the defaults. The child `use_point` is a separate `CheckpointUsePoint`. The parent is not tagged `sabotage_objective`.

The sabotage and eavesdrop missions were opened: they search the live scene for this script by tag or by type. No official scene file places one.
