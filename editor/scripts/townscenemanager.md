# Town Scene Manager

!!! info "v1.4.8"

Native panel. One marker on the campaign map so settlement meshes can be packed.

Find placed ones with `s:Town Scene Manager`. See [Filter](/editor/editor/#filter).

## Where you put it

One empty entity in the main map. Both official main maps name it `empty_object`. The SandBox map places it at `(463, 375, 7)`. The Naval map places it at `(502, 366, 17)`. Leave the entity empty.

## What you set

Native panel. Both main maps store the script with no variables.

The script adds no fields on the entities that were opened.

## What it does

The campaign map requires this entity. It is the marker the map uses when it packs settlement meshes. The SandBox entity sits beside `_occ_quad` and `campaign_icon_capsule_*` entities.

## Where it lives

Native script. `SandBox\SceneObj\Main_map` and `NavalDLC\SceneObj\Main_map`.

## Examples

**Campaign map.** `SandBox\SceneObj\Main_map`. Entity `empty_object`. The script tag has no variables.

**Naval campaign map.** `NavalDLC\SceneObj\Main_map`. Entity `empty_object`. The script tag has no variables.
