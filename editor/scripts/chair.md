# Chair

!!! info "v1.4.8"

A sittable prop. The sit points are child [ChairUsePoint](/editor/scripts/chairusepoint/) entities. **ChairType** is the only field this script adds. It picks the use text.

Find placed ones in the Scene panel with `s:Chair`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the furniture entity. Tag the entity `npc_wait` or `npc_common` if civilians should use it. Each sit point is an editor-only child with a blue stand arrow.

## What you set

`nord_throne` stores this order. `bd_chair_empire_d` leaves **ChairType** off the file, so the default `Chair` applies.

| Panel field | What to do with it |
|---|---|
| **ChairType** | `Chair`, `Log`, `Sofa`, or `Ground`. This is the name in the use text. The throne stores `Chair`. |
| **PilotStandingPointTag** | Pilot stand tag. Both chairs store `Pilot`. |
| **AmmoPickUpTag** | Ammo stand tag. Both chairs store `ammopickup`. |
| **WaitStandingPointTag** | Wait stand tag. Both chairs store `Wait`. |
| **NavMeshPrefabName** | Navmesh prefab. Both chairs leave it empty. |

## What it does

The use text is **Sit**, or **Get Up** once the player is sitting. **ChairType** changes the object name: chair, log, sofa, or ground. At mission start every stand point on the chair sheathes weapons. If the sit points share a **GroupId**, the chair sends an agent to the closest free point in that group.

## Where it lives

Managed script. Furniture prefabs are in `SandBox\Prefabs\sp_props_furniture.xml`. Town, tavern, dungeon, and shipyard scenes place them.

## Examples

**Nord throne.** `SandBox\Prefabs\sp_props_furniture.xml`, entity `nord_throne`, tags `npc_wait` and `npc_common`, mesh `nord_throne`. **ChairType** is `Chair`. The sit child is `chair_sit_position_front`.

**Empire chair.** The same file, entity `bd_chair_empire_d`, mesh `bd_chair_empire_d`, the same two tags. **ChairType** is not written, so it stays `Chair`. The sit child is `chair_sit_position_front_left`.
