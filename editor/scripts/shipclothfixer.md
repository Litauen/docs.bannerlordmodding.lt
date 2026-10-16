# ShipClothFixer

!!! info "v1.4.8"

Feeds the ship's motion into every cloth on the hull so sails and banners move with the ship. The class has no panel fields.

Find placed ones in the Scene panel with `s:ShipClothFixer`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the ship root, next to the physics. It walks every child and every skeleton and takes each cloth simulator.

## What you set

The class declares no editor fields. Placements store an empty script tag.

## What it does

Each frame it sets a forced velocity on those cloths from how far the ship moved since the previous physics step. In the editor it does the same from the editor tick.

## Where it lives

Managed script. Packed ship prefabs in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`.

## Examples

**Conversation longship.** `NavalDLC\SceneObj\conversation_scene_sea_multi_agent`. Entity `longship_ship_nested`. The script tag is empty. The same entity also has **ShipColorAssigner**.

**Packed hulls, empty tag.** Prefabs `cog_ship_nested` and `corbita_ship_nested` in `nested_prefabs_packed.xml`. The same empty tag is on `burning_cog_ship_nested_storyline` and `catship_ship_nested`.
