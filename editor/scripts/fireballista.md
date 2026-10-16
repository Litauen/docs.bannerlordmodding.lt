# FireBallista

!!! info "v1.4.8"

A ballista that fires a burning bolt. The machine, crew points, and aim limits match the stone ballista. The missile id is the burning bolt.

Find placed ones with `s:FireBallista`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the ballista entity. Vanilla keeps the script on the prefab root, next to the destructible body. `ballista_a_fire` uses the attacker crew animations. `ballista_b_fire` uses the defender set. Both use the same stand tags: `BallistaNavel`, `BallistaBody`, and `SkeletonEntity`.

## What you set

The prefab stores these rows, in this order. `ballista_a_fire` is the attacker copy. `ballista_b_fire` swaps every action name to the `_defender` clip and keeps the other values.

| Panel field | What to do with it |
|---|---|
| **NavelTag** | Child that yaws the ballista. Prefab: `BallistaNavel`. |
| **BodyTag** | Child that pitches. Prefab: `BallistaBody`. |
| **SkeletonTag** | Skeleton child. Prefab: `SkeletonEntity`. |
| **AnimationHeightDifference** | Height fix for the crew pose. Prefab stores `0`. |
| **IdleActionName** | Crew idle. Attacker: `act_usage_ballista_idle_attacker`. |
| **ReloadActionName** | Reload. Attacker: `act_usage_ballista_reload_attacker`. |
| **PlaceAmmoStartActionName** | Start of placing a bolt. Attacker: `act_usage_ballista_ammo_place_start_attacker`. |
| **PlaceAmmoEndActionName** | End of placing a bolt. Attacker: `act_usage_ballista_ammo_place_end_attacker`. |
| **PickUpAmmoStartActionName** | Start of picking a bolt up. Attacker: `act_usage_ballista_ammo_pick_up_start_attacker`. |
| **PickUpAmmoEndActionName** | End of picking a bolt up. Attacker: `act_usage_ballista_ammo_pick_up_end_attacker`. |
| **BallistaShootingSpeed** | Bolt speed. Both fire prefabs store `170`. |
| **MissileItemID** | The shot. Both fire prefabs store `ballista_projectile_burning`. The stone prefabs store `ballista_projectile`. |
| **Focus** | What the AI prefers. Prefab: `Troops`. Also `Walls`, `RangedSiegeWeapons`, `PrimarySiegeWeapons`. |
| **StartingAmmoCount** | Bolts on the machine at start. Prefab stores `0`, so it draws from a pile. |
| **TopReleaseAngleRestriction** | Highest pitch, in radians. Prefab: `0.7`. |
| **BottomReleaseAngleRestriction** | Lowest pitch, in radians. Prefab: `-0.7`. |
| **RemoveOnDeployTag** | Scene entities with this tag are removed when the engine deploys. Prefab leaves it empty. |
| **AddOnDeployTag** | Scene entities with this tag are shown when the engine deploys. Prefab leaves it empty. |
| **PilotStandingPointTag** | Pilot point. Prefab: `Pilot`. |
| **AmmoPickUpTag** | Ammo pickup points. Prefab: `ammopickup`. |
| **WaitStandingPointTag** | Waiting points. Prefab: `Wait`. |
| **NavMeshPrefabName** | Navmesh swapped in under the machine. Prefab: `ballista_a_dnm`. |

## What it does

Crew use, reload, and pick up bolts the same way as a stone ballista. The shot is `ballista_projectile_burning`. The AI raises the value of flammable targets and of other siege engines, and almost ignores walls.

## Where it lives

`Modules\Native\Prefabs\props_siege_ballistas.xml`, prefabs `ballista_a_fire` and `ballista_b_fire`. The stone machines in the same file are `ballista_a` and `ballista_b`.

## Examples

No scene file stores this script. The machines are the two prefabs.

**Attacker fire ballista.** `Native\Prefabs\props_siege_ballistas.xml`, entity `ballista_a_fire`. Missile `ballista_projectile_burning`, speed `170`, attacker action names, navmesh `ballista_a_dnm`.

**Defender fire ballista.** Same file, entity `ballista_b_fire`. Same missile and speed. Every action name uses the `_defender` clip. The stand mesh on that prefab is `ballista_b_stand`.
