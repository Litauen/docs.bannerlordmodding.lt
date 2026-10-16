# SkeletonAnimatedCamera

!!! info "v1.4.8"

Drives an entity tagged `camera_instance` from a bone on this entity's skeleton. **SkeletonName** and **AnimationName** pick the clip. **BoneIndex** picks the bone.

Find placed ones in the Scene panel with `s:SkeletonAnimatedCamera`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to an empty entity in a cutscene. The scene also needs an entity tagged `camera_instance`. The three vanilla cutscenes leave **AnimationName** empty, so they do not build a skeleton until that name is set.

## What you set

The cutscenes store this order. **Restart** is a button, so it is not in the saved rows.

| Panel field | What to do with it |
|---|---|
| **SkeletonName** | Skeleton to create. Default `human_skeleton`. The cutscenes store that. |
| **BoneIndex** | Bone the camera follows. The cutscenes store `0`. |
| **AttachmentOffset** | Shift added after the bone frame. The cutscenes store `0, 0, 0`. |
| **AnimationName** | Clip played on that skeleton. Empty skips the skeleton. The cutscenes store empty. |
| **Restart** | Builds the skeleton again and resyncs animation points in the scene. |

Changing **SkeletonName** or **AnimationName** rebuilds the skeleton when both names are set.

## What it does

Each tick, if the skeleton exists, the entity tagged `camera_instance` is moved to **BoneIndex** plus **AttachmentOffset**. The listener moves with that frame.

## Where it lives

Managed script. No prefab stores it. Three SandBox cutscenes do.

## Examples

**Faction-join cutscene.** `SandBox\SceneObj\scn_cutscene_factionjoin`. Entity `DELETE`. Skeleton `human_skeleton`, bone `0`, offset zero, animation empty.

**Heir coming of age.** `SandBox\SceneObj\scn_cutscene_heir_coming_of_age`. Entity `empty_object` at about `54, 53`. Same four values.

**Female coming of age.** `SandBox\SceneObj\scn_hero_come_of_age_female`. Entity `empty_object` at the same numbers. Same four values.
