# ShipFireBallista

!!! info "v1.4.8"

The burning-bolt version of [ShipBallista](/editor/scripts/shipballista/). It scores flammable targets higher. The siege engine type is the fire ballista.

Find placed ones in the Scene panel with `s:ShipFireBallista`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the weapon entity, with the same navel, body, and skeleton children as a ship ballista. Packed prefab `ballista_c_fire` in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`. `ship_ballista_holder` places further copies.

## What you set

The panel is the ship ballista panel. `ballista_c_fire` stores the same defender clips and these differences from a plain bolt ballista.

| Panel field | What to do with it |
|---|---|
| **_horizontalAimSensitivity** | `ballista_c_fire` stores `0.950`. A holder copy stores `0.500`. |
| **_verticalAimSensitivity** | `ballista_c_fire` stores `0.650`. A holder copy stores `0.500`. |
| **NavelTag** | `BallistaNavel`. |
| **BodyTag** | `BallistaBody`. |
| **SkeletonTag** | `SkeletonEntity`. |
| **AnimationHeightDifference** | `0`. |
| **IdleActionName** | `act_usage_ballista_idle_defender`. |
| **ReloadActionName** | `act_usage_ballista_reload_defender`. |
| **PlaceAmmoStartActionName** | Defender place-start clip. |
| **PlaceAmmoEndActionName** | Defender place-end clip. |
| **PickUpAmmoStartActionName** | Defender pick-up start clip. |
| **PickUpAmmoEndActionName** | Defender pick-up end clip. |
| **HorizontalDirectionRestriction** | Yaw limit, in radians. `ballista_c_fire` stores `2.250`. A holder copy stores `1.571`. Another holder copy stores `3.130` with speed `110`. |
| **BallistaShootingSpeed** | `ballista_c_fire` stores `170`. |
| **MissileItemID** | `ballista_c_projectile_burning`. |
| **MultipleProjectileCount** | `5`. |
| **Focus** | `Troops`. |
| **StartingAmmoCount** | `ballista_c_fire` stores `30`. A holder copy stores `20`. |
| **TopReleaseAngleRestriction** | `0.700`. |
| **BottomReleaseAngleRestriction** | `-0.700`. |
| **RemoveOnDeployTag** | Empty. |
| **AddOnDeployTag** | Empty. |
| **PilotStandingPointTag** | `Pilot`. |
| **AmmoPickUpTag** | `ammopickup`. |
| **WaitStandingPointTag** | `Wait`. |
| **NavMeshPrefabName** | `ballista_a_dnm`. |

Burning grapeshot ids on this weapon are `ballista_c_projectile_grape_fire` and `ballista_c_projectile_grape_fire_projectile`.

## What it does

Same aim and load loop as a ship ballista. A target flagged as a ship scores twice as high, and a flammable target scores twice as high again. The shot is the burning bolt.

## Where it lives

Managed script. Packed prefabs in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`. No scene file stores this script.

## Examples

**Fire ballista.** Prefab `ballista_c_fire`, entity `ballista_c_fire`. Ammo `ballista_c_projectile_burning`, speed `170`, starting ammo `30`, yaw limit `2.250`, aim sensitivity `0.950` / `0.650`.

**Holder copy with a tighter yaw.** Prefab `ship_ballista_holder`, entity `ballista_c_fire`. Same ammo, speed `170`, starting ammo `20`, yaw limit `1.571`, aim sensitivity `0.500` / `0.500`.
