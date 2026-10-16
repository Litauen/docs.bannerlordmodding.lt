# animation_instance

!!! info "v1.4.8"

Plays a skeleton clip on the entity. A second clip can be set as the morph animation.

Find placed ones with `s:animation_instance`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on an entity that already has that skeleton. Banners, cutscene characters, and birds use it.

## What you set

Native panel. The banner prefab stores this order.

| Panel field | What to do with it |
|---|---|
| **Skeleton Name** | Skeleton to play on. |
| **Animation Name** | Clip name. |
| **Morph Animation Name** | Second clip. Opened banners and the father cutscene leave it empty. |
| **Speed Multiplier** | Clip speed. Opened banners and the father store `1`. |
| **Animation Phase** | Start phase. Opened placements store `0`. |
| **Morph Animation Phase** | Morph start phase. Opened placements store `0`. |
| **Freeze Animation** | Checkbox. Every placement opened below stores off. |

## What it does

The entity stores **Animation Name** on **Skeleton Name**. **Morph Animation Name** is a second clip when it is filled in. The bird stores **Speed Multiplier** `0`.

## Where it lives

Native script. The banner is `Modules\Native\Prefabs\Banner.xml`. Cutscene characters are in `SandBox\Prefabs\cutscene_props.xml`.

## Examples

**Dragon banner.** `Banner.xml`, entity `dragon_banner_animated`. Skeleton `dragon_banner_skeleton`, clip `banner_animated_end`. Morph empty, speed `1`, both phases `0`, freeze off.

**Cutscene father.** `cutscene_props.xml`, entity `father_character`. Skeleton `human_skeleton`, clip `act_cutscene_heir_coming_of_age_father`. Morph empty, speed `1`, freeze off. The entity is editor-only.

**Flying bird, speed held at zero.** `Native\SceneObj\mp_sergeant_map_005`. Entity `Bird`. Skeleton `bird_skeleton`. **Animation Name** and **Morph Animation Name** are both `anim_bird_flying`. **Speed Multiplier** is `0`. A second `Bird` uses path name `forest` on its navigator and the same clip.
