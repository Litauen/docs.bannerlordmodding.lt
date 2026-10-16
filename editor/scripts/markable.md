# Markable

!!! info "v1.4.8"

A destructible target that can show a beam over itself. The tutorial turns that beam on and off. The beam prefab spawned in a mission is always `highlight_beam`.

Find placed ones with `s:Markable`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the same entity as `DestructableComponent`. The tutorial marks every destructible in a target group that also has this script. The archery pot is the vanilla placement.

## What you set

| Panel field | What to do with it |
|---|---|
| **MarkerPrefabName** | Stored on the pot as `highlight_beam`. The mission does not read this row. It always spawns the prefab named `highlight_beam`. |

## What it does

On mission start a `highlight_beam` is created at the entity and hidden. When marking is turned on, the beam is shown and its particle plays for the active time (the tutorial uses `3` seconds), then hides for the passive time (the tutorial uses `10` seconds), then repeats. A destroyed destructible disables the beam. Resetting the passive timer delays the next show while the beam is hidden.

## Where it lives

`Modules\Native\Prefabs\props_other.xml`, inside `pot_archery_target_breakable`.

## Examples

**Breakable archery pot.** `pot_archery_target_breakable` / child `archery_target_pot`, tag `ranged_bow`. The child has `DestructableComponent` (destruction state `pot_archery_destroyed`, **DestroyOnAnyHit** on, **MaxHitPoint** `0.1`) and `Markable` with **MarkerPrefabName** `highlight_beam`. The same pair is on the second `archery_target_pot` in that file.

No scene file stores this script. Scenes place the pot prefab.
