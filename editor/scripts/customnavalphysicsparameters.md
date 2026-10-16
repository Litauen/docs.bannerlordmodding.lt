# CustomNavalPhysicsParameters

!!! info "v1.4.8"

Overrides buoyancy and drag on the `NavalPhysics` script on the same entity. Debris props use it so a barrel does not float like a ship.

Find placed ones in the Scene panel with `s:CustomNavalPhysicsParameters`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the same entity as `NavalPhysics`. Floating props and buoys do this. A ship with `MissionShip` ignores these numbers: naval physics only applies them when that entity is not a mission ship.

## What you set

`NavalDLC\Prefabs\editor_barriers.xml` stores the full row list. Declaration order matches that file.

| Panel field | What to do with it |
|---|---|
| **BehaveLikeShip** | On: ship buoyancy reference. Off: debris reference. Vanilla props leave it off. |
| **FloatingForceMultiplier** | Scales the upward float force. Default `1`. Props use a small number. |
| **LinearFrictionMultiplierRight** | Drag to the right. Default `1`. |
| **LinearFrictionMultiplierLeft** | Drag to the left. Default `1`. |
| **LinearFrictionMultiplierForward** | Drag forward. Default `1`. |
| **LinearFrictionMultiplierBackward** | Drag backward. Default `1`. |
| **LinearFrictionMultiplierUp** | Drag upward. Default `1`. |
| **LinearFrictionMultiplierDown** | Drag downward. Default `1`. |
| **AngularFrictionMultiplier** | Spin drag as three numbers. Default `1, 1, 1`. |
| **ContinuousDriftSpeed** | Passed to the naval physics as a constant drift. Default `0`. Prefabs store the row as `ContinousDriftSpeed`. |

## What it does

When naval physics initialises on an entity that is not a mission ship, it reads this script. **BehaveLikeShip** picks the ship or debris reference. The friction rows scale linear and angular drag. **FloatingForceMultiplier** scales the float force. Each editor tick also pushes **ContinuousDriftSpeed** onto that physics script.

## Where it lives

Managed script. Loose prefabs: `NavalDLC\Prefabs\floatable_props.xml` and `NavalDLC\Prefabs\editor_barriers.xml`. More copies are in `NavalDLC\AssetPackages\nested_prefabs_packed.xml`.

## Examples

**Door slab, barely buoyant.** `floatable_props.xml`, entity `floatable_door_top`. **BehaveLikeShip** off, **FloatingForceMultiplier** `0.030`. The other rows are left at default, so they are not in the file.

**Barrel.** Same file, entity `floatable_barrel`. Multiplier `0.700`.

**Planks.** Same file, entity `floatable_bd_planks_a`. Multiplier `0.010`.

**Winery prop.** Same file, entity `floatable_shop_prop_merchantwinery_c`. Multiplier `0.210`.

**Big buoy, extra yaw drag.** `editor_barriers.xml`, entity `buoy_big_a`. Multiplier `0.150`. **AngularFrictionMultiplier** is `1, 1, 10`. Drift is `0`.

**Small buoy.** Same file, entity `buoy_small_a`. Multiplier `0.300`. Angular friction stays `1, 1, 1`.

**Packed debris barrel.** `nested_prefabs_packed.xml`, entity `floatable_debris_barrel_a`. **BehaveLikeShip** off, float multiplier `0.400`, drift `0`, linear friction all `1`.
