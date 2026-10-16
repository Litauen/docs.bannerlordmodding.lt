# NavalPhysics

!!! info "v1.4.8"

Floats this entity. Buoyancy comes from child volumes named under `floater_volume_holder`. The script picker groups it under `ship_visual_only`. The dropdown name is **NavalPhysics**.

Find placed ones in the Scene panel with `s:NavalPhysics`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to the root of a dynamic body that should float: debris, a buoy, or a ship visual. Give that body a dynamic physics shape, and a child entity named `floater_volume_holder` whose children are the float volumes. Pair it with **CustomNavalPhysicsParameters** on the same entity for the float strength. If a ship script is already on this entity, this script does not build its own float setup.

## What you set

| Panel field | What to do with it |
|---|---|
| **Sink** | Button on the panel. This script does not run anything when the button is clicked. |

Vanilla prefabs store the script with no variables. Float strength is the other script, **CustomNavalPhysicsParameters**.

## What it does

Without a ship script on the same entity, it reads the float volumes, turns gravity off, and ticks buoyancy, drag, and weight from agents standing on the body. **BehaveLikeShip** false, the debris default, uses the lighter debris reference. A child tagged `batched_physics_entity` is the first physics body; other children get their own.

## Where it lives

Managed script. Loose prefabs in `Modules\NavalDLC\Prefabs\floatable_props.xml` and `editor_barriers.xml`. More copies, including ones with **NavalPhysicsAgentCollisionSolver**, are in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`.

## Examples

**Door slab.** `floatable_props.xml`, `floatable_door_top`. Mass `20`. **FloatingForceMultiplier** `0.03`. **BehaveLikeShip** off. Has `floater_volume_holder`.

**Barrel.** Same file, `floatable_barrel`. Mass `262`. **FloatingForceMultiplier** `0.7`.

**Buoy.** `editor_barriers.xml`, `buoy_big_a`. Mass `500`. **FloatingForceMultiplier** `0.15`. Child `floater_volume_holder` with 25 volumes. `buoy_small_a` in the same file uses multiplier `0.3`.

**Conversation longship.** `NavalDLC\SceneObj\conversation_scene_sea_multi_agent`. Entity `longship_ship_nested`. The script is present with no variables, next to a volume generator (`102` volumes, width and depth `0.6`).
