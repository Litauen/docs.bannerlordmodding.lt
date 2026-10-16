# ship_water_effects

!!! info "v1.4.8"

Wake, splash, and in-hull water for a ship. The dropdown name is `ship_water_effects`. The script picker files it under `ship_visual_only`.

Find placed ones in the Scene panel with `s:ship_water_effects`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the ship. The template entity is `ship_water_effects` in `NavalDLC\AssetPackages\nested_prefabs_packed.xml`.

Children the script looks up:

- `movement_particles` for the wake
- `splash_particles` for spray
- a sibling tagged `wetness_decals` for the wet stripe

Hull water also uses the ship's [signed_distance_field](/editor/scripts/signed_distance_field/) clip.

## What you set

Field order is the class order. The template only writes the bounding box. Packed hulls write the rest, in a shuffled order. Some hulls also store `Number Of Particle To Spawn`. This class does not declare that name.

| Panel field | What to do with it |
|---|---|
| **Water Simulation Bounding Box** | Size of the in-hull water volume. Template stores `1, 1, 1`. A cog-sized hull stores about `7.86, 24.13, 11.43`. |
| **Show Water Simulation Bounding Box** | Draws that box. Template stores off. |
| **Reset Water Simulation Bounding Box** | Button. |
| **Re-render Depth Texture** | Button. |
| **Reset In-Hull Water** | Button. |
| **Show Hull Water Debug Panel** | Debug numbers. Hulls store off. |
| **Hull Water Simulation Resolution Scale** | `one`, `half`, `quarter`, `one_eight`, or `one_sixteenth`. Class default and the hulls store `half`. |
| **Hull Water Splash Water Multiplier** | How much a splash adds inside the hull. Class default `1.75`. Hulls store about `1` to `2`. |
| **Hull Water Splash Point Initial Offset** | Where a splash starts. Class default `0.5`. |
| **Hull Water Splash Point Speed Multiplier** | Splash speed scale. Class default `1`. |
| **Ship Hull Height Type** | `Small`, `Medium`, or `Large`. Class default is `Small`. A cog-sized hull stores `Medium`. |
| **Movement Particle Height Offset** | Wake height above the waterline. Class default `0.34`. Hulls often store `0`. |
| **Splash Particle Height Offset** | Spray height. Class default `0.4`. |
| **Movement Particle Surface Distance Offset** | How far the wake sits out from the hull. Class default `0.7`. |
| **Splash Particle Surface Distance Offset** | Same for spray. Class default `0.7`. |
| **Movement Particle Type** | `Small`, `Medium`, or `Large`. Hulls store `Small` or `Medium`. |
| **Movement Particle Side Speed Vector** | Sideways push on the wake. Class default and the hulls store `0.5`. |
| **Show Movement Particles** | Draws the wake while you edit. Hulls store off, except one cog-sized hull that stores on. Mission play turns this off. |
| **Show Splash Particles** | Draws spray while you edit. Hulls store off. |
| **Show Water Balance Plane** | Debug plane. Hulls store off. |
| **Show Wetness Decal Values** | Debug for the wet stripe. Hulls store off. |
| **Force Wetness Decal To Full** | Pushes the wet stripe to full strength. Hulls store off. |

## What it does

In a mission it spawns wake particles from `movement_particles` and spray from `splash_particles`, using the hull height type and the offsets. Wetness decals on `wetness_decals` fade in as the hull takes water. The bounding box is the in-hull water simulation. On the campaign map the wake path is skipped.

## Where it lives

Managed script. Template and hull copies are in `NavalDLC\AssetPackages\nested_prefabs_packed.xml`. No `scene.xscene` stores it.

## Examples

**Template.** Packed entity `ship_water_effects`. Bounding box `1, 1, 1`. Show box off. The other fields are left at the class defaults.

**Cog-sized hull.** A packed hull that also uses texture `sdf_ship_cog`. Bounding box about `7.86, 24.13, 11.43`, hull type `Medium`, splash multiplier `1`, movement particles shown in the file.
