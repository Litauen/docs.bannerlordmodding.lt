# Ballista

!!! info "v1.4.8"

The siege ballista a crew loads and fires. Scenes place a [BallistaSpawner](/editor/scripts/ballistaspawner/), which spawns this prefab. The weapon prefabs are hidden in the prefab browser.

Find placed ones in the Scene panel with `s:Ballista`. See [Filter](/editor/editor/#filter).

## Where you put it

Use the spawner in a siege scene. The weapon entity needs children tagged with **BodyTag** and **NavelTag**. A child tagged with **SkeletonTag** supplies the skeleton; if that child is missing, the body is used.

## What you set

`Native\Prefabs\props_siege_ballistas.xml` stores this order on `ballista_a`. **HorizontalDirectionRestriction** is a public field that prefab does not write.

| Panel field | What to do with it |
|---|---|
| **NavelTag** | Child that yaws. The prefab stores `BallistaNavel`. |
| **BodyTag** | Child that pitches. The prefab stores `BallistaBody`. |
| **SkeletonTag** | Child with the weapon skeleton. The prefab stores `SkeletonEntity`. |
| **AnimationHeightDifference** | Height fix for the pilot's hands. The prefab stores `0`. |
| **IdleActionName** | Pilot idle. Attacker prefab: `act_usage_ballista_idle_attacker`. Defender: `act_usage_ballista_idle_defender`. |
| **ReloadActionName** | Reload action. Attacker `act_usage_ballista_reload_attacker`, defender `act_usage_ballista_reload_defender`. |
| **PlaceAmmoStartActionName** | Start of placing the bolt. Attacker and defender each have their own clip. |
| **PlaceAmmoEndActionName** | End of placing the bolt. |
| **PickUpAmmoStartActionName** | Start of picking the bolt up. |
| **PickUpAmmoEndActionName** | End of picking the bolt up. |
| **HorizontalDirectionRestriction** | Yaw limit, in radians. Code default is a right angle. The prefab does not write it. A spawner overwrites it from **DirectionRestrictionDegree**. |
| **BallistaShootingSpeed** | Bolt speed. Code default is `120`. The prefab stores `170`. |
| **MissileItemID** | Ammo item id. The prefab stores `ballista_projectile`. |
| **Focus** | What the crew prefers to shoot. The prefab stores `Troops`. |
| **StartingAmmoCount** | Bolts at start. The prefab stores `0`. |
| **TopReleaseAngleRestriction** | Highest pitch, in radians. The prefab stores `0.7`. |
| **BottomReleaseAngleRestriction** | Lowest pitch, in radians. The prefab stores `-0.7`. |
| **RemoveOnDeployTag** | Entities with this tag are removed when the engine deploys. Empty on the prefab. |
| **AddOnDeployTag** | Entities with this tag are added when it deploys. Empty on the prefab. The spawner copies its own value here. |
| **PilotStandingPointTag** | Pilot stand tag. The prefab stores `Pilot`. |
| **AmmoPickUpTag** | Ammo stand tag. The prefab stores `ammopickup`. |
| **WaitStandingPointTag** | Wait stand tag. The prefab stores `Wait`. |
| **NavMeshPrefabName** | Navmesh prefab. Both `ballista_a` and `ballista_b` store `ballista_a_dnm`. |

Move, reload, and fire sounds are `event:/mission/siege/ballista/move`, `reload`, and `fire`.

## What it does

A pilot aims with the mouse. The navel yaws and the body pitches, inside the limits above. Loading plays the pick-up and place actions, then the reload action. Firing uses the skeleton clips `ballista_fire` and `ballista_set_up`. Grapeshot ids on this weapon are `ballista_c_projectile_grape` and `ballista_c_projectile_grape_projectile`. The crew stops using it when an enemy is within `7` meters.

## Where it lives

Managed script. Weapon prefabs are in `Native\Prefabs\props_siege_ballistas.xml`: `ballista_a`, `ballista_a_fire`, `ballista_b`, `ballista_b_fire`. No scene file stores the weapon script. Siege scenes store the spawner.

## Examples

**Attacker ballista.** `ballista_a` in that file. Idle and reload use the `_attacker` actions. Shooting speed `170`, focus `Troops`, ammo item `ballista_projectile`.

**Defender ballista.** `ballista_b` in the same file. The six action fields use the `_defender` clips. Speed, ammo, focus, and pitch limits match `ballista_a`.
