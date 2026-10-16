# Passage

!!! info "v1.4.8"

A door volume. The child use point is what the player and AI actually use. This script supplies the action text from that point.

Find placed ones in the Scene panel with `s:Passage`. See [Filter](/editor/editor/#filter).

## Where you put it

Use a barrier prefab from `Modules\SandBox\Prefabs\sp_editor_barriers.xml`. The root has this script, tag `npc_passage`, and a barrier physics shape. The child `passage_use_point` is tagged `Pilot` and has **PassageUsePoint**. Scale the barrier to the doorway. These barriers are editor-only.

## What you set

No fields of its own. The prefab stores the usable-machine rows:

| Panel field | What to do with it |
|---|---|
| **PilotStandingPointTag** | Must match the use-point tag. Prefab `Pilot`. |
| **AmmoPickUpTag** | Prefab `ammopickup`. Not used by the door. |
| **WaitStandingPointTag** | Prefab `Wait`. |
| **NavMeshPrefabName** | Left empty. |

The door target is **ToLocationId** and **IsMissionExit** on the child, not here.

## What it does

The action text and description come from the pilot point. AI treats the machine as a passage. The child decides whether the door changes location or ends the mission.

## Where it lives

Managed script. Prefabs in `Modules\SandBox\Prefabs\sp_editor_barriers.xml`. Names include `_barrier_house_1` through `_barrier_house_3`, `_barrier_shop_01` through `_barrier_shop_10`, `_barrier_passage_tavern`, `_barrier_passage_exit_mission`, `_barrier_passage_center`, `_barrier_passage_castle`, `_barrier_passage_prison`, `_barrier_passage_arena`, `_barrier_passage_port`, and `_barrier_mission_exit`.

## Examples

**House door.** `_barrier_house_1`. Child **ToLocationId** `house_1`.

**Exit that stays a location door.** `_barrier_passage_exit_mission`. Child **ToLocationId** `center`, **IsMissionExit** off.

**Mission exit.** `_barrier_mission_exit`. Child **ToLocationId** empty, **IsMissionExit** on.

**Scene turns the exit-mission barrier into a real exit.** `StoryMode\SceneObj\villa_singular_c`. Prefab `_barrier_passage_exit_mission`. The scene sets the child **IsMissionExit** on and **ToLocationId** empty.
