# BurningNode

!!! info "v1.4.8"

One point of fire on a ship. A child particle and an optional child light follow this node's fire amount. The dropdown name is **BurningNode**.

Find placed ones in the Scene panel with `s:BurningNode`. See [Filter](/editor/editor/#filter).

## Where you put it

Use the packed prefab `burning_node`, or one of the ship copies (`ship_burning_node_a` through `ship_burning_node_j`, `ship_railing_burning_node`). The node entity should be tagged `burning_node`. A child tagged `particle_entity` is the flame. A child tagged `light_entity` is the light.

## What you set

The packed prefab stores one row. The panel label is **Node Index**.

| Panel field | What to do with it |
|---|---|
| **Node Index** | Order of this node along a sail or a rail. The packed prefab stores `-1`. The ship fire system reads this index. |

## What it does

Fire amount runs from `0` to `1`. Above `0`, the particle emits. The light and the drip sparks turn on only after the fire system enables them. Emission scales with the fire amount. If the node drops under the water surface, the fire amount is cleared. The ship fire system and the sail visual read these nodes. They are not started by the scene on their own.

## Where it lives

Managed script. No loose scene stores it. Prefabs are in `NavalDLC\AssetPackages\nested_prefabs_packed.xml`: `burning_node`, `burning_node_rope`, `burning_node_yard`, and the `ship_burning_node_*` copies.

## Examples

**Sail flame.** Packed prefab `burning_node`. **Node Index** is `-1`. Child `particle_entity` is tagged `particle_entity` and uses `psys_fire_on_sail_nosound`.
