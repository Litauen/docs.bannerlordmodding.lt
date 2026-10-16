# ScenePropPositiveLight

!!! info "v1.4.8"

Paints a direct color and an ambient color onto the entity’s first meta-mesh, plus a flatness vector. No vanilla scene or prefab stores this script.

Find placed ones in the Scene panel with `s:ScenePropPositiveLight`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an entity that has a meta-mesh. The prefab named `_positive_light` in `Modules\Native\Prefabs\lighting.xml` does not use this script. It uses [ScenePropNegativeLight](/editor/scripts/scenepropnegativelight/) on `negative_light_mesh`.

## What you set

| Panel field | What to do with it |
|---|---|
| **Flatness_X** | Flatness along X. Default `0`. |
| **Flatness_Y** | Flatness along Y. Default `0`. |
| **Flatness_Z** | Flatness along Z. Default `0`. |
| **DirectLightRed** | Direct color, red. Default `1`. |
| **DirectLightGreen** | Direct color, green. Default `1`. |
| **DirectLightBlue** | Direct color, blue. Default `1`. |
| **DirectLightIntensity** | Direct strength. Clamped from `0` to `10`. Default `1`. |
| **AmbientLightRed** | Ambient color, red. Default `0`. |
| **AmbientLightGreen** | Ambient color, green. Default `0`. |
| **AmbientLightBlue** | Ambient color, blue. Default `1`. |
| **AmbientLightIntensity** | Ambient strength. Clamped from `0` to `10`. Default `1`. |

## What it does

The direct color and intensity become the mesh’s first color factor. The ambient color and intensity become the second. Flatness is written to the mesh vector with a fourth component of `1`. Init and the editor tick both apply it. Without a meta-mesh, the script does nothing.

## Where it lives

Managed script. No `scene.xscene` and no prefab under Native, SandBox, SandBoxCore, StoryMode, or NavalDLC stores it.

## Examples

No placed entity. The lighting prefab `_positive_light` is a negative-light volume: script `ScenePropNegativeLight`, **Is_Dark_Light** on, **Alpha** `1`.
