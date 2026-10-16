# road_instance

!!! info "v1.4.8"

Builds a road, or a river, along a path from one entity. The only copy is the prefab `road_instance`. No scene places it.

Find placed ones in the Scene panel with `s:road_instance`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an empty entity, or drop the prefab `road_instance` from `Modules\Native\Prefabs\scene_prop_prefabs.xml`. **Path Name** is the path it follows.

## What you set

Native panel. The prefab stores this order.

| Panel field | What to do with it |
|---|---|
| **Path Name** | Path to follow. The prefab stores `Test`. |
| **River Mod** | On: treat the path as water. The prefab stores off. |
| **Mesh Mod** | On: place the three mesh variations instead of a tiled road material. The prefab stores off. |
| **Road Material Name** | Road material. The prefab stores `road_tool_brick_road`. |
| **Road Max Turn Angle** | Sharpest corner, in degrees. The prefab stores `30`. |
| **Road Texture Tile** | How often the road texture repeats. The prefab stores `1`. |
| **Road Tesselation Distance Threshold** | Point spacing along the path. The prefab stores `3`. |
| **Align Road to Terrain** | Drop the road onto the terrain. The prefab stores off. |
| **Road Elevation Offset From Terrain** | Extra height when the road is aligned. The prefab stores `0`. |
| **Align Terrain to Road** | Lift the terrain up to the road. The prefab stores off. |
| **River Water Density** | Used when **River Mod** is on. The prefab stores `0.5`. |
| **River Water Foam Amount** | Used when **River Mod** is on. The prefab stores `0.5`. |
| **Road Mesh Variation 1** | First mesh, used when **Mesh Mod** is on. The prefab stores empty. |
| **Road Mesh Variation 2** | Second mesh. The prefab stores empty. |
| **Road Mesh Variation 3** | Third mesh. The prefab stores empty. |
| **Mesh 1 Local Rotation** | Yaw for variation 1, in degrees. The prefab stores `0`. |
| **Mesh 2 Local Rotation** | Yaw for variation 2. The prefab stores `0`. |
| **Mesh 3 Local Rotation** | Yaw for variation 3. The prefab stores `0`. |
| **Mesh Random Variation Seed** | Seed for which mesh is picked. The prefab stores `0`. |

## What it does

With **River Mod** and **Mesh Mod** off, it tiles **Road Material Name** along **Path Name**. **River Mod** uses the density and foam fields. **Mesh Mod** scatters the three mesh variations. The generated mesh is tagged `road_mesh`.

## Where it lives

Native script. Prefab: `Modules\Native\Prefabs\scene_prop_prefabs.xml`, entity `road_instance`. No `scene.xscene` stores it.

## Examples

**Brick test path.** Prefab `road_instance`. **Path Name** `Test`, material `road_tool_brick_road`, max turn `30`, tile `1`, tessellation `3`. River mode, mesh mode, and both align flags are off.
