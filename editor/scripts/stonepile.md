# StonePile

!!! info "v1.4.8"

A defender boulder pile. Crew pick up the item on the ammo point and throw from the throwing points.

Find placed ones with `s:StonePile`. See [Filter](/editor/editor/#filter).

## Where you put it

Use `stone_pile_a_usable` or `stone_pile_l_usable` from `Modules\Native\Prefabs\props_siege.xml`. Ammo points are tagged `ammopickup`. Throwing points are tagged `throwing` and can contain a child tagged `wait_to_throw`. A child `VolumeBox` shares the throwing point's volume tag. Targets in the scene are tagged `throwing_target`.

## What you set

`stone_pile_a_usable` stores this order. It also stores `PileTypeText` and `IsWaiting`. The class does not declare those two rows.

| Panel field | What to do with it |
|---|---|
| **StartingAmmoCount** | Stones at start. Code default is `12`. The prefab does not write this row. |
| **GivenItemID** | Item the crew is handed. `stone_pile_a_usable` stores `boulder`. `stone_pile_l_usable` stores `boulder_carry`. |
| **PilotStandingPointTag** | Pilot tag. Prefab: `Pilot`. |
| **AmmoPickUpTag** | Pickup points. Prefab: `ammopickup`. |
| **WaitStandingPointTag** | Wait points. Prefab: `Wait`. |
| **NavMeshPrefabName** | Navmesh under the pile. Prefab: empty. |

## What it does

On start the pile hands `GivenItemID` to the pickup points and requires that item on the throwing points. Pickup text is Pick Up, and the pile label is Stone. Ammo starts at **StartingAmmoCount**. The mesh argument tracks how many stones are gone. Throwing points are for AI. A thrower waits on `wait_to_throw` until an enemy is in the volume.

## Where it lives

Managed script. Prefabs: `Modules\Native\Prefabs\props_siege.xml`.

## Examples

**Hand boulder.** `stone_pile_a_usable`. **GivenItemID** `boulder`.

**Hand the carry boulder.** `stone_pile_l_usable`. **GivenItemID** `boulder_carry`.

**Siege scenes.** `Native\SceneObj\mp_siege_map_005`, `SandBox\SceneObj\empire_town_f`, and `NavalDLC\SceneObj\nord_castle_b` store the script. `SandBoxCore` towns and castles store it as well.
