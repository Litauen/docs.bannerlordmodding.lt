# StandingPointForRangedArea

!!! info "v1.4.8"

A [StandingPoint](/editor/scripts/standingpoint/) that only a ranged weapon may use. Thrown weapons and other ranged weapons have separate score multipliers.

Find placed ones in the Scene panel with `s:StandingPointForRangedArea`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to a stand the way you would add a standing point. No vanilla prefab or scene stores it.

## What you set

No file writes these, so the numbers are the code defaults. The standing-point rows are the same as [StandingPoint](/editor/scripts/standingpoint/). On start this script forces **AutoSheathWeapons** off, leaves user frames unlocked, and locks the user position.

| Panel field | What to do with it |
|---|---|
| **ThrowingValueMultiplier** | Score added for a weapon in the throw slot. Default `5`. `0` or below disables the point for thrown weapons. |
| **RangedWeaponValueMultiplier** | Score added for any other ranged weapon in hand. Default `2`. `0` or below disables the point for those weapons. |
| **AutoSheathWeapons** | Sheathe on use. Code default on the base is on. This script sets it off when the mission starts. |
| **AutoEquipWeaponsOnUseStopped** | Equip when the user leaves. Default off. |
| **AutoWieldWeapons** | Wield when the user leaves. Default off. |
| **TranslateUser** | Moves the user onto the point. Default on. |
| **HasRecentlyBeenRechecked** | Runtime flag. Default off. |
| **_useOwnPositionInsteadOfWorldPosition** | Own position instead of the world frame. Default off. |
| **_customPlayerInteractionDistance** | Extra player use distance. Default `0`. |
| **NavMeshPrefabName** | Navmesh prefab. Default empty. |

## What it does

An agent with no ranged weapon in hand cannot use the point. A throw-slot weapon adds **ThrowingValueMultiplier**. Any other ranged weapon adds **RangedWeaponValueMultiplier**. If the weapon in hand stops qualifying, the user is dropped off the point.

## Where it lives

Managed script, on the standing-point base. No scene file and no prefab stores it. No other class reads it.

## Examples

No placed entity. The class is the only caller. It scores the ranged weapon in hand and rejects everyone else.
