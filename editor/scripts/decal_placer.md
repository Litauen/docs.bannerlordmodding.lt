# decal_placer

!!! info "v1.4.8"

Scatters decals on a surface. The helper prefab stores the material names, the surface, the count, and the size range.

Find placed ones with `s:decal_placer`. See [Filter](/editor/editor/#filter).

## Where you put it

Use prefab `decal_spawner` in `Modules\Native\Prefabs\editor_helpers_&_tests.xml`. No scene under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC places it.

## What you set

Native panel. `decal_spawner` stores this order. **Place Decals** and **Visualize Last Operation** are buttons and are not stored.

| Panel field | What to do with it |
|---|---|
| **Value** | Stored float. The helper stores `20`. |
| **decal0** | First decal material. Helper: `decals_moss_atlassed_1`. |
| **decal1** | Second. Helper: `decals_moss_atlassed_2`. |
| **decal2** | Third. Helper: `decals_moss_atlassed_3`. |
| **decal3** | Fourth. Helper: `decals_moss_atlassed_4`. |
| **Placement Surface** | Surface mesh. Helper: `stone_wall_2_remade_d`. |
| **Decal Count** | How many to place. Helper: `350`. |
| **Minimum Size** | Smallest size. Helper: `4`. |
| **Maximum Size** | Largest size. Helper: `6`. |
| **Place Decals** | Button. Drops entities named `auto_placed_decal_` plus a number. Not stored. |
| **Visualize Last Operation** | Button. Not stored. |

## What it does

**Place Decals** scatters the four materials on **Placement Surface**, between **Minimum Size** and **Maximum Size**, up to **Decal Count**.

## Where it lives

Native script. Only `decal_spawner` in `editor_helpers_&_tests.xml`.

## Examples

**Moss on a stone wall.** `editor_helpers_&_tests.xml`, entity `decal_spawner`. **Value** `20`, four `decals_moss_atlassed` materials, surface `stone_wall_2_remade_d`, count `350`, size `4` to `6`. Levels base, 1, 2, 3, civilian, and siege.
