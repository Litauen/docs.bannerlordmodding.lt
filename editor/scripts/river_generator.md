# river_generator

!!! info "v1.4.8"

Builds the campaign-map rivers from a flow mask, a normal map, and a height map. Both main maps put it on an entity named `river_script`.

Find placed ones in the Scene panel with `s:river_generator`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to one entity for the whole map. `SandBox\SceneObj\Main_map` and `NavalDLC\SceneObj\Main_map` each have a single `river_script` at the origin.

## What you set

Native panel. Both maps store this order.

| Panel field | What to do with it |
|---|---|
| **FlowMap** | River mask texture. Both maps store `worldmap_river_mask`. |
| **NormalMap** | River normal texture. Both maps store `worldmap_river_normal`. |
| **HeightMap** | Terrain height texture. Both maps store `world_map_heightmap`. |
| **Material** | River material. SandBox stores `worldmap_river_mat`. NavalDLC stores `naval_worldmap_river`. |
| **Extra Pixel Count** | Extra texels around the river. SandBox stores `1`. NavalDLC stores `0`. |
| **Show River Area** | Debug draw of the river area. Both maps store off. |
| **Terrain Height Offset** | Lifts the river off the height map. SandBox stores `0.32`. NavalDLC stores `0`. |
| **Water Plane Height Offset** | Extra lift against water planes. Both maps store `0`. |
| **Snap To Water Planes** | Stick the river to water planes. Both maps store on. |

## What it does

It generates the river mesh from those three textures and the material. **Snap To Water Planes** uses entities tagged `snapped_water_body`. The NavalDLC map also puts `water_flowmap` on the same `river_script`.

## Where it lives

Native script. No prefab stores it. The two placements are the campaign `Main_map` scenes.

## Examples

**Campaign rivers.** `SandBox\SceneObj\Main_map`. Entity `river_script`. Flow mask `worldmap_river_mask`, material `worldmap_river_mat`, terrain offset `0.32`, extra pixels `1`, snap on.

**Naval campaign rivers.** `NavalDLC\SceneObj\Main_map`. Entity `river_script`. Same masks and height map. Material `naval_worldmap_river`, terrain offset `0`, extra pixels `0`, snap on. `water_flowmap` is the next script on that entity.
