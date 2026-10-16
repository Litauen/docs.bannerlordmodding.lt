# Mangonel

!!! info "v1.4.8"

A stone-throwing mangonel. The large machine is an onager on the attacker side and a catapult on the defender side. The small machine is the same split. Fire ammunition is `FireMangonel`, not this script.

Find placed ones with `s:Mangonel`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the body entity. `mangonel_a` holds `mangonel_a_body`. `mangonel_b` holds `mangonel_b_body`. Grapeshot copies reuse those body names inside `mangonel_a_grapeshot` and `mangonel_b_grapeshot`. The body needs children tagged `body`, `rope`, `rotate`, and `vertical_adjuster`.

Siege scenes place `MangonelSpawner`, which instantiates the grapeshot prefab.

## What you set

`mangonel_a` stores these rows. `mangonel_b` uses the `mangonel_b_*` skeletons, the short crew clips, bone `bn_projectile_holder`, speed `45`, bottom angle `-0.2`, and navmesh `mangonel_b_dnm`.

| Panel field | What to do with it |
|---|---|
| **MangonelBodySkeleton** | Body skeleton. Large: `mangonel_skeleton`. Small: `mangonel_b_skeleton`. |
| **MangonelBodyFire** | Shoot anim. Large: `mangonel_fire`. Small: `mangonel_b_fire`. |
| **MangonelBodyReload** | Reload anim. Large: `mangonel_set_up`. Small: `mangonel_b_set_up`. |
| **MangonelRopeFire** | Rope shoot. Large: `mangonel_holder_fire`. Small: `mangonel_b_holder_fire`. |
| **MangonelRopeReload** | Rope reload. Large: `mangonel_holder_set_up`. Small: `mangonel_b_holder_set_up`. |
| **MangonelAimAnimation** | Pitch pose. Large: `mangonel_a_anglearm_state`. Small: `mangonel_b_anglearm_state`. |
| **ProjectileBoneName** | Bone the shot sits on. Large: `end_throwarm`. Small: `bn_projectile_holder`. |
| **IdleActionName** | Crew idle. Large: `act_usage_mangonel_big_idle`. Small: `act_usage_mangonel_idle`. |
| **ShootActionName** | Crew shoot. Large: `act_usage_mangonel_big_shoot`. Small: `act_usage_mangonel_shoot`. |
| **Reload1ActionName** | First reload. Large: `act_usage_mangonel_big_reload`. Small: `act_usage_mangonel_reload`. |
| **Reload2ActionName** | Second reload. Both: `act_usage_mangonel_reload_2`. |
| **RotateLeftActionName** | Turn left. Both: `act_usage_mangonel_rotate_left`. |
| **RotateRightActionName** | Turn right. Both: `act_usage_mangonel_rotate_right`. |
| **LoadAmmoBeginActionName** | Start loading. Large: `act_usage_mangonel_big_load_ammo_begin`. Small: `act_usage_mangonel_load_ammo_begin`. |
| **LoadAmmoEndActionName** | Finish loading. Large: `act_usage_mangonel_big_load_ammo_end`. Small: `act_usage_mangonel_load_ammo_end`. |
| **Reload2IdleActionName** | Idle in the second reload. Both: `act_usage_mangonel_reload_2_idle`. |
| **ProjectileSpeed** | Shot speed. Large boulder: `55`. Small boulder: `45`. Small grapeshot: `36`. |
| **MissileItemID** | The shot. Boulder machines: `boulder`. Grapeshot: `grapeshot_stack`. |
| **MultipleProjectileCount** | Pellets in a stack. Grapeshot prefabs store `5`. Boulder prefabs do not write this row. |
| **Focus** | AI preference. Prefab: `Troops`. |
| **StartingAmmoCount** | Shots on the machine. Prefab: `20`. |
| **TopReleaseAngleRestriction** | Highest pitch, radians. Prefab: `0.7`. |
| **BottomReleaseAngleRestriction** | Lowest pitch, radians. Large: `0.1`. Small: `-0.2`. |
| **RemoveOnDeployTag** | Tag removed on deploy. Empty. The spawner can fill this. |
| **AddOnDeployTag** | Tag shown on deploy. Empty. The spawner copies its value here. |
| **PilotStandingPointTag** | Pilot point. `Pilot`. |
| **AmmoPickUpTag** | Ammo pickups. `ammopickup`. |
| **WaitStandingPointTag** | Waiting points. `Wait`. |
| **NavMeshPrefabName** | Navmesh under the machine. Large: `mangonel_a_dnm`. Small: `mangonel_b_dnm`. |

## What it does

Crew load, aim, and throw. A large machine throws `boulder` at `55`. A small one throws `boulder` at `45`. Grapeshot throws `grapeshot_stack` (five pellets): large at `55`, small at `36`. Attacker engines are onagers. Defender engines are catapults. Use text on the points is Use, Rearm, Rotate, Pick Up, or Load Ammo, from the point's tag.

## Where it lives

`Modules\Native\Prefabs\props_siege_mangonels.xml`.

## Examples

No scene file stores this script.

**Large boulder.** `mangonel_a` / `mangonel_a_body`. Missile `boulder`, speed `55`, bone `end_throwarm`.

**Small boulder.** `mangonel_b` / `mangonel_b_body`. Missile `boulder`, speed `45`, bone `bn_projectile_holder`.

**Grapeshot the spawner uses.** `mangonel_a_grapeshot` stores `grapeshot_stack`, count `5`, speed `55`. `mangonel_b_grapeshot` stores `grapeshot_stack`, count `5`, speed `36`.
