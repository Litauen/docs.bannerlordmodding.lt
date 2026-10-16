# rotator

!!! info "v1.4.8"

Spins the entity. **Speed** is the only stored field.

Find placed ones in the Scene panel with `s:rotator`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the entity that should turn. Children turn with it. A camera rig uses it on an empty parent. A fly cloud uses it on the particle entity.

## What you set

Native panel.

| Panel field | What to do with it |
|---|---|
| **Speed** | Spin rate. Fly clouds store `100`. The banner-piece camera stores `25`. |

## What it does

The entity keeps turning at **Speed**. On the fly prefab the same entity also has an ambient sound. On the banner screen the camera is a child of the rotator, so the view orbits.

## Where it lives

Native script. No standalone prefab stores the script. The placements are scene entities.

## Examples

**Flies over a skirmish pit.** `Native\SceneObj\mp_skirmish_map_007`. Entity `flow_flies`. **Speed** is `100`. The parent cloud also has an ambient fly sound. The winter copy of this scene uses the same speed.

**Orbiting camera.** `SandBox\SceneObj\scn_first_banner_piece_notification`. Entity `camera_rotator`. **Speed** is `25`. The child is `camera_static`. `scn_second_banner_piece_notification` has the same script.
