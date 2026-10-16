# Lightning

!!! info "v1.4.8"

A storm flash on a map icon. It moves the entity, pulses two lights, and plays a thunder hit. The bolt mesh is the particle already on the entity.

Find placed ones with `s:Lightning`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on an entity that has two light components and a particle. The first light is the bright flash. The second light is dimmer. Vanilla hangs that entity under the rain-cloud child of a campaign-map storm icon.

The script also shifts the entity in X and Y, inside **LightTravelRadius**, each time a bolt finishes. Leave room for that shift.

## What you set

Both storm icons store the same numbers except **IsBoltEnabled**.

| Panel field | What to do with it |
|---|---|
| **LightIntensity** | Peak brightness. Prefab: `9999`. The hour of the day scales this further. |
| **LightningRate** | Seconds between flashes. Prefab: `10`. The first wait is a random fraction of this. |
| **BoltSpeed** | How fast the flash fades, and how fast the bolt timer runs. Prefab: `1.5`. |
| **LightTravelRadius** | How far the entity jumps, in X and Y, when a bolt ends. Prefab: `15`. |
| **BoltLife** | Length of one flash. Prefab: `1`. Each flash is two bolts back to back. |
| **IsBoltEnabled** | On: the particle plays with the flash. Off: the lights still pulse, and the particle stays paused. |

## What it does

Each cycle waits **LightningRate** seconds, then runs two flashes. During a flash the two lights ramp down from **LightIntensity**. At night (before `3.5` or after `21`) the lights stay in a low range. In the day they are much brighter. When **IsBoltEnabled** is on, the particle bursts as the bolt ends and a thunder sound plays at the entity: `event:/map/ambient/node/thunder`.

## Where it lives

`Modules\NavalDLC\Prefabs\naval_map_icons.xml`.

## Examples

No scene file stores this script.

**Typhoon, bolts on.** `psys_mapicon_typhoon`, child `lightning_point_light` under `psys_campaign_event_rain_clouds`. Two lights (radius `50` and `30`) and a particle. **IsBoltEnabled** `true`. Intensity `9999`, rate `10`, speed `1.5`, travel `15`, life `1`.

**Dark clouds, lights only.** `psys_mapicon_darkclouds` uses the same child and the same numbers, with **IsBoltEnabled** `false`.

`psys_mapicon_lightclouds` and the snow icon in that file have no `Lightning` script.
