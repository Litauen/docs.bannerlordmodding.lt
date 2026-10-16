# GenericMissionEventScript

!!! info "v1.4.8"

A named event stored on an entity. A `GenericMissionEventBox` on the same entity fires it when a matching agent enters. Other mission code can fire the same pair of strings.

Find placed ones with `s:GenericMissionEventScript`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the same entity as the volume or the usable that should raise the event. One entity can hold several of these scripts. The box fires every one that is still enabled.

## What you set

| Panel field | What to do with it |
|---|---|
| **EventId** | The event name a listener checks. Empty on the helper prefab. |
| **Parameter** | Extra text for that event. Empty on the helper prefab. |

The enabled flag is not a panel field. Listeners turn a script off after it has fired so it does not repeat.

## What it does

Firing sends **EventId** and **Parameter** to the mission. Listeners in the game include:

- `show_quick_information_event` — **Parameter** is a disabling tag, a space, then a text id. The text is shown, then the script is disabled.
- `activate_agent_ai` and `deactivate_agent_ai`
- `open_inventory_with_given_items`
- `start_eavesdropping` — also read directly off a usable, not only off a volume
- `sabotage_objective_used_event`

## Where it lives

`Modules\SandBox\Prefabs\sp_editor_helpers_&_tests.xml`, on `generic_mission_event_box` next to `GenericMissionEventBox`.

## Examples

No scene file stores this script.

**Empty helper.** On `generic_mission_event_box`, **EventId** and **Parameter** are both empty. Fill them to match one of the listener ids above.
