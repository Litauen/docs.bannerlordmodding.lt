# FireTrebuchet

!!! info "v1.4.8"

A trebuchet that throws fire ammunition. The arm, sling, and rope animations match the stone trebuchet. The missile id is the fire shot.

Find placed ones with `s:FireTrebuchet`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the trebuchet body. In `trebuchet_a_fire` the body entity has the same name as the prefab. The grapeshot fire machine is `trebuchet_a_grapeshot_fire`, and its body is also named `trebuchet_a_fire`. The body needs children tagged `body`, `sling`, `rope`, `rotate`, and `vertical_adjuster`. The shot bone is `bn_projectile_holder`.

Siege scenes place a trebuchet spawner, not this script.

## What you set

`trebuchet_a_fire` stores these rows. The prefab also stores `AIAmmoLoadTag` (`ammoload_ai`) and `VisualizeReleaseTrajectoryAngle` (`1`). Those two names are not fields on this script.

| Panel field | What to do with it |
|---|---|
| **ProjectileSpeed** | Shot speed. Prefab: `45`. |
| **IdleWithAmmoAnimation** | Sling idle with a shot loaded. `trebuchet_a_sling_idle_with_ammo`. |
| **IdleEmptyAnimation** | Sling idle empty. `trebuchet_a_sling_idle_emtpy`. |
| **BodyFireAnimation** | Body shoot. `trebuchet_a_body_fire`. |
| **BodySetUpAnimation** | Body reload. `trebuchet_a_body_setup`. |
| **SlingFireAnimation** | Sling shoot. `trebuchet_a_sling_fire`. |
| **SlingSetUpAnimation** | Sling reload. `trebuchet_a_sling_setup`. |
| **RopeFireAnimation** | Rope shoot. `trebuchet_a_rope_fire`. |
| **RopeSetUpAnimation** | Rope reload. `trebuchet_a_rope_setup`. |
| **VerticalAdjusterAnimation** | Pitch pose. `trebuchet_a_anglearm_state`. |
| **TimeGapBetweenShootActionAndProjectileLeaving** | Seconds from the shoot anim to the release. Prefab: `1.6`. |
| **MissileItemID** | The shot. Pot machine: `pot`. Grapeshot fire: `grapeshot_fire_stack`. |
| **Focus** | AI preference. Prefab: `Troops`. |
| **StartingAmmoCount** | Shots on the machine. The pot prefab stores `20`. |
| **TopReleaseAngleRestriction** | Highest pitch, radians. Prefab: `1`. |
| **BottomReleaseAngleRestriction** | Lowest pitch, radians. Fire prefabs store `0.31`. The stone trebuchet stores `0.3`. |
| **RemoveOnDeployTag** | Tag removed on deploy. Empty. |
| **AddOnDeployTag** | Tag shown on deploy. Empty. |
| **ForcedUse** | Prefab stores `false`. |
| **PilotStandingPointTag** | Pilot point. `Pilot`. |
| **AmmoPickUpTag** | Ammo pickups. `ammopickup`. |
| **WaitStandingPointTag** | Waiting points. `Wait`. |
| **NavMeshPrefabName** | Navmesh under the machine. `trebuchet_a_dnm`. |

The grapeshot fire body also writes `ReloadTag` `reload`, `AmmoLoadTag` `ammoload`, `CameraHolderTag` `cameraHolder`, and `ProjectileTag` `projectile`. It writes the ammo count as `startingAmmoCount` (different spelling) at `20`.

## What it does

Crew load and shoot like a stone trebuchet. `trebuchet_a_fire` throws `pot` at speed `45`. `trebuchet_a_grapeshot_fire` throws `grapeshot_fire_stack`. The AI prefers flammable engines and other siege engines, and values small or moving targets less.

## Where it lives

`Modules\Native\Prefabs\props_siege_trebuchets.xml`. Stone machines in the same file: `trebuchet_a` (`boulder`) and `trebuchet_a_grapeshot` (`grapeshot_stack`).

## Examples

No scene file stores this script.

**Fire pot.** `trebuchet_a_fire`. Missile `pot`, speed `45`, release gap `1.6`, navmesh `trebuchet_a_dnm`.

**Fire grapeshot.** `trebuchet_a_grapeshot_fire`, body entity `trebuchet_a_fire`. Missile `grapeshot_fire_stack`. Same animations and the same `1.6` release gap.
