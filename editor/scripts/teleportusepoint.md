# TeleportUsePoint

!!! info "v1.4.8"

A player-only use point that moves the user onto another entity.

Find placed ones with `s:TeleportUsePoint`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to an empty use point. Use is instant. **TargetPointTag** must match a tag on the destination entity. AI never uses it.

## What you set

No prefab stores this script. The panel order is the class order.

| Panel field | What to do with it |
|---|---|
| **TypeOfTeleport** | `Lair`, `Door`, or `Gate`. Code default is `Lair`. |
| **TargetPointTag** | Tag of the entity the user lands on. |
| **IsLeave** | On: the prompt is the mission-exit text. |
| **AutoSheathWeapons** | Sheath on use. Code default is on. |
| **AutoEquipWeaponsOnUseStopped** | Code default is off. |
| **AutoWieldWeapons** | Code default is off. |
| **TranslateUser** | Code default is on. |
| **HasRecentlyBeenRechecked** | Leave off. |
| **_useOwnPositionInsteadOfWorldPosition** | Standing-point distance switch. Code default is off. |
| **_customPlayerInteractionDistance** | Extra use distance. Code default is `0`. The reach below is separate. |
| **NavMeshPrefabName** | Left empty unless this point imports a navmesh. |

## What it does

A player inside the reach can use it. `Lair` reaches `0.5`. `Door` and `Gate` reach `2.5`. The prompt is the lair, door, or gate text, or the mission-exit text when **IsLeave** is on. A successful use moves the player to the ground position of the entity tagged **TargetPointTag** and fades them in.

## Where it lives

Managed script. No `scene.xscene` and no prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it.

## Examples

No placed entity. The class is the only caller: using the point finds the entity tagged **TargetPointTag** and moves the player there.
