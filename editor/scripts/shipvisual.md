# ShipVisual

!!! info "v1.4.8"

Holds sail colors, a pattern seed, and a health fraction for a ship shown outside a battle. No prefab stores the script. The campaign and port screens add it when they build a visual ship.

Find placed ones in the Scene panel with `s:ShipVisual`. See [Filter](/editor/editor/#filter).

## Where you put it

Do not place this script on a mission ship. The visual factory instantiates a ship prefab with the `ship_visual_only` filter and adds this script to the root.

## What you set

The only public field is **SailColors**. No prefab writes it. The factory overwrites it after adding the script.

| Panel field | What to do with it |
|---|---|
| **SailColors** | Two colors for the sails. The field starts as white and white. The factory replaces both from the ship's sail colors. |

Seed, custom sail pattern, and health are set by the factory. They are not panel fields.

## What it does

`CreateVisualShip` adds the script, then sets the seed, **SailColors**, and health (hit points divided by max hit points). Meshes tagged `faction_color` get the same two colors. The campaign copy also stores a custom sail pattern id. Sail visuals on that entity read **SailColors** for the sail meshes.

## Where it lives

Managed script. Added in code onto whatever ship prefab the visual factory instantiates. No scene file stores it.

## Examples

No scene file and no prefab stores this script.

**Battle visual.** The factory instantiates the ship's prefab, adds `ShipVisual`, and sets the seed, both sail colors, and the health fraction.

**Campaign visual.** The cached ship entity already has `ShipVisual`. The campaign path sets the seed, the custom sail pattern id, and both sail colors.
