# PassageUsePoint

!!! info "v1.4.8"

The spot on a door the player and AI use. **ToLocationId** is the location it opens. **IsMissionExit** ends the mission instead.

Find placed ones in the Scene panel with `s:PassageUsePoint`. See [Filter](/editor/editor/#filter).

## Where you put it

Child of a **Passage**, tagged with that machine's pilot tag (`Pilot` on the barrier prefabs). Vanilla entity name is `passage_use_point`. It uses the arrow icon and is hidden in play (`sp_common_hidden`). Use is instant.

## What you set

`_barrier_passage_exit_mission` stores the rows in this order.

| Panel field | What to do with it |
|---|---|
| **ToLocationId** | Location id this door opens (`house_1`, `center`, …). Empty when **IsMissionExit** is on. |
| **IsMissionExit** | On: using it ends the mission. The mission-exit barrier stores this on. |
| **AutoSheathWeapons** | Sheath on use. Prefab on. |
| **AutoEquipWeaponsOnUseStopped** | Prefab off. |
| **AutoWieldWeapons** | Prefab off. |
| **TranslateUser** | Prefab on. |
| **HasRecentlyBeenRechecked** | Prefab writes false. Leave it. |
| **NavMeshPrefabName** | Left empty. |
| **_useOwnPositionInsteadOfWorldPosition** | Standing-point distance switch. Default off. The barrier files do not store it. |
| **_customPlayerInteractionDistance** | Custom use distance. Default `0`. The barrier files do not store it. |

## What it does

The prompt is the location door name, or the mission-exit text when **IsMissionExit** is on. A player who cannot enter gets the locked-door message. A successful use plays the door sound. The player either changes location or the mission ends. AI can walk through when the target location allows them. A mount cannot use it.

## Where it lives

Managed script. On every passage barrier in `Modules\SandBox\Prefabs\sp_editor_barriers.xml`.

## Examples

**House.** `_barrier_house_1`. **ToLocationId** `house_1`.

**Center door.** `_barrier_passage_exit_mission`. **ToLocationId** `center`, **IsMissionExit** off.

**Leave the mission.** `_barrier_mission_exit`. **ToLocationId** empty, **IsMissionExit** on.

**Villa overrides that center door.** `StoryMode\SceneObj\villa_singular_c`. Prefab `_barrier_passage_exit_mission`. The scene sets **IsMissionExit** on and clears **ToLocationId**.
