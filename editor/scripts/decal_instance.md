# decal_instance

!!! info "v1.4.8"

A decal on `decal_mesh`. **Decal Material** is the material name.

Find placed ones with `s:decal_instance`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on an entity whose mesh is `decal_mesh`. Scale and rotate the entity to size the decal.

## What you set

Native panel.

| Panel field | What to do with it |
|---|---|
| **Decal Material** | Material name. The test prefab stores `decal_white`. |

## What it does

The field is the decal material. The skirmish map also sets that same name on the `decal_mesh` component.

## Where it lives

Native script. The white decal is entity `_decal` in `Modules\Native\Prefabs\props_testing.xml`.

## Examples

**White test decal.** `props_testing.xml`, entity `_decal`. Mesh `decal_mesh`, material `decal_white`, **Decal Material** `decal_white`.

**Skirmish decals.** `Native\SceneObj\mp_skirmish_map_004`. Prefab `_decal`. One stores **Decal Material** `decal_type_a`, another `decal_type_b`, another `decal_type_c`. The mesh material on each entity matches that value.
