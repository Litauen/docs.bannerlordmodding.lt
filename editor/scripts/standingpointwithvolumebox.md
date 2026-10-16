# StandingPointWithVolumeBox

!!! info "v1.4.8"

The throw stand on a rock pile. **VolumeBoxTag** names the volume the pile watches. The agent also has to be near the stand, and has to pass the weapon check inherited from [StandingPointWithWeaponRequirement](/editor/scripts/standingpointwithweaponrequirement/).

Find placed ones in the Scene panel with `s:StandingPointWithVolumeBox`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on `throw_pos` of `throwable_rock_pile`. A sibling `volume_box` is tagged `volumebox` and scaled over the ground in front of the pile. Ammo stands beside it use the weapon-requirement script.

## What you set

`Native\Prefabs\props_siege.xml` stores this order. The baked castle pile also writes own-position off.

| Panel field | What to do with it |
|---|---|
| **VolumeBoxTag** | Tag of the volume box this throw point belongs to. The pile stores `volumebox`. |
| **AutoSheathWeapons** | Sheathe on use. The pile stores off. The constructor also forces this off. |
| **AutoEquipWeaponsOnUseStopped** | Equip on leave. The pile stores off. |
| **AutoWieldWeapons** | Wield on leave. The pile stores off. |
| **TranslateUser** | Moves the user onto the point. The pile stores on. |
| **HasRecentlyBeenRechecked** | Runtime flag. The pile stores off. |
| **NavMeshPrefabName** | Navmesh prefab. The pile leaves it empty. |

## What it does

The stand rejects an agent who is more than 10 m from the point, or more than 2 m above or below it. The rock pile looks for a volume box whose tag matches **VolumeBoxTag** and uses that volume for the throw. The prefab volume is scaled to about `5 × 5 × 3` and sits 7 m in front of the pile. The thrown item on that pile is `boulder`.

## Where it lives

Managed script. The prefab is `throwable_rock_pile` in `Native\Prefabs\props_siege.xml`. Castle and town scenes bake that pile and store the script.

## Examples

**Prefab pile.** `throwable_rock_pile` / `throw_pos`. **VolumeBoxTag** `volumebox`. Sibling `volume_box` has that tag. Ammo `icon_man` children use the weapon-requirement script.

**Baked pile.** `SandBoxCore\SceneObj\battania_castle_a`. Entity `throwable_rock_pile` (`old_prefab_name` `throwable_rock_pile`) at about `732, 769, 63`. The throw stands keep **VolumeBoxTag** `volumebox` and set own-position off. The pile script on that entity stores **GivenItemID** `boulder` and **StartingAmmoCount** `12`.
