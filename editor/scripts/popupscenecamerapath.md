# PopupSceneCameraPath

!!! info "v1.4.8"

Camera for a scene notification. Three slots: initial, positive, and negative. Each slot is either a path or a skeleton clip, plus an optional sound.

Find placed ones in the Scene panel with `s:PopupSceneCameraPath`. See [Filter](/editor/editor/#filter).

## Where you put it

On the camera entity tagged `camera_instance` (vanilla name `camera_static`). A skeleton camera also needs the skeleton named in **SkeletonName**. A path camera needs a scene path and, if it should look at something, an entity named in **LookAtEntity**. The notification finds the first entity with this script.

## What you set

`cutscene_props.xml` stores the rows in this order. The three **Test** buttons are on the panel and are not saved.

| Panel field | What to do with it |
|---|---|
| **LookAtEntity** | Entity name to aim at. Empty keeps the path frame's rotation. Born-baby uses `lookat2`. |
| **SkeletonName** | Skeleton on this entity. `skeleton_camera` when the camera is animated. Empty uses the path instead. |
| **BoneIndex** | Bone the camera reads. `0`. |
| **AttachmentOffset** | Offset from that bone. `0, 0, 0`. |
| **InitialPath** | Path name for the opening shot. Empty if a skeleton clip is used. |
| **InitialAnimationClip** | Skeleton clip for the opening shot. |
| **InitialSound** | Sound id. Code default is `event:/mission/siege/siegetower/doorland`. Scenes replace it or clear it. |
| **InitialPathStartTime** | Seconds before the move starts. Default `0`. |
| **InitialPathDuration** | Seconds the move takes. Default `1`. |
| **InitialInterpolation** | `Linear`, `EaseIn`, `EaseOut`, or `EaseInOut`. |
| **InitialFadeOut** | On: the camera fade follows this move. |
| **PositivePath** | Path for the positive choice. |
| **PositiveAnimationClip** | Clip for the positive choice. |
| **PositiveSound** | Sound for that choice. Often empty. |
| **PositivePathStartTime** | Delay. Default `0`. |
| **PositivePathDuration** | Duration. Default `1`. |
| **PositiveInterpolation** | Same four blends. |
| **PositiveFadeOut** | Fade with the positive move. |
| **NegativePath** | Path for the negative slot. |
| **NegativeAnimationClip** | Clip for the negative slot. |
| **NegativeSound** | Sound for the negative slot. |
| **NegativePathStartTime** | Delay. Default `0`. |
| **NegativePathDuration** | Duration. Default `1`. |
| **NegativeInterpolation** | Same four blends. |
| **NegativeFadeOut** | Fade with the negative move. |
| **TestInitial** | Editor button. Plays the initial slot. |
| **TestPositive** | Editor button. Plays the positive slot. |
| **TestNegative** | Editor button. Plays the negative slot. |

If **SkeletonName** is set, the bone drives the camera and the path is ignored. If it is empty and a path is set, the camera rides that path and looks at **LookAtEntity** when that name exists.

## What it does

The notification starts the initial slot. The positive choice plays the positive slot. The negative slot is what **TestNegative** plays in the editor. A non-empty sound on the active slot plays from the camera. **InitialFadeOut** (and the matching flags) drive the fade from 0 to 1 across that move.

## Where it lives

Managed script. Camera prefabs in `Modules\SandBox\Prefabs\cutscene_props.xml`, entity `camera_static` under `camera_parent`. Notification scenes under `Modules\SandBox\SceneObj` and `Modules\NavalDLC\SceneObj` place that camera.

## Examples

**Skeleton, conspiracy letter.** `cutscene_props.xml`, `camera_static`. **SkeletonName** `skeleton_camera`, clip `conspiracy_supports_camera_anim`, sound cleared, duration `1`, linear, no fade.

**Heir, delayed clip.** Same file, the second `camera_static`. Clip `cutscene_heir_coming_of_age_camera`, sound `event:/mission/siege/siegetower/doorland`, start `40`, duration `5`, **InitialFadeOut** on.

**Path camera, newborn.** `SandBox\SceneObj\scn_born_baby`. **LookAtEntity** `lookat2`, **SkeletonName** empty, **InitialPath** `campath2`, sound `event:/ui/cutscenes/child_newborn`, duration `55`, **EaseOut**.

**Naval death, long hold.** `NavalDLC\SceneObj\scn_cutscene_main_hero_naval_battle_death`. **LookAtEntity** `camera_look_entity`, skeleton `skeleton_camera`, no clip, sound `event:/ui/cutscenes/death_naval`, duration `45`, linear. **PositiveFadeOut** is on.
