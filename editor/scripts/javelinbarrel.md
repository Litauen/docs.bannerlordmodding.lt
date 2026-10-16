# JavelinBarrel

!!! info "v1.4.8"

A shipboard ammo barrel. The user picks up arrows, bolts, sling stones, cartridges, throwing axes, throwing knives, or javelins.

Find placed ones with `s:JavelinBarrel`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the barrel entity, with a standing point tagged `ammopickup` as a child. Vanilla barrels sit on the deck upgrade holder inside packed ship prefabs, on entities such as `deck_javelin_2`. There is no loose prefab under `Prefabs`.

## What you set

The packed barrel stores these rows. This script adds no fields of its own.

| Panel field | What to do with it |
|---|---|
| **AmmoPickUpTag** | Standing points that count as the pickup. Stored value: `ammopickup`. |
| **NavMeshPrefabName** | Navmesh for this entity. Stored empty. |
| **WaitStandingPointTag** | Waiting points. Stored value: `Wait`. |
| **PilotStandingPointTag** | Pilot point. Stored value: `Pilot`. A barrel does not use a pilot. |

## What it does

A soldier using the pickup point refills a weapon of one of these classes: arrow, bolt, sling stone, cartridge, throwing axe, throwing knife, javelin. The use text is the pick-up prompt. The sound is `event:/mission/combat/pickup_arrows`. The barrel description is the ammunition-barrels line.

## Where it lives

`Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`. Fifty copies are under `deck_upgrade_holder`, four under `deck_upgtade_holder`, and two under `deck_upgrade_assets_nested`.

## Examples

**Deck javelin crate.** In `deck_upgrade_holder`, entity `deck_javelin_2`, tag `deck_arrow_and_javelin_crates_lvl2`. The script stores **AmmoPickUpTag** `ammopickup`, empty **NavMeshPrefabName**, **WaitStandingPointTag** `Wait`, and **PilotStandingPointTag** `Pilot`. Child `icon_man` is a `StandingPointWithWeaponRequirement`, visible only while editing.

No scene file stores this script. Ships pick it up through the packed deck holder.
