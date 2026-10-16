# PopupSceneSkeletonAnimationScript

!!! info "v1.4.8"

Plays a skeleton clip on this entity, then a continue clip when that clip finishes. The notification camera path sets the state on every copy in the scene.

Find placed ones in the Scene panel with `s:PopupSceneSkeletonAnimationScript`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the entity that already has the skeleton, or let **SkeletonName** create one. State `0` is initial, `1` is positive, anything else is negative.

## What you set

| Panel field | What to do with it |
|---|---|
| **SkeletonName** | Skeleton resource. Empty removes a skeleton that is already on the entity. |
| **BoneIndex** | Stored. Default `0`. This script does not read it. |
| **AttachmentOffset** | Stored. Default `(0, 0, 0)`. This script does not read it. |
| **InitialAnimationClip** | Clip for state `0`. |
| **PositiveAnimationClip** | Clip for state `1`. |
| **NegativeAnimationClip** | Clip for any other state. |
| **InitialAnimationContinueClip** | Plays when the initial clip reaches the end. |
| **PositiveAnimationContinueClip** | Plays when the positive clip reaches the end. |
| **NegativeAnimationContinueClip** | Plays when the negative clip reaches the end. |

An empty clip is skipped.

## What it does

The camera-path script finds every copy, creates the skeleton from **SkeletonName** when that name is set, and on each state change plays the matching clip. When that clip’s parameter reaches `1`, the continue clip replaces it.

## Where it lives

Managed script. Two notification scenes store it. Both are dragon-banner shots.

## Examples

**Banner that assembles, then holds.** `SandBox\SceneObj\scn_third_banner_piece_notification`. Entity `dragon_banner_animated`, tag `banner_1`, skeleton `dragon_banner_skeleton`, scale `0.01`. Initial clip `anim_main_story_dragon_banner_idle_start`. Positive clip `anim_main_story_dragon_banner_assemble`, then continue `anim_main_story_dragon_banner_idle_end`. Negative clips are empty.

**Banner head that plays once.** `SandBox\SceneObj\scn_second_banner_piece_notification`. Entity `Banner_Head` under `Banner_Root`, tag `banner_1`, skeleton `dragon_banner_head_only_skeleton`. Initial clip `anim_main_story_banner_head_only_animation`. The other clips are empty.
