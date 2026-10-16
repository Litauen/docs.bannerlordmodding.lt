# ShipMangonel

!!! info "v1.4.8"

A mangonel mounted on a ship. Standing points are closed to the player. The loose prefab throws `boulder`. A packed copy throws burning grapeshot.

Find placed ones in the Scene panel with `s:ShipMangonel`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the body entity, next to [DestructableComponent](/editor/scripts/destructablecomponent/). The loose prefab is the child `mangonel_b_body` in `NavalDLC\Prefabs\ship_machines.xml`, on a parent tagged `mangonel_c`. Packed copies sit on `mangonel_b_body` inside `ship_ballista_holder`.

## What you set

`ship_machines.xml` stores this order on `mangonel_b_body`. **_directionRestriction** is a field that file does not write. A packed copy stores it.

| Panel field | What to do with it |
|---|---|
| **_directionRestriction** | Yaw limit, in radians. Code default is two-thirds of a turn (`2π/3`). The loose prefab does not write it. A packed `mangonel_b_body` stores `3.294`. |
| **MangonelBodySkeleton** | Body skeleton. Loose prefab: `mangonel_b_skeleton`. |
| **MangonelBodyFire** | Shoot anim. `mangonel_b_fire`. |
| **MangonelBodyReload** | Reload anim. `mangonel_b_set_up`. |
| **MangonelRopeFire** | Rope shoot. `mangonel_b_holder_fire`. |
| **MangonelRopeReload** | Rope reload. `mangonel_b_holder_set_up`. |
| **MangonelAimAnimation** | Pitch pose. `mangonel_b_anglearm_state`. |
| **ProjectileBoneName** | Bone the shot sits on. `bn_projectile_holder`. |
| **IdleActionName** | Crew idle. `act_usage_mangonel_idle`. |
| **ShootActionName** | Crew shoot. `act_usage_mangonel_shoot`. |
| **Reload1ActionName** | First reload. `act_usage_mangonel_reload`. |
| **Reload2ActionName** | Second reload. `act_usage_mangonel_reload_2`. |
| **RotateLeftActionName** | Turn left. `act_usage_mangonel_rotate_left`. |
| **RotateRightActionName** | Turn right. `act_usage_mangonel_rotate_right`. |
| **LoadAmmoBeginActionName** | Start loading. `act_usage_mangonel_load_ammo_begin`. |
| **LoadAmmoEndActionName** | Finish loading. `act_usage_mangonel_load_ammo_end`. |
| **Reload2IdleActionName** | Idle in the second reload. `act_usage_mangonel_reload_2_idle`. |
| **ProjectileSpeed** | Shot speed. Loose prefab: `40`. Packed grapeshot copy: `100`. Reload on this weapon is `6.2` times the land reload. |
| **MissileItemID** | The shot. Loose prefab: `boulder`. Packed copy: `mangonel_c_grapeshot_fire_stack`. |
| **MultipleProjectileCount** | Pellets in a stack. Both store `5`. |
| **Focus** | AI preference. Both store `Troops`. |
| **StartingAmmoCount** | Shots on the machine. Both store `20`. |
| **TopReleaseAngleRestriction** | Highest pitch, radians. Loose: `0.700`. Packed grapeshot: `0.900`. |
| **BottomReleaseAngleRestriction** | Lowest pitch, radians. Loose: `-0.200`. Packed grapeshot: `-0.300`. |
| **RemoveOnDeployTag** | Tag removed on deploy. Empty. |
| **AddOnDeployTag** | Tag shown on deploy. Empty. |
| **PilotStandingPointTag** | Pilot point. `Pilot`. |
| **AmmoPickUpTag** | Ammo pickups. `ammopickup`. |
| **WaitStandingPointTag** | Waiting points. `Wait`. |
| **NavMeshPrefabName** | Navmesh under the machine. `mangonel_b_dnm`. |

One packed copy also stores ballista pick-up and place action names. This class does not declare those fields.

Grapeshot ids on this weapon are `mangonel_c_grapeshot_stack` and `mangonel_c_grapeshot_projectile`. Fire grapeshot ids are `mangonel_c_grapeshot_fire_stack` and `mangonel_c_grapeshot_fire_projectile`.

## What it does

Crew load, aim, and throw. The weapon's side is the ship's side. Standing points are disabled for players.

## Where it lives

Managed script. Loose body in `NavalDLC\Prefabs\ship_machines.xml`. Packed copies in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`.

## Examples

No scene file stores this script.

**Boulder mangonel.** `NavalDLC\Prefabs\ship_machines.xml`, entity `mangonel_b_body`. Missile `boulder`, speed `40`, pitch `-0.200` to `0.700`. The destructible on the same entity has **MaxHitPoint** `900` and **BattleSide** `Defender`.

**Burning grapeshot.** Packed prefab `ship_ballista_holder`, entity `mangonel_b_body`. Missile `mangonel_c_grapeshot_fire_stack`, speed `100`, yaw limit `3.294`, pitch `-0.300` to `0.900`.
