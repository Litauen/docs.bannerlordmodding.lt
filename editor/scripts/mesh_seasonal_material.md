# mesh_seasonal_material

!!! info "v1.4.8"

Swaps the entity's material by season. Each field is the material name for that season.

Find placed ones with `s:mesh_seasonal_material`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the outer mesh or the border. Every mesh under the entity should use the same material. A missing name keeps the mesh's own material for that season.

## What you set

Native panel. `outermesh_mountains_a` stores this order. Older borders omit **SummerMaterial**.

| Panel field | What to do with it |
|---|---|
| **SummerMaterial** | Material name for summer. |
| **FallMaterial** | Material name for fall. |
| **WinterMaterial** | Material name for winter. Often a `_winter` material. |
| **SpringMaterial** | Material name for spring. |

## What it does

The season picks one of the four names and applies it. If that name is empty or missing, the mesh keeps its original material for that season.

## Where it lives

Native script. Mountain shells are in `Modules\Native\Prefabs\outermeshes.xml`. Borders are in `Modules\Native\Prefabs\natural_landscapes.xml`.

## Examples

**Mountain shell, winter swap.** `outermeshes.xml`, entity `outermesh_mountains_a`. Summer, fall, and spring are `outermesh_mountains_a_mat`. Winter is `outermesh_mountains_a_winter_mat`. The entity is flagged `not_affected_by_season`.

**Desert outer mesh.** `natural_landscapes.xml`, entity `outer_mesh_a`. The mesh material is `outer_mesh_desert_a`. Summer, fall, and spring are empty. Winter is `outer_mesh_desert_a_winter`.

**Border with three empty seasons.** `natural_landscapes.xml`, entity `mountain_border_b`. **FallMaterial**, **WinterMaterial**, and **SpringMaterial** are empty. **SummerMaterial** is not written.
