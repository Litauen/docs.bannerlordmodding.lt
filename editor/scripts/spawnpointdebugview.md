# SpawnPointDebugView

!!! info "v1.4.8"

An editor-only spawn-point checker. **ActivateDebugUIEditor** opens the Mission Spawn Point Debugger while the editor is on.

Find placed ones in the Scene panel with `s:SpawnPointDebugView`. See [Filter](/editor/editor/#filter).

## Where you put it

Drop the prefab `SpawnPointDebugView` from `SandBox\Prefabs\sp_editor_helpers_&_tests.xml`. It is visible only in the editor. The window warns when a scene has more than one.

## What you set

The prefab stores the script with no variables, so the checkbox starts off. Scenes that rewrite the script turn it on.

| Panel field | What to do with it |
|---|---|
| **ActivateDebugUIEditor** | On: the debugger window ticks in the editor. The prefab leaves it off. Opened scenes store `true`. |

## What it does

With the checkbox on in edit mode, the window titled Mission Spawn Point Debugger lists spawn-point, chair, passage, and navmesh problems for the scene. A second copy of the script is reported in that window.

## Where it lives

Managed script. The prefab is in `SandBox\Prefabs\sp_editor_helpers_&_tests.xml`. Scenes that place it store the checkbox on.

## Examples

**Empty prefab.** `SpawnPointDebugView`. The script is present and no variable is stored. Editor-only visibility.

**Arena.** `NavalDLC\SceneObj\arena_nord_a`. One `SpawnPointDebugView` with **ActivateDebugUIEditor** `true`.

**Village.** `SandBoxCore\SceneObj\aserai_village_j`. One `SpawnPointDebugView` with **ActivateDebugUIEditor** `true`.
