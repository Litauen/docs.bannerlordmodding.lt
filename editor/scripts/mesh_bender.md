# mesh_bender

!!! info "v1.4.8"

Bends the entity's mesh. **Bend Amount** is how far. **Bend Center Ratio** is where along the mesh the bend sits.

Find placed ones with `s:mesh_bender`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the wall, plank, or patch you want bent. An entity tagged `not_bendable` is left alone.

## What you set

Native panel.

| Panel field | What to do with it |
|---|---|
| **Bend Amount** | Bend strength. Negative bends the other way. Opened pieces use `-20`, `20`, and `30`. |
| **Bend Center Ratio** | Where the bend is centered. Opened pieces store `0.5`. |

## What it does

The mesh is bent by **Bend Amount** around **Bend Center Ratio**. A scene leveler that duplicates an entity copies this script onto the copy.

## Where it lives

Native script. Planks are in `Modules\Native\Prefabs\siege_barricades.xml`. Wall patches are in `Modules\Native\Prefabs\props_archhitectural.xml`.

## Examples

**Siege plank, bent back.** `siege_barricades.xml`, entity `castle_plank_wall_a`. **Bend Amount** `-20`, **Bend Center Ratio** `0.5`.

**Arena wall patch.** `props_archhitectural.xml`, entity `battania_arena_wall_patch_a`. One stores **Bend Amount** `30`, another `20`. Both keep **Bend Center Ratio** `0.5`.
