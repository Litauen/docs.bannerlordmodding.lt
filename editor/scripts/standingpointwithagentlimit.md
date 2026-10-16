# StandingPointWithAgentLimit

!!! info "v1.4.8"

A [StandingPoint](/editor/scripts/standingpoint/) that stays closed until a caller adds the agents who may use it. The script adds no fields of its own.

Find placed ones in the Scene panel with `s:StandingPointWithAgentLimit`. See [Filter](/editor/editor/#filter).

## Where you put it

The only vanilla copy is `stand_position` under `flag_pole_big` in `Native\Prefabs\props_other.xml`.

## What you set

That stand stores this order.

| Panel field | What to do with it |
|---|---|
| **AutoSheathWeapons** | Sheathe on use. This stand stores on. |
| **TranslateUser** | Moves the user onto the point. This stand stores on. |
| **DescriptionMessage** | Focus description. This stand leaves it empty. |
| **ActionMessage** | Focus action. This stand leaves it empty. |

The stand does not write **AutoEquipWeaponsOnUseStopped**, **AutoWieldWeapons**, **HasRecentlyBeenRechecked**, the two position fields, or **NavMeshPrefabName**.

## What it does

Every agent is rejected until something calls the add-agent method on this point. Clearing the list closes it again. No other class in the game source calls that method, so the vanilla stand stays closed.

## Where it lives

Managed script. One prefab child in `Native\Prefabs\props_other.xml`. No scene file stores the script.

## Examples

**Flag pole.** `flag_pole_big` / `stand_position`. Sheath on, **TranslateUser** on, description and action empty.
