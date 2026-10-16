# MultiplayerSceneValidator

!!! info "v1.4.8"

Editor button that selects every entity in the scene whose script is a mission object. Saving the scene does not change those entities.

Find placed ones in the Scene panel with `s:MultiplayerSceneValidator`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on one empty entity in a multiplayer scene. One is enough. The prefab of the same name is `MultiplayerSceneValidator` in `Modules\Native\Prefabs\editor_helpers_&_tests.xml`. The skirmish map below does not use that prefab; it adds the script to an empty object.

## What you set

| Panel field | What to do with it |
|---|---|
| **SelectFaultyEntities** | Button. Selects every entity that carries a mission-object script (siege machines, usable points, and the same family). |

The prefab stores the script with no variables. The button is still on the panel.

## What it does

Press the button and the current entity is deselected, then each matching entity is selected. On scene save the same list is walked and left as it is.

## Where it lives

Managed script. Prefab in `Modules\Native\Prefabs\editor_helpers_&_tests.xml`. Also on multiplayer battle, sergeant, and skirmish scenes under `Modules\Native\SceneObj`.

## Examples

**Empty helper prefab.** `editor_helpers_&_tests.xml`, entity `MultiplayerSceneValidator`. No variables.

**Skirmish map.** `Native\SceneObj\mp_skirmish_map_013`. An empty object at about `524, 223, 18` has the script and no variables.
