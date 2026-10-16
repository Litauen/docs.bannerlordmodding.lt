# MapColorGradeManager

!!! info "v1.4.8"

Drives the campaign map's time of day, season tint, and ground color grade from one entity.

Find placed ones with `s:MapColorGradeManager`. See [Filter](/editor/editor/#filter).

## Where you put it

Put one on the map scene. Both campaign maps use an entity named `color_grade_manager`. The map screen looks that script up when the map loads and applies the atmosphere.

## What you set

| Panel field | What to do with it |
|---|---|
| **ColorGradeEnabled** | Blend the ground color grade in the editor. Both maps store on. Turning it off clears the blend. |
| **AtmosphereSimulationEnabled** | In the editor, advance **TimeOfDay** every tick and reload the atmosphere. Both maps store off. |
| **TimeOfDay** | Hour, clamped to `0`–`23.99`. The sandbox map stores `22`. The naval map stores `6`. Changing it reloads the atmosphere. |
| **SeasonTimeFactor** | Season blend, `0` to `1`. Both maps store `0`. Changing it reloads the atmosphere. |

## What it does

**TimeOfDay** and **SeasonTimeFactor** are pushed into the map atmosphere. The season value tints the terrain between a greener mix and a barer mix.

The color grade comes from a grid texture, not from these fields. The grid name and the texture names are in `Modules\Native\ModuleData\worldmap_color_grades.xml`:

- default / high camera (above `400`): `worldmap_colorgrade_stratosphere`
- night, hour after `22` or before `2`: `worldmap_colorgrade_night`
- rain near the ground (camera below `50`): `worldmap_colorgrade_rain`
- region grades: desert `20`, steppe `40`, central Europe `60`, highlands `80`, Normandy `100`, north `120`, Mediterranean `140`

While playing, the hour and the season are taken from the scene each tick. The saved **TimeOfDay** is the editor starting hour.

## Where it lives

The grid file is `Modules\Native\ModuleData\worldmap_color_grades.xml`. The grid texture name in that file is `worldmap_colorgrade_grid`.

## Examples

**Sandbox campaign map.** `SandBox\SceneObj\Main_map`, entity `color_grade_manager`. Color grade on, atmosphere simulation off, **TimeOfDay** `22`, **SeasonTimeFactor** `0`.

**Naval campaign map.** `NavalDLC\SceneObj\Main_map`, same entity name. Color grade on, simulation off, **TimeOfDay** `6`, **SeasonTimeFactor** `0`.
