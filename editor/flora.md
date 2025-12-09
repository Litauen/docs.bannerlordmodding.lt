# Flora

## Terrain Layer Flora Guide

by Horris

Tested all of the below on a 1.6km x 1.6km map, just to see the differences on a larger scale using larger numbers. On small patches it is really hard to figure out what these individual settings do I've found out.

Everything below is explained with trees as an example, but works the same for grass, rocks, etc.


`Colony`<br>
Is a group of trees

`Density`<br>
How close individual trees are together within a colony.

`Seed`<br>
Different seeds cause different noise patterns to be generated. <br>
Can be helpful to slightly adjust the tree placements if clipping with rocks occur for example. Or you simply don't like the current arrangement's looks.

Another use case for it is if you are combining different tree types for example, and want the colonies to either overlap in the exact same location, or not overlap and not mix as much.

* Same seed = colony locations overlap exactly
* Different seed = colony locations are different

`Colony Radius`<br>
Distance between groups of trees<br>
lower value = less distance between groups of trees

`Colony Threshold`<br>
Size of every single group of trees.<br>
lower value = bigger groups<br>
It can seem as if it also creates new colonies, but it doesn't. It sometimes looks like trees spawn in new places, but that position is already part of an existing colony, however small it might be.
Colonies can be made to overlap one another if the threshold is set low enough, combined with a low enough colony radius.

`Size min/Size max`<br>
Minimal and maximal sizes of the individual trees you are placing

`Weight`<br>
Affects areas where you have only lightly applied the terrain layer.<br>
It determines whether colonies can spawn when only a light layer of terrain is applied.<br>
Usually around edges of your terrain, where it blends into other terrain.<br>
Or where you quickly 'painted' over another layer and can still see the layer below it through the top one.


## Wide spacing for trees on the world map


Problems with: map_pine_a, map_pine_b, map_pine_c, worldmap_tree_acacia_01, worldmap_tree_beech_01, worldmap_tree_high_01,worldmap_tree_aspen_01

![](/pics/SjGUgif.png)

??? info "1.2.12"

    (When drawn using paint layers)

    !!! quote "I'm struggling with flora paint layer forests. However, I change the density, colony radius, colony threshold and weight, I can't get trees [close enough](https://docs.google.com/document/d/1npGJ9p1ySdu2RDU19P_2aE-OCsKWie_G02vcws36UIs/edit)."

    This happens when the map is saved under the custom name.

    Save/load your map as "Main_map" and spacing will be OK.


!!! info "1.3.1"

Renaming to Main_map does not help in 1.3.1

In the editor trees are with wide spaces, but in the game they are ok.
This does not allow to edit the forests properly in the Editor.

I had to recreate trees in `flora_kinds.xml`.

Workflow:

* Create `flora_kinds.xml` in `YOUR_MOD/ModuleData`
* Copy flora you are using in your layers from `Native/ModuleData/flora_kinds.xml`
* Rename them
* Set flags like this (it seems tree=true is the problematic flag in the original flora configuration)
    ```xml
        <flags>
            <flag name="point_up" value="true"/>
            <flag name="align_with_ground" value="true"/>
            <flag name="do_not_use_smooth_lod" value="true"/>
            <flag name="align_with_slope" value="false"/>
            <flag name="fading_out" value="true"/>
            <flag name="no_lods" value="false"/>
        </flags>
    ```
* Restart the Editor so it will see your new flora
* Change in the layers the flora to your duplicates

After this what you see in the Editor is the same what you get in the Game.

## Fix grass

If grass is standing like this in the scene:

![](/pics/2509251742a.png)

Sellect `Terrain Elevation` Alt+8, Mode: Smoothen, Weight and Hardness to 0.00:

![](/pics/2509251742b.png)

And tap anywhere on the ground:

![](/pics/2509251742c.png)

Save the scene.