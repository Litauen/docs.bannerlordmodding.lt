# FireMangonel

!!! info "v1.4.8"

A mangonel that throws fire ammunition. Attacker machines are fire onagers. Defender machines are fire catapults. The crew, skeleton, and aim limits match the stone mangonel of the same size.

Find placed ones with `s:FireMangonel`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the mangonel body, not on the empty parent. Vanilla uses `mangonel_a_fire_body` under `mangonel_a_fire` for the large machine, and `mangonel_b_fire_body` under `mangonel_b_fire` for the small one. Grapeshot fire copies are `mangonel_a_grapeshot_fire` and `mangonel_b_grapeshot_fire`. The body needs children tagged `body`, `rope`, `rotate`, and `vertical_adjuster`.

Siege scenes do not place this script. They place `mangonel_a_spawner` or `mangonel_b_spawner`, and that spawner instantiates the grapeshot fire prefab when the engine is on fire.

## What you set

`mangonel_a_fire` stores these rows. `mangonel_b_fire` uses the `mangonel_b_*` skeleton names, the short crew actions (`act_usage_mangonel_idle` and the matching reload, shoot, and load clips), bone `bn_projectile_holder`, speed `45`, bottom angle `-0.2`, and navmesh `mangonel_b_dnm`.

| Panel field | What to do with it |
|---|---|
| **MangonelBodySkeleton** | Body skeleton. Large fire: `mangonel_skeleton`. Small: `mangonel_b_skeleton`. |
| **MangonelBodyFire** | Shoot anim on the body. Large: `mangonel_fire`. Small: `mangonel_b_fire`. |
| **MangonelBodyReload** | Reload anim on the body. Large: `mangonel_set_up`. Small: `mangonel_b_set_up`. |
| **MangonelRopeFire** | Shoot anim on the rope. Large: `mangonel_holder_fire`. Small: `mangonel_b_holder_fire`. |
| **MangonelRopeReload** | Reload anim on the rope. Large: `mangonel_holder_set_up`. Small: `mangonel_b_holder_set_up`. |
| **MangonelAimAnimation** | Pitch pose. Large: `mangonel_a_anglearm_state`. Small: `mangonel_b_anglearm_state`. |
| **ProjectileBoneName** | Bone the shot sits on. Large: `end_throwarm`. Small: `bn_projectile_holder`. |
| **IdleActionName** | Crew idle. Large: `act_usage_mangonel_big_idle`. |
| **ShootActionName** | Crew shoot. Large: `act_usage_mangonel_big_shoot`. |
| **Reload1ActionName** | First reload. Large: `act_usage_mangonel_big_reload`. |
| **Reload2ActionName** | Second reload. Both sizes: `act_usage_mangonel_reload_2`. |
| **RotateLeftActionName** | Turn left. Both: `act_usage_mangonel_rotate_left`. |
| **RotateRightActionName** | Turn right. Both: `act_usage_mangonel_rotate_right`. |
| **LoadAmmoBeginActionName** | Start loading. Large: `act_usage_mangonel_big_load_ammo_begin`. |
| **LoadAmmoEndActionName** | Finish loading. Large: `act_usage_mangonel_big_load_ammo_end`. |
| **Reload2IdleActionName** | Idle during the second reload. Both: `act_usage_mangonel_reload_2_idle`. |
| **ProjectileSpeed** | Shot speed. Large fire pot: `60`. Small fire pot: `45`. |
| **MissileItemID** | The shot. Pot machines: `pot`. Grapeshot fire: `grapeshot_fire_stack`. |
| **MultipleProjectileCount** | Pellets in a grapeshot stack. Grapeshot fire prefabs store `5`. The pot prefabs do not write this row. |
| **Focus** | AI preference. Prefab: `Troops`. |
| **StartingAmmoCount** | Shots on the machine. Prefab: `20`. |
| **TopReleaseAngleRestriction** | Highest pitch, radians. Prefab: `0.7`. |
| **BottomReleaseAngleRestriction** | Lowest pitch, radians. Large: `0.1`. Small: `-0.2`. |
| **RemoveOnDeployTag** | Tag removed when the engine deploys. Empty on the prefab. |
| **AddOnDeployTag** | Tag shown when the engine deploys. Empty on the prefab. The spawner copies its own value here. |
| **PilotStandingPointTag** | Pilot point. Prefab: `Pilot`. |
| **AmmoPickUpTag** | Ammo pickups. Prefab: `ammopickup`. |
| **WaitStandingPointTag** | Waiting points. Prefab: `Wait`. |
| **NavMeshPrefabName** | Navmesh under the machine. Large: `mangonel_a_dnm`. Small: `mangonel_b_dnm`. |

## What it does

Crew load, aim, and shoot like a stone mangonel. A large fire machine throws `pot` at speed `60`. A small one throws `pot` at `45`. Grapeshot fire throws `grapeshot_fire_stack` (five pellets): large at `60`, small at `45`. On the attacker side the engine counts as a fire onager. On the defender side it counts as a fire catapult. The AI heavily prefers flammable targets, siege towers, moving targets, and other engines.

## Where it lives

`Modules\Native\Prefabs\props_siege_mangonels.xml`.

## Examples

No scene file stores this script. These prefabs do.

**Large fire pot.** `mangonel_a_fire` / `mangonel_a_fire_body`. Missile `pot`, speed `60`, big-mangonel crew clips, bone `end_throwarm`.

**Small fire pot.** `mangonel_b_fire` / `mangonel_b_fire_body`. Missile `pot`, speed `45`, short crew clips, bone `bn_projectile_holder`.

**Large grapeshot fire.** `mangonel_a_grapeshot_fire`. Missile `grapeshot_fire_stack`, `MultipleProjectileCount` `5`, speed `60`. This is what `mangonel_a_spawner` spawns for the fire version.

**Small grapeshot fire.** `mangonel_b_grapeshot_fire`. Missile `grapeshot_fire_stack`, count `5`. `mangonel_b_spawner` spawns this for the fire version.
