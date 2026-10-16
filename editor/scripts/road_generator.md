# road_generator

!!! info "v1.4.8"

Lays a road decal along a named path. The entity is the generator. Its children are the baked `road_part` decals.

Find placed ones in the Scene panel with `s:road_generator`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an empty entity whose name follows the path, such as `path_road_road_2`. **Path Name** must match the path you already drew. The generator writes child entities named `road_part`.

## What you set

Native panel. The opened roads store this order.

| Panel field | What to do with it |
|---|---|
| **Path Name** | Path to follow. Hideout roads use `road_2`, `road_4`, `road_5`, `road_6`. The battle map uses `path_test`. |
| **Decal Material** | Road decal. The hideout stores `road_01`. The battle map stores `road_mud_02`. |
| **Width** | Decal width. Hideout stores `3`. Battle map stores `2`. |
| **Height** | Decal height. Both store `3`. |
| **Mip** | Texture mip. Hideout `road_2` stores `10`. Later hideout roads store `4`. Battle map stores `5`. |
| **Start Fadeout** | Fade at the start of the path. Hideout values are about `4` to `5.4`. Battle map stores `25`. |
| **End Fadeout** | Fade at the end. Hideout values are about `4` to `5`. Battle map stores `25`. |
| **Start Offset** | Shift along the path. Opened roads store `0`. |
| **Connection Path Name** | Other path this road joins. Opened roads leave it empty. |
| **Connection Type** | How the join is made. Opened roads leave it empty. |
| **Connection Node** | Node index for the join. Opened roads store an unset number, not a path name. |
| **Connection Fade** | Fade at the join. Opened roads store an unset number. |

## What it does

The generator builds `road_part` children. Each child is a decal on **Decal Material**, locked for selection, with the width, height, and fade copied onto the decal. A material used here has to be a road decal.

## Where it lives

Native script. No prefab stores the script. Placements are scene entities.

## Examples

**Dirt road through a hideout.** `SandBox\SceneObj\mountain_hideout_002`. Entity `path_road_road_2`. **Path Name** `road_2`, material `road_01`, width `3`, mip `10`, start fade `5.4`, end fade `4.7`. Children are `road_part` decals. The same scene has `road_4`, `road_5`, and `road_6` at mip `4`.

**Mud road on a battle map.** `Native\SceneObj\mp_battle_map_001`. Entity `path_road_path_test`. **Path Name** `path_test`, material `road_mud_02`, width `2`, mip `5`, both fades `25`.
