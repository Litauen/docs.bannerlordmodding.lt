# UsablePlace

!!! info "v1.4.8"

A chair, throne, or other spot civilians and patrols use. The prompt comes from the pilot standing point, not from this script.

Find placed ones with `s:UsablePlace`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the furniture root. `bd_chair_king_a` in `Modules\SandBox\Prefabs\sp_props_furniture.xml` is a throne. The sit point is a child (`chair_sit_position_front`, tagged `sp_throne`) and carries the animation script. Tag the pilot child with **PilotStandingPointTag**.

## What you set

`bd_chair_king_a` stores this order.

| Panel field | What to do with it |
|---|---|
| **PilotStandingPointTag** | Tag of the use point. Prefab: `Pilot`. |
| **AmmoPickUpTag** | Prefab: `ammopickup`. |
| **WaitStandingPointTag** | Prefab: `Wait`. |
| **NavMeshPrefabName** | Left empty. |

## What it does

The description and the use text are whatever the pilot standing point already has. Patrols and spawn groups look up this script on the parent of their standing point and send the agent there.

## Where it lives

Managed script. Furniture prefabs in `Modules\SandBox\Prefabs\sp_props_furniture.xml`. Also `merchant_sets.xml`, `sp_props_merchandise.xml`, and `sp_editor_paired_spawnpoints.xml`.

## Examples

**Throne.** `bd_chair_king_a`. The four rows are `Pilot`, `ammopickup`, `Wait`, and an empty navmesh. The sit child is `chair_sit_position_front`.

**Interiors and arenas.** `SandBox\SceneObj\arena_empire_a` and `SandBox\SceneObj\arabian_house_new_a_interior_a_house` store the script. `NavalDLC\Prefabs\shipyards.xml` stores it on shipyard props.
