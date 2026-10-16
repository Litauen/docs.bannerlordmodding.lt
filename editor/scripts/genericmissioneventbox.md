# GenericMissionEventBox

!!! info "v1.4.8"

A volume that fires every `GenericMissionEventScript` on the same entity when a matching agent walks inside.

Find placed ones with `s:GenericMissionEventBox`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on a `volume_box` entity, scaled to the room or the trigger. Add one or more `GenericMissionEventScript` components on that same entity. The box only checks agents whose visual entity has a tag listed in **ActivatorAgentTags**.

The helper prefab is editor-only.

## What you set

| Panel field | What to do with it |
|---|---|
| **ActivatorAgentTags** | Tags that may trip the box. The check is a substring of the agent's visual tags. The helper prefab leaves this empty, which trips nothing. |
| **NavMeshPrefabName** | Navmesh for this entity. The helper leaves it empty. |

## What it does

Each tick, if any script on the entity is still enabled, the box looks for an agent inside the volume whose visual tags contain **ActivatorAgentTags**. The first match fires every enabled `GenericMissionEventScript` on the entity: event id plus parameter.

Mission listeners that read those events include:

- `show_quick_information_event` — parameter is a tag, then a space, then a text id. The text is shown once, then that script is disabled.
- `activate_agent_ai` and `deactivate_agent_ai`
- `open_inventory_with_given_items`
- `start_eavesdropping`
- `sabotage_objective_used_event`

## Where it lives

`Modules\SandBox\Prefabs\sp_editor_helpers_&_tests.xml`, entity `generic_mission_event_box`.

## Examples

No scene file stores this script.

**Empty helper.** `generic_mission_event_box` has a `volume_box` mesh, **ActivatorAgentTags** empty, and one `GenericMissionEventScript` with empty **EventId** and **Parameter**. Scale the box and fill both scripts before it does anything.
