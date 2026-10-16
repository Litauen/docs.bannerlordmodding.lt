# Trebuchet

!!! info "v1.4.8"

The siege trebuchet a crew loads and fires. Scenes place a [TrebuchetSpawner](/editor/scripts/trebuchetspawner/), which spawns this prefab.

Find placed ones with `s:Trebuchet`. See [Filter](/editor/editor/#filter).

## Where you put it

The body entity inside `trebuchet_a` carries the script. Children are tagged `body`, `sling`, `rope`, `vertical_adjuster`, and `rotate_entity`. The shot bone is `bn_projectile_holder`.

## What you set

`trebuchet_a` stores this order. **MultipleProjectileCount** is a panel field that body does not write. The same body also stores `AIAmmoLoadTag`, `TimeGapBetweenShootActionAndProjectileLeaving`, `VisualizeReleaseTrajectoryAngle`, and `ForcedUse`. Those four are not panel fields.

| Panel field | What to do with it |
|---|---|
| **ProjectileSpeed** | Shot speed. Both opened bodies store `45`. |
| **IdleWithAmmoAnimation** | Sling idle with a stone. `trebuchet_a_sling_idle_with_ammo`. |
| **IdleEmptyAnimation** | Sling idle empty. The file stores `trebuchet_a_sling_idle_emtpy`. |
| **BodyFireAnimation** | Body shoot. `trebuchet_a_body_fire`. |
| **BodySetUpAnimation** | Body reload. `trebuchet_a_body_setup`. |
| **SlingFireAnimation** | Sling shoot. `trebuchet_a_sling_fire`. |
| **SlingSetUpAnimation** | Sling reload. `trebuchet_a_sling_setup`. |
| **RopeFireAnimation** | Rope shoot. `trebuchet_a_rope_fire`. |
| **RopeSetUpAnimation** | Rope reload. `trebuchet_a_rope_setup`. |
| **VerticalAdjusterAnimation** | Pitch pose. `trebuchet_a_anglearm_state`. |
| **MissileItemID** | The shot. Boulder body: `boulder`. Grapeshot body: `grapeshot_stack`. |
| **MultipleProjectileCount** | Pellets when the shot is a stack. Code default is `5`. Neither opened body writes this row. |
| **Focus** | What the crew prefers. Both store `Troops`. |
| **StartingAmmoCount** | Shots on the machine. The boulder body stores `StartingAmmoCount` `20`. |
| **TopReleaseAngleRestriction** | Highest pitch, radians. Both store `1`. |
| **BottomReleaseAngleRestriction** | Lowest pitch, radians. Both store `0.3`. |
| **RemoveOnDeployTag** | Tag removed when the engine deploys. Empty. The spawner can fill this. |
| **AddOnDeployTag** | Tag shown when the engine deploys. Empty. The spawner copies its value here. |
| **PilotStandingPointTag** | Pilot point. `Pilot`. |
| **AmmoPickUpTag** | Ammo pickups. `ammopickup`. |
| **WaitStandingPointTag** | Waiting points. `Wait`. |
| **NavMeshPrefabName** | Navmesh under the machine. Both store `trebuchet_a_dnm`. |

## What it does

Crew load, aim, and throw. Use text on the points is Use, Rearm, Rotate, Pick Up, or Load Ammo, from the point's tag. A boulder machine throws `boulder`. The grapeshot body throws `grapeshot_stack`. The fire prefabs are `trebuchet_a_fire` and `trebuchet_a_grapeshot_fire`.

## Where it lives

Managed script. `Modules\Native\Prefabs\props_siege_trebuchets.xml`.

## Examples

No scene file stores this script.

**Boulder.** `trebuchet_a` / body `trebuchet_a`. Missile `boulder`, speed `45`, ammo `20`.

**Grapeshot.** `trebuchet_a_grapeshot` / body `trebuchet_a`. Missile `grapeshot_stack`, speed `45`. That body stores the ammo row as `startingAmmoCount`.
