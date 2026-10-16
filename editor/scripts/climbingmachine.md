# ClimbingMachine

!!! info "v1.4.8"

A climbing net. The user is pulled up the standing point until they reach the child tagged `climb_end`, then the climb ends.

Find placed ones in the Scene panel with `s:ClimbingMachine`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the net entity. Add standing-point children for the climbers, and one child tagged `climb_end` at the top. The prompt is **Climbing Net**, action **Use**.

The script adds no fields of its own. Ships also collect every `ClimbingMachine` under the hull as a boarding detachment.

## What you set

`NavalDLC\Prefabs\ship_machines.xml` stores the usable-machine rows at their defaults.

| Panel field | What to do with it |
|---|---|
| **PilotStandingPointTag** | Tag of the pilot standing point. Default `Pilot`. |
| **AmmoPickUpTag** | Ammo point tag. Default `ammopickup`. The net does not use one. |
| **WaitStandingPointTag** | Wait-point tag. Default `Wait`. |
| **NavMeshPrefabName** | Navmesh prefab. Vanilla leaves it empty. |

## What it does

A user on a standing point plays `act_climb_net` and is moved up the point. Near the `climb_end` child the action becomes `act_climb_net_ending`, then `act_climb_net_ending_continue`. At the end of that action the user is released and gravity is turned back on. While someone is climbing, the other standing points on that net are deactivated.

## Where it lives

Managed script on the usable-machine base. Loose prefabs are in `NavalDLC\Prefabs\ship_machines.xml`. More copies are packed in `NavalDLC\AssetPackages\nested_prefabs_packed.xml`.

## Examples

**Test net.** `NavalDLC\Prefabs\ship_machines.xml`, entity `ship_climber`. The four tags are the defaults. A child is tagged `climb_end` (another child is tagged `climb_begin`; the climb uses `climb_end`).

**Tuned longboat net.** Same file, entity `longboat_climber_test_fine_tuned`. Same four defaults, and its own `climb_end` child.

**Packed longboat net.** `nested_prefabs_packed.xml`, entity `longboat_climber_nested`. Same four defaults. A child is named `climb_point`, and `climb_end` is present on that prefab.

**Sea conversation hull.** `NavalDLC\SceneObj\conversation_scene_sea_multi_agent`. A `longboat_rope_climber_small_nested` under `climbers_holder` stores the same four defaults.
