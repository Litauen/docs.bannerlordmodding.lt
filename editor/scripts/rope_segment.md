# rope_segment

!!! info "v1.4.8"

A rope from this entity to an end point. The dropdown name is `rope_segment`. The script picker files it under `ship_visual_only`.

Find placed ones in the Scene panel with `s:rope_segment`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the rope mesh. The end is the sibling tagged `simple_rope_end` on the parent, unless a pulley has already set the end.

Cosmetic children use [rope_segment_cosmetics](/editor/scripts/rope_segment_cosmetics/). A physics rope also needs an entity tagged `rope_physics_body` under the root, and **Uses Physics Body** on.

## What you set

| Panel field | What to do with it |
|---|---|
| **Segment Index** | Order of this piece along the rope. The pulley prefab stores `0`. |
| **Is Fixed** | On: this side does not pay out. Fixed ropes in `rope_piles.xml` store on. Free ropes store off. |
| **Loose Amount** | Sag. Class default is `0.1`. The prefab's free rope stores `0.3`, its fixed rope stores `0`, and the end rope stores `0.1`. |
| **Default Rope Length** | Rest length. Class default is `25.9`, and that is what the prefab stores. Changing it sets the live length to half of this value. |
| **Uses Physics Body** | On: swing against the `rope_physics_body` entity. The pulley prefab does not write it, so it stays off. |
| **Swing Multiplier** | How hard that body swings the rope. Class default is `1`. The pulley prefab does not write it. |

## What it does

The mesh runs from this entity to the end entity. **Loose Amount** is the sag. **Is Fixed** keeps that side from paying out when a pulley pulls. With **Uses Physics Body** on, the rope swings from the tagged physics body.

## Where it lives

Managed script. Loose copies are in `Modules\NavalDLC\Prefabs\rope_piles.xml`. Packed copies are in `NavalDLC\AssetPackages\nested_prefabs_packed.xml`. No `scene.xscene` stores it.

## Examples

**Free side of a pulley.** `NavalDLC\Prefabs\rope_piles.xml`, inside entity `pulley_system`. **Segment Index** `0`, **Is Fixed** off, **Loose Amount** `0.3`, **Default Rope Length** `25.9`.

**Fixed side.** Same prefab, entity `right_rope0`. **Is Fixed** on, **Loose Amount** `0`, length `25.9`.

**End rope.** Same prefab, entity `end_point_rope`, tagged `end_point_rope`. **Is Fixed** off, **Loose Amount** `0.1`, length `25.9`.
