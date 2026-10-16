# water_flowmap

!!! info "v1.4.8"

Marks a campaign water surface for flow. The opened placements store no fields.

Find placed ones in the Scene panel with `s:water_flowmap`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to a water plane or to the river entity. The NavalDLC campaign map puts it on the big sea plane and on `river_script`.

## What you set

Native panel. Every opened placement is an empty script tag. No `<variable>` row is stored.

## What it does

The sea plane tagged `snapped_water_body` carries this script. `river_script` carries it beside [river_generator](/editor/scripts/river_generator/). The script stores no texture name of its own.

## Where it lives

Native script. No prefab stores it. The placements are `NavalDLC\SceneObj\Main_map`.

## Examples

**Sea plane.** `NavalDLC\SceneObj\Main_map`. Entity `water_plane`, tag `snapped_water_body`, scale about `2744 × 2744`, material `naval_worldmap_water`. The script tag is `water_flowmap` with no variables. `CampaignMapAmbientOccluder` is the next script.

**Rivers.** Same scene, entity `river_script`. `river_generator` is first. `water_flowmap` follows, with no variables.

**A smaller plane.** Same scene, another `water_plane` tagged `snapped_water_body`, scale about `29 × 18`. Again `water_flowmap` with no variables.
