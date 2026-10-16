# ShipControllerMachine

!!! info "v1.4.8"

The helm a crewman uses to steer the ship, including the rudder pose and the three chase cameras. On your own ship the description is **Control the Ship**. On an enemy ship that can be taken it is **Capture the ship**.

Find placed ones in the Scene panel with `s:ShipControllerMachine`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the helm entity. Name a child `controller` for the rudder mesh, and put `hand_position` under it or under the helm. Optional children: `camera_target`, `shoulder_camera_target`, `front_camera_target`. If there is no `controller` child, a descendant of the ship tagged `rudder_rotation_entity` is used.

Every descendant, including this entity, needs **Does not Affect Parent's Local Bounding Box**.

Packed prefabs: `ShipControl_nested`, `ShipControlRudder_nested`, `rudder_right`, and per-ship overrides on `control_point_holder` in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`. The loose editor cube is `ShipControl` in `NavalDLC\Prefabs\ship_machines.xml`.

## What you set

`ShipControl_nested` stores the full row set. Code defaults are listed where that prefab matches them. A holder copy writes only the rows it changes.

| Panel field | What to do with it |
|---|---|
| **_rudderRotationEntity** | Public entity link. Prefabs do not write it. At runtime it is the `controller` child, or the `rudder_rotation_entity` tag. |
| **_cameraOffset** | Back camera offset. Code default `0, -20, 5`. `ShipControl_nested` stores that. The loose `ShipControl` stores `0, -18, 5`. |
| **_shipCaptureAction** | Capture clip. Code default `act_ship_capture`. `ShipControl_nested` stores that. `rudder_right` stores `act_rudder_backward_stand_idle`. |
| **_shipControlActionTurnLeft** | Left-turn clip. Code default `act_rudder_backward_push_idle`. |
| **_shipControlActionTurnRight** | Right-turn clip. Code default `act_rudder_backward_pull_idle`. |
| **_shipControlActionRelaxed** | Idle helm clip. Code default `act_rudder_backward_stand_idle`. |
| **_isRightHandOnly** | On: the right hand drives the helm. `ShipControl_nested` stores on. `rudder_right` stores off. |
| **_shoulderCameraOffset** | Shoulder camera offset. Code default `0, 0, 0`. |
| **_isLeftHandOnly** | On: the left hand drives the helm. `ShipControl_nested` stores off. |
| **_frontCameraOffset** | Front camera offset. Code default `0, -10, 2`. |
| **_shoulderCameraDistance** | Shoulder camera distance. Code default `2`. |
| **_frontCameraDistance** | Front camera distance. Code default `10`. |
| **_cameraFovMultiplier** | Back camera field of view. Code default `1`. |
| **_shoulderCameraFovMultiplier** | Shoulder field of view. Code default `1`. |
| **_frontCameraFovMultiplier** | Front field of view. Code default `1`. |
| **PilotStandingPointTag** | Pilot stand tag. Prefabs store `Pilot`. |
| **AmmoPickUpTag** | Ammo stand tag. Prefabs store `ammopickup`. The helm does not use one. |
| **WaitStandingPointTag** | Wait stand tag. Prefabs store `Wait`. |
| **NavMeshPrefabName** | Navmesh prefab. Prefabs leave it empty. |

The loose `ShipControl` writes only **_cameraOffset**, the three standing-point tags, and **NavMeshPrefabName**.

## What it does

A pilot at the standing point plays the relaxed, left, or right clip and the rudder mesh turns with the input. Taking an enemy helm plays **_shipCaptureAction** for `3` seconds. The three camera offsets are the back, shoulder, and front views while the player steers.

## Where it lives

Managed script on a usable machine. Loose prefab `ShipControl` in `NavalDLC\Prefabs\ship_machines.xml`. Packed copies in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`.

## Examples

No scene file stores this script.

**Full helm.** Packed prefab `ShipControl_nested`. Right hand only, capture clip `act_ship_capture`, back camera `0, -20, 5`, front camera `0, -10, 2`.

**Rudder with both hands.** Packed prefab `rudder_right`. Both hand-only flags off, capture clip `act_rudder_backward_stand_idle`, back camera `5, -20, 5`.

**A ship that pulls the camera back.** Packed prefab `control_point_holder`, entity `ShipControl_nested`. One copy stores back camera `0, -64, 21.5`, front distance `15`, left hand only, and the three control clips `act_roundship_captain_idle`.
