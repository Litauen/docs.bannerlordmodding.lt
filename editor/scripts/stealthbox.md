# StealthBox

!!! info "v1.4.8"

A scaled box that hides an agent from a sentry's view. Crouching agents inside count. **_coversStandingAgents** also covers agents who are standing.

Find placed ones in the Scene panel with `s:StealthBox`. See [Filter](/editor/editor/#filter).

## Where you put it

Place `stealth_box` from `Native\Prefabs\editor_barriers.xml`. The mesh is `editor_cube` and the physics shape is `bo_editor_cube` with the body flag `stealth_box`. Scale the entity to the cover. The mesh is removed when the mission starts, so the player does not see the cube.

## What you set

The visible field is the cover flag. The prefab does not write it, so it stays off. The prefab does store `NavMeshPrefabName` empty. That name is not a field on this script.

| Panel field | What to do with it |
|---|---|
| **_coversStandingAgents** | On: standing agents inside the box are covered. Off: a covered agent is crouching. Default off. The prefab does not write the row. |

## What it does

An agent whose position is inside the scaled box can count as covered. A weapon in the off hand that has to be held up keeps them visible. Sentries skip the view check for a covered agent unless that agent is already on top of them.

## Where it lives

Managed script. Prefab `stealth_box` is in `Native\Prefabs\editor_barriers.xml`. No scene file stores the script. One packed Naval prefab includes an entity of the same name.

## Examples

**Editor cube.** `stealth_box` in `editor_barriers.xml`. Shape `bo_editor_cube`, mesh `editor_cube`. The script stores only the unused navmesh name.

**Packed cover.** `NavalDLC\AssetPackages\nested_prefabs_packed.xml`, prefab file `storyline_mesh_holder`, entity `stealth_box`. Scale about `5.54 × 10.49 × 2.8`. The script is present and stores no variables.
