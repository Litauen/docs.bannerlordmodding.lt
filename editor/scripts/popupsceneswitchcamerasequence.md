# PopupSceneSwitchCameraSequence

!!! info "v1.4.8"

After the initial wait, moves the `customcamera` tag onto a named entity. Positive and negative do nothing.

Find placed ones in the Scene panel with `s:PopupSceneSwitchCameraSequence`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to an entity whose tag is the **name** of the `PopupSceneSpawnPoint` that should trigger it. **EntityName** is the camera entity to promote, not this entity.

No vanilla scene stores it.

## What you set

Own field first, then the three times from [PopupSceneSequence](/editor/scripts/popupscenesequence/).

| Panel field | What to do with it |
|---|---|
| **EntityName** | Name of the entity that should become the camera. Default empty, which does nothing. |
| **InitialActivationTime** | Seconds before the tag moves. Default `0`. |
| **PositiveActivationTime** | Seconds before the empty positive step. Default `0`. |
| **NegativeActivationTime** | Seconds before the empty negative step. Default `0`. |

## What it does

On the initial state, the current `customcamera` tag is removed and added to the entity named **EntityName**. The positive and negative steps are empty.

The notification plays the initial state when the scene opens.

## Where it lives

Managed script, on the sequence base. No `scene.xscene` and no prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it.

## Examples

No placed entity. The spawn point is the caller: it finds entities tagged with its own name and runs the sequence timer. Vanilla notification cameras put `customcamera` on the camera entity directly, as `camera_static` does in `scn_third_banner_piece_notification`.
