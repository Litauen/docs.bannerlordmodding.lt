# CameraJumpScript

!!! info "v1.4.8"

Moves this entity to a stored position and rotation after a wait. The cutscene camera uses it to cut to a second frame.

Find placed ones in the Scene panel with `s:CameraJumpScript`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to the camera entity, next to the camera scripts. In the sister-rescue cutscene it sits on `camera_static` under `Camera_Root`.

## What you set

`NavalDLC\SceneObj\cutscene_saving_sister` stores the first three rows. The last three are buttons and are not written into the scene.

| Panel field | What to do with it |
|---|---|
| **WaitBeforeCameraJump** | Seconds before the jump. Code default is `2`. This cutscene stores `9`. |
| **CameraJumpPosition** | World position the entity jumps to. |
| **CameraJumpRotation** | Euler rotation, in radians, applied at the jump. |
| **SetCurrentCameraTransform** | Button. Copies the entity's current position and rotation into the two fields above. |
| **Preview** | Button. Starts the wait, then jumps. |
| **Reset** | Button. Puts the entity back where it was when the scene opened in the editor, and cancels the wait. |

## What it does

When the mission starts, the wait begins immediately. After **WaitBeforeCameraJump** seconds the entity frame becomes **CameraJumpPosition** and **CameraJumpRotation**. In the editor, **Preview** runs that same wait. **Reset** restores the frame from when the entity was opened.

## Where it lives

Managed script. The only scene that stores it is `NavalDLC\SceneObj\cutscene_saving_sister`. No prefab stores it.

## Examples

**Sister-rescue cut.** That scene, entity `camera_static` under `Camera_Root`, tags `customcamera` and `camera_instance`. **WaitBeforeCameraJump** is `9`. **CameraJumpPosition** is `-15.275, 16.227, 5.274`. **CameraJumpRotation** is `0.841, -0.030, -1.778`. The same entity also has `camera_instance` and `PopupSceneCameraPath`.
