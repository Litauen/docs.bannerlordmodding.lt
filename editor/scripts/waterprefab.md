# Water Prefab

!!! info "v1.4.8"

Native panel. A water look: ocean, lake, river, or puddle. No official scene stores this script on an entity.

Find placed ones with `s:Water Prefab`. See [Filter](/editor/editor/#filter).

## Where you put it

The script is in the editor list. Native, SandBox, SandBoxCore, StoryMode, and NavalDLC scenes do not store `<script name="Water Prefab">`. Water that is actually placed uses the script `water_body`, and that script's field **Water Prefab** is a name from the catalog below.

## What you set

Native panel. No opened scene or prefab stores variables for this script, so there is no saved field list.

The looks live in `Modules\Native\ModuleData\water_prefabs.xml`. Each `WaterPrefab` row has `PrefabName`, `MaterialName`, `Thumbnail`, and `IsGlobal`.

## What it does

`PrefabName` is the string a water volume selects. `Open Ocean Global` uses material `wat_open_ocean_g` and `IsGlobal` true. `Muddy Puddle` uses `wat_puddle_muddy` and `IsGlobal` false. `Modules\NavalDLC\ModuleData\water_prefabs.xml` adds naval names such as `Naval Ocean`.

## Where it lives

Native script. Catalog: `Modules\Native\ModuleData\water_prefabs.xml` and `Modules\NavalDLC\ModuleData\water_prefabs.xml`.

## Examples

No placed entity with this script name.

**A water volume that picks a catalog row.** `Native\SceneObj\mp_siege_map_010` stores `<script name="water_body">` and sets **Water Prefab** to `Muddy Puddle`. That name is the `PrefabName` on the muddy-puddle row.
