# WaveFloater

!!! info "v1.4.8"

Rocks and bobs the entity. Oscillation tilts it. Bounce slides it. The two buttons fill a large-object preset and a small-object preset.

Find placed ones with `s:WaveFloater`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the mesh that should move. Map-icon ships and loose props such as pots use it. Saving the scene puts the entity back on the frame captured when the script started, so the motion is not baked into the transform.

## What you set

The two buttons are not stored. The ship prefab stores the fourteen fields in this order.

| Panel field | What to do with it |
|---|---|
| **largeObject** | Button. Turns on X and Y tilt, angle `7.4`, frequency `1.5`, and all three bounces (X `2` / `0.1`, Y `0.2` / `0.5`, Z `0.6` / `0.22`). |
| **smallObject** | Button. Turns on X, Y, and Z tilt, angle `11`, frequency `1`, and all three bounces (X `1.5` / `0.3`, Y `1.5` / `0.2`, Z `1` / `0.1`). |
| **oscillateAtX** | Tilt on X. |
| **oscillateAtY** | Tilt on Y. |
| **oscillateAtZ** | Tilt on Z. |
| **oscillationFrequency** | Tilt speed. Class default `1`. |
| **maxOscillationAngle** | Tilt amount in degrees, clamped to `0`–`90`. Class default `10`. |
| **bounceX** | Slide on X. |
| **bounceXFrequency** | X slide speed. Default `14`. |
| **maxBounceXDistance** | X slide distance. Default `0.3`. |
| **bounceY** | Slide on Y. |
| **bounceYFrequency** | Y slide speed. Default `14`. |
| **maxBounceYDistance** | Y slide distance. Default `0.3`. |
| **bounceZ** | Slide on Z. |
| **bounceZFrequency** | Z slide speed. Default `14`. |
| **maxBounceZDistance** | Z slide distance. Default `0.3`. |

## What it does

Any checked tilt axis rocks the entity between plus and minus the angle. Any checked bounce axis slides it by that distance. Tilt and bounce run together. Turning the last tilt off, and the last bounce off, puts the entity back on the start frame.

## Where it lives

Managed script. The map-icon ships are in `Modules\Native\Prefabs\map_icon_parts.xml`.

## Examples

**Map-icon ship, tilt only.** `map_icon_parts.xml`, entity `map_icon_ship_b`. Y and Z tilt, angle `15`, frequency `1`. All three bounces off. Bounce speeds stay `14` and distances stay `0.3`.

**Training-field pot, tilt and bounce.** `StoryMode\SceneObj\training_field`. Entity `pot_b`. All three tilts on, angle `8`, and all three bounces on. Distances are `0.1`, `0.16`, and `0.1`. A second pot uses angle `9` and a Y distance of `0.12`.
