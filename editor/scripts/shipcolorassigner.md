# ShipColorAssigner

!!! info "v1.4.8"

Paints every mesh tagged `auto_factor_color` on the ship. The dropdown name is **ShipColorAssigner**. The editor tag is `ship_visual_only`.

Find placed ones in the Scene panel with `s:ShipColorAssigner`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the ship root. Broken pieces spawned later on that hull pick up the same factor color.

## What you set

Panel order is the class order. The conversation longship stores the two colors and leaves the button unwritten.

| Panel field | What to do with it |
|---|---|
| **Factor Color** | Color written onto meshes tagged `auto_factor_color`. The conversation longship stores `0.157, 0.118, 0.086, 1.000`. |
| **Ram Debris Color** | Color used for ram debris and ship damage decals. The conversation longship stores `0.302, 0.235, 0.200, 1.000`. |
| **Set Colors** | Editor button. Applies **Factor Color** again. The prefab does not write it. Changing **Factor Color** also reapplies it. |

## What it does

On load, and when **Factor Color** or **Set Colors** changes, every mesh tagged `auto_factor_color` under this entity gets **Factor Color**. The floatsam manager reads both colors for debris and decals. The ram reads **Ram Debris Color** for broken ram pieces.

## Where it lives

Managed script. Packed ship prefabs in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`.

## Examples

**Conversation longship.** `NavalDLC\SceneObj\conversation_scene_sea_multi_agent`. Entity `longship_ship_nested`. **Factor Color** `0.157, 0.118, 0.086, 1.000`. **Ram Debris Color** `0.302, 0.235, 0.200, 1.000`.

**Burning cog, both colors.** Packed prefab `burning_cog_ship_nested_storyline`. **Factor Color** `0.071, 0.075, 0.078`. **Ram Debris Color** `0.208, 0.192, 0.173`.

**Factor color only.** Packed prefab `dromon_ship_nested_storyline_q5` stores **Factor Color** `0.149, 0.125, 0.106` and does not write **Ram Debris Color**. Packed prefab `roundship_nested_storyline` stores **Factor Color** `0.071, 0.051, 0.043`.
