# ArrowBarrel

!!! info "v1.4.8"

A barrel an archer uses to pick up arrows or bolts. The script adds no fields of its own. The panel rows are the usable-machine tags.

Find placed ones in the Scene panel with `s:ArrowBarrel`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the barrel entity, mesh `arrow_barrel`, physics `bo_arrow_barrel`. A child stand point with **StandingPointWithWeaponRequirement** is the spot the archer stands.

## What you set

`Native\Prefabs\props_siege.xml` stores this order. There is no barrel-specific field.

| Panel field | What to do with it |
|---|---|
| **PilotStandingPointTag** | Tag of the pilot stand. The barrel stores `Pilot`. |
| **AmmoPickUpTag** | Tag of the ammo stand. The barrel stores `ammopickup`. |
| **WaitStandingPointTag** | Tag of the wait stand. The barrel stores `Wait`. |
| **NavMeshPrefabName** | Navmesh prefab. The barrel leaves it empty. |

## What it does

The use text is **Pick Up**. Only an agent whose weapon is an arrow or a bolt can use it. The pickup plays `event:/mission/combat/pickup_arrows` (about `1.03` seconds, `max_distance` `100000`, so it is not a short local bed).

## Where it lives

Managed script, on the ammo-barrel base. The prop is `arrow_barrel` in `Native\Prefabs\props_siege.xml` and again in `Native\Prefabs\quest_props.xml`. The sound row is in `Native\ModuleData\sound_event_data.gen.xml`.

## Examples

**Siege prop.** `Native\Prefabs\props_siege.xml`, entity `arrow_barrel`. Mesh `arrow_barrel`, shape `bo_arrow_barrel`. The four fields above are the whole script. A child `icon_man` is the stand.

**Quest prop.** `Native\Prefabs\quest_props.xml` has the same entity name, mesh, and the same four values.
