# PirateSpawnPoint

!!! info "v1.4.8"

A sea zone on the campaign map where one pirate clan patrols. **ClanStringId** is the clan. **Radius** is the zone size.

Find placed ones in the Scene panel with `s:PirateSpawnPoint`. See [Filter](/editor/editor/#filter).

## Where you put it

Empty entity on the campaign map, over water. The position is the center. There is no prefab. `NavalDLC\SceneObj\Main_map` places them by hand.

## What you set

| Panel field | What to do with it |
|---|---|
| **ClanStringId** | Clan id. The campaign map uses `southern_pirates` and `northern_pirates`. |
| **ToggleDebugRadius** | On: the editor draws a red circle of **Radius** even when the entity is not selected. Off: the circle shows only while the entity is selected. Map stores false. |
| **Radius** | Zone radius. Code default `10`. The map uses `30` or `50`. |

## What it does

When the campaign map loads, each point is stored under its clan id, with the entity position and **Radius**. Pirate parties of that clan use those zones as patrol areas. The count of points is how many zones that clan has.

## Where it lives

Managed script. Only `Modules\NavalDLC\SceneObj\Main_map\scene.xscene`.

## Examples

**Southern zone, wide.** `Main_map`, entity `southern_pirates` at about `296, 365`. **ClanStringId** `southern_pirates`, **Radius** `50`.

**Southern zone, tighter.** Same scene, another `southern_pirates` at about `296, 365` with **Radius** `30`.

**Northern zone.** Entity `northern_pirates` at about `409, 739`. **Radius** `30`. Another at about `322, 796` uses **Radius** `50`.
