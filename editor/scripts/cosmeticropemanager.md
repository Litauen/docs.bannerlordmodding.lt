# CosmeticRopeManager

!!! info "v1.4.8"

Hides a ship's decorative ropes when the camera is far away, and straightens them at mid distance. The script adds no fields.

Find placed ones in the Scene panel with `s:CosmeticRopeManager`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to a holder entity whose children are the ropes. The script picker files it under `ship_visual_only`. The dropdown name is still `CosmeticRopeManager`.

Each rope the script should drive is a child that contains an entity tagged `simple_rope_start` with a rope segment on it.

## What you set

No panel fields. Placements store no variables.

## What it does

Every tick it measures the camera against the holder.

- Farther than 100 meters: the holder is hidden.
- Farther than 45 meters: each collected rope is set to a straight line.
- Closer than that: the ropes are visible and not forced straight.

## Where it lives

Managed script. The only copies are in `NavalDLC\AssetPackages\nested_prefabs_packed.xml`. No loose prefab and no `scene.xscene` stores it.

## Examples

**Decorative ropes on a packed hull.** Entity `rope_cosmetics_holder`. No variables. One child is `simple_rope_nested`. The packed file contains 22 of these holders. The first one sits beside an oar seat and a retracted frame, not in a scene file.
