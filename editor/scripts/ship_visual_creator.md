# ship_visual_creator

!!! info "v1.4.8"

Native script on a ship hull, next to the volume generator and the upgrade viewer. The one opened placement stores no fields.

Find placed ones in the Scene panel with `s:ship_visual_creator`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the ship entity. The opened hull is `cog_ship_nested` in a cutscene, with physics shape `bo_composite_conv_cogship`.

## What you set

Native panel. The opened entity stores the script with no `<variable>` rows.

## What it does

On that hull the script sits with `Volume_generator` and `EditorShipUpgradeViewer`. Those two have their own fields. This one adds none on the saved entity.

## Where it lives

Native script. The only placement found is `NavalDLC\SceneObj\cutscene_saving_sister`. No prefab stores it.

## Examples

**Cog in a cutscene.** `NavalDLC\SceneObj\cutscene_saving_sister`. Entity `cog_ship_nested` at about `(-22, 18, 4)`. Scripts, in order: `ship_visual_creator` with no variables, `Volume_generator`, `EditorShipUpgradeViewer`.
