# NavigationMeshDeactivator

!!! info "v1.4.8"

Names two navmesh face groups. Town and village missions read the entity tagged `navigation_mesh_deactivator` and keep people off the first group and animals off the second.

Find placed ones in the Scene panel with `s:NavigationMeshDeactivator`. See [Filter](/editor/editor/#filter).

## Where you put it

Use prefab `Navigation_Mesh_Deactivator` from `Modules\Native\Prefabs\editor_helpers_&_tests.xml`. It is editor-only, uses the pandora mesh, and already has the tag `navigation_mesh_deactivator`. One per scene. The mission finds that tag.

## What you set

| Panel field | What to do with it |
|---|---|
| **DisableFaceWithId** | Face-group id people are kept off. Code default `-1` means unused. The prefab writes `0`. Scenes usually write `1`. |
| **DisableFaceWithIdForAnimals** | Face-group id animals are kept off. Default `-1` (the prefab does not store it). A town writes `3`. |

`-1` is skipped. Any other id is a real face group in that scene.

## What it does

On a town or village mission the entity with tag `navigation_mesh_deactivator` supplies both ids. People are excluded from **DisableFaceWithId** when it is not `-1`. Animals are excluded from **DisableFaceWithIdForAnimals**.

## Where it lives

Managed script. Prefab in `Modules\Native\Prefabs\editor_helpers_&_tests.xml`. Almost every town, castle, village, and shipyard scene places that prefab.

## Examples

**Editor prefab.** `Navigation_Mesh_Deactivator`. **DisableFaceWithId** `0`. Animals id left off the file.

**Shipyard, people only.** `NavalDLC\SceneObj\vlandia_shipyard`. The prefab with **DisableFaceWithId** `1`.

**Town, people and animals.** `SandBoxCore\SceneObj\empire_town_l`. **DisableFaceWithId** `1`, **DisableFaceWithIdForAnimals** `3`.
