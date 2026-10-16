# MapAtmosphereProbe

!!! info "v1.4.8"

A campaign-map sphere that blends rain, temperature, and a color-grade texture by distance from the probe.

Find placed ones with `s:MapAtmosphereProbe`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on an entity tagged `atmosphere_probe`. The campaign map only reads probes with that tag. The prefab `atmosphere_probe` already has it.

In the editor, **visualizeRadius** draws two spheres when probes are not hidden. **hideAllProbes** hides those spheres on every probe at once. Selecting a probe draws the spheres brighter.

**minRadius** cannot go below `0.1` or above **maxRadius**. **maxRadius** cannot go below **minRadius**.

## What you set

| Panel field | What to do with it |
|---|---|
| **visualizeRadius** | Draw the inner and outer spheres in the editor. Default and prefab: on. |
| **hideAllProbes** | Hide every probe's spheres. Default on. The prefab does not store this row. |
| **minRadius** | Inner distance. Inside this, the probe is at full strength. Script default `1`. The prefab stores `10`. |
| **maxRadius** | Outer distance. Past this, the probe has no strength. Script default `2`. The prefab stores `20`. |
| **rainDensity** | Rain amount the map reads as humidity. Script default `0`. |
| **temperature** | Temperature the map reads. Prefab and default: `0`. |
| **atmosphereType** | Text on the panel. The map probe reader does not use it. |
| **colorGrade** | Color-grade texture name copied onto the atmosphere at this probe. The prefab does not store a value. |

The prefab also stores a row named `humitidy` at `0`. That is not **rainDensity**. Humidity stays `0` unless you set **rainDensity**.

## What it does

For each tagged probe the map records position, **rainDensity** (spread `5`), **temperature** (spread `5`), **minRadius** as the full-strength distance, **maxRadius** as the zero-strength distance, and **colorGrade** as the texture. Strength falls off smoothly between the two radii.

## Where it lives

`Modules\Native\Prefabs\scene_prop_prefabs.xml`, entity `atmosphere_probe`.

## Examples

No scene file stores this script.

**Prefab probe.** `atmosphere_probe`, tag `atmosphere_probe`. **visualizeRadius** on, **minRadius** `10`, **maxRadius** `20`, **temperature** `0`. **rainDensity** and **colorGrade** are not in the file, so they stay empty / `0`. The unused `humitidy` row is `0`.
