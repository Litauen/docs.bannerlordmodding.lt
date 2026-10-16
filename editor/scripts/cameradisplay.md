# CameraDisplay

!!! info "v1.4.8"

An editor camera on an entity. While the entity is selected, the editor draws that camera's frustum and a small view of the scene from the entity. The script has one unused number.

Find placed ones in the Scene panel with `s:CameraDisplay`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to an empty entity. The entity frame is the camera frame. Select the entity to see the frustum and the small view.

## What you set

No prefab stores a variable. The only public field is not read by the script.

| Panel field | What to do with it |
|---|---|
| **renderOrder** | Public number, default `0`. This script never reads it. |

## What it does

The view is a fifth of the screen on each side. It clears depth and leaves the colour buffer. The camera uses the entity frame, a 45-degree vertical field, near `0.2`, and far `200`. Deselecting the entity turns the view off. Removing the entity drops the view.

## Where it lives

Managed script. No scene and no prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it. No other vanilla class adds it at runtime.

## Examples

No placement in those modules. The behaviour above is what the script does when the entity is selected in the editor.
