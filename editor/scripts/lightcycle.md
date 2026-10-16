# LightCycle

!!! info "v1.4.8"

Turns a light, and the flame meshes under it, on at night and off during the day. **alwaysBurn** keeps it on.

Find placed ones with `s:LightCycle`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the light entity. The script reads the light on that entity and shows or hides every child. Torches, candles, and town lights use it. Day is from hour `6` through hour `20`. Before `6` or after `20`, or in an indoor atmosphere, the light stays on.

If the mission starts while the light is off, the children are removed and the script is removed. An editor tick only toggles visibility, so you can still preview the cycle.

## What you set

| Panel field | What to do with it |
|---|---|
| **alwaysBurn** | On: the light and its children stay visible at every hour. Off: night, or an indoor atmosphere, only. |

## What it does

The light component is shown or hidden with the children. A torch that is off at mission start loses its flame entities for that mission. **alwaysBurn** skips the hour test.

## Where it lives

`Modules\Native\Prefabs\lighting.xml` is the main set (`town_light`, `torch_wall_a`, `torch_wall_b`, `torch_long_a`, `candle_flame`, and others). Wall torches are also in `Modules\Native\Prefabs\archhitecture_aserai.xml`. Kitchen, particle, and quest props copy the same script.

## Examples

**Town light, night only.** `lighting.xml`, `town_light`. **alwaysBurn** `false`.

**Wall torch that always burns.** `lighting.xml`, `torch_wall_b`. **alwaysBurn** `true`. `torch_wall_a_cycleoff` in the same file also stores `true`.

**Candle, night only.** `lighting.xml`, `candle_flame`. **alwaysBurn** `false`.

**Castle override.** `SandBoxCore\SceneObj\empire_castle_004` places `torch_wall_a` and `torch_wall_c` with **alwaysBurn** `true`, and `torch_long_a` with **alwaysBurn** `true`. `candle_flame` entities in that scene keep **alwaysBurn** `false`. The `torch_wall_a` prefab itself stores `false`, so the scene is the override.

**Town walls.** `SandBoxCore\SceneObj\khuzait_town_g` places `torch_wall_a` with **alwaysBurn** `true`.
