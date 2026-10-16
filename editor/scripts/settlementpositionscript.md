# SettlementPositionScript

!!! info "v1.4.8"

Editor tool on the campaign map. It writes settlement positions into the module’s `settlements.xml`, and it can rebuild the settlement distance cache.

Find placed ones in the Scene panel with `s:SettlementPositionScript`. See [Filter](/editor/editor/#filter).

## Where you put it

One empty entity in the main map scene. Both vanilla maps name it `settlements_scripts`.

## What you set

| Panel field | What to do with it |
|---|---|
| **_partyNavigationModelOverriddenClassName** | Navigation model class name. The land map stores `DefaultPartyNavigationModel`. The naval map stores `NavalPartyNavigationModel`. |
| **_distanceModelOverridenClassName** | Distance model class name. The land map stores `DefaultMapDistanceModel`. The naval map stores `NavalDLCMapDistanceModel`. |
| **CheckPositions** | Button. Walks settlements and frames the editor on the first gate or port that sits on a blocked navmesh. |
| **SavePositions** | Button. Writes positions into `settlements.xml`. Saving the scene does this too. |
| **ComputeAndSaveSettlementDistanceCache** | Button. Builds the distance cache. The editor stays busy until it finishes. The module needs a `ModuleData\DistanceCaches` folder first. |

The buttons are not saved.

## What it does

For each settlement id that has a campaign entity of the same name, **SavePositions** writes `posX` and `posY` from that entity. A child tagged `main_map_city_gate` writes `gate_posX` and `gate_posY`. A child tagged `main_map_city_port` or `main_map_village_dropoff` writes `port_posX` and `port_posY`.

The cache button writes `settlements_distance_cache_Default.bin`. On a naval map it also writes the `Naval` and `All` caches, next to that file.

## Where it lives

Managed script. The scene reads `ModuleData/settlements.xml` next to the map’s module. The cache files go in that module’s `ModuleData/DistanceCaches`.

## Examples

**Land campaign map.** `SandBox\SceneObj\Main_map`. Entity `settlements_scripts`. Navigation `DefaultPartyNavigationModel`. Distance `DefaultMapDistanceModel`.

**Naval campaign map.** `NavalDLC\SceneObj\Main_map`. Entity `settlements_scripts`. Navigation `NavalPartyNavigationModel`. Distance `NavalDLCMapDistanceModel`.
