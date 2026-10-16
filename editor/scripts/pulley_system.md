# pulley_system

!!! info "v1.4.8"

Drives a sail pulley so the free rope and the fixed rope meet an end target. The dropdown name is `pulley_system`. The script picker files it under `ship_visual_only`.

Find placed ones in the Scene panel with `s:pulley_system`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the holder. The ready-made holder is `pulley_system` in `Modules\NavalDLC\Prefabs\rope_piles.xml`.

The holder looks for these child tags:

- `pulley`, with children `pulley_wheel`, `pulley_left_point`, and `pulley_right_point`
- `end_point_rope` and `end_point_target`
- rope segments on the free side and the fixed side

## What you set

| Panel field | What to do with it |
|---|---|
| **End Rope Length** | How far the pulley sits from the end target, multiplied by the pulley entity's scale. The prefab stores `2`. |

## What it does

Each tick it places the `pulley` entity between the last free segment, the last fixed segment, and `end_point_target`. **End Rope Length** is that gap. The end rope is pinned to the pulley and aimed at the target. Sail code moves `end_point_target` when the sail folds or rolls.

## Where it lives

Managed script. The loose prefab is `Modules\NavalDLC\Prefabs\rope_piles.xml`. Packed copies are in `NavalDLC\AssetPackages\nested_prefabs_packed.xml`. No `scene.xscene` stores it.

## Examples

**Sail pulley with a 2 m end rope.** `NavalDLC\Prefabs\rope_piles.xml`, entity `pulley_system`. **End Rope Length** is `2`. Children include `pulley`, `end_point_rope`, and `end_point_target`.
