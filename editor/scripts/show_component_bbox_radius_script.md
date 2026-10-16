# show_component_bbox_radius_script

!!! info "v1.4.8"

Draws the bounding radius of the mesh on this entity. The item-tableau frames store **Show** on.

Find placed ones in the Scene panel with `s:show_component_bbox_radius_script`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the entity whose mesh you want measured. The only scene is the item tableau. Those frames are marked visible only while editing.

## What you set

Native panel.

| Panel field | What to do with it |
|---|---|
| **Show** | On: draw the radius. Every opened frame stores on. |

## What it does

With **Show** on, the editor draws the component's bounding radius around the mesh. The tableau uses that while framing shields, bows, and slings for icons.

## Where it lives

Native script. No prefab stores it. The only scene is `Native\SceneObj\scn_item_tableau`.

## Examples

**Shield icon frame.** `Native\SceneObj\scn_item_tableau`. Entity `shield_frame`, mesh `kite_shield_b`, tag `shield_frame`. **Show** is on. A `holster_script` sits on the same entity.

**Bow icon frame.** Same scene, entity `bow_frame`, mesh `shortbow_g`. **Show** is on.

**Sling and stones.** Same scene. Entity `sling_frame` uses mesh `sling`. Entity `slingstones_frame` uses mesh `stone_holster`. Both store **Show** on.
