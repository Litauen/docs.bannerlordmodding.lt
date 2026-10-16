# SwingingObject

!!! info "v1.4.8"

A hanging prop that swings from ship motion and hits. The lantern prefab is the one vanilla places.

Find placed ones with `s:SwingingObject`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the root of `nord_lantern_nested`. The script swings the child tagged `swinging_entity`. A child named `collision_sphere` sets how far the swing may go.

## What you set

The quest scene stores this order. **Reset Simulation** and **Test Collision** are editor buttons. The files do not write them.

| Panel field | What to do with it |
|---|---|
| **Damping** | How fast the swing dies. Code default is `5`. The lantern stores `0.5`. |
| **Center of Mass Height** | Pivot height. Lantern: `-0.8`. |
| **Mass** | Mass of the bob. Code default is `1`. Lantern: `3`. |
| **Moment Of Inertia** | Resistance to the swing. Code default is `0.5`. Lantern: `0.4`. |
| **Reset Simulation** | Button. Zeroes the swing in the editor. |
| **Test Collision** | Button. Pulls the swing out to the collision limit. |
| **NavMeshPrefabName** | Left empty on the lantern. |

## What it does

Each physics tick the child tagged `swinging_entity` pitches and rolls from the parent's motion. **Damping**, **Mass**, **Center of Mass Height**, and **Moment Of Inertia** shape that swing. The `collision_sphere` child caps it. A hit adds to the swing.

## Where it lives

Managed script. Packed prefab `nord_lantern_nested` in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`.

## Examples

**Hanging lantern.** `NavalDLC\SceneObj\naval_storyline_act_3_quest_5`. Entity `nord_lantern_nested`. Damping `0.5`, center of mass `-0.8`, mass `3`, moment `0.4`. The child `collision_sphere` sits on the entity.

The packed copy of `nord_lantern_nested` stores those same four numbers.
