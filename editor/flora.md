# Flora


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