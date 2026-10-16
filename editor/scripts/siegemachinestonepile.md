# SiegeMachineStonePile

!!! info "v1.4.8"

The stone pile that sits beside a mangonel or trebuchet. Crew use the pickup stands on the pile. The pile itself adds no fields of its own.

Find placed ones in the Scene panel with `s:SiegeMachineStonePile`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the child named `projectile_pile`, under the machine body. The mesh on the vanilla piles is `mangonel_rock_pile`. Pickup stands are children tagged `ammopickup`.

## What you set

`mangonel_a` stores this order. Trebuchet and ship piles store the same four values.

| Panel field | What to do with it |
|---|---|
| **PilotStandingPointTag** | Pilot stand tag. The piles store `Pilot`. |
| **AmmoPickUpTag** | Pickup stand tag. The piles store `ammopickup`. The use text is shown on entities with this tag. |
| **WaitStandingPointTag** | Wait stand tag. The piles store `Wait`. |
| **NavMeshPrefabName** | Navmesh under the pile. The piles store `projectile_pile_a_blocker_dnm`. |

## What it does

A tagged pickup stand shows Pick Up, with the pile type Stone. When a deployment point shows or hides its siege engine, it shows or hides this pile with it.

## Where it lives

Managed script, on the usable-machine base. The piles are children in `Native\Prefabs\props_siege_mangonels.xml`, `Native\Prefabs\props_siege_trebuchets.xml`, and `NavalDLC\Prefabs\ship_machines.xml`. No scene file stores the script.

## Examples

**Mangonel pile.** `mangonel_a` / `projectile_pile`. The same block is on `mangonel_a_fire`, `mangonel_b`, `mangonel_b_fire`, and the grapeshot copies of each.

**Trebuchet pile.** `trebuchet_a` / `projectile_pile`. Same tags and the same navmesh. `trebuchet_a_fire` and both grapeshot trebuchets match it.

**Ship mangonel.** `NavalDLC\Prefabs\ship_machines.xml`, `mangonel_c` / `projectile_pile`, scaled to `0.6`. Same four values.
