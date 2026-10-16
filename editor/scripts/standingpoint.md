# StandingPoint

!!! info "v1.4.8"

A place an agent stands to use a machine. The entity forward is the direction they face. Tags on the entity (`attacker`, `defender`, `move`, `Pilot`) decide who may use it and what the parent machine calls it.

Find placed ones in the Scene panel with `s:StandingPoint`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the stand, usually an editor-only `icon_man`. Siege engines, gates, and checkpoints use it. A chair or a shop idle uses a more specific stand script.

## What you set

`Native\SceneObj\mp_siege_map_004` stores this order on a gate stand. The checkpoint prefab in `SandBox\Prefabs\sp_editor_spawnpoints.xml` skips the two position fields.

| Panel field | What to do with it |
|---|---|
| **AutoSheathWeapons** | Sheathe on use. Code default is on. Gate stands and the checkpoint store off. |
| **AutoEquipWeaponsOnUseStopped** | Equip when the user leaves. Those stands store off. |
| **AutoWieldWeapons** | Wield when the user leaves. Those stands store off. |
| **TranslateUser** | Moves the user onto the point. Stored `true`. |
| **HasRecentlyBeenRechecked** | Runtime flag. Those stands store `false`. |
| **_useOwnPositionInsteadOfWorldPosition** | On: the user's frame comes from the point's own position. The siege stands store off. The checkpoint prefab does not write this row. |
| **_customPlayerInteractionDistance** | Extra player use distance. The siege stands store `0`. The checkpoint prefab does not write this row. |
| **NavMeshPrefabName** | Navmesh prefab. Those stands leave it empty. |

An entity tagged `attacker` or `defender` is limited to that side. Tagging both, or neither, leaves the side open.

## What it does

The user walks onto the point and the parent machine reads the tag to decide the action text. **TranslateUser** puts them on the point. **AutoSheathWeapons** sheaths while they are there.

## Where it lives

Managed script. Siege prefabs under `Native\Prefabs` store hundreds of stands. Five scene files also store the script on a baked machine: three multiplayer siege maps and two StoryMode training fields.

## Examples

**Checkpoint pilot.** `SandBox\Prefabs\sp_editor_spawnpoints.xml`, a child tagged `Pilot` and `sp_checkpoint`. Sheath, equip, and wield are off. **TranslateUser** is on. The two position rows are absent.

**Gate stands.** `Native\SceneObj\mp_siege_map_004`, on the inner gate (`OwningTeam` Defenders). Several `icon_man` stands store sheath off, **TranslateUser** on, own-position off, and interaction distance `0`.
