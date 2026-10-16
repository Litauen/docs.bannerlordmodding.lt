# signed_distance_field

!!! info "v1.4.8"

A water clip volume for a ship hull. The class name is `signed_distance_field`. **SDF Texture** is the volume. The script picker files it under `ship_visual_only`.

Find placed ones in the Scene panel with `s:signed_distance_field`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to a child of the ship. The template entity is `ship_water_clip` in `NavalDLC\AssetPackages\nested_prefabs_packed.xml`. The entity's frame is the clip's transform. [ship_water_effects](/editor/scripts/ship_water_effects/) uses this clip for water inside the hull.

## What you set

| Panel field | What to do with it |
|---|---|
| **SDF Texture** | The signed-distance texture. The template stores empty. Hulls store names such as `sdf_ship_cog`, `sdf_ship_fishing`, `sdf_ship_dromon`, `sdf_ship_galley`, `sdf_ship_drakkar`, and `sdf_funeral_boat`. |
| **Visualize SDF** | Draws the volume. Hulls that write it store off. |

A few older hulls store a row named `Visualize` instead of **Visualize SDF**. The field on this class is **Visualize SDF**.

## What it does

On init the texture is registered as a water clip on this entity. Each tick the clip's box follows the entity frame, and the clip is on only while the entity is visible. Removing the script unregisters the clip.

## Where it lives

Managed script. Template `ship_water_clip` and the hull copies are in `NavalDLC\AssetPackages\nested_prefabs_packed.xml`. No `scene.xscene` stores it.

## Examples

**Empty template.** Packed entity `ship_water_clip`. **SDF Texture** is empty. **Visualize SDF** is not written.

**Cog hull.** A packed hull stores **SDF Texture** `sdf_ship_cog` and **Visualize SDF** off.

**Fishing boat.** A packed hull stores `sdf_ship_fishing` and **Visualize SDF** off.

**Funeral boat.** A packed hull stores `sdf_funeral_boat` and **Visualize SDF** off.
