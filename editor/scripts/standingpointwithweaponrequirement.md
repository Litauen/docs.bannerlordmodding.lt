# StandingPointWithWeaponRequirement

!!! info "v1.4.8"

A [StandingPoint](/editor/scripts/standingpoint/) that opens only for a weapon the parent machine assigns at mission start. The weapon id is not a field on this script.

Find placed ones in the Scene panel with `s:StandingPointWithWeaponRequirement`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on an `icon_man` stand tagged for the job: `ammopickup` to receive a weapon, or a load / defender tag to require one. Siege engines, ladders, ammo barrels, and rock piles use it.

## What you set

`SandBox\Prefabs\sp_editor_spawnpoints.xml` stores this order on a hideout stand. Rock-pile ammo stands use the same rows. The constructor forces **AutoSheathWeapons** off.

| Panel field | What to do with it |
|---|---|
| **AutoSheathWeapons** | Sheathe on use. The stands store off. |
| **AutoEquipWeaponsOnUseStopped** | Equip on leave. The stands store off. |
| **AutoWieldWeapons** | Wield on leave. The stands store off. |
| **TranslateUser** | Moves the user onto the point. The stands store on. |
| **HasRecentlyBeenRechecked** | Runtime flag. The stands store off. |
| **NavMeshPrefabName** | Navmesh prefab. The stands leave it empty. |

Baked scenes also write **_useOwnPositionInsteadOfWorldPosition** off. A ship stand in the sea conversation scene writes interaction distance `0` as well.

## What it does

The parent fills the requirement after the mission starts.

- A ladder's defender stand requires `push_fork`. The ammo stand gives `push_fork`.
- A ranged engine's ammo stand gives the engine's missile. A load stand requires that missile's weapon class.
- An ammo barrel requires the barrel's weapon classes.
- A rock pile's ammo stands use this script beside the throw stand.

Until that assignment exists, the point stays closed. An agent holding the required weapon, or missing the given weapon, is the one who can use it.

## Where it lives

Managed script. Siege prefabs under `Native\Prefabs` store it on engines, ladders, and piles. Castle, town, and ship scenes bake those stands.

## Examples

**Rock-pile ammo.** `Native\Prefabs\props_siege.xml`, `throwable_rock_pile` / `icon_man` tagged `ammopickup`. Sheath off, **TranslateUser** on.

**Baked pile.** `SandBoxCore\SceneObj\battania_castle_a`, the same `icon_man` on `throwable_rock_pile`, with own-position off.

**Ship stand.** `NavalDLC\SceneObj\conversation_scene_sea_multi_agent`, an `icon_man` with sheath off, own-position off, and interaction distance `0`.

**Ladder fork.** Ladder prefabs store **PushForkItemID** `push_fork`. At mission start the defender stand requires that item and the ammo stand gives it.
