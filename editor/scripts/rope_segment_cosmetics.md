# rope_segment_cosmetics

!!! info "v1.4.8"

A point slid along a parent rope. The dropdown name is `rope_segment_cosmetics`. The script picker files it under `ship_visual_only`.

Find placed ones in the Scene panel with `s:rope_segment_cosmetics`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to a child of a [rope_segment](/editor/scripts/rope_segment/). The rope reads each child and parks it at **Normalized Location wrt Rope**.

Tag the child `burning_node` if it is a burn point. The rope checks that tag.

## What you set

| Panel field | What to do with it |
|---|---|
| **Normalized Location wrt Rope** | `0` is the start of the rope, `1` is the end. Class default is `0.5`. |

## What it does

The parent rope places this entity along its curve. A value of `0.5` is the midpoint. The rope hides its own visibility check and moves these children with the curve.

## Where it lives

Managed script. Packed copies are in `NavalDLC\AssetPackages\nested_prefabs_packed.xml`. No loose prefab under Native or SandBox, and no `scene.xscene`, stores it.

## Examples

**Cloth bit at the midpoint.** Packed prefab `cloth_fragment_a`. **Normalized Location wrt Rope** is `0.5`. The same value is on `cloth_fragment` copies that share this script.

**A point at the start of a rope.** A packed rope child stores `0`.

**A point about seven tenths along.** Another packed rope child stores `0.702`.
