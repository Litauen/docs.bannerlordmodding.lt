# CampaignMapAmbientOccluder

!!! info "v1.4.8"

The storm shadow on the naval campaign map. The first mesh on this entity is stamped once per storm. The script has no panel fields.

Find placed ones in the Scene panel with `s:CampaignMapAmbientOccluder`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the map water plane, on an entity whose first mesh is the storm patch. The naval map uses `water_plane`.

## What you set

Nothing. The script declares no visible field, and the map stores no variables.

## What it does

Each campaign tick it places one copy of that mesh on every spawned storm, and on each quest storm registered with it. Each copy is scaled to `60`. Unused copies are cleared. The corsair-hunt quest registers its storm entity here and unregisters it when the storm is gone.

The editor prepares sixteen slots and does not move them.

## Where it lives

Managed script. The placement is `NavalDLC\SceneObj\Main_map`, entity `water_plane`. No prefab stores it.

## Examples

**Naval map water.** `NavalDLC\SceneObj\Main_map`, entity `water_plane`, tag `snapped_water_body`, mesh `editor_plane_high` with material `naval_worldmap_water`. The script sits beside `water_flowmap` and stores no variables.
