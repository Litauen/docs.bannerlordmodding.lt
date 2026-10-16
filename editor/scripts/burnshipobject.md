# BurnShipObject

!!! info "v1.4.8"

A usable oil barrel on the blocked-estuary ship. Holding the use point for **UseTime** seconds marks the barrel used and starts that part of the fire.

Find placed ones in the Scene panel with `s:BurnShipObject`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the barrel entity. The pilot stand must be a dynamic-object animation point. The blocked-estuary mission collects every copy under the burning ship.

## What you set

The packed storyline mesh stores this order on `gangradir_incinerate_1`. **UseTime** is the only field this script adds. The other rows are the usable-machine tags.

| Panel field | What to do with it |
|---|---|
| **UseTime** | Seconds the user must hold the point. Code default is `5`. This barrel stores `3`. |
| **AmmoPickUpTag** | Ammo stand tag. This barrel stores `ammopickup`. |
| **NavMeshPrefabName** | Navmesh prefab. This barrel leaves it empty. |
| **WaitStandingPointTag** | Wait stand tag. This barrel stores `Wait`. |
| **PilotStandingPointTag** | Pilot stand tag. This barrel stores `Pilot`. |

The use text is the combat use key. The description is **Barrel of oil**.

## What it does

The stand starts disabled for the player and is turned on for agents. While someone is using it, a timer runs. Past **UseTime**, the user is dropped, the machine disables itself, and the estuary mission treats that barrel as spent. The mission will not pick a barrel that already has a user or is already spent.

## Where it lives

Managed script. No loose scene stores it. The placement is in `NavalDLC\AssetPackages\nested_prefabs_packed.xml`, inside `storyline_mesh_holder`.

## Examples

**Storyline oil barrel.** That packed file, entity `gangradir_incinerate_1`, tag `gangradir_incinerate_1`, physics `bo_bd_barrel_a`. **UseTime** is `3`. The pilot, wait, and ammo tags are the usable-machine defaults.
