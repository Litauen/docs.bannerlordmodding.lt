# SnowAndRainTextureDefiner

!!! info "v1.4.8"

Paints the campaign-map snow and rain mask. **SnowAndRainTexture** is the mask. **WeatherNodeGridWidthAndHeight** is how many nodes that mask is divided into.

Find placed ones in the Scene panel with `s:SnowAndRainTextureDefiner`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on one entity in a map scene that has terrain. The ready-made prefab is `MapSnowAndRainDefiner` in `SandBox\Prefabs\map_icons.xml`.

## What you set

That prefab stores this order.

| Panel field | What to do with it |
|---|---|
| **SnowAndRainTexture** | The mask texture. The prefab stores `main_map_snow_flowmap`. Changing it in the editor updates the dynamic snow texture when the scene has terrain. |
| **WeatherNodeGridWidthAndHeight** | Grid size passed with the texture. The prefab stores `32`. |

## What it does

On the campaign map, the texture and the grid size are handed to the map as the snow and rain data. A terrain reload applies them again.

## Where it lives

Managed script. The only prefab is `MapSnowAndRainDefiner`. No scene file stores the script.

## Examples

**Map mask.** `SandBox\Prefabs\map_icons.xml`, entity `MapSnowAndRainDefiner`. Texture `main_map_snow_flowmap`, grid `32`.
