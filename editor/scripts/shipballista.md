# ShipBallista

!!! info "v1.4.8"

A ballista bolted to a ship. Aim follows the ship's movement. Shots prefer other ships. Ships place the weapon prefab. [ShipBallistaSpawner](/editor/scripts/shipballistaspawner/) is a separate spawner.

Find placed ones in the Scene panel with `s:ShipBallista`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the weapon entity. The body needs children tagged with **NavelTag**, **BodyTag**, and **SkeletonTag**. Packed prefabs live in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`: `ballista_c`, `ballista_c_explosive`, and copies under `ship_ballista_holder`.

## What you set

`ballista_c` stores these rows. Panel order is the ship fields, then the ballista fields, then the ranged-weapon fields. A holder copy writes only the rows it changes.

| Panel field | What to do with it |
|---|---|
| **_horizontalAimSensitivity** | How fast yaw follows the mouse. Code default `0.5`. `ballista_c` stores `0.950`. |
| **_verticalAimSensitivity** | How fast pitch follows the mouse. Code default `0.5`. `ballista_c` stores `0.650`. |
| **NavelTag** | Child that yaws. `ballista_c` stores `BallistaNavel`. |
| **BodyTag** | Child that pitches. `ballista_c` stores `BallistaBody`. |
| **SkeletonTag** | Child with the weapon skeleton. `ballista_c` stores `SkeletonEntity`. |
| **AnimationHeightDifference** | Height fix for the pilot's hands. `ballista_c` stores `0`. |
| **IdleActionName** | Pilot idle. `ballista_c` stores `act_usage_ballista_idle_defender`. |
| **ReloadActionName** | Reload. `ballista_c` stores `act_usage_ballista_reload_defender`. |
| **PlaceAmmoStartActionName** | Start of placing the bolt. Defender clip on `ballista_c`. |
| **PlaceAmmoEndActionName** | End of placing the bolt. |
| **PickUpAmmoStartActionName** | Start of picking the bolt up. |
| **PickUpAmmoEndActionName** | End of picking the bolt up. |
| **HorizontalDirectionRestriction** | Yaw limit, in radians. Code default is a right angle. `ballista_c` stores `1.750`. |
| **BallistaShootingSpeed** | Bolt speed. Code default `120`. `ballista_c` stores `170`. |
| **MissileItemID** | Ammo item id. `ballista_c` stores `ballista_c_projectile`. |
| **MultipleProjectileCount** | Pellets when the ammo is a stack. `ballista_c` stores `5`. |
| **Focus** | What the crew prefers to shoot. `ballista_c` stores `Troops`. |
| **StartingAmmoCount** | Bolts at start. `ballista_c` stores `30`. |
| **TopReleaseAngleRestriction** | Highest pitch, in radians. `ballista_c` stores `0.700`. |
| **BottomReleaseAngleRestriction** | Lowest pitch, in radians. `ballista_c` stores `-0.700`. |
| **RemoveOnDeployTag** | Entities with this tag are removed when the engine deploys. Empty on `ballista_c`. |
| **AddOnDeployTag** | Entities with this tag are added when it deploys. Empty on `ballista_c`. |
| **PilotStandingPointTag** | Pilot stand tag. `ballista_c` stores `Pilot`. |
| **AmmoPickUpTag** | Ammo stand tag. `ballista_c` stores `ammopickup`. |
| **WaitStandingPointTag** | Wait stand tag. `ballista_c` stores `Wait`. |
| **NavMeshPrefabName** | Navmesh under the weapon. `ballista_c` stores `ballista_a_dnm`. |

Move, reload, and fire sounds are `event:/mission/ballista_naval/move`, `reload`, and `fire`. Grapeshot ids on this weapon are `ballista_c_projectile_grape` and `ballista_c_projectile_grape_projectile`.

## What it does

A pilot aims with the mouse. The navel yaws and the body pitches, inside the limits above. The shot's start speed includes the ship's velocity at the muzzle. The crew will not fire through a friendly ship. A target flagged as a ship scores twice as high.

## Where it lives

Managed script. Packed prefabs: `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`. No scene file stores this script.

## Examples

**Bolt ballista.** Prefab `ballista_c`, entity `ballista_c`. Ammo `ballista_c_projectile`, speed `170`, starting ammo `30`, yaw limit `1.750`, aim sensitivity `0.950` / `0.650`. Defender load clips.

**Pot ballista.** Prefab `ballista_c_explosive`, entity `ballista_c_explosive`. Ammo `ballista_c_projectile_pot`, speed `145`, starting ammo `20`, yaw limit `2.500`, aim sensitivity `0.935` / `0.650`.

**Stone and grapeshot on a holder.** Prefab `ship_ballista_holder`. Entity `ballista_c_stone` stores ammo `ballista_c_projectile_stone`, speed `170`, yaw limit `1.571`. Entity `ballista_c_grapeshot` stores ammo `ballista_c_projectile_grape`. One grapeshot copy stores **MultipleProjectileCount** `4`, speed `40`, and yaw limit `3.100`.
